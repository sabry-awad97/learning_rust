# Strings and Text

## Some Unicode Background

Unicode is a standard encoding system that assigns unique code points to each character, symbol, or digit from various writing systems, including Latin, Cyrillic, Greek, Chinese, Arabic, and many others. Unicode allows computers to store, process, and transmit text in a consistent and interoperable manner, regardless of the language or script used.

In the early days of computing, different character encodings were used to represent text, depending on the language or region. For example, ASCII (American Standard Code for Information Interchange) was a widely used character encoding system for English-language text, while EBCDIC (Extended Binary Coded Decimal Interchange Code) was used for IBM mainframe systems. However, as computers became more global, it became clear that a unified character encoding standard was needed.

In the 1980s, Unicode was developed as a joint effort of major software and hardware companies to address this problem. The first version of Unicode was released in 1991, and since then it has been expanded to include over 143,000 characters from more than 150 scripts.

Unicode can be encoded using different schemes, such as UTF-8, UTF-16, and UTF-32, which map each code point to a specific sequence of bytes. UTF-8 is the most common encoding scheme used on the web, as it is compact, efficient, and backwards-compatible with ASCII. UTF-16 is commonly used on Microsoft Windows systems, while UTF-32 is less common but offers a fixed-length encoding for all code points.

In Rust, Unicode support is built into the language and the standard library, with various methods and types designed to handle Unicode text manipulation tasks.

### ASCII, Latin-1, and Unicode

ASCII, Latin-1, and Unicode are all character encoding systems used to represent text in digital form. However, they differ in terms of their range of characters, encoding schemes, and compatibility with different languages and scripts.

ASCII (American Standard Code for Information Interchange) is a 7-bit character encoding system that was developed in the 1960s to represent English-language text on computers. ASCII uses 128 code points to represent uppercase and lowercase letters, digits, punctuation marks, and control characters. ASCII is a simple and efficient encoding system, but it has limited support for non-English languages and scripts.

Latin-1, also known as ISO-8859-1, is an 8-bit character encoding system that extends ASCII by adding 128 additional characters for Western European languages, such as French, German, and Spanish. Latin-1 uses the first 256 code points to represent these characters, with the first 128 code points being identical to ASCII. Latin-1 is widely used in legacy systems and some web applications, but it has limited support for other scripts and languages.

Unicode is a more comprehensive character encoding system that assigns unique code points to each character, symbol, or digit from various writing systems, including Latin, Cyrillic, Greek, Chinese, Arabic, and many others. Unicode allows computers to store, process, and transmit text in a consistent and interoperable manner, regardless of the language or script used. Unicode uses variable-length encoding schemes, such as UTF-8, UTF-16, and UTF-32, to map each code point to a specific sequence of bytes. Unicode is widely used on the web, in mobile devices, and in many software applications.

Unicode and ASCII match for all of ASCII's code points, which range from 0 to 0x7f (127 in decimal). ASCII was one of the first character encoding systems used on computers, and it assigned unique codes to each character within this range. Unicode was designed to be backward-compatible with ASCII, meaning that the first 128 Unicode code points are identical to ASCII.

This means that the ASCII characters, such as uppercase and lowercase letters, digits, and punctuation marks, can be represented using the same codes in Unicode. For example, the ASCII code for uppercase letter 'A' is 65 (0x41 in hexadecimal), and the Unicode code point for uppercase letter 'A' is also 65 (U+0041 in Unicode notation).

However, Unicode extends beyond the ASCII range to include characters from other scripts and languages, such as Cyrillic, Greek, Chinese, Arabic, and many others. These characters are represented using higher Unicode code points that require more than one byte to encode.

Since Unicode is a superset of Latin-1, it's straightforward to convert a Latin-1 character to Unicode by simply casting it to a char type.

```rs
fn latin1_to_char(latin1: u8) -> char {
    atin1 as char
}
```

The `latin1_to_char` function in Rust takes a `u8` value representing a Latin-1 character and converts it to a `char` type by using a simple cast.

Similarly, if a Unicode character falls within the Latin-1 range (i.e., its code point is less than or equal to 0xFF), it can be converted to a Latin-1 character by casting it to a u8 type.

```rs
fn char_to_latin1(c: char) -> Option<u8> {
    if c as u32 <= 0xff {
        Some(c as u8)
    } else {
        None
    }
```

The `char_to_latin1` function takes a `char` value and checks whether its Unicode code point is within the Latin-1 range. If it is, the function returns `Some(u8)` with the corresponding `u8` value. Otherwise, the function returns `None`.

### UTF-8

UTF-8 (Unicode Transformation Format, 8-bit) is a variable-length encoding scheme for representing Unicode characters. It uses 8-bit bytes to represent ASCII characters (which have the same code points as in ASCII) and multi-byte sequences to represent non-ASCII characters.

UTF-8 was designed to be backward-compatible with ASCII, meaning that any ASCII text is also valid UTF-8. For example, the ASCII string "hello" can be represented in UTF-8 using the same sequence of bytes: 0x68 0x65 0x6c 0x6c 0x6f.

UTF-8 encodes a Unicode character as a sequence of one to four bytes, depending on the code point of the character. Here's a breakdown of how UTF-8 encodes characters:

- For characters in the ASCII range (code points 0 to 127), UTF-8 uses a single byte with the same value as the ASCII code.

- For characters in the range 128 to 2047, UTF-8 uses two bytes to represent the character. The first byte has the prefix 110, and the second byte has the prefix 10.

- For characters in the range 2048 to 65535, UTF-8 uses three bytes to represent the character. The first byte has the prefix 1110, and the second and third bytes have the prefix 10.

- For characters in the range 65536 to 1114111, UTF-8 uses four bytes to represent the character. The first byte has the prefix 11110, and the second, third, and fourth bytes have the prefix 10.

The exact encoding of a character depends on its code point, which is a numerical value assigned to each character in the Unicode standard.

The `char` type in Rust represents a Unicode character, and the `String` and `&str` types represent UTF-8 encoded strings.

There are two important restrictions on well-formed UTF-8 sequences:

1. Only the shortest encoding for any given code point is considered well-formed: In other words, you can't use more bytes than necessary to represent a particular code point in UTF-8. This ensures that there is exactly one valid UTF-8 encoding for a given code point. For example, the code point U+0041 (Latin capital letter "A") can be encoded in UTF-8 using a single byte: `0x41`. If you were to use two or more bytes to encode this code point, the resulting sequence would not be considered well-formed UTF-8.

1. Well-formed UTF-8 must not encode numbers from 0xD800 through 0xDFFF or beyond 0x10FFFF: These numbers are reserved for special purposes within Unicode and are not valid code points. Attempting to encode these numbers in UTF-8 results in an invalid sequence.

These two restrictions ensure that any UTF-8 encoded text can be correctly interpreted by any software that understands the UTF-8 encoding. If a sequence of bytes does not satisfy these restrictions, it is not a valid UTF-8 sequence and may cause errors or unexpected behavior when processed by software that expects valid UTF-8.

In UTF-8, the encoding of each code point is spread across one to four bytes, with the first byte indicating how many bytes are used to represent the code point. The first byte of a UTF-8 sequence is always in the range 0x00 to 0xFD, and its upper bits indicate the length of the sequence.

The following table shows the encoding format for UTF-8:

| Code point range    | UTF-8 octet sequence                |
| ------------------- | ----------------------------------- |
| U+0000 to U+007F    | 0xxxxxxx                            |
| U+0080 to U+07FF    | 110xxxxx 10xxxxxx                   |
| U+0800 to U+FFFF    | 1110xxxx 10xxxxxx 10xxxxxx          |
| U+10000 to U+10FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |

The "x" in the above table represents a bit that can take on any value.

The first byte of a UTF-8 sequence is always in the range 0x00 to 0xFD, and the upper bits of this byte indicate the length of the sequence. Specifically:

- If the first byte starts with the bits "0xxx", then it is a single-byte sequence, and the remaining 7 bits are used to represent the code point.
- If the first byte starts with the bits "110x", then it is a two-byte sequence, and the remaining 5 bits in the first byte and the 6 bits in the second byte are used to represent the code point.
- If the first byte starts with the bits "1110", then it is a three-byte sequence, and the remaining 4 bits in the first byte and the 6 bits in each of the second and third bytes are used to represent the code point.
- If the first byte starts with the bits "11110", then it is a four-byte sequence, and the remaining 3 bits in the first byte and the 6 bits in each of the second, third, and fourth bytes are used to represent the code point.

Therefore, by looking at the upper bits of any byte in a UTF-8 sequence, you can determine whether it is the first byte of a sequence or a continuation byte that belongs to a multi-byte sequence.

One of the advantages of UTF-8 is that it has a fixed-length encoding for all code points, with no need for unbounded loops when processing the data. This makes it easier and safer to work with untrusted data, as you can quickly validate the length of the encoded character and ensure that it is well-formed. Additionally, because ASCII characters are encoded as single bytes in UTF-8, ASCII-compatible text can be processed quickly and efficiently using UTF-8 encoding.

Because of the way UTF-8 encodes characters, it is always possible to unambiguously determine where each character's encoding begins and ends, even when starting from an arbitrary point within the string. This makes it easy to search for specific byte sequences, such as ASCII delimiters, within UTF-8 encoded strings, as you can simply scan for the relevant byte without needing to keep track of the UTF-8 encoding structure.

Additionally, because ASCII characters are encoded as single bytes in UTF-8, many algorithms that work on byte strings can be used directly on UTF-8 encoded strings without modification. This is because these algorithms do not need to examine every byte of the string in order to work correctly. For example, a simple algorithm to find the first occurrence of a substring within a string can be implemented using a sliding window approach that examines only a small portion of the string at a time, making it efficient even for very large UTF-8 encoded strings.

### Text Directionality

Text directionality refers to the way that text is laid out on a page or screen, and is particularly relevant for scripts that are written from right to left, such as Arabic and Hebrew. When working with text in Rust, it is important to be aware of text directionality issues in order to ensure that your text is displayed correctly.

One common issue with text directionality is the display of mixed-direction text, where a string contains both left-to-right and right-to-left text. This can happen, for example, when a sentence in English is followed by a name in Arabic. In order to ensure that mixed-direction text is displayed correctly, it is necessary to use special Unicode characters called directional formatting characters, which tell the text renderer which direction to use for each part of the string.

Another issue with text directionality is the possibility of bidirectional text overrides, where a special Unicode character is used to force the text directionality to be the opposite of what it should be. This can cause text to be displayed incorrectly, and can even be used to inject malicious code into a document.

To deal with text directionality issues in Rust, you can use the Unicode bidi algorithm to automatically determine the correct directionality of a string. Rust provides a crate called unicode-bidi that implements the Unicode bidi algorithm and provides functions for working with bidirectional text.

Directional formatting characters are special characters in Unicode that are used to control the directionality of text. They are used to override the default direction of text and ensure that it is displayed correctly.

There are several directional formatting characters in Unicode, including:

- Left-to-Right Mark (LRM): This character is used to indicate that the text following it should be displayed from left to right, even if the surrounding text is displayed from right to left.

- Right-to-Left Mark (RLM): This character is used to indicate that the text following it should be displayed from right to left, even if the surrounding text is displayed from left to right.

- Left-to-Right Embedding (LRE) and Right-to-Left Embedding (RLE): These characters are used to embed text with a different directionality in a line of text with a different directionality. The LRE and RLE characters are followed by the embedded text, which is then followed by a Pop Directional Format (PDF) character.

- Pop Directional Format (PDF): This character is used to mark the end of an embedded section of text.

- Left-to-Right Override (LRO) and Right-to-Left Override (RLO): These characters are used to override the default directionality of text. Text between an LRO or RLO character and a PDF character will be displayed in the specified directionality, even if the surrounding text is displayed in the opposite directionality.

Directional formatting characters are important for displaying text correctly in languages that are written from right to left, such as Arabic and Hebrew. They are also used in mixed-language documents where text in different directionality is displayed on the same line.

## Characters (char)

A `char` represents a single Unicode character and is 4 bytes (32 bits) in size. Rust’s `char` type provides support for Unicode characters and grapheme clusters, and it is important to understand how to work with them when dealing with text in Rust.

Creating a `char` is easy, simply surround a Unicode character with single quotes.

```rs
let c = 'A';
let heart_emoji = '❤';
let euro_symbol = '€';
let japanese_hiragana = 'ひ';
```

To create a char containing a Unicode character that requires more than one byte in UTF-8 encoding, use the appropriate escape sequence. For example, to create a char containing the Japanese hiragana character ‘あ’, you would write:

```rs
let c = '\u{3042}';
```

Iterating over a string will yield a sequence of char values, which can be processed or manipulated as necessary.

```rs
let s = "Hello, 世界!";
for c in s.chars() {
    println!("{}", c);
}
```

### Classifying Characters

In Rust, characters can be classified into different categories based on their properties. This can be useful for performing operations on strings, such as checking if a string contains certain types of characters or converting all characters in a string to a specific category.

Rust provides the `std::char::UnicodeChar` trait, which defines methods for classifying characters. Here are some of the methods available:

- `is_alphabetic`: Returns true if the character is alphabetic, which includes all characters in the Unicode - category "Letter", as well as certain other characters that are considered to be letters in certain contexts.
- `is_ascii`: Returns true if the character is an ASCII character, meaning it has a code point between 0 and 127.
- `is_digit`: Returns true if the character is a digit (0-9).
- `is_lowercase`: Returns true if the character is a lowercase letter.
- `is_uppercase`: Returns true if the character is an uppercase letter.
- `is_whitespace`: Returns true if the character is whitespace, including spaces, tabs, and line breaks.
- `is_control`: Returns true if the character is a control character, such as a newline or tab.
- `is_punctuation`: Returns true if the character is punctuation, such as commas, periods, or brackets.
  Here's an example of using some of these methods:

```rs
fn main() {
    let c1 = 'A';
    let c2 = 'a';
    let c3 = '0';
    let c4 = ' ';

    println!("{} is alphabetic: {}", c1, c1.is_alphabetic());
    println!("{} is alphabetic: {}", c2, c2.is_alphabetic());
    println!("{} is digit: {}", c3, c3.is_digit(10));
    println!("{} is whitespace: {}", c4, c4.is_whitespace());
}
```

Classification methods for char type

| Method               | Description                                                                                                                     | Examples                                                                                  |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| ch.is_numeric()      | A numeric character. This includes the Unicode general categories “Number; digit” and “Number; letter” but not “Number; other”. | '4'.is_numeric() &lt;br&gt; 'ᛮ'.is_numeric() &lt;br&gt; '⑧'.is_numeric()                  |
| ch.is_alphabetic()   | An alphabetic character: Unicode’s “Alphabetic” derived property.                                                               | 'q'.is_alphabetic() &lt;br&gt; '七'.is_alphabetic()                                       |
| ch.is_alphanumeric() | Either numeric or alphabetic, as defined earlier.                                                                               | '9'.is_alphanumeric() &lt;br&gt; '饂'.is_alphanumeric() &lt;br&gt;!'\*'.is_alphanumeric() |
| ch.is_whitespace()   | A whitespace character: Unicode character property “WSpace=Y”.                                                                  | ' '.is_whitespace() &lt;br&gt; '\\n'.is_whitespace() &lt;br&gt; '\\u{A0}'.is_whitespace() |
| ch.is_control()      | A control character: Unicode’s “Other, control” general category.                                                               | '\\n'.is_control() &lt;br&gt; '\\u{85}'.is_control()                                      |

ASCII classification methods for char

| Method                      | Description                                                                         | Examples                                                                                                  |
| --------------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `ch.is_ascii()`             | Whether the character is within the ASCII range (0-127).                            | `'a'.is_ascii()`&lt;br&gt;`'🙂'.is_ascii()`                                                               |
| `ch.is_ascii_alphabetic()`  | Whether the character is an ASCII alphabetic character (a-z or A-Z).                | `'a'.is_ascii_alphabetic()` &lt;br&gt; `'Z'.is_ascii_alphabetic()` &lt;br&gt; `'5'.is_ascii_alphabetic()` |
| `ch.is_ascii_uppercase()`   | Whether the character is an ASCII uppercase character (A-Z).                        | `'A'.is_ascii_uppercase()` &lt;br&gt; `'z'.is_ascii_uppercase()` &lt;br&gt; `'5'.is_ascii_uppercase()`    |
| `ch.is_ascii_lowercase()`   | Whether the character is an ASCII lowercase character (a-z).                        | `'a'.is_ascii_lowercase()` &lt;br&gt; `'Z'.is_ascii_lowercase()` &lt;br&gt; `'5'.is_ascii_lowercase()`    |
| `ch.is_ascii_digit()`       | Whether the character is an ASCII decimal digit (0-9).                              | `'1'.is_ascii_digit()` &lt;br&gt; `'a'.is_ascii_digit()`                                                  |
| `ch.is_ascii_hexdigit()`    | Whether the character is an ASCII hexadecimal digit (0-9, a-f, A-F).                | `'1'.is_ascii_hexdigit()` &lt;br&gt; `'f'.is_ascii_hexdigit()` &lt;br&gt; `'G'.is_ascii_hexdigit()`       |
| `ch.is_ascii_punctuation()` | Whether the character is an ASCII punctuation character.                            | `'!'.is_ascii_punctuation()` &lt;br&gt; `'a'.is_ascii_punctuation()`                                      |
| `ch.is_ascii_graphic()`     | Whether the character is an ASCII graphic character (has a visible representation). | `'a'.is_ascii_graphic()` &lt;br&gt; `' '.is_ascii_graphic()` &lt;br&gt; `'\n'.is_ascii_graphic()`         |

The `is_ascii_...` methods are also available on the `u8` byte type, which can be useful when working with byte strings.

`is_whitespace()` and `is_ascii_whitespace()` differ in their definition of whitespace characters.

The `is_whitespace()` method is defined for the char type and checks whether the character is a Unicode whitespace character. This includes all whitespace characters in the Unicode standard, such as space, tab, newline, and various other types of space characters.

The `is_ascii_whitespace()` method, on the other hand, is defined for the u8 type and checks whether the byte represents an ASCII whitespace character. This includes only the ASCII space character (32), tab (9), newline (10), vertical tab (11), form feed (12), and carriage return (13).

So while `is_whitespace()` considers a broader set of characters as whitespace, `is_ascii_whitespace()` only considers ASCII whitespace characters.

### Handling Digits

In Rust, digits can be represented as characters or as numeric values. The `char` type can represent Unicode scalar values that include numeric characters, while the `u8` and `u32` types can represent the numerical values of these characters.

The `char` type provides methods to check if a character represents a digit. For example, the `is_digit` method returns true if a character is a valid digit in the given radix. The radix is specified as an integer argument, and it should be between 2 and 36 inclusive.

Here is an example of using `is_digit` method:

```rs
assert!('7'.is_digit(10));
assert!(!'z'.is_digit(10));
assert!('F'.is_digit(16));
assert!(!'G'.is_digit(16));
```

In addition to the `is_digit` method, the `char` type provides methods to get the numeric value of a digit. The `to_digit` method returns the numeric value of a digit in the given radix, or `None` if the character is not a valid digit. Here is an example:

```rs
assert_eq!('7'.to_digit(10), Some(7));
assert_eq!('z'.to_digit(10), None);
assert_eq!('F'.to_digit(16), Some(15));
assert_eq!('G'.to_digit(16), None);
```

On the other hand, the `u8` and `u32` types provide methods to convert digits represented as ASCII characters to their numerical values. The `is_digit` method on `u8` checks if a byte represents a valid ASCII digit, while `is_ascii_digit` method checks for digits encoded in UTF-8. Here is an example:

```rs
assert!(b'7'.isdigit());
assert!(!b'z'.isdigit());
assert!(b'7'.is_ascii_digit());
assert!(!b'z'.is_ascii_digit());
```

The `b'7'` notation represents a byte with the ASCII value of 7. The `is_digit` method returns true if the byte represents a valid ASCII digit (in the range 0x30-0x39), while `is_ascii_digit` method returns true if the byte represents a valid ASCII digit encoded in UTF-8.

The `std::char::from_digit` function is a method that converts a digit to the corresponding Unicode character.

It takes two arguments: `num`, the digit to convert, and `radix`, the base of the number system in which the digit is represented.

If `num` is a valid digit, the function returns the corresponding Unicode character. The returned character will be in the range 0 to 0x10FFFF, which is the valid range for Unicode code points.

```rs
let digit = 7;
let base = 16;
if let Some(c) = std::char::from_digit(digit, base) {
    println!("The digit {} in base {} is {}", digit, base, c);
} else {
    println!("Invalid digit {} in base {}", digit, base);
}
```

### Case Conversion for Characters

Characters have methods to check whether they are lowercase or uppercase, as well as methods to convert them to lowercase or uppercase. These methods are:

- `ch.is_lowercase()`: Returns `true` if the character is a lowercase letter, `false` otherwise.
- `ch.is_uppercase()`: Returns `true` if the character is an uppercase letter, `false` otherwise.
- `ch.to_lowercase()`: Converts the character to its lowercase equivalent, if any. If the character does not have a lowercase equivalent, it returns the original character.
- `ch.to_uppercase()`: Converts the character to its uppercase equivalent, if any. If the character does not have an uppercase equivalent, it returns the original character.

Here are some examples of using these methods:

```rs
let ch1 = 'a';
let ch2 = 'A';
let ch3 = '7';

assert!(ch1.is_lowercase());
assert!(!ch1.is_uppercase());
assert_eq!(ch1.to_uppercase(), 'A');

assert!(ch2.is_uppercase());
assert!(!ch2.is_lowercase());
assert_eq!(ch2.to_lowercase(), 'a');

assert!(!ch3.is_uppercase());
assert!(!ch3.is_lowercase());
assert_eq!(ch3.to_uppercase(), '7');
assert_eq!(ch3.to_lowercase(), '7');
```

Note that some characters do not have an uppercase or lowercase equivalent. In those cases, the `to_uppercase()` or `to_lowercase()` methods will return the original character.

### Conversions to and from Integers

To convert a character to its corresponding Unicode code point as a 32-bit integer, you can use the `as u32`. Here is an example:

```rs
let ch = '🙂';
let code_point = ch as u32;
println!("The code point of {} is {}.", ch, code_point);
```

To convert an integer to a character, you can use the `from_u32` method. Here is an example:

```rs
let code_point = 128578;
let ch = char::from_u32(code_point).unwrap();
println!("The character corresponding to {} is {}.", code_point, ch);
```

It's important to note that not all integers can be converted to characters, since only valid Unicode code points are allowed. Therefore, `from_u32` returns an `Option<char>` instead of a char. You should always check the return value to ensure that the conversion was successful before using the resulting character.
