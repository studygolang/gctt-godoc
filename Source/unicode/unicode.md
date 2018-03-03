version: 1.10
## package unicode

  `import "unicode"`

## Overview

Package unicode provides data and functions to test some properties of Unicode
code points.

<a id="example_is"></a>
Example:

    // constant with mixed type runes
    const mixed = "\b5Ὂg̀9! ℃ᾭG"
    for _, c := range mixed {
        fmt.Printf("For %q:\n", c)
        if unicode.IsControl(c) {
            fmt.Println("\tis control rune")
        }
        if unicode.IsDigit(c) {
            fmt.Println("\tis digit rune")
        }
        if unicode.IsGraphic(c) {
            fmt.Println("\tis graphic rune")
        }
        if unicode.IsLetter(c) {
            fmt.Println("\tis letter rune")
        }
        if unicode.IsLower(c) {
            fmt.Println("\tis lower case rune")
        }
        if unicode.IsMark(c) {
            fmt.Println("\tis mark rune")
        }
        if unicode.IsNumber(c) {
            fmt.Println("\tis number rune")
        }
        if unicode.IsPrint(c) {
            fmt.Println("\tis printable rune")
        }
        if !unicode.IsPrint(c) {
            fmt.Println("\tis not printable rune")
        }
        if unicode.IsPunct(c) {
            fmt.Println("\tis punct rune")
        }
        if unicode.IsSpace(c) {
            fmt.Println("\tis space rune")
        }
        if unicode.IsSymbol(c) {
            fmt.Println("\tis symbol rune")
        }
        if unicode.IsTitle(c) {
            fmt.Println("\tis title case rune")
        }
        if unicode.IsUpper(c) {
            fmt.Println("\tis upper case rune")
        }
    }

    // Output:
    // For '\b':
    // 	is control rune
    // 	is not printable rune
    // For '5':
    // 	is digit rune
    // 	is graphic rune
    // 	is number rune
    // 	is printable rune
    // For 'Ὂ':
    // 	is graphic rune
    // 	is letter rune
    // 	is printable rune
    // 	is upper case rune
    // For 'g':
    // 	is graphic rune
    // 	is letter rune
    // 	is lower case rune
    // 	is printable rune
    // For '̀':
    // 	is graphic rune
    // 	is mark rune
    // 	is printable rune
    // For '9':
    // 	is digit rune
    // 	is graphic rune
    // 	is number rune
    // 	is printable rune
    // For '!':
    // 	is graphic rune
    // 	is printable rune
    // 	is punct rune
    // For ' ':
    // 	is graphic rune
    // 	is printable rune
    // 	is space rune
    // For '℃':
    // 	is graphic rune
    // 	is printable rune
    // 	is symbol rune
    // For 'ᾭ':
    // 	is graphic rune
    // 	is letter rune
    // 	is printable rune
    // 	is title case rune
    // For 'G':
    // 	is graphic rune
    // 	is letter rune
    // 	is printable rune
    // 	is upper case rune

## Index

- [Constants](#pkg-constants)
- [Variables](#pkg-variables)
- [func In(r rune, ranges ...*RangeTable) bool](#In)
- [func Is(rangeTab *RangeTable, r rune) bool](#Is)
- [func IsControl(r rune) bool](#IsControl)
- [func IsDigit(r rune) bool](#IsDigit)
- [func IsGraphic(r rune) bool](#IsGraphic)
- [func IsLetter(r rune) bool](#IsLetter)
- [func IsLower(r rune) bool](#IsLower)
- [func IsMark(r rune) bool](#IsMark)
- [func IsNumber(r rune) bool](#IsNumber)
- [func IsOneOf(ranges []*RangeTable, r rune) bool](#IsOneOf)
- [func IsPrint(r rune) bool](#IsPrint)
- [func IsPunct(r rune) bool](#IsPunct)
- [func IsSpace(r rune) bool](#IsSpace)
- [func IsSymbol(r rune) bool](#IsSymbol)
- [func IsTitle(r rune) bool](#IsTitle)
- [func IsUpper(r rune) bool](#IsUpper)
- [func SimpleFold(r rune) rune](#SimpleFold)
- [func To(_case int, r rune) rune](#To)
- [func ToLower(r rune) rune](#ToLower)
- [func ToTitle(r rune) rune](#ToTitle)
- [func ToUpper(r rune) rune](#ToUpper)
- [type CaseRange](#CaseRange)
- [type Range16](#Range16)
- [type Range32](#Range32)
- [type RangeTable](#RangeTable)
- [type SpecialCase](#SpecialCase)
  - [func (special SpecialCase) ToLower(r rune) rune](#SpecialCase.ToLower)
  - [func (special SpecialCase) ToTitle(r rune) rune](#SpecialCase.ToTitle)
  - [func (special SpecialCase) ToUpper(r rune) rune](#SpecialCase.ToUpper)
- [Bugs](#pkg-note-BUG)

### Examples

- [SimpleFold](#exampleSimpleFold)
- [SpecialCase](#exampleSpecialCase)
- [To](#exampleTo)
- [ToLower](#exampleToLower)
- [ToTitle](#exampleToTitle)
- [ToUpper](#exampleToUpper)
- [Package (Is)](#example_is)

### Package files
 [casetables.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/casetables.go) [digit.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/digit.go) [graphic.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go) [letter.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go) [tables.go](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/tables.go)

<h2 id="pkg-constants">Constants</h2>

<pre>const (
    <span id="MaxRune">MaxRune</span>         = &#39;\U0010FFFF&#39; <span class="comment">// Maximum valid Unicode code point.</span>
    <span id="ReplacementChar">ReplacementChar</span> = &#39;\uFFFD&#39;     <span class="comment">// Represents invalid code points.</span>
    <span id="MaxASCII">MaxASCII</span>        = &#39;\u007F&#39;     <span class="comment">// maximum ASCII value.</span>
    <span id="MaxLatin1">MaxLatin1</span>       = &#39;\u00FF&#39;     <span class="comment">// maximum Latin-1 value.</span>
)</pre>


<pre>const (
    <span id="UpperCase">UpperCase</span> = <a href="/builtin/#iota">iota</a>
    <span id="LowerCase">LowerCase</span>
    <span id="TitleCase">TitleCase</span>
    <span id="MaxCase">MaxCase</span>
)</pre>

Indices into the Delta arrays inside CaseRanges for case mapping.

<pre>const (
    <span id="UpperLower">UpperLower</span> = <a href="#MaxRune">MaxRune</a> + 1 <span class="comment">// (Cannot be a valid delta.)</span>
)</pre>

If the Delta field of a CaseRange is UpperLower, it means this CaseRange
represents a sequence of the form (say) Upper Lower Upper Lower.

<pre>const <span id="Version">Version</span> = &#34;10.0.0&#34;</pre>

Version is the Unicode edition from which the tables are derived.

<h2 id="pkg-variables">Variables</h2>

<pre>var (
    <span id="Cc">Cc</span>     = _Cc <span class="comment">// Cc is the set of Unicode characters in category Cc.</span>
    <span id="Cf">Cf</span>     = _Cf <span class="comment">// Cf is the set of Unicode characters in category Cf.</span>
    <span id="Co">Co</span>     = _Co <span class="comment">// Co is the set of Unicode characters in category Co.</span>
    <span id="Cs">Cs</span>     = _Cs <span class="comment">// Cs is the set of Unicode characters in category Cs.</span>
    <span id="Digit">Digit</span>  = _Nd <span class="comment">// Digit is the set of Unicode characters with the &#34;decimal digit&#34; property.</span>
    <span id="Nd">Nd</span>     = _Nd <span class="comment">// Nd is the set of Unicode characters in category Nd.</span>
    <span id="Letter">Letter</span> = _L  <span class="comment">// Letter/L is the set of Unicode letters, category L.</span>
    <span id="L">L</span>      = _L
    <span id="Lm">Lm</span>     = _Lm <span class="comment">// Lm is the set of Unicode characters in category Lm.</span>
    <span id="Lo">Lo</span>     = _Lo <span class="comment">// Lo is the set of Unicode characters in category Lo.</span>
    <span id="Lower">Lower</span>  = _Ll <span class="comment">// Lower is the set of Unicode lower case letters.</span>
    <span id="Ll">Ll</span>     = _Ll <span class="comment">// Ll is the set of Unicode characters in category Ll.</span>
    <span id="Mark">Mark</span>   = _M  <span class="comment">// Mark/M is the set of Unicode mark characters, category M.</span>
    <span id="M">M</span>      = _M
    <span id="Mc">Mc</span>     = _Mc <span class="comment">// Mc is the set of Unicode characters in category Mc.</span>
    <span id="Me">Me</span>     = _Me <span class="comment">// Me is the set of Unicode characters in category Me.</span>
    <span id="Mn">Mn</span>     = _Mn <span class="comment">// Mn is the set of Unicode characters in category Mn.</span>
    <span id="Nl">Nl</span>     = _Nl <span class="comment">// Nl is the set of Unicode characters in category Nl.</span>
    <span id="No">No</span>     = _No <span class="comment">// No is the set of Unicode characters in category No.</span>
    <span id="Number">Number</span> = _N  <span class="comment">// Number/N is the set of Unicode number characters, category N.</span>
    <span id="N">N</span>      = _N
    <span id="Other">Other</span>  = _C <span class="comment">// Other/C is the set of Unicode control and special characters, category C.</span>
    <span id="C">C</span>      = _C
    <span id="Pc">Pc</span>     = _Pc <span class="comment">// Pc is the set of Unicode characters in category Pc.</span>
    <span id="Pd">Pd</span>     = _Pd <span class="comment">// Pd is the set of Unicode characters in category Pd.</span>
    <span id="Pe">Pe</span>     = _Pe <span class="comment">// Pe is the set of Unicode characters in category Pe.</span>
    <span id="Pf">Pf</span>     = _Pf <span class="comment">// Pf is the set of Unicode characters in category Pf.</span>
    <span id="Pi">Pi</span>     = _Pi <span class="comment">// Pi is the set of Unicode characters in category Pi.</span>
    <span id="Po">Po</span>     = _Po <span class="comment">// Po is the set of Unicode characters in category Po.</span>
    <span id="Ps">Ps</span>     = _Ps <span class="comment">// Ps is the set of Unicode characters in category Ps.</span>
    <span id="Punct">Punct</span>  = _P  <span class="comment">// Punct/P is the set of Unicode punctuation characters, category P.</span>
    <span id="P">P</span>      = _P
    <span id="Sc">Sc</span>     = _Sc <span class="comment">// Sc is the set of Unicode characters in category Sc.</span>
    <span id="Sk">Sk</span>     = _Sk <span class="comment">// Sk is the set of Unicode characters in category Sk.</span>
    <span id="Sm">Sm</span>     = _Sm <span class="comment">// Sm is the set of Unicode characters in category Sm.</span>
    <span id="So">So</span>     = _So <span class="comment">// So is the set of Unicode characters in category So.</span>
    <span id="Space">Space</span>  = _Z  <span class="comment">// Space/Z is the set of Unicode space characters, category Z.</span>
    <span id="Z">Z</span>      = _Z
    <span id="Symbol">Symbol</span> = _S <span class="comment">// Symbol/S is the set of Unicode symbol characters, category S.</span>
    <span id="S">S</span>      = _S
    <span id="Title">Title</span>  = _Lt <span class="comment">// Title is the set of Unicode title case letters.</span>
    <span id="Lt">Lt</span>     = _Lt <span class="comment">// Lt is the set of Unicode characters in category Lt.</span>
    <span id="Upper">Upper</span>  = _Lu <span class="comment">// Upper is the set of Unicode upper case letters.</span>
    <span id="Lu">Lu</span>     = _Lu <span class="comment">// Lu is the set of Unicode characters in category Lu.</span>
    <span id="Zl">Zl</span>     = _Zl <span class="comment">// Zl is the set of Unicode characters in category Zl.</span>
    <span id="Zp">Zp</span>     = _Zp <span class="comment">// Zp is the set of Unicode characters in category Zp.</span>
    <span id="Zs">Zs</span>     = _Zs <span class="comment">// Zs is the set of Unicode characters in category Zs.</span>
)</pre>

These variables have type *RangeTable.

<pre>var (
    <span id="Adlam">Adlam</span>                  = _Adlam                  <span class="comment">// Adlam is the set of Unicode characters in script Adlam.</span>
    <span id="Ahom">Ahom</span>                   = _Ahom                   <span class="comment">// Ahom is the set of Unicode characters in script Ahom.</span>
    <span id="Anatolian_Hieroglyphs">Anatolian_Hieroglyphs</span>  = _Anatolian_Hieroglyphs  <span class="comment">// Anatolian_Hieroglyphs is the set of Unicode characters in script Anatolian_Hieroglyphs.</span>
    <span id="Arabic">Arabic</span>                 = _Arabic                 <span class="comment">// Arabic is the set of Unicode characters in script Arabic.</span>
    <span id="Armenian">Armenian</span>               = _Armenian               <span class="comment">// Armenian is the set of Unicode characters in script Armenian.</span>
    <span id="Avestan">Avestan</span>                = _Avestan                <span class="comment">// Avestan is the set of Unicode characters in script Avestan.</span>
    <span id="Balinese">Balinese</span>               = _Balinese               <span class="comment">// Balinese is the set of Unicode characters in script Balinese.</span>
    <span id="Bamum">Bamum</span>                  = _Bamum                  <span class="comment">// Bamum is the set of Unicode characters in script Bamum.</span>
    <span id="Bassa_Vah">Bassa_Vah</span>              = _Bassa_Vah              <span class="comment">// Bassa_Vah is the set of Unicode characters in script Bassa_Vah.</span>
    <span id="Batak">Batak</span>                  = _Batak                  <span class="comment">// Batak is the set of Unicode characters in script Batak.</span>
    <span id="Bengali">Bengali</span>                = _Bengali                <span class="comment">// Bengali is the set of Unicode characters in script Bengali.</span>
    <span id="Bhaiksuki">Bhaiksuki</span>              = _Bhaiksuki              <span class="comment">// Bhaiksuki is the set of Unicode characters in script Bhaiksuki.</span>
    <span id="Bopomofo">Bopomofo</span>               = _Bopomofo               <span class="comment">// Bopomofo is the set of Unicode characters in script Bopomofo.</span>
    <span id="Brahmi">Brahmi</span>                 = _Brahmi                 <span class="comment">// Brahmi is the set of Unicode characters in script Brahmi.</span>
    <span id="Braille">Braille</span>                = _Braille                <span class="comment">// Braille is the set of Unicode characters in script Braille.</span>
    <span id="Buginese">Buginese</span>               = _Buginese               <span class="comment">// Buginese is the set of Unicode characters in script Buginese.</span>
    <span id="Buhid">Buhid</span>                  = _Buhid                  <span class="comment">// Buhid is the set of Unicode characters in script Buhid.</span>
    <span id="Canadian_Aboriginal">Canadian_Aboriginal</span>    = _Canadian_Aboriginal    <span class="comment">// Canadian_Aboriginal is the set of Unicode characters in script Canadian_Aboriginal.</span>
    <span id="Carian">Carian</span>                 = _Carian                 <span class="comment">// Carian is the set of Unicode characters in script Carian.</span>
    <span id="Caucasian_Albanian">Caucasian_Albanian</span>     = _Caucasian_Albanian     <span class="comment">// Caucasian_Albanian is the set of Unicode characters in script Caucasian_Albanian.</span>
    <span id="Chakma">Chakma</span>                 = _Chakma                 <span class="comment">// Chakma is the set of Unicode characters in script Chakma.</span>
    <span id="Cham">Cham</span>                   = _Cham                   <span class="comment">// Cham is the set of Unicode characters in script Cham.</span>
    <span id="Cherokee">Cherokee</span>               = _Cherokee               <span class="comment">// Cherokee is the set of Unicode characters in script Cherokee.</span>
    <span id="Common">Common</span>                 = _Common                 <span class="comment">// Common is the set of Unicode characters in script Common.</span>
    <span id="Coptic">Coptic</span>                 = _Coptic                 <span class="comment">// Coptic is the set of Unicode characters in script Coptic.</span>
    <span id="Cuneiform">Cuneiform</span>              = _Cuneiform              <span class="comment">// Cuneiform is the set of Unicode characters in script Cuneiform.</span>
    <span id="Cypriot">Cypriot</span>                = _Cypriot                <span class="comment">// Cypriot is the set of Unicode characters in script Cypriot.</span>
    <span id="Cyrillic">Cyrillic</span>               = _Cyrillic               <span class="comment">// Cyrillic is the set of Unicode characters in script Cyrillic.</span>
    <span id="Deseret">Deseret</span>                = _Deseret                <span class="comment">// Deseret is the set of Unicode characters in script Deseret.</span>
    <span id="Devanagari">Devanagari</span>             = _Devanagari             <span class="comment">// Devanagari is the set of Unicode characters in script Devanagari.</span>
    <span id="Duployan">Duployan</span>               = _Duployan               <span class="comment">// Duployan is the set of Unicode characters in script Duployan.</span>
    <span id="Egyptian_Hieroglyphs">Egyptian_Hieroglyphs</span>   = _Egyptian_Hieroglyphs   <span class="comment">// Egyptian_Hieroglyphs is the set of Unicode characters in script Egyptian_Hieroglyphs.</span>
    <span id="Elbasan">Elbasan</span>                = _Elbasan                <span class="comment">// Elbasan is the set of Unicode characters in script Elbasan.</span>
    <span id="Ethiopic">Ethiopic</span>               = _Ethiopic               <span class="comment">// Ethiopic is the set of Unicode characters in script Ethiopic.</span>
    <span id="Georgian">Georgian</span>               = _Georgian               <span class="comment">// Georgian is the set of Unicode characters in script Georgian.</span>
    <span id="Glagolitic">Glagolitic</span>             = _Glagolitic             <span class="comment">// Glagolitic is the set of Unicode characters in script Glagolitic.</span>
    <span id="Gothic">Gothic</span>                 = _Gothic                 <span class="comment">// Gothic is the set of Unicode characters in script Gothic.</span>
    <span id="Grantha">Grantha</span>                = _Grantha                <span class="comment">// Grantha is the set of Unicode characters in script Grantha.</span>
    <span id="Greek">Greek</span>                  = _Greek                  <span class="comment">// Greek is the set of Unicode characters in script Greek.</span>
    <span id="Gujarati">Gujarati</span>               = _Gujarati               <span class="comment">// Gujarati is the set of Unicode characters in script Gujarati.</span>
    <span id="Gurmukhi">Gurmukhi</span>               = _Gurmukhi               <span class="comment">// Gurmukhi is the set of Unicode characters in script Gurmukhi.</span>
    <span id="Han">Han</span>                    = _Han                    <span class="comment">// Han is the set of Unicode characters in script Han.</span>
    <span id="Hangul">Hangul</span>                 = _Hangul                 <span class="comment">// Hangul is the set of Unicode characters in script Hangul.</span>
    <span id="Hanunoo">Hanunoo</span>                = _Hanunoo                <span class="comment">// Hanunoo is the set of Unicode characters in script Hanunoo.</span>
    <span id="Hatran">Hatran</span>                 = _Hatran                 <span class="comment">// Hatran is the set of Unicode characters in script Hatran.</span>
    <span id="Hebrew">Hebrew</span>                 = _Hebrew                 <span class="comment">// Hebrew is the set of Unicode characters in script Hebrew.</span>
    <span id="Hiragana">Hiragana</span>               = _Hiragana               <span class="comment">// Hiragana is the set of Unicode characters in script Hiragana.</span>
    <span id="Imperial_Aramaic">Imperial_Aramaic</span>       = _Imperial_Aramaic       <span class="comment">// Imperial_Aramaic is the set of Unicode characters in script Imperial_Aramaic.</span>
    <span id="Inherited">Inherited</span>              = _Inherited              <span class="comment">// Inherited is the set of Unicode characters in script Inherited.</span>
    <span id="Inscriptional_Pahlavi">Inscriptional_Pahlavi</span>  = _Inscriptional_Pahlavi  <span class="comment">// Inscriptional_Pahlavi is the set of Unicode characters in script Inscriptional_Pahlavi.</span>
    <span id="Inscriptional_Parthian">Inscriptional_Parthian</span> = _Inscriptional_Parthian <span class="comment">// Inscriptional_Parthian is the set of Unicode characters in script Inscriptional_Parthian.</span>
    <span id="Javanese">Javanese</span>               = _Javanese               <span class="comment">// Javanese is the set of Unicode characters in script Javanese.</span>
    <span id="Kaithi">Kaithi</span>                 = _Kaithi                 <span class="comment">// Kaithi is the set of Unicode characters in script Kaithi.</span>
    <span id="Kannada">Kannada</span>                = _Kannada                <span class="comment">// Kannada is the set of Unicode characters in script Kannada.</span>
    <span id="Katakana">Katakana</span>               = _Katakana               <span class="comment">// Katakana is the set of Unicode characters in script Katakana.</span>
    <span id="Kayah_Li">Kayah_Li</span>               = _Kayah_Li               <span class="comment">// Kayah_Li is the set of Unicode characters in script Kayah_Li.</span>
    <span id="Kharoshthi">Kharoshthi</span>             = _Kharoshthi             <span class="comment">// Kharoshthi is the set of Unicode characters in script Kharoshthi.</span>
    <span id="Khmer">Khmer</span>                  = _Khmer                  <span class="comment">// Khmer is the set of Unicode characters in script Khmer.</span>
    <span id="Khojki">Khojki</span>                 = _Khojki                 <span class="comment">// Khojki is the set of Unicode characters in script Khojki.</span>
    <span id="Khudawadi">Khudawadi</span>              = _Khudawadi              <span class="comment">// Khudawadi is the set of Unicode characters in script Khudawadi.</span>
    <span id="Lao">Lao</span>                    = _Lao                    <span class="comment">// Lao is the set of Unicode characters in script Lao.</span>
    <span id="Latin">Latin</span>                  = _Latin                  <span class="comment">// Latin is the set of Unicode characters in script Latin.</span>
    <span id="Lepcha">Lepcha</span>                 = _Lepcha                 <span class="comment">// Lepcha is the set of Unicode characters in script Lepcha.</span>
    <span id="Limbu">Limbu</span>                  = _Limbu                  <span class="comment">// Limbu is the set of Unicode characters in script Limbu.</span>
    <span id="Linear_A">Linear_A</span>               = _Linear_A               <span class="comment">// Linear_A is the set of Unicode characters in script Linear_A.</span>
    <span id="Linear_B">Linear_B</span>               = _Linear_B               <span class="comment">// Linear_B is the set of Unicode characters in script Linear_B.</span>
    <span id="Lisu">Lisu</span>                   = _Lisu                   <span class="comment">// Lisu is the set of Unicode characters in script Lisu.</span>
    <span id="Lycian">Lycian</span>                 = _Lycian                 <span class="comment">// Lycian is the set of Unicode characters in script Lycian.</span>
    <span id="Lydian">Lydian</span>                 = _Lydian                 <span class="comment">// Lydian is the set of Unicode characters in script Lydian.</span>
    <span id="Mahajani">Mahajani</span>               = _Mahajani               <span class="comment">// Mahajani is the set of Unicode characters in script Mahajani.</span>
    <span id="Malayalam">Malayalam</span>              = _Malayalam              <span class="comment">// Malayalam is the set of Unicode characters in script Malayalam.</span>
    <span id="Mandaic">Mandaic</span>                = _Mandaic                <span class="comment">// Mandaic is the set of Unicode characters in script Mandaic.</span>
    <span id="Manichaean">Manichaean</span>             = _Manichaean             <span class="comment">// Manichaean is the set of Unicode characters in script Manichaean.</span>
    <span id="Marchen">Marchen</span>                = _Marchen                <span class="comment">// Marchen is the set of Unicode characters in script Marchen.</span>
    <span id="Masaram_Gondi">Masaram_Gondi</span>          = _Masaram_Gondi          <span class="comment">// Masaram_Gondi is the set of Unicode characters in script Masaram_Gondi.</span>
    <span id="Meetei_Mayek">Meetei_Mayek</span>           = _Meetei_Mayek           <span class="comment">// Meetei_Mayek is the set of Unicode characters in script Meetei_Mayek.</span>
    <span id="Mende_Kikakui">Mende_Kikakui</span>          = _Mende_Kikakui          <span class="comment">// Mende_Kikakui is the set of Unicode characters in script Mende_Kikakui.</span>
    <span id="Meroitic_Cursive">Meroitic_Cursive</span>       = _Meroitic_Cursive       <span class="comment">// Meroitic_Cursive is the set of Unicode characters in script Meroitic_Cursive.</span>
    <span id="Meroitic_Hieroglyphs">Meroitic_Hieroglyphs</span>   = _Meroitic_Hieroglyphs   <span class="comment">// Meroitic_Hieroglyphs is the set of Unicode characters in script Meroitic_Hieroglyphs.</span>
    <span id="Miao">Miao</span>                   = _Miao                   <span class="comment">// Miao is the set of Unicode characters in script Miao.</span>
    <span id="Modi">Modi</span>                   = _Modi                   <span class="comment">// Modi is the set of Unicode characters in script Modi.</span>
    <span id="Mongolian">Mongolian</span>              = _Mongolian              <span class="comment">// Mongolian is the set of Unicode characters in script Mongolian.</span>
    <span id="Mro">Mro</span>                    = _Mro                    <span class="comment">// Mro is the set of Unicode characters in script Mro.</span>
    <span id="Multani">Multani</span>                = _Multani                <span class="comment">// Multani is the set of Unicode characters in script Multani.</span>
    <span id="Myanmar">Myanmar</span>                = _Myanmar                <span class="comment">// Myanmar is the set of Unicode characters in script Myanmar.</span>
    <span id="Nabataean">Nabataean</span>              = _Nabataean              <span class="comment">// Nabataean is the set of Unicode characters in script Nabataean.</span>
    <span id="New_Tai_Lue">New_Tai_Lue</span>            = _New_Tai_Lue            <span class="comment">// New_Tai_Lue is the set of Unicode characters in script New_Tai_Lue.</span>
    <span id="Newa">Newa</span>                   = _Newa                   <span class="comment">// Newa is the set of Unicode characters in script Newa.</span>
    <span id="Nko">Nko</span>                    = _Nko                    <span class="comment">// Nko is the set of Unicode characters in script Nko.</span>
    <span id="Nushu">Nushu</span>                  = _Nushu                  <span class="comment">// Nushu is the set of Unicode characters in script Nushu.</span>
    <span id="Ogham">Ogham</span>                  = _Ogham                  <span class="comment">// Ogham is the set of Unicode characters in script Ogham.</span>
    <span id="Ol_Chiki">Ol_Chiki</span>               = _Ol_Chiki               <span class="comment">// Ol_Chiki is the set of Unicode characters in script Ol_Chiki.</span>
    <span id="Old_Hungarian">Old_Hungarian</span>          = _Old_Hungarian          <span class="comment">// Old_Hungarian is the set of Unicode characters in script Old_Hungarian.</span>
    <span id="Old_Italic">Old_Italic</span>             = _Old_Italic             <span class="comment">// Old_Italic is the set of Unicode characters in script Old_Italic.</span>
    <span id="Old_North_Arabian">Old_North_Arabian</span>      = _Old_North_Arabian      <span class="comment">// Old_North_Arabian is the set of Unicode characters in script Old_North_Arabian.</span>
    <span id="Old_Permic">Old_Permic</span>             = _Old_Permic             <span class="comment">// Old_Permic is the set of Unicode characters in script Old_Permic.</span>
    <span id="Old_Persian">Old_Persian</span>            = _Old_Persian            <span class="comment">// Old_Persian is the set of Unicode characters in script Old_Persian.</span>
    <span id="Old_South_Arabian">Old_South_Arabian</span>      = _Old_South_Arabian      <span class="comment">// Old_South_Arabian is the set of Unicode characters in script Old_South_Arabian.</span>
    <span id="Old_Turkic">Old_Turkic</span>             = _Old_Turkic             <span class="comment">// Old_Turkic is the set of Unicode characters in script Old_Turkic.</span>
    <span id="Oriya">Oriya</span>                  = _Oriya                  <span class="comment">// Oriya is the set of Unicode characters in script Oriya.</span>
    <span id="Osage">Osage</span>                  = _Osage                  <span class="comment">// Osage is the set of Unicode characters in script Osage.</span>
    <span id="Osmanya">Osmanya</span>                = _Osmanya                <span class="comment">// Osmanya is the set of Unicode characters in script Osmanya.</span>
    <span id="Pahawh_Hmong">Pahawh_Hmong</span>           = _Pahawh_Hmong           <span class="comment">// Pahawh_Hmong is the set of Unicode characters in script Pahawh_Hmong.</span>
    <span id="Palmyrene">Palmyrene</span>              = _Palmyrene              <span class="comment">// Palmyrene is the set of Unicode characters in script Palmyrene.</span>
    <span id="Pau_Cin_Hau">Pau_Cin_Hau</span>            = _Pau_Cin_Hau            <span class="comment">// Pau_Cin_Hau is the set of Unicode characters in script Pau_Cin_Hau.</span>
    <span id="Phags_Pa">Phags_Pa</span>               = _Phags_Pa               <span class="comment">// Phags_Pa is the set of Unicode characters in script Phags_Pa.</span>
    <span id="Phoenician">Phoenician</span>             = _Phoenician             <span class="comment">// Phoenician is the set of Unicode characters in script Phoenician.</span>
    <span id="Psalter_Pahlavi">Psalter_Pahlavi</span>        = _Psalter_Pahlavi        <span class="comment">// Psalter_Pahlavi is the set of Unicode characters in script Psalter_Pahlavi.</span>
    <span id="Rejang">Rejang</span>                 = _Rejang                 <span class="comment">// Rejang is the set of Unicode characters in script Rejang.</span>
    <span id="Runic">Runic</span>                  = _Runic                  <span class="comment">// Runic is the set of Unicode characters in script Runic.</span>
    <span id="Samaritan">Samaritan</span>              = _Samaritan              <span class="comment">// Samaritan is the set of Unicode characters in script Samaritan.</span>
    <span id="Saurashtra">Saurashtra</span>             = _Saurashtra             <span class="comment">// Saurashtra is the set of Unicode characters in script Saurashtra.</span>
    <span id="Sharada">Sharada</span>                = _Sharada                <span class="comment">// Sharada is the set of Unicode characters in script Sharada.</span>
    <span id="Shavian">Shavian</span>                = _Shavian                <span class="comment">// Shavian is the set of Unicode characters in script Shavian.</span>
    <span id="Siddham">Siddham</span>                = _Siddham                <span class="comment">// Siddham is the set of Unicode characters in script Siddham.</span>
    <span id="SignWriting">SignWriting</span>            = _SignWriting            <span class="comment">// SignWriting is the set of Unicode characters in script SignWriting.</span>
    <span id="Sinhala">Sinhala</span>                = _Sinhala                <span class="comment">// Sinhala is the set of Unicode characters in script Sinhala.</span>
    <span id="Sora_Sompeng">Sora_Sompeng</span>           = _Sora_Sompeng           <span class="comment">// Sora_Sompeng is the set of Unicode characters in script Sora_Sompeng.</span>
    <span id="Soyombo">Soyombo</span>                = _Soyombo                <span class="comment">// Soyombo is the set of Unicode characters in script Soyombo.</span>
    <span id="Sundanese">Sundanese</span>              = _Sundanese              <span class="comment">// Sundanese is the set of Unicode characters in script Sundanese.</span>
    <span id="Syloti_Nagri">Syloti_Nagri</span>           = _Syloti_Nagri           <span class="comment">// Syloti_Nagri is the set of Unicode characters in script Syloti_Nagri.</span>
    <span id="Syriac">Syriac</span>                 = _Syriac                 <span class="comment">// Syriac is the set of Unicode characters in script Syriac.</span>
    <span id="Tagalog">Tagalog</span>                = _Tagalog                <span class="comment">// Tagalog is the set of Unicode characters in script Tagalog.</span>
    <span id="Tagbanwa">Tagbanwa</span>               = _Tagbanwa               <span class="comment">// Tagbanwa is the set of Unicode characters in script Tagbanwa.</span>
    <span id="Tai_Le">Tai_Le</span>                 = _Tai_Le                 <span class="comment">// Tai_Le is the set of Unicode characters in script Tai_Le.</span>
    <span id="Tai_Tham">Tai_Tham</span>               = _Tai_Tham               <span class="comment">// Tai_Tham is the set of Unicode characters in script Tai_Tham.</span>
    <span id="Tai_Viet">Tai_Viet</span>               = _Tai_Viet               <span class="comment">// Tai_Viet is the set of Unicode characters in script Tai_Viet.</span>
    <span id="Takri">Takri</span>                  = _Takri                  <span class="comment">// Takri is the set of Unicode characters in script Takri.</span>
    <span id="Tamil">Tamil</span>                  = _Tamil                  <span class="comment">// Tamil is the set of Unicode characters in script Tamil.</span>
    <span id="Tangut">Tangut</span>                 = _Tangut                 <span class="comment">// Tangut is the set of Unicode characters in script Tangut.</span>
    <span id="Telugu">Telugu</span>                 = _Telugu                 <span class="comment">// Telugu is the set of Unicode characters in script Telugu.</span>
    <span id="Thaana">Thaana</span>                 = _Thaana                 <span class="comment">// Thaana is the set of Unicode characters in script Thaana.</span>
    <span id="Thai">Thai</span>                   = _Thai                   <span class="comment">// Thai is the set of Unicode characters in script Thai.</span>
    <span id="Tibetan">Tibetan</span>                = _Tibetan                <span class="comment">// Tibetan is the set of Unicode characters in script Tibetan.</span>
    <span id="Tifinagh">Tifinagh</span>               = _Tifinagh               <span class="comment">// Tifinagh is the set of Unicode characters in script Tifinagh.</span>
    <span id="Tirhuta">Tirhuta</span>                = _Tirhuta                <span class="comment">// Tirhuta is the set of Unicode characters in script Tirhuta.</span>
    <span id="Ugaritic">Ugaritic</span>               = _Ugaritic               <span class="comment">// Ugaritic is the set of Unicode characters in script Ugaritic.</span>
    <span id="Vai">Vai</span>                    = _Vai                    <span class="comment">// Vai is the set of Unicode characters in script Vai.</span>
    <span id="Warang_Citi">Warang_Citi</span>            = _Warang_Citi            <span class="comment">// Warang_Citi is the set of Unicode characters in script Warang_Citi.</span>
    <span id="Yi">Yi</span>                     = _Yi                     <span class="comment">// Yi is the set of Unicode characters in script Yi.</span>
    <span id="Zanabazar_Square">Zanabazar_Square</span>       = _Zanabazar_Square       <span class="comment">// Zanabazar_Square is the set of Unicode characters in script Zanabazar_Square.</span>
)</pre>

These variables have type *RangeTable.

<pre>var (
    <span id="ASCII_Hex_Digit">ASCII_Hex_Digit</span>                    = _ASCII_Hex_Digit                    <span class="comment">// ASCII_Hex_Digit is the set of Unicode characters with property ASCII_Hex_Digit.</span>
    <span id="Bidi_Control">Bidi_Control</span>                       = _Bidi_Control                       <span class="comment">// Bidi_Control is the set of Unicode characters with property Bidi_Control.</span>
    <span id="Dash">Dash</span>                               = _Dash                               <span class="comment">// Dash is the set of Unicode characters with property Dash.</span>
    <span id="Deprecated">Deprecated</span>                         = _Deprecated                         <span class="comment">// Deprecated is the set of Unicode characters with property Deprecated.</span>
    <span id="Diacritic">Diacritic</span>                          = _Diacritic                          <span class="comment">// Diacritic is the set of Unicode characters with property Diacritic.</span>
    <span id="Extender">Extender</span>                           = _Extender                           <span class="comment">// Extender is the set of Unicode characters with property Extender.</span>
    <span id="Hex_Digit">Hex_Digit</span>                          = _Hex_Digit                          <span class="comment">// Hex_Digit is the set of Unicode characters with property Hex_Digit.</span>
    <span id="Hyphen">Hyphen</span>                             = _Hyphen                             <span class="comment">// Hyphen is the set of Unicode characters with property Hyphen.</span>
    <span id="IDS_Binary_Operator">IDS_Binary_Operator</span>                = _IDS_Binary_Operator                <span class="comment">// IDS_Binary_Operator is the set of Unicode characters with property IDS_Binary_Operator.</span>
    <span id="IDS_Trinary_Operator">IDS_Trinary_Operator</span>               = _IDS_Trinary_Operator               <span class="comment">// IDS_Trinary_Operator is the set of Unicode characters with property IDS_Trinary_Operator.</span>
    <span id="Ideographic">Ideographic</span>                        = _Ideographic                        <span class="comment">// Ideographic is the set of Unicode characters with property Ideographic.</span>
    <span id="Join_Control">Join_Control</span>                       = _Join_Control                       <span class="comment">// Join_Control is the set of Unicode characters with property Join_Control.</span>
    <span id="Logical_Order_Exception">Logical_Order_Exception</span>            = _Logical_Order_Exception            <span class="comment">// Logical_Order_Exception is the set of Unicode characters with property Logical_Order_Exception.</span>
    <span id="Noncharacter_Code_Point">Noncharacter_Code_Point</span>            = _Noncharacter_Code_Point            <span class="comment">// Noncharacter_Code_Point is the set of Unicode characters with property Noncharacter_Code_Point.</span>
    <span id="Other_Alphabetic">Other_Alphabetic</span>                   = _Other_Alphabetic                   <span class="comment">// Other_Alphabetic is the set of Unicode characters with property Other_Alphabetic.</span>
    <span id="Other_Default_Ignorable_Code_Point">Other_Default_Ignorable_Code_Point</span> = _Other_Default_Ignorable_Code_Point <span class="comment">// Other_Default_Ignorable_Code_Point is the set of Unicode characters with property Other_Default_Ignorable_Code_Point.</span>
    <span id="Other_Grapheme_Extend">Other_Grapheme_Extend</span>              = _Other_Grapheme_Extend              <span class="comment">// Other_Grapheme_Extend is the set of Unicode characters with property Other_Grapheme_Extend.</span>
    <span id="Other_ID_Continue">Other_ID_Continue</span>                  = _Other_ID_Continue                  <span class="comment">// Other_ID_Continue is the set of Unicode characters with property Other_ID_Continue.</span>
    <span id="Other_ID_Start">Other_ID_Start</span>                     = _Other_ID_Start                     <span class="comment">// Other_ID_Start is the set of Unicode characters with property Other_ID_Start.</span>
    <span id="Other_Lowercase">Other_Lowercase</span>                    = _Other_Lowercase                    <span class="comment">// Other_Lowercase is the set of Unicode characters with property Other_Lowercase.</span>
    <span id="Other_Math">Other_Math</span>                         = _Other_Math                         <span class="comment">// Other_Math is the set of Unicode characters with property Other_Math.</span>
    <span id="Other_Uppercase">Other_Uppercase</span>                    = _Other_Uppercase                    <span class="comment">// Other_Uppercase is the set of Unicode characters with property Other_Uppercase.</span>
    <span id="Pattern_Syntax">Pattern_Syntax</span>                     = _Pattern_Syntax                     <span class="comment">// Pattern_Syntax is the set of Unicode characters with property Pattern_Syntax.</span>
    <span id="Pattern_White_Space">Pattern_White_Space</span>                = _Pattern_White_Space                <span class="comment">// Pattern_White_Space is the set of Unicode characters with property Pattern_White_Space.</span>
    <span id="Prepended_Concatenation_Mark">Prepended_Concatenation_Mark</span>       = _Prepended_Concatenation_Mark       <span class="comment">// Prepended_Concatenation_Mark is the set of Unicode characters with property Prepended_Concatenation_Mark.</span>
    <span id="Quotation_Mark">Quotation_Mark</span>                     = _Quotation_Mark                     <span class="comment">// Quotation_Mark is the set of Unicode characters with property Quotation_Mark.</span>
    <span id="Radical">Radical</span>                            = _Radical                            <span class="comment">// Radical is the set of Unicode characters with property Radical.</span>
    <span id="Regional_Indicator">Regional_Indicator</span>                 = _Regional_Indicator                 <span class="comment">// Regional_Indicator is the set of Unicode characters with property Regional_Indicator.</span>
    <span id="STerm">STerm</span>                              = _Sentence_Terminal                  <span class="comment">// STerm is an alias for Sentence_Terminal.</span>
    <span id="Sentence_Terminal">Sentence_Terminal</span>                  = _Sentence_Terminal                  <span class="comment">// Sentence_Terminal is the set of Unicode characters with property Sentence_Terminal.</span>
    <span id="Soft_Dotted">Soft_Dotted</span>                        = _Soft_Dotted                        <span class="comment">// Soft_Dotted is the set of Unicode characters with property Soft_Dotted.</span>
    <span id="Terminal_Punctuation">Terminal_Punctuation</span>               = _Terminal_Punctuation               <span class="comment">// Terminal_Punctuation is the set of Unicode characters with property Terminal_Punctuation.</span>
    <span id="Unified_Ideograph">Unified_Ideograph</span>                  = _Unified_Ideograph                  <span class="comment">// Unified_Ideograph is the set of Unicode characters with property Unified_Ideograph.</span>
    <span id="Variation_Selector">Variation_Selector</span>                 = _Variation_Selector                 <span class="comment">// Variation_Selector is the set of Unicode characters with property Variation_Selector.</span>
    <span id="White_Space">White_Space</span>                        = _White_Space                        <span class="comment">// White_Space is the set of Unicode characters with property White_Space.</span>
)</pre>

These variables have type *RangeTable.

<pre>var <span id="CaseRanges">CaseRanges</span> = _CaseRanges</pre>

CaseRanges is the table describing case mappings for all letters with non-self
mappings.

<pre>var <span id="Categories">Categories</span> = map[<a href="/builtin/#string">string</a>]*<a href="#RangeTable">RangeTable</a>{
    &#34;C&#34;:  <a href="#C">C</a>,
    &#34;Cc&#34;: <a href="#Cc">Cc</a>,
    &#34;Cf&#34;: <a href="#Cf">Cf</a>,
    &#34;Co&#34;: <a href="#Co">Co</a>,
    &#34;Cs&#34;: <a href="#Cs">Cs</a>,
    &#34;L&#34;:  <a href="#L">L</a>,
    &#34;Ll&#34;: <a href="#Ll">Ll</a>,
    &#34;Lm&#34;: <a href="#Lm">Lm</a>,
    &#34;Lo&#34;: <a href="#Lo">Lo</a>,
    &#34;Lt&#34;: <a href="#Lt">Lt</a>,
    &#34;Lu&#34;: <a href="#Lu">Lu</a>,
    &#34;M&#34;:  <a href="#M">M</a>,
    &#34;Mc&#34;: <a href="#Mc">Mc</a>,
    &#34;Me&#34;: <a href="#Me">Me</a>,
    &#34;Mn&#34;: <a href="#Mn">Mn</a>,
    &#34;N&#34;:  <a href="#N">N</a>,
    &#34;Nd&#34;: <a href="#Nd">Nd</a>,
    &#34;Nl&#34;: <a href="#Nl">Nl</a>,
    &#34;No&#34;: <a href="#No">No</a>,
    &#34;P&#34;:  <a href="#P">P</a>,
    &#34;Pc&#34;: <a href="#Pc">Pc</a>,
    &#34;Pd&#34;: <a href="#Pd">Pd</a>,
    &#34;Pe&#34;: <a href="#Pe">Pe</a>,
    &#34;Pf&#34;: <a href="#Pf">Pf</a>,
    &#34;Pi&#34;: <a href="#Pi">Pi</a>,
    &#34;Po&#34;: <a href="#Po">Po</a>,
    &#34;Ps&#34;: <a href="#Ps">Ps</a>,
    &#34;S&#34;:  <a href="#S">S</a>,
    &#34;Sc&#34;: <a href="#Sc">Sc</a>,
    &#34;Sk&#34;: <a href="#Sk">Sk</a>,
    &#34;Sm&#34;: <a href="#Sm">Sm</a>,
    &#34;So&#34;: <a href="#So">So</a>,
    &#34;Z&#34;:  <a href="#Z">Z</a>,
    &#34;Zl&#34;: <a href="#Zl">Zl</a>,
    &#34;Zp&#34;: <a href="#Zp">Zp</a>,
    &#34;Zs&#34;: <a href="#Zs">Zs</a>,
}</pre>

Categories is the set of Unicode category tables.

<pre>var <span id="FoldCategory">FoldCategory</span> = map[<a href="/builtin/#string">string</a>]*<a href="#RangeTable">RangeTable</a>{
    &#34;L&#34;:  foldL,
    &#34;Ll&#34;: foldLl,
    &#34;Lt&#34;: foldLt,
    &#34;Lu&#34;: foldLu,
    &#34;M&#34;:  foldM,
    &#34;Mn&#34;: foldMn,
}</pre>

FoldCategory maps a category name to a table of code points outside the category
that are equivalent under simple case folding to code points inside the
category. If there is no entry for a category name, there are no such points.

<pre>var <span id="FoldScript">FoldScript</span> = map[<a href="/builtin/#string">string</a>]*<a href="#RangeTable">RangeTable</a>{
    &#34;Common&#34;:    foldCommon,
    &#34;Greek&#34;:     foldGreek,
    &#34;Inherited&#34;: foldInherited,
}</pre>

FoldScript maps a script name to a table of code points outside the script that
are equivalent under simple case folding to code points inside the script. If
there is no entry for a script name, there are no such points.

<pre>var <span id="GraphicRanges">GraphicRanges</span> = []*<a href="#RangeTable">RangeTable</a>{
    <a href="#L">L</a>, <a href="#M">M</a>, <a href="#N">N</a>, <a href="#P">P</a>, <a href="#S">S</a>, <a href="#Zs">Zs</a>,
}</pre>

GraphicRanges defines the set of graphic characters according to Unicode.

<pre>var <span id="PrintRanges">PrintRanges</span> = []*<a href="#RangeTable">RangeTable</a>{
    <a href="#L">L</a>, <a href="#M">M</a>, <a href="#N">N</a>, <a href="#P">P</a>, <a href="#S">S</a>,
}</pre>

PrintRanges defines the set of printable characters according to Go. ASCII
space, U+0020, is handled separately.

<pre>var <span id="Properties">Properties</span> = map[<a href="/builtin/#string">string</a>]*<a href="#RangeTable">RangeTable</a>{
    &#34;ASCII_Hex_Digit&#34;:                    <a href="#ASCII_Hex_Digit">ASCII_Hex_Digit</a>,
    &#34;Bidi_Control&#34;:                       <a href="#Bidi_Control">Bidi_Control</a>,
    &#34;Dash&#34;:                               <a href="#Dash">Dash</a>,
    &#34;Deprecated&#34;:                         <a href="#Deprecated">Deprecated</a>,
    &#34;Diacritic&#34;:                          <a href="#Diacritic">Diacritic</a>,
    &#34;Extender&#34;:                           <a href="#Extender">Extender</a>,
    &#34;Hex_Digit&#34;:                          <a href="#Hex_Digit">Hex_Digit</a>,
    &#34;Hyphen&#34;:                             <a href="#Hyphen">Hyphen</a>,
    &#34;IDS_Binary_Operator&#34;:                <a href="#IDS_Binary_Operator">IDS_Binary_Operator</a>,
    &#34;IDS_Trinary_Operator&#34;:               <a href="#IDS_Trinary_Operator">IDS_Trinary_Operator</a>,
    &#34;Ideographic&#34;:                        <a href="#Ideographic">Ideographic</a>,
    &#34;Join_Control&#34;:                       <a href="#Join_Control">Join_Control</a>,
    &#34;Logical_Order_Exception&#34;:            <a href="#Logical_Order_Exception">Logical_Order_Exception</a>,
    &#34;Noncharacter_Code_Point&#34;:            <a href="#Noncharacter_Code_Point">Noncharacter_Code_Point</a>,
    &#34;Other_Alphabetic&#34;:                   <a href="#Other_Alphabetic">Other_Alphabetic</a>,
    &#34;Other_Default_Ignorable_Code_Point&#34;: <a href="#Other_Default_Ignorable_Code_Point">Other_Default_Ignorable_Code_Point</a>,
    &#34;Other_Grapheme_Extend&#34;:              <a href="#Other_Grapheme_Extend">Other_Grapheme_Extend</a>,
    &#34;Other_ID_Continue&#34;:                  <a href="#Other_ID_Continue">Other_ID_Continue</a>,
    &#34;Other_ID_Start&#34;:                     <a href="#Other_ID_Start">Other_ID_Start</a>,
    &#34;Other_Lowercase&#34;:                    <a href="#Other_Lowercase">Other_Lowercase</a>,
    &#34;Other_Math&#34;:                         <a href="#Other_Math">Other_Math</a>,
    &#34;Other_Uppercase&#34;:                    <a href="#Other_Uppercase">Other_Uppercase</a>,
    &#34;Pattern_Syntax&#34;:                     <a href="#Pattern_Syntax">Pattern_Syntax</a>,
    &#34;Pattern_White_Space&#34;:                <a href="#Pattern_White_Space">Pattern_White_Space</a>,
    &#34;Prepended_Concatenation_Mark&#34;:       <a href="#Prepended_Concatenation_Mark">Prepended_Concatenation_Mark</a>,
    &#34;Quotation_Mark&#34;:                     <a href="#Quotation_Mark">Quotation_Mark</a>,
    &#34;Radical&#34;:                            <a href="#Radical">Radical</a>,
    &#34;Regional_Indicator&#34;:                 <a href="#Regional_Indicator">Regional_Indicator</a>,
    &#34;Sentence_Terminal&#34;:                  <a href="#Sentence_Terminal">Sentence_Terminal</a>,
    &#34;STerm&#34;:                              <a href="#Sentence_Terminal">Sentence_Terminal</a>,
    &#34;Soft_Dotted&#34;:                        <a href="#Soft_Dotted">Soft_Dotted</a>,
    &#34;Terminal_Punctuation&#34;:               <a href="#Terminal_Punctuation">Terminal_Punctuation</a>,
    &#34;Unified_Ideograph&#34;:                  <a href="#Unified_Ideograph">Unified_Ideograph</a>,
    &#34;Variation_Selector&#34;:                 <a href="#Variation_Selector">Variation_Selector</a>,
    &#34;White_Space&#34;:                        <a href="#White_Space">White_Space</a>,
}</pre>

Properties is the set of Unicode property tables.

<pre>var <span id="Scripts">Scripts</span> = map[<a href="/builtin/#string">string</a>]*<a href="#RangeTable">RangeTable</a>{
    &#34;Adlam&#34;:                  <a href="#Adlam">Adlam</a>,
    &#34;Ahom&#34;:                   <a href="#Ahom">Ahom</a>,
    &#34;Anatolian_Hieroglyphs&#34;:  <a href="#Anatolian_Hieroglyphs">Anatolian_Hieroglyphs</a>,
    &#34;Arabic&#34;:                 <a href="#Arabic">Arabic</a>,
    &#34;Armenian&#34;:               <a href="#Armenian">Armenian</a>,
    &#34;Avestan&#34;:                <a href="#Avestan">Avestan</a>,
    &#34;Balinese&#34;:               <a href="#Balinese">Balinese</a>,
    &#34;Bamum&#34;:                  <a href="#Bamum">Bamum</a>,
    &#34;Bassa_Vah&#34;:              <a href="#Bassa_Vah">Bassa_Vah</a>,
    &#34;Batak&#34;:                  <a href="#Batak">Batak</a>,
    &#34;Bengali&#34;:                <a href="#Bengali">Bengali</a>,
    &#34;Bhaiksuki&#34;:              <a href="#Bhaiksuki">Bhaiksuki</a>,
    &#34;Bopomofo&#34;:               <a href="#Bopomofo">Bopomofo</a>,
    &#34;Brahmi&#34;:                 <a href="#Brahmi">Brahmi</a>,
    &#34;Braille&#34;:                <a href="#Braille">Braille</a>,
    &#34;Buginese&#34;:               <a href="#Buginese">Buginese</a>,
    &#34;Buhid&#34;:                  <a href="#Buhid">Buhid</a>,
    &#34;Canadian_Aboriginal&#34;:    <a href="#Canadian_Aboriginal">Canadian_Aboriginal</a>,
    &#34;Carian&#34;:                 <a href="#Carian">Carian</a>,
    &#34;Caucasian_Albanian&#34;:     <a href="#Caucasian_Albanian">Caucasian_Albanian</a>,
    &#34;Chakma&#34;:                 <a href="#Chakma">Chakma</a>,
    &#34;Cham&#34;:                   <a href="#Cham">Cham</a>,
    &#34;Cherokee&#34;:               <a href="#Cherokee">Cherokee</a>,
    &#34;Common&#34;:                 <a href="#Common">Common</a>,
    &#34;Coptic&#34;:                 <a href="#Coptic">Coptic</a>,
    &#34;Cuneiform&#34;:              <a href="#Cuneiform">Cuneiform</a>,
    &#34;Cypriot&#34;:                <a href="#Cypriot">Cypriot</a>,
    &#34;Cyrillic&#34;:               <a href="#Cyrillic">Cyrillic</a>,
    &#34;Deseret&#34;:                <a href="#Deseret">Deseret</a>,
    &#34;Devanagari&#34;:             <a href="#Devanagari">Devanagari</a>,
    &#34;Duployan&#34;:               <a href="#Duployan">Duployan</a>,
    &#34;Egyptian_Hieroglyphs&#34;:   <a href="#Egyptian_Hieroglyphs">Egyptian_Hieroglyphs</a>,
    &#34;Elbasan&#34;:                <a href="#Elbasan">Elbasan</a>,
    &#34;Ethiopic&#34;:               <a href="#Ethiopic">Ethiopic</a>,
    &#34;Georgian&#34;:               <a href="#Georgian">Georgian</a>,
    &#34;Glagolitic&#34;:             <a href="#Glagolitic">Glagolitic</a>,
    &#34;Gothic&#34;:                 <a href="#Gothic">Gothic</a>,
    &#34;Grantha&#34;:                <a href="#Grantha">Grantha</a>,
    &#34;Greek&#34;:                  <a href="#Greek">Greek</a>,
    &#34;Gujarati&#34;:               <a href="#Gujarati">Gujarati</a>,
    &#34;Gurmukhi&#34;:               <a href="#Gurmukhi">Gurmukhi</a>,
    &#34;Han&#34;:                    <a href="#Han">Han</a>,
    &#34;Hangul&#34;:                 <a href="#Hangul">Hangul</a>,
    &#34;Hanunoo&#34;:                <a href="#Hanunoo">Hanunoo</a>,
    &#34;Hatran&#34;:                 <a href="#Hatran">Hatran</a>,
    &#34;Hebrew&#34;:                 <a href="#Hebrew">Hebrew</a>,
    &#34;Hiragana&#34;:               <a href="#Hiragana">Hiragana</a>,
    &#34;Imperial_Aramaic&#34;:       <a href="#Imperial_Aramaic">Imperial_Aramaic</a>,
    &#34;Inherited&#34;:              <a href="#Inherited">Inherited</a>,
    &#34;Inscriptional_Pahlavi&#34;:  <a href="#Inscriptional_Pahlavi">Inscriptional_Pahlavi</a>,
    &#34;Inscriptional_Parthian&#34;: <a href="#Inscriptional_Parthian">Inscriptional_Parthian</a>,
    &#34;Javanese&#34;:               <a href="#Javanese">Javanese</a>,
    &#34;Kaithi&#34;:                 <a href="#Kaithi">Kaithi</a>,
    &#34;Kannada&#34;:                <a href="#Kannada">Kannada</a>,
    &#34;Katakana&#34;:               <a href="#Katakana">Katakana</a>,
    &#34;Kayah_Li&#34;:               <a href="#Kayah_Li">Kayah_Li</a>,
    &#34;Kharoshthi&#34;:             <a href="#Kharoshthi">Kharoshthi</a>,
    &#34;Khmer&#34;:                  <a href="#Khmer">Khmer</a>,
    &#34;Khojki&#34;:                 <a href="#Khojki">Khojki</a>,
    &#34;Khudawadi&#34;:              <a href="#Khudawadi">Khudawadi</a>,
    &#34;Lao&#34;:                    <a href="#Lao">Lao</a>,
    &#34;Latin&#34;:                  <a href="#Latin">Latin</a>,
    &#34;Lepcha&#34;:                 <a href="#Lepcha">Lepcha</a>,
    &#34;Limbu&#34;:                  <a href="#Limbu">Limbu</a>,
    &#34;Linear_A&#34;:               <a href="#Linear_A">Linear_A</a>,
    &#34;Linear_B&#34;:               <a href="#Linear_B">Linear_B</a>,
    &#34;Lisu&#34;:                   <a href="#Lisu">Lisu</a>,
    &#34;Lycian&#34;:                 <a href="#Lycian">Lycian</a>,
    &#34;Lydian&#34;:                 <a href="#Lydian">Lydian</a>,
    &#34;Mahajani&#34;:               <a href="#Mahajani">Mahajani</a>,
    &#34;Malayalam&#34;:              <a href="#Malayalam">Malayalam</a>,
    &#34;Mandaic&#34;:                <a href="#Mandaic">Mandaic</a>,
    &#34;Manichaean&#34;:             <a href="#Manichaean">Manichaean</a>,
    &#34;Marchen&#34;:                <a href="#Marchen">Marchen</a>,
    &#34;Masaram_Gondi&#34;:          <a href="#Masaram_Gondi">Masaram_Gondi</a>,
    &#34;Meetei_Mayek&#34;:           <a href="#Meetei_Mayek">Meetei_Mayek</a>,
    &#34;Mende_Kikakui&#34;:          <a href="#Mende_Kikakui">Mende_Kikakui</a>,
    &#34;Meroitic_Cursive&#34;:       <a href="#Meroitic_Cursive">Meroitic_Cursive</a>,
    &#34;Meroitic_Hieroglyphs&#34;:   <a href="#Meroitic_Hieroglyphs">Meroitic_Hieroglyphs</a>,
    &#34;Miao&#34;:                   <a href="#Miao">Miao</a>,
    &#34;Modi&#34;:                   <a href="#Modi">Modi</a>,
    &#34;Mongolian&#34;:              <a href="#Mongolian">Mongolian</a>,
    &#34;Mro&#34;:                    <a href="#Mro">Mro</a>,
    &#34;Multani&#34;:                <a href="#Multani">Multani</a>,
    &#34;Myanmar&#34;:                <a href="#Myanmar">Myanmar</a>,
    &#34;Nabataean&#34;:              <a href="#Nabataean">Nabataean</a>,
    &#34;New_Tai_Lue&#34;:            <a href="#New_Tai_Lue">New_Tai_Lue</a>,
    &#34;Newa&#34;:                   <a href="#Newa">Newa</a>,
    &#34;Nko&#34;:                    <a href="#Nko">Nko</a>,
    &#34;Nushu&#34;:                  <a href="#Nushu">Nushu</a>,
    &#34;Ogham&#34;:                  <a href="#Ogham">Ogham</a>,
    &#34;Ol_Chiki&#34;:               <a href="#Ol_Chiki">Ol_Chiki</a>,
    &#34;Old_Hungarian&#34;:          <a href="#Old_Hungarian">Old_Hungarian</a>,
    &#34;Old_Italic&#34;:             <a href="#Old_Italic">Old_Italic</a>,
    &#34;Old_North_Arabian&#34;:      <a href="#Old_North_Arabian">Old_North_Arabian</a>,
    &#34;Old_Permic&#34;:             <a href="#Old_Permic">Old_Permic</a>,
    &#34;Old_Persian&#34;:            <a href="#Old_Persian">Old_Persian</a>,
    &#34;Old_South_Arabian&#34;:      <a href="#Old_South_Arabian">Old_South_Arabian</a>,
    &#34;Old_Turkic&#34;:             <a href="#Old_Turkic">Old_Turkic</a>,
    &#34;Oriya&#34;:                  <a href="#Oriya">Oriya</a>,
    &#34;Osage&#34;:                  <a href="#Osage">Osage</a>,
    &#34;Osmanya&#34;:                <a href="#Osmanya">Osmanya</a>,
    &#34;Pahawh_Hmong&#34;:           <a href="#Pahawh_Hmong">Pahawh_Hmong</a>,
    &#34;Palmyrene&#34;:              <a href="#Palmyrene">Palmyrene</a>,
    &#34;Pau_Cin_Hau&#34;:            <a href="#Pau_Cin_Hau">Pau_Cin_Hau</a>,
    &#34;Phags_Pa&#34;:               <a href="#Phags_Pa">Phags_Pa</a>,
    &#34;Phoenician&#34;:             <a href="#Phoenician">Phoenician</a>,
    &#34;Psalter_Pahlavi&#34;:        <a href="#Psalter_Pahlavi">Psalter_Pahlavi</a>,
    &#34;Rejang&#34;:                 <a href="#Rejang">Rejang</a>,
    &#34;Runic&#34;:                  <a href="#Runic">Runic</a>,
    &#34;Samaritan&#34;:              <a href="#Samaritan">Samaritan</a>,
    &#34;Saurashtra&#34;:             <a href="#Saurashtra">Saurashtra</a>,
    &#34;Sharada&#34;:                <a href="#Sharada">Sharada</a>,
    &#34;Shavian&#34;:                <a href="#Shavian">Shavian</a>,
    &#34;Siddham&#34;:                <a href="#Siddham">Siddham</a>,
    &#34;SignWriting&#34;:            <a href="#SignWriting">SignWriting</a>,
    &#34;Sinhala&#34;:                <a href="#Sinhala">Sinhala</a>,
    &#34;Sora_Sompeng&#34;:           <a href="#Sora_Sompeng">Sora_Sompeng</a>,
    &#34;Soyombo&#34;:                <a href="#Soyombo">Soyombo</a>,
    &#34;Sundanese&#34;:              <a href="#Sundanese">Sundanese</a>,
    &#34;Syloti_Nagri&#34;:           <a href="#Syloti_Nagri">Syloti_Nagri</a>,
    &#34;Syriac&#34;:                 <a href="#Syriac">Syriac</a>,
    &#34;Tagalog&#34;:                <a href="#Tagalog">Tagalog</a>,
    &#34;Tagbanwa&#34;:               <a href="#Tagbanwa">Tagbanwa</a>,
    &#34;Tai_Le&#34;:                 <a href="#Tai_Le">Tai_Le</a>,
    &#34;Tai_Tham&#34;:               <a href="#Tai_Tham">Tai_Tham</a>,
    &#34;Tai_Viet&#34;:               <a href="#Tai_Viet">Tai_Viet</a>,
    &#34;Takri&#34;:                  <a href="#Takri">Takri</a>,
    &#34;Tamil&#34;:                  <a href="#Tamil">Tamil</a>,
    &#34;Tangut&#34;:                 <a href="#Tangut">Tangut</a>,
    &#34;Telugu&#34;:                 <a href="#Telugu">Telugu</a>,
    &#34;Thaana&#34;:                 <a href="#Thaana">Thaana</a>,
    &#34;Thai&#34;:                   <a href="#Thai">Thai</a>,
    &#34;Tibetan&#34;:                <a href="#Tibetan">Tibetan</a>,
    &#34;Tifinagh&#34;:               <a href="#Tifinagh">Tifinagh</a>,
    &#34;Tirhuta&#34;:                <a href="#Tirhuta">Tirhuta</a>,
    &#34;Ugaritic&#34;:               <a href="#Ugaritic">Ugaritic</a>,
    &#34;Vai&#34;:                    <a href="#Vai">Vai</a>,
    &#34;Warang_Citi&#34;:            <a href="#Warang_Citi">Warang_Citi</a>,
    &#34;Yi&#34;:                     <a href="#Yi">Yi</a>,
    &#34;Zanabazar_Square&#34;:       <a href="#Zanabazar_Square">Zanabazar_Square</a>,
}</pre>

Scripts is the set of Unicode script tables.

<h2 id="In">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L59">In</a>
    <a href="#In">¶</a></h2>
<pre>func In(r <a href="/builtin/#rune">rune</a>, ranges ...*<a href="#RangeTable">RangeTable</a>) <a href="/builtin/#bool">bool</a></pre>

In reports whether the rune is a member of one of the ranges.

<h2 id="Is">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L148">Is</a>
    <a href="#Is">¶</a></h2>
<pre>func Is(rangeTab *<a href="#RangeTable">RangeTable</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

Is reports whether the rune is in the specified table of ranges.

<h2 id="IsControl">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L71">IsControl</a>
    <a href="#IsControl">¶</a></h2>
<pre>func IsControl(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsControl reports whether the rune is a control character. The C (Other) Unicode
category includes more code points such as surrogates; use Is(C, r) to test for
them.

<h2 id="IsDigit">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/digit.go#L1">IsDigit</a>
    <a href="#IsDigit">¶</a></h2>
<pre>func IsDigit(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsDigit reports whether the rune is a decimal digit.

<h2 id="IsGraphic">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L26">IsGraphic</a>
    <a href="#IsGraphic">¶</a></h2>
<pre>func IsGraphic(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsGraphic reports whether the rune is defined as a Graphic by Unicode. Such
characters include letters, marks, numbers, punctuation, symbols, and spaces,
from categories L, M, N, P, S, Zs.

<h2 id="IsLetter">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L80">IsLetter</a>
    <a href="#IsLetter">¶</a></h2>
<pre>func IsLetter(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsLetter reports whether the rune is a letter (category L).

<h2 id="IsLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L182">IsLower</a>
    <a href="#IsLower">¶</a></h2>
<pre>func IsLower(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsLower reports whether the rune is a lower case letter.

<h2 id="IsMark">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L88">IsMark</a>
    <a href="#IsMark">¶</a></h2>
<pre>func IsMark(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsMark reports whether the rune is a mark character (category M).

<h2 id="IsNumber">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L94">IsNumber</a>
    <a href="#IsNumber">¶</a></h2>
<pre>func IsNumber(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsNumber reports whether the rune is a number (category N).

<h2 id="IsOneOf">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L49">IsOneOf</a>
    <a href="#IsOneOf">¶</a></h2>
<pre>func IsOneOf(ranges []*<a href="#RangeTable">RangeTable</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsOneOf reports whether the rune is a member of one of the ranges. The function
"In" provides a nicer signature and should be used in preference to IsOneOf.

<h2 id="IsPrint">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L40">IsPrint</a>
    <a href="#IsPrint">¶</a></h2>
<pre>func IsPrint(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsPrint reports whether the rune is defined as printable by Go. Such characters
include letters, marks, numbers, punctuation, symbols, and the ASCII space
character, from categories L, M, N, P, S and the ASCII space character. This
categorization is the same as IsGraphic except that the only spacing character
is ASCII space, U+0020.

<h2 id="IsPunct">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L103">IsPunct</a>
    <a href="#IsPunct">¶</a></h2>
<pre>func IsPunct(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsPunct reports whether the rune is a Unicode punctuation character (category
P).

<h2 id="IsSpace">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L116">IsSpace</a>
    <a href="#IsSpace">¶</a></h2>
<pre>func IsSpace(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsSpace reports whether the rune is a space character as defined by Unicode's
White Space property; in the Latin-1 space this is

    '\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL), U+00A0 (NBSP).

Other definitions of spacing characters are set by category Z and property
Pattern_White_Space.

<h2 id="IsSymbol">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/graphic.go#L129">IsSymbol</a>
    <a href="#IsSymbol">¶</a></h2>
<pre>func IsSymbol(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsSymbol reports whether the rune is a symbolic character.

<h2 id="IsTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L191">IsTitle</a>
    <a href="#IsTitle">¶</a></h2>
<pre>func IsTitle(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsTitle reports whether the rune is a title case letter.

<h2 id="IsUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L173">IsUpper</a>
    <a href="#IsUpper">¶</a></h2>
<pre>func IsUpper(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#bool">bool</a></pre>

IsUpper reports whether the rune is an upper case letter.

<h2 id="SimpleFold">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L327">SimpleFold</a>
    <a href="#SimpleFold">¶</a></h2>
<pre>func SimpleFold(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

SimpleFold iterates over Unicode code points equivalent under the
Unicode-defined simple case folding. Among the code points equivalent to rune
(including rune itself), SimpleFold returns the smallest rune > r if one exists,
or else the smallest rune >= 0. If r is not a valid Unicode code point,
SimpleFold(r) returns r.

For example:

    SimpleFold('A') = 'a'
    SimpleFold('a') = 'A'

    SimpleFold('K') = 'k'
    SimpleFold('k') = '\u212A' (Kelvin symbol, K)
    SimpleFold('\u212A') = 'K'

    SimpleFold('1') = '1'

    SimpleFold(-2) = -2

<a id="exampleSimpleFold"></a>
Example:

    fmt.Printf("%#U\n", unicode.SimpleFold('A'))      // 'a'
    fmt.Printf("%#U\n", unicode.SimpleFold('a'))      // 'A'
    fmt.Printf("%#U\n", unicode.SimpleFold('K'))      // 'k'
    fmt.Printf("%#U\n", unicode.SimpleFold('k'))      // '\u212A' (Kelvin symbol, K)
    fmt.Printf("%#U\n", unicode.SimpleFold('\u212A')) // 'K'
    fmt.Printf("%#U\n", unicode.SimpleFold('1'))      // '1'

    // Output:
    // U+0061 'a'
    // U+0041 'A'
    // U+006B 'k'
    // U+212A 'K'
    // U+004B 'K'
    // U+0031 '1'

<h2 id="To">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L236">To</a>
    <a href="#To">¶</a></h2>
<pre>func To(_case <a href="/builtin/#int">int</a>, r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

To maps the rune to the specified case: UpperCase, LowerCase, or TitleCase.

<a id="exampleTo"></a>
Example:

    const lcG = 'g'
    fmt.Printf("%#U\n", unicode.To(unicode.UpperCase, lcG))
    fmt.Printf("%#U\n", unicode.To(unicode.LowerCase, lcG))
    fmt.Printf("%#U\n", unicode.To(unicode.TitleCase, lcG))

    const ucG = 'G'
    fmt.Printf("%#U\n", unicode.To(unicode.UpperCase, ucG))
    fmt.Printf("%#U\n", unicode.To(unicode.LowerCase, ucG))
    fmt.Printf("%#U\n", unicode.To(unicode.TitleCase, ucG))

    // Output:
    // U+0047 'G'
    // U+0067 'g'
    // U+0047 'G'
    // U+0047 'G'
    // U+0067 'g'
    // U+0047 'G'

<h2 id="ToLower">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L252">ToLower</a>
    <a href="#ToLower">¶</a></h2>
<pre>func ToLower(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToLower maps the rune to lower case.

<a id="exampleToLower"></a>
Example:

    const ucG = 'G'
    fmt.Printf("%#U\n", unicode.ToLower(ucG))

    // Output:
    // U+0067 'g'

<h2 id="ToTitle">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L263">ToTitle</a>
    <a href="#ToTitle">¶</a></h2>
<pre>func ToTitle(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToTitle maps the rune to title case.

<a id="exampleToTitle"></a>
Example:

    const ucG = 'g'
    fmt.Printf("%#U\n", unicode.ToTitle(ucG))

    // Output:
    // U+0047 'G'

<h2 id="ToUpper">func <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L241">ToUpper</a>
    <a href="#ToUpper">¶</a></h2>
<pre>func ToUpper(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToUpper maps the rune to upper case.

<a id="exampleToUpper"></a>
Example:

    const ucG = 'g'
    fmt.Printf("%#U\n", unicode.ToUpper(ucG))

    // Output:
    // U+0047 'G'

<h2 id="CaseRange">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L47">CaseRange</a>
    <a href="#CaseRange">¶</a></h2>
<pre>type CaseRange struct {
<span id="CaseRange.Lo"></span>    Lo    <a href="/builtin/#uint32">uint32</a>
<span id="CaseRange.Hi"></span>    Hi    <a href="/builtin/#uint32">uint32</a>
<span id="CaseRange.Delta"></span>    Delta d
}</pre>

CaseRange represents a range of Unicode code points for simple (one code point
to one code point) case conversion. The range runs from Lo to Hi inclusive, with
a fixed stride of 1. Deltas are the number to add to the code point to reach the
code point for a different case for that character. They may be negative. If
zero, it means the character is in the corresponding case. There is a special
case representing sequences of alternating corresponding Upper and Lower pairs.
It appears with a fixed Delta of

    {UpperLower, UpperLower, UpperLower}

The constant UpperLower has an otherwise impossible delta value.

<h2 id="Range16">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L22">Range16</a>
    <a href="#Range16">¶</a></h2>
<pre>type Range16 struct {
<span id="Range16.Lo"></span>    Lo     <a href="/builtin/#uint16">uint16</a>
<span id="Range16.Hi"></span>    Hi     <a href="/builtin/#uint16">uint16</a>
<span id="Range16.Stride"></span>    Stride <a href="/builtin/#uint16">uint16</a>
}</pre>

Range16 represents of a range of 16-bit Unicode code points. The range runs from
Lo to Hi inclusive and has the specified stride.

<h2 id="Range32">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L31">Range32</a>
    <a href="#Range32">¶</a></h2>
<pre>type Range32 struct {
<span id="Range32.Lo"></span>    Lo     <a href="/builtin/#uint32">uint32</a>
<span id="Range32.Hi"></span>    Hi     <a href="/builtin/#uint32">uint32</a>
<span id="Range32.Stride"></span>    Stride <a href="/builtin/#uint32">uint32</a>
}</pre>

Range32 represents of a range of Unicode code points and is used when one or
more of the values will not fit in 16 bits. The range runs from Lo to Hi
inclusive and has the specified stride. Lo and Hi must always be >= 1<<16.

<h2 id="RangeTable">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L14">RangeTable</a>
    <a href="#RangeTable">¶</a></h2>
<pre>type RangeTable struct {
<span id="RangeTable.R16"></span>    R16         []<a href="#Range16">Range16</a>
<span id="RangeTable.R32"></span>    R32         []<a href="#Range32">Range32</a>
<span id="RangeTable.LatinOffset"></span>    LatinOffset <a href="/builtin/#int">int</a> <span class="comment">// number of entries in R16 with Hi &lt;= MaxLatin1</span>
}</pre>

RangeTable defines a set of Unicode code points by listing the ranges of code
points within the set. The ranges are listed in two slices to save space: a
slice of 16-bit ranges and a slice of 32-bit ranges. The two slices must be in
sorted order and non-overlapping. Also, R32 should contain only values >=
0x10000 (1<<16).

<h2 id="SpecialCase">type <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L55">SpecialCase</a>
    <a href="#SpecialCase">¶</a></h2>
<pre>type SpecialCase []<a href="#CaseRange">CaseRange</a></pre>

SpecialCase represents language-specific case mappings such as Turkish. Methods
of SpecialCase customize (by overriding) the standard mappings.

<pre>var <span id="AzeriCase">AzeriCase</span> <a href="#SpecialCase">SpecialCase</a> = _TurkishCase</pre>


<pre>var <span id="TurkishCase">TurkishCase</span> <a href="#SpecialCase">SpecialCase</a> = _TurkishCase</pre>


<a id="exampleSpecialCase"></a>
Example:

    t := unicode.TurkishCase

    const lci = 'i'
    fmt.Printf("%#U\n", t.ToLower(lci))
    fmt.Printf("%#U\n", t.ToTitle(lci))
    fmt.Printf("%#U\n", t.ToUpper(lci))

    const uci = 'İ'
    fmt.Printf("%#U\n", t.ToLower(uci))
    fmt.Printf("%#U\n", t.ToTitle(uci))
    fmt.Printf("%#U\n", t.ToUpper(uci))

    // Output:
    // U+0069 'i'
    // U+0130 'İ'
    // U+0130 'İ'
    // U+0069 'i'
    // U+0130 'İ'
    // U+0130 'İ'

<h3 id="SpecialCase.ToLower">func (SpecialCase) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L292">ToLower</a>
    <a href="#SpecialCase.ToLower">¶</a></h3>
<pre>func (special <a href="#SpecialCase">SpecialCase</a>) ToLower(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToLower maps the rune to lower case giving priority to the special mapping.

<h3 id="SpecialCase.ToTitle">func (SpecialCase) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L283">ToTitle</a>
    <a href="#SpecialCase.ToTitle">¶</a></h3>
<pre>func (special <a href="#SpecialCase">SpecialCase</a>) ToTitle(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToTitle maps the rune to title case giving priority to the special mapping.

<h3 id="SpecialCase.ToUpper">func (SpecialCase) <a href="//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L274">ToUpper</a>
    <a href="#SpecialCase.ToUpper">¶</a></h3>
<pre>func (special <a href="#SpecialCase">SpecialCase</a>) ToUpper(r <a href="/builtin/#rune">rune</a>) <a href="/builtin/#rune">rune</a></pre>

ToUpper maps the rune to upper case giving priority to the special mapping.

<h2 id="pkg-note-BUG">Bugs</h2>

- [☞](//github.com/golang/go/blob/2ea7d3461bb41d0ae12b56ee52d43314bcdb97f9/src/unicode/letter.go#L57)  There is no mechanism for full case folding, that is, for characters that
  involve multiple runes in the input or output.

## Subdirectories
- [..](..)
- [utf16](utf16/)
- [utf8](utf8/)
