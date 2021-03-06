unicode-hax
===========

A library to assist in security-testing Unicode enabled applications. The original intent of putting this together 
was threefold:

1. To provide a reduced set of useful Unicode input to a software fuzzer
2. To document historically problematic Unicode characters sequences which 
might negatively affect protocols and Web applications.  
3. To lookup mappings for ASCII equivalent characters

For example, the __best-fit__ and __normalization__ mappings can be useful for testing Web applications for 
cross-site scripting (XSS) or SQL injection (SQLi) vulnerabilities, by providing you with alternative
characters which map back, or transform, to the intended ASCII encoded input - such as "<", "'", etc.

Additionally, many __problem characters__ have been pre-defined as a small set, reducing the number of iterations
a fuzzer might need to perform.

Major features: 
- best fit mappings 
- Unicode normalization mappings 
- hard-coded Unicode characters useful in fuzzing

For fuzzing applications it includes: 
- ill-formed byte sequences 
- non-characters
- private use area (PUA)
- unassigned code points 
- code points with special meaning such as the BOM and RLO 
- half-surrogate values

/TestUniHax
-----------
This Windows form application loads the UniHax library mainly to test the best-fit and normalization mappings.  
If you simply input a single ASCII character, all of its equivalent characters will be displayed.  

e.g. If you're testing a Web-application and want to test equivalents for the "<" character U+003C, 
enter that as input and select either "best-fit mapping", which is linked to a charset encoding,
or "normalization" equivalents.  For this character, the following are best-fits:

- U+003B in the APL-ISO-IR-68 encoding
- U+0014 in the CP424 encoding
- etc...

Also, the following are normalization decomposition mappings:

- U+FE64 SMALL LESS-THAN SIGN
- U+FF1C FULLWIDTH LESS-THAN SIGN

/UniHax
-------
This library contains a small set of __problematic Unicode characters__ in **Fuzzer.cs** such as the following:

```csharp
        /// <summary>
        /// An unassigned code point U+0FED
        /// </summary>
        public static readonly string uUnassigned = "\u0FED";
        /// <summary>
        ///  An illegal low half-surrogate U+DEAD
        /// </summary>
        public static readonly string uDEAD = "\uDEAD";
```

Also the following method to return those characters as a byte array in any encoding.  

```csharp
public byte[] GetCharacterBytes(string encoding, string character)
```

There's also the following method to return any Unicode character as a malformed byte sequence, simply by 
trimming the last byte.

```csharp
public byte[] GetCharacterBytesMalformed(string encoding, string character)
```

This project also contains the data files, pre-created in the __/data__ folder, and a __Mapping.cs__ Mapping 
class which can lookup mapping equivalents for the following:

- ASCII equivalent best-fit mappings across legacy character encodings
- ASCII equivalent mappings for Unicode normalization types.  For example, Web browsers commonly use 
  a form of normalization for keeping URL content and host names compatible.

For more on Unicode Normalization see TR15:  http://www.unicode.org/reports/tr15/

License
-------
Unicode-Hax by <a href="http://www.lookout.net/">Chris Weber</a> is licensed under a 
<a href="http://creativecommons.org/licenses/by-nc-sa/3.0/deed.en_US">
Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported License </a>. 
Based on a work at https://github.com/cweb/unicode-hax.
