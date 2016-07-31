---
layout: post
title: HTML Encoder
date: 2016-07-31
author: "0xEDD1E"
---

### HTML Encoder

##### Description

This is a simple js-regex based HTML Encoder. This encoder is capable of encoding almost all characters defined in `HTML4 DTDs`. As found on [wikipedia](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references). 

##### Implemetation

`htmlenc.js` is the main javascript which contains the encoder. Internally the encoder function uses an array of available characters with their HTML character references (name, unicode hex, unicode decimal). At the time of encoding, encoder just replace all the (encodable) characters with their HTML character references.

#### Source Code

Here is the `htmlenc.js`  

```javascript
/**
 * Created by 0xEDD1E on 2016-06-24.
 */
var entities = [
    ['&amp;'     , '&', '&#x0026;', '&#38;' ],
    ['&quot;'    , '"', '&#x0022;', '&#34;' ],
    ['&apos;'    , "'", '&#x0027;', '&#39;' ],
    ['&lt;'      , '<', '&#x003C;', '&#60;' ],
    ['&gt;'      , '>', '&#x003E;', '&#62;' ],
    ['&iexcl;'   , '¡', '&#x00A1;', '&#161;'],
    ['&cent;'    , '¢', '&#x00A2;', '&#162;'],
    ['&pound;'   , '£', '&#x00A3;', '&#163;'],
    ['&curren;'  , '¤', '&#x00A4;', '&#164;'],
    ['&yen;'     , '¥', '&#x00A5;', '&#165;'],
    ['&brvbar;'  , '¦', '&#x00A6;', '&#166;'],
    ['&sect;'    , '§', '&#x00A7;', '&#167;'],
    ['&uml;'     , '¨', '&#x00A8;', '&#168;'],
    ['&copy;'    , '©', '&#x00A9;', '&#169;'],
    ['&ordf;'    , 'ª', '&#x00AA;', '&#170;'],
    ['&laquo;'   , '«', '&#x00AB;', '&#171;'],
    ['&not;'     , '¬', '&#x00AC;', '&#172;'],
    ['&reg;'     , '®', '&#x00AE;', '&#174;'],
    ['&macr;'    , '¯', '&#x00AF;', '&#175;'],
    ['&deg;'     , '°', '&#x00B0;', '&#176;'],
    ['&plusmn;'  , '±', '&#x00B1;', '&#177;'],
    ['&sup2;'    , '²', '&#x00B2;', '&#178;'],
    ['&sup3;'    , '³', '&#x00B3;', '&#179;'],
    ['&acute;'   , '´', '&#x00B4;', '&#180;'],
    ['&micro;'   , 'µ', '&#x00B5;', '&#181;'],
    ['&para;'    , '¶', '&#x00B6;', '&#182;'],
    ['&middot;'  , '·', '&#x00B7;', '&#183;'],
    ['&cedil;'   , '¸', '&#x00B8;', '&#184;'],
    ['&sup1;'    , '¹', '&#x00B9;', '&#185;'],
    ['&ordm;'    , 'º', '&#x00BA;', '&#186;'],
    ['&raquo;'   , '»', '&#x00BB;', '&#187;'],
    ['&frac14;'  , '¼', '&#x00BC;', '&#188;'],
    ['&frac12;'  , '½', '&#x00BD;', '&#189;'],
    ['&frac34;'  , '¾', '&#x00BE;', '&#190;'],
    ['&iquest;'  , '¿', '&#x00BF;', '&#191;'],
    ['&Agrave;'  , 'À', '&#x00C0;', '&#192;'],
    ['&Aacute;'  , 'Á', '&#x00C1;', '&#193;'],
    ['&Acirc;'   , 'Â', '&#x00C2;', '&#194;'],
    ['&Atilde;'  , 'Ã', '&#x00C3;', '&#195;'],
    ['&Auml;'    , 'Ä', '&#x00C4;', '&#196;'],
    ['&Aring;'   , 'Å', '&#x00C5;', '&#197;'],
    ['&AElig;'   , 'Æ', '&#x00C6;', '&#198;'],
    ['&Ccedil;'  , 'Ç', '&#x00C7;', '&#199;'],
    ['&Egrave;'  , 'È', '&#x00C8;', '&#200;'],
    ['&Eacute;'  , 'É', '&#x00C9;', '&#201;'],
    ['&Ecirc;'   , 'Ê', '&#x00CA;', '&#202;'],
    ['&Euml;'    , 'Ë', '&#x00CB;', '&#203;'],
    ['&Igrave;'  , 'Ì', '&#x00CC;', '&#204;'],
    ['&Iacute;'  , 'Í', '&#x00CD;', '&#205;'],
    ['&Icirc;'   , 'Î', '&#x00CE;', '&#206;'],
    ['&Iuml;'    , 'Ï', '&#x00CF;', '&#207;'],
    ['&ETH;'     , 'Ð', '&#x00D0;', '&#208;'],
    ['&Ntilde;'  , 'Ñ', '&#x00D1;', '&#209;'],
    ['&Ograve;'  , 'Ò', '&#x00D2;', '&#210;'],
    ['&Oacute;'  , 'Ó', '&#x00D3;', '&#211;'],
    ['&Ocirc;'   , 'Ô', '&#x00D4;', '&#212;'],
    ['&Otilde;'  , 'Õ', '&#x00D5;', '&#213;'],
    ['&Ouml;'    , 'Ö', '&#x00D6;', '&#214;'],
    ['&times;'   , '×', '&#x00D7;', '&#215;'],
    ['&Oslash;'  , 'Ø', '&#x00D8;', '&#216;'],
    ['&Ugrave;'  , 'Ù', '&#x00D9;', '&#217;'],
    ['&Uacute;'  , 'Ú', '&#x00DA;', '&#218;'],
    ['&Ucirc;'   , 'Û', '&#x00DB;', '&#219;'],
    ['&Uuml;'    , 'Ü', '&#x00DC;', '&#220;'],
    ['&Yacute;'  , 'Ý', '&#x00DD;', '&#221;'],
    ['&THORN;'   , 'Þ', '&#x00DE;', '&#222;'],
    ['&szlig;'   , 'ß', '&#x00DF;', '&#223;'],
    ['&agrave;'  , 'à', '&#x00E0;', '&#224;'],
    ['&aacute;'  , 'á', '&#x00E1;', '&#225;'],
    ['&acirc;'   , 'â', '&#x00E2;', '&#226;'],
    ['&atilde;'  , 'ã', '&#x00E3;', '&#227;'],
    ['&auml;'    , 'ä', '&#x00E4;', '&#228;'],
    ['&aring;'   , 'å', '&#x00E5;', '&#229;'],
    ['&aelig;'   , 'æ', '&#x00E6;', '&#230;'],
    ['&ccedil;'  , 'ç', '&#x00E7;', '&#231;'],
    ['&egrave;'  , 'è', '&#x00E8;', '&#232;'],
    ['&eacute;'  , 'é', '&#x00E9;', '&#233;'],
    ['&ecirc;'   , 'ê', '&#x00EA;', '&#234;'],
    ['&euml;'    , 'ë', '&#x00EB;', '&#235;'],
    ['&igrave;'  , 'ì', '&#x00EC;', '&#236;'],
    ['&iacute;'  , 'í', '&#x00ED;', '&#237;'],
    ['&icirc;'   , 'î', '&#x00EE;', '&#238;'],
    ['&iuml;'    , 'ï', '&#x00EF;', '&#239;'],
    ['&eth;'     , 'ð', '&#x00F0;', '&#240;'],
    ['&ntilde;'  , 'ñ', '&#x00F1;', '&#241;'],
    ['&ograve;'  , 'ò', '&#x00F2;', '&#242;'],
    ['&oacute;'  , 'ó', '&#x00F3;', '&#243;'],
    ['&ocirc;'   , 'ô', '&#x00F4;', '&#244;'],
    ['&otilde;'  , 'õ', '&#x00F5;', '&#245;'],
    ['&ouml;'    , 'ö', '&#x00F6;', '&#246;'],
    ['&divide;'  , '÷', '&#x00F7;', '&#247;'],
    ['&oslash;'  , 'ø', '&#x00F8;', '&#248;'],
    ['&ugrave;'  , 'ù', '&#x00F9;', '&#249;'],
    ['&uacute;'  , 'ú', '&#x00FA;', '&#250;'],
    ['&ucirc;'   , 'û', '&#x00FB;', '&#251;'],
    ['&uuml;'    , 'ü', '&#x00FC;', '&#252;'],
    ['&yacute;'  , 'ý', '&#x00FD;', '&#253;'],
    ['&thorn;'   , 'þ', '&#x00FE;', '&#254;'],
    ['&yuml;'    , 'ÿ', '&#x00FF;', '&#255;'],
    ['&OElig;'   , 'Œ', '&#x0152;', '&#338;'],
    ['&oelig;'   , 'œ', '&#x0153;', '&#339;'],
    ['&Scaron;'  , 'Š', '&#x0160;', '&#352;'],
    ['&scaron;'  , 'š', '&#x0161;', '&#353;'],
    ['&Yuml;'    , 'Ÿ', '&#x0178;', '&#376;'],
    ['&fnof;'    , 'ƒ', '&#x0192;', '&#402;'],
    ['&circ;'    , 'ˆ', '&#x02C6;', '&#710;'],
    ['&tilde;'   , '˜', '&#x02DC;', '&#732;'],
    ['&Alpha;'   , 'Α', '&#x0391;', '&#913;'],
    ['&Beta;'    , 'Β', '&#x0392;', '&#914;'],
    ['&Gamma;'   , 'Γ', '&#x0393;', '&#915;'],
    ['&Delta;'   , 'Δ', '&#x0394;', '&#916;'],
    ['&Epsilon;' , 'Ε', '&#x0395;', '&#917;'],
    ['&Zeta;'    , 'Ζ', '&#x0396;', '&#918;'],
    ['&Eta;'     , 'Η', '&#x0397;', '&#919;'],
    ['&Theta;'   , 'Θ', '&#x0398;', '&#920;'],
    ['&Iota;'    , 'Ι', '&#x0399;', '&#921;'],
    ['&Kappa;'   , 'Κ', '&#x039A;', '&#922;'],
    ['&Lambda;'  , 'Λ', '&#x039B;', '&#923;'],
    ['&Mu;'      , 'Μ', '&#x039C;', '&#924;'],
    ['&Nu;'      , 'Ν', '&#x039D;', '&#925;'],
    ['&Xi;'      , 'Ξ', '&#x039E;', '&#926;'],
    ['&Omicron;' , 'Ο', '&#x039F;', '&#927;'],
    ['&Pi;'      , 'Π', '&#x03A0;', '&#928;'],
    ['&Rho;'     , 'Ρ', '&#x03A1;', '&#929;'],
    ['&Sigma;'   , 'Σ', '&#x03A3;', '&#931;'],
    ['&Tau;'     , 'Τ', '&#x03A4;', '&#932;'],
    ['&Upsilon;' , 'Υ', '&#x03A5;', '&#933;'],
    ['&Phi;'     , 'Φ', '&#x03A6;', '&#934;'],
    ['&Chi;'     , 'Χ', '&#x03A7;', '&#935;'],
    ['&Psi;'     , 'Ψ', '&#x03A8;', '&#936;'],
    ['&Omega;'   , 'Ω', '&#x03A9;', '&#937;'],
    ['&alpha;'   , 'α', '&#x03B1;', '&#945;'],
    ['&beta;'    , 'β', '&#x03B2;', '&#946;'],
    ['&gamma;'   , 'γ', '&#x03B3;', '&#947;'],
    ['&delta;'   , 'δ', '&#x03B4;', '&#948;'],
    ['&epsilon;' , 'ε', '&#x03B5;', '&#949;'],
    ['&zeta;'    , 'ζ', '&#x03B6;', '&#950;'],
    ['&eta;'     , 'η', '&#x03B7;', '&#951;'],
    ['&theta;'   , 'θ', '&#x03B8;', '&#952;'],
    ['&iota;'    , 'ι', '&#x03B9;', '&#953;'],
    ['&kappa;'   , 'κ', '&#x03BA;', '&#954;'],
    ['&lambda;'  , 'λ', '&#x03BB;', '&#955;'],
    ['&mu;'      , 'μ', '&#x03BC;', '&#956;'],
    ['&nu;'      , 'ν', '&#x03BD;', '&#957;'],
    ['&xi;'      , 'ξ', '&#x03BE;', '&#958;'],
    ['&omicron;' , 'ο', '&#x03BF;', '&#959;'],
    ['&pi;'      , 'π', '&#x03C0;', '&#960;'],
    ['&rho;'     , 'ρ', '&#x03C1;', '&#961;'],
    ['&sigmaf;'  , 'ς', '&#x03C2;', '&#962;'],
    ['&sigma;'   , 'σ', '&#x03C3;', '&#963;'],
    ['&tau;'     , 'τ', '&#x03C4;', '&#964;'],
    ['&upsilon;' , 'υ', '&#x03C5;', '&#965;'],
    ['&phi;'     , 'φ', '&#x03C6;', '&#966;'],
    ['&chi;'     , 'χ', '&#x03C7;', '&#967;'],
    ['&psi;'     , 'ψ', '&#x03C8;', '&#968;'],
    ['&omega;'   , 'ω', '&#x03C9;', '&#969;'],
    ['&thetasym;', 'ϑ', '&#x03D1;', '&#977;'],
    ['&upsih;'   , 'ϒ', '&#x03D2;', '&#978;'],
    ['&piv;'     , 'ϖ', '&#x03D6;', '&#982;'],
    ['&ndash;'   , '–', '&#x2013;', '&#8211;'],
    ['&mdash;'   , '—', '&#x2014;', '&#8212;'],
    ['&lsquo;'   , '‘', '&#x2018;', '&#8216;'],
    ['&rsquo;'   , '’', '&#x2019;', '&#8217;'],
    ['&sbquo;'   , '‚', '&#x201A;', '&#8218;'],
    ['&ldquo;'   , '“', '&#x201C;', '&#8220;'],
    ['&rdquo;'   , '”', '&#x201D;', '&#8221;'],
    ['&bdquo;'   , '„', '&#x201E;', '&#8222;'],
    ['&dagger;'  , '†', '&#x2020;', '&#8224;'],
    ['&Dagger;'  , '‡', '&#x2021;', '&#8225;'],
    ['&bull;'    , '•', '&#x2022;', '&#8226;'],
    ['&hellip;'  , '…', '&#x2026;', '&#8230;'],
    ['&permil;'  , '‰', '&#x2030;', '&#8240;'],
    ['&prime;'   , '′', '&#x2032;', '&#8242;'],
    ['&Prime;'   , '″', '&#x2033;', '&#8243;'],
    ['&lsaquo;'  , '‹', '&#x2039;', '&#8249;'],
    ['&rsaquo;'  , '›', '&#x203A;', '&#8250;'],
    ['&oline;'   , '‾', '&#x203E;', '&#8254;'],
    ['&frasl;'   , '⁄', '&#x2044;', '&#8260;'],
    ['&euro;'    , '€', '&#x20AC;', '&#8364;'],
    ['&image;'   , 'ℑ', '&#x2111;', '&#8465;'],
    ['&weierp;'  , '℘', '&#x2118;', '&#8472;'],
    ['&real;'    , 'ℜ', '&#x211C;', '&#8476;'],
    ['&trade;'   , '™', '&#x2122;', '&#8482;'],
    ['&alefsym;' , 'ℵ', '&#x2135;', '&#8501;'],
    ['&larr;'    , '←', '&#x2190;', '&#8592;'],
    ['&uarr;'    , '↑', '&#x2191;', '&#8593;'],
    ['&rarr;'    , '→', '&#x2192;', '&#8594;'],
    ['&darr;'    , '↓', '&#x2193;', '&#8595;'],
    ['&harr;'    , '↔', '&#x2194;', '&#8596;'],
    ['&crarr;'   , '↵', '&#x21B5;', '&#8629;'],
    ['&lArr;'    , '⇐', '&#x21D0;', '&#8656;'],
    ['&uArr;'    , '⇑', '&#x21D1;', '&#8657;'],
    ['&rArr;'    , '⇒', '&#x21D2;', '&#8658;'],
    ['&dArr;'    , '⇓', '&#x21D3;', '&#8659;'],
    ['&hArr;'    , '⇔', '&#x21D4;', '&#8660;'],
    ['&forall;'  , '∀', '&#x2200;', '&#8704;'],
    ['&part;'    , '∂', '&#x2202;', '&#8706;'],
    ['&exist;'   , '∃', '&#x2203;', '&#8707;'],
    ['&empty;'   , '∅', '&#x2205;', '&#8709;'],
    ['&nabla;'   , '∇', '&#x2207;', '&#8711;'],
    ['&isin;'    , '∈', '&#x2208;', '&#8712;'],
    ['&notin;'   , '∉', '&#x2209;', '&#8713;'],
    ['&ni;'      , '∋', '&#x220B;', '&#8715;'],
    ['&prod;'    , '∏', '&#x220F;', '&#8719;'],
    ['&sum;'     , '∑', '&#x2211;', '&#8721;'],
    ['&minus;'   , '−', '&#x2212;', '&#8722;'],
    ['&lowast;'  , '∗', '&#x2217;', '&#8727;'],
    ['&radic;'   , '√', '&#x221A;', '&#8730;'],
    ['&prop;'    , '∝', '&#x221D;', '&#8733;'],
    ['&infin;'   , '∞', '&#x221E;', '&#8734;'],
    ['&ang;'     , '∠', '&#x2220;', '&#8736;'],
    ['&and;'     , '∧', '&#x2227;', '&#8743;'],
    ['&or;'      , '∨', '&#x2228;', '&#8744;'],
    ['&cap;'     , '∩', '&#x2229;', '&#8745;'],
    ['&cup;'     , '∪', '&#x222A;', '&#8746;'],
    ['&int;'     , '∫', '&#x222B;', '&#8747;'],
    ['&there4;'  , '∴', '&#x2234;', '&#8756;'],
    ['&sim;'     , '∼', '&#x223C;', '&#8764;'],
    ['&cong;'    , '≅', '&#x2245;', '&#8773;'],
    ['&asymp;'   , '≈', '&#x2248;', '&#8776;'],
    ['&ne;'      , '≠', '&#x2260;', '&#8800;'],
    ['&equiv;'   , '≡', '&#x2261;', '&#8801;'],
    ['&le;'      , '≤', '&#x2264;', '&#8804;'],
    ['&ge;'      , '≥', '&#x2265;', '&#8805;'],
    ['&sub;'     , '⊂', '&#x2282;', '&#8834;'],
    ['&sup;'     , '⊃', '&#x2283;', '&#8835;'],
    ['&nsub;'    , '⊄', '&#x2284;', '&#8836;'],
    ['&sube;'    , '⊆', '&#x2286;', '&#8838;'],
    ['&supe;'    , '⊇', '&#x2287;', '&#8839;'],
    ['&oplus;'   , '⊕', '&#x2295;', '&#8853;'],
    ['&otimes;'  , '⊗', '&#x2297;', '&#8855;'],
    ['&perp;'    , '⊥', '&#x22A5;', '&#8869;'],
    ['&sdot;'    , '⋅', '&#x22C5;', '&#8901;'],
    ['&lceil;'   , '⌈', '&#x2308;', '&#8968;'],
    ['&rceil;'   , '⌉', '&#x2309;', '&#8969;'],
    ['&lfloor;'  , '⌊', '&#x230A;', '&#8970;'],
    ['&rfloor;'  , '⌋', '&#x230B;', '&#8971;'],
    ['&lang;'    , '〈', '&#x2329;', '&#9001;'],
    ['&rang;'    , '〉', '&#x232A;', '&#9002;'],
    ['&loz;'     , '◊', '&#x25CA;', '&#9674;'],
    ['&spades;'  , '♠', '&#x2660;', '&#9824;'],
    ['&clubs;'   , '♣', '&#x2663;', '&#9827;'],
    ['&hearts'   , '♥', '&#x2665;', '&#9829;'],
    ['&diams;'   , '♦', '&#x2666;', '&#9830;']
];

var helpstr = [ 'Encode with entity name. eg: &amp;name;',
    'Encode with Unicode Code Codepoint (Decimal). eg: &amp;#nnnn;',
    'Encode with Unicode Code Codepoint (Hexadecimal). eg: &amp;#xhhhh;',
    'If checked only (&quot;), (&amp;), (&lt;), (&gt;) and (&apos;) will be encoded. (good for source codes)',
    'Please input the text you want to encode'];

function encoder_name(form, pb) {
    var ro = form.rawText.value;

    if (ro == "" || ro == undefined || ro == null) {
        showHelp(4);
        return;
    }
    //var pb = document.getElementById('pbar');
    var enc = 0;
    var enl = 5;

    if (document.getElementById('entname').checked == true) {
        enc = 0;
    } else if (document.getElementById('entdec').checked == true) {
        enc = 3;
    } else
        enc = 2;

    if (form.minitxt.checked == false)
        enl = entities.length;
    //pb.style.width = '0%';
    for(var i = 0; i < enl; ++i) {
        ro = ro.replace(new RegExp(entities[i][1], 'g'), entities[i][enc]);
        pb.style.width = (i * 100 / (enl-1)) + "%";
    }
    form.encText.value = ro;

}

function encoder(form) {
    var pb = document.getElementById('pbar');
    pb.style.width = '0%';
    pb.style.opacity = 1.0;
    encoder_name(form, pb);
    var bef = pb.className;
    pb.className += " animate-fade-out";
    setTimeout(function() {pb.style.opacity = 0.0; pb.style.width = "0%"; pb.className = bef;}, 1000);

}

function showHelp(item) {
    if (item > 5) return;
    document.getElementById('helps').innerHTML = helpstr[item];

}

function hideHelp() {
    document.getElementById('helps').innerHTML = "&nbsp;";
}
```

functions `showHelp(), hideHelp()` and variable `helpstr` are not directly related to the encoder.  
function `encoder()` calls `encoder_name()` to do the conversion, and perform some house-keepings. *`encoder_name()` had that name because originally I wanted to encode characters with their names, but later I decided to add support for all three encodings. But I didn't want to change the name of the function.*