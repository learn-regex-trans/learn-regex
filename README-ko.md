<br/>
<p align="center">
<img src="https://i.imgur.com/bYwl7Vf.png" alt="Learn Regex">
</p><br/>

## 번역:

* [English](README.md)
* [Español](README-es.md)
* [中文版](README-cn.md)
* [日本語](README-ja.md)
* [한국어](README-ko.md)

## 정규식(Regular Expression)이란 무엇인가?

> 정규 표현식은 텍스트에서 특정 패턴을 찾는데 사용되는 문자나 심볼릭의 집합이다.

정규 표현식(Regular expression)은 문자열에 대해 왼쪽에서 오른쪽으로 일치하는 패턴으로, 보통 "regex" 또는 "regexp", "정규식"으로 줄여 말할 수 있다. 정규 표현식은 문자열 재배치, validating form, 패턴 일치에 따라 문자열에서 하위 문자열 추출 등에 사용된다.

사용자가 username을 입력할때 규칙을 설정한다고 가정하자. 우리는 username이 이상하지 않도록 문자, 숫자 밑줄(`_`), 하이픈(`-`)만 포함하고 길이제한을 설정하고자 할 때, 다음과 같은 정규 표현식으로 검증한다:
<br/><br/>
<p align="center">
<img src="https://i.imgur.com/ekFpQUg.png" alt="Regular expression">
</p>

위의 정규식은 `join_doe`, `jo-hn_doe`, `john12_as`를 허용한다. 그러나 대문자와 너무 짧은 문자열은 허용하지 않으므로 `Jo`는 매칭되지 않는다.

## Table of Contents

- [기본 Matchers](#1-기본-matchers)
- [메타 문자열](#2-메타-문자열)
  - [Full stop](#21-full-stop)
  - [Character set](#22-character-set)
    - [Negated character set](#221-negated-character-set)
  - [Repetitions](#23-repetitions)
    - [The Star](#231-the-star)
    - [The Plus](#232-the-plus)
    - [The Question Mark](#233-the-question-mark)
  - [Braces](#24-braces)
  - [Character Group](#25-character-group)
  - [Alternation](#26-alternation)
  - [Escaping special character](#27-escaping-special-character)
  - [Anchors](#28-anchors)
    - [Caret](#281-caret)
    - [Dollar](#282-dollar)
- [Shorthand Character Sets](#3-shorthand-character-sets)
- [Lookaround](#4-lookaround)
  - [Positive Lookahead](#41-positive-lookahead)
  - [Negative Lookahead](#42-negative-lookahead)
  - [Positive Lookbehind](#43-positive-lookbehind)
  - [Negative Lookbehind](#44-negative-lookbehind)
- [Flags](#5-flags)
  - [Case Insensitive](#51-case-insensitive)
  - [Global search](#52-global-search)
  - [Multiline](#53-multiline)
- [Bonus](#bonus)

## 1. 기본 Matchers

정규식은 단지 텍스트에서 검색하는데 사용하는 문자열의 패턴이다. 예를 들어, 정규식 `the`는 다음과 같다: 문자 `t`, 다음 문자 `h`, 다음 문자 `e`를 의미한다.

<pre>
"the" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

[정규식 테스트](https://regex101.com/r/dmRygT/1)

정규식 `123`은 문자열 `123`과 일치한다. The regular expression is matched against an input string by comparing each
character in the regular expression to each character in the input string, one after another. 정규식은 일반적으로 대소 문자를 구분하므로 정규식 `The`는 문자열 `the`와 일치하지 않는다.

<pre>
"The" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

[정규식 테스트](https://regex101.com/r/1paXsy/1)

## 2. 메타 문자열

메타 문자열은 정규식의 구성요소이며, 그대로를 의미하지 않고 어떤 특별한 방식으로 해석된다. 일부 메타 문자는 특별한 의미를 가지며 대괄호 안에 작성된다.
메타 문자는 다음과 같다:

메타 문자 | 설명
:---: | ------------------------------------------------------------------------------------------------------
  .   | 마침표는 줄 바꿈을 제외한 모든 단일 문자와 일치한다.
 [ ]  | 문자 집합(Character class). 대괄호 사이에 포함 된 모든 문자와 일치한다.
[^ ]  | 부정 문자 집합. 대괄호 사이에 포함되지 않은 문자와 일치한다.
  \*  | 앞의 문자가 없거나 하나 이상 연속하는 문자와 일치한다.
  \+  | 앞의 문자라 하나 이상 연속하는 문자와 일치한다.
  ?   | 앞의 문자가 없거나 하나만 있을 경우 일치한다.
{n,m} | Braces. 앞의 문자가 "n"개 이상 "m"개 이하일 경우 일치한다.
(xyz) | 문자 집합(Character group). 정확한 순서로 문자열 xyz와 매칭된다.
  \|  | Alternation. 기호(`|`) 앞에있는 문자 또는 기호 뒤에 나오는 문자와 일치한다.
  \\  | 다음 문자를 이스케이프 처리한다. 즉, 예약문자를 매칭시킬 수 있다 `[ ] ( ) { } . * + ? ^ $ \ |`
  ^   | 입력의 시작과 일치한다.
  $   | 입력의 끝과 일치한다.


## 2.1 Full stop

Full stop `.` is the simplest example of meta character. The meta character `.` matches any single character. It will not match return
or newline characters. For example, the regular expression `.ar` means: any character, followed by the letter `a`, followed by the
letter `r`.

<pre>
".ar" => The <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[Test the regular expression](https://regex101.com/r/xc9GkU/1)

## 2.2 Character set

Character sets are also called character class. Square brackets are used to specify character sets. Use a hyphen inside a character set to
specify the characters' range. The order of the character range inside square brackets doesn't matter. For example, the regular
expression `[Tt]he` means: an uppercase `T` or lowercase `t`, followed by the letter `h`, followed by the letter `e`.

<pre>
"[Tt]he" => <a href="#learn-regex"><strong>The</strong></a> car parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[Test the regular expression](https://regex101.com/r/2ITLQ4/1)

A period inside a character set, however, means a literal period. The regular expression `ar[.]` means: a lowercase character `a`, followed by letter `r`, followed by a period `.` character.

<pre>
"ar[.]" => A garage is a good place to park a c<a href="#learn-regex"><strong>ar.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/wL3xtE/1)

### 2.2.1 Negated character set

In general, the caret symbol represents the start of the string, but when it is typed after the opening square bracket it negates the
character set. For example, the regular expression `[^c]ar` means: any character except `c`, followed by the character `a`, followed by
the letter `r`.

<pre>
"[^c]ar" => The car <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[Test the regular expression](https://regex101.com/r/nNNlq3/1)

## 2.3 Repetitions

Following meta characters `+`, `*` or `?` are used to specify how many times a subpattern can occur. These meta characters act
differently in different situations.

### 2.3.1 The Star

The symbol `*` matches zero or more repetitions of the preceding matcher. The regular expression `a*` means: zero or more repetitions
of preceding lowercase character `a`. But if it appears after a character set or class then it finds the repetitions of the whole
character set. For example, the regular expression `[a-z]*` means: any number of lowercase letters in a row.

<pre>
"[a-z]*" => T<a href="#learn-regex"><strong>he</strong></a> <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>parked</strong></a> <a href="#learn-regex"><strong>in</strong></a> <a href="#learn-regex"><strong>the</strong></a> <a href="#learn-regex"><strong>garage</strong></a> #21.
</pre>

[Test the regular expression](https://regex101.com/r/7m8me5/1)

The `*` symbol can be used with the meta character `.` to match any string of characters `.*`. The `*` symbol can be used with the
whitespace character `\s` to match a string of whitespace characters. For example, the expression `\s*cat\s*` means: zero or more
spaces, followed by lowercase character `c`, followed by lowercase character `a`, followed by lowercase character `t`, followed by
zero or more spaces.

<pre>
"\s*cat\s*" => The fat<a href="#learn-regex"><strong> cat </strong></a>sat on the <a href="#learn-regex">con<strong>cat</strong>enation</a>.
</pre>

[Test the regular expression](https://regex101.com/r/gGrwuz/1)

### 2.3.2 The Plus

The symbol `+` matches one or more repetitions of the preceding character. For example, the regular expression `c.+t` means: lowercase
letter `c`, followed by at least one character, followed by the lowercase character `t`. It needs to be clarified that `t` is the last `t` in the sentence.

<pre>
"c.+t" => The fat <a href="#learn-regex"><strong>cat sat on the mat</strong></a>.
</pre>

[Test the regular expression](https://regex101.com/r/Dzf9Aa/1)

### 2.3.3 The Question Mark

In regular expression the meta character `?` makes the preceding character optional. This symbol matches zero or one instance of
the preceding character. For example, the regular expression `[T]?he` means: Optional the uppercase letter `T`, followed by the lowercase
character `h`, followed by the lowercase character `e`.

<pre>
"[T]he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

[Test the regular expression](https://regex101.com/r/cIg9zm/1)

<pre>
"[T]?he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in t<a href="#learn-regex"><strong>he</strong></a> garage.
</pre>

[Test the regular expression](https://regex101.com/r/kPpO2x/1)

## 2.4 Braces

In  regular expression braces that are also called quantifiers are used to specify the number of times that a
character or a group of characters can be repeated. For example, the regular expression `[0-9]{2,3}` means: Match at least 2 digits but not more than 3 (
characters in the range of 0 to 9).

<pre>
"[0-9]{2,3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

[Test the regular expression](https://regex101.com/r/juM86s/1)

We can leave out the second number. For example, the regular expression `[0-9]{2,}` means: Match 2 or more digits. If we also remove
the comma the regular expression `[0-9]{3}` means: Match exactly 3 digits.

<pre>
"[0-9]{2,}" => The number was 9.<a href="#learn-regex"><strong>9997</strong></a> but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

[Test the regular expression](https://regex101.com/r/Gdy4w5/1)

<pre>
"[0-9]{3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to 10.0.
</pre>

[Test the regular expression](https://regex101.com/r/Sivu30/1)

## 2.5 Character Group

Character group is a group of sub-patterns that is written inside Parentheses `(...)`. As we discussed before that in regular expression
if we put a quantifier after a character then it will repeat the preceding character. But if we put quantifier after a character group then
it repeats the whole character group. For example, the regular expression `(ab)*` matches zero or more repetitions of the character "ab".
We can also use the alternation `|` meta character inside character group. For example, the regular expression `(c|g|p)ar` means: lowercase character `c`,
`g` or `p`, followed by character `a`, followed by character `r`.

<pre>
"(c|g|p)ar" => The <a href="#learn-regex"><strong>car</strong></a> is <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[Test the regular expression](https://regex101.com/r/tUxrBG/1)

## 2.6 Alternation

In regular expression Vertical bar `|` is used to define alternation. Alternation is like a condition between multiple expressions. Now,
you may be thinking that character set and alternation works the same way. But the big difference between character set and alternation
is that character set works on character level but alternation works on expression level. For example, the regular expression
`(T|t)he|car` means: uppercase character `T` or lowercase `t`, followed by lowercase character `h`, followed by lowercase character `e`
or lowercase character `c`, followed by lowercase character `a`, followed by lowercase character `r`.

<pre>
"(T|t)he|car" => <a href="#learn-regex"><strong>The</strong></a> <a href="#learn-regex"><strong>car</strong></a> is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[Test the regular expression](https://regex101.com/r/fBXyX0/1)

## 2.7 Escaping special character

Backslash `\` is used in regular expression to escape the next character. This allows to to specify a symbol as a matching character
including reserved characters `{ } [ ] / \ + * . $ ^ | ?`. To use a special character as a matching character prepend `\` before it.
For example, the regular expression `.` is used to match any character except newline. Now to match `.` in an input string the regular
expression `(f|c|m)at\.?` means: lowercase letter `f`, `c` or `m`, followed by lowercase character `a`, followed by lowercase letter
`t`, followed by optional `.` character.

<pre>
"(f|c|m)at\.?" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> sat on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/DOc5Nu/1)

## 2.8 Anchors

In regular expressions, we use anchors to check if the matching symbol is the starting symbol or ending symbol of the
input string. Anchors are of two types: First type is Caret `^` that check if the matching character is the start
character of the input and the second type is Dollar `$` that checks if matching character is the last character of the
input string.

### 2.8.1 Caret

Caret `^` symbol is used to check if matching character is the first character of the input string. If we apply the following regular
expression `^a` (if a is the starting symbol) to input string `abc` it matches `a`. But if we apply regular expression `^b` on above
input string it does not match anything. Because in input string `abc` "b" is not the starting symbol. Let's take a look at another
regular expression `^(T|t)he` which means: uppercase character `T` or lowercase character `t` is the start symbol of the input string,
followed by lowercase character `h`, followed by lowercase character `e`.

<pre>
"(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[Test the regular expression](https://regex101.com/r/5ljjgB/1)

<pre>
"^(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

[Test the regular expression](https://regex101.com/r/jXrKne/1)

### 2.8.2 Dollar

Dollar `$` symbol is used to check if matching character is the last character of the input string. For example, regular expression
`(at\.)$` means: a lowercase character `a`, followed by lowercase character `t`, followed by a `.` character and the matcher
must be end of the string.

<pre>
"(at\.)" => The fat c<a href="#learn-regex"><strong>at.</strong></a> s<a href="#learn-regex"><strong>at.</strong></a> on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/y4Au4D/1)

<pre>
"(at\.)$" => The fat cat. sat. on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/t0AkOd/1)

##  3. Shorthand Character Sets

Regular expression provides shorthands for the commonly used character sets, which offer convenient shorthands for commonly used
regular expressions. The shorthand character sets are as follows:

|Shorthand|Description|
|:----:|----|
|.|Any character except new line|
|\w|Matches alphanumeric characters: `[a-zA-Z0-9_]`|
|\W|Matches non-alphanumeric characters: `[^\w]`|
|\d|Matches digit: `[0-9]`|
|\D|Matches non-digit: `[^\d]`|
|\s|Matches whitespace character: `[\t\n\f\r\p{Z}]`|
|\S|Matches non-whitespace character: `[^\s]`|

## 4. Lookaround

Lookbehind and lookahead sometimes known as lookaround are specific type of ***non-capturing group*** (Use to match the pattern but not
included in matching list). Lookaheads are used when we have the condition that this pattern is preceded or followed by another certain
pattern. For example, we want to get all numbers that are preceded by `$` character from the following input string `$4.44 and $10.88`.
We will use following regular expression `(?<=\$)[0-9\.]*` which means: get all the numbers which contain `.` character and  are preceded
by `$` character. Following are the lookarounds that are used in regular expressions:

|Symbol|Description|
|:----:|----|
|?=|Positive Lookahead|
|?!|Negative Lookahead|
|?<=|Positive Lookbehind|
|?<!|Negative Lookbehind|

### 4.1 Positive Lookahead

The positive lookahead asserts that the first part of the expression must be followed by the lookahead expression. The returned match
only contains the text that is matched by the first part of the expression. To define a positive lookahead, parentheses are used. Within
those parentheses, a question mark with equal sign is used like this: `(?=...)`. Lookahead expression is written after the equal sign inside
parentheses. For example, the regular expression `[T|t]he(?=\sfat)` means: optionally match lowercase letter `t` or uppercase letter `T`,
followed by letter `h`, followed by letter `e`. In parentheses we define positive lookahead which tells regular expression engine to match
`The` or `the` which are followed by the word `fat`.

<pre>
"[T|t]he(?=\sfat)" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

[Test the regular expression](https://regex101.com/r/IDDARt/1)

### 4.2 Negative Lookahead

Negative lookahead is used when we need to get all matches from input string that are not followed by a pattern. Negative lookahead
defined same as we define positive lookahead but the only difference is instead of equal `=` character we use negation `!` character
i.e. `(?!...)`. Let's take a look at the following regular expression `[T|t]he(?!\sfat)` which means: get all `The` or `the` words from
input string that are not followed by the word `fat` precedes by a space character.

<pre>
"[T|t]he(?!\sfat)" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

[Test the regular expression](https://regex101.com/r/V32Npg/1)

### 4.3 Positive Lookbehind

Positive lookbehind is used to get all the matches that are preceded by a specific pattern. Positive lookbehind is denoted by
`(?<=...)`. For example, the regular expression `(?<=[T|t]he\s)(fat|mat)` means: get all `fat` or `mat` words from input string that
are after the word `The` or `the`.

<pre>
"(?<=[T|t]he\s)(fat|mat)" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

[Test the regular expression](https://regex101.com/r/avH165/1)

### 4.4 Negative Lookbehind

Negative lookbehind is used to get all the matches that are not preceded by a specific pattern. Negative lookbehind is denoted by
`(?<!...)`. For example, the regular expression `(?<!(T|t)he\s)(cat)` means: get all `cat` words from input string that
are not after the word `The` or `the`.

<pre>
"(?&lt;![T|t]he\s)(cat)" => The cat sat on <a href="#learn-regex"><strong>cat</strong></a>.
</pre>

[Test the regular expression](https://regex101.com/r/8Efx5G/1)

## 5. Flags

Flags are also called modifiers because they modify the output of a regular expression. These flags can be used in any order or
combination, and are an integral part of the RegExp.

|Flag|Description|
|:----:|----|
|i|Case insensitive: Sets matching to be case-insensitive.|
|g|Global Search: Search for a pattern throughout the input string.|
|m|Multiline: Anchor meta character works on each line.|

### 5.1 Case Insensitive

The `i` modifier is used to perform case-insensitive matching. For example, the regular expression `/The/gi` means: uppercase letter
`T`, followed by lowercase character `h`, followed by character `e`. And at the end of regular expression the `i` flag tells the
regular expression engine to ignore the case. As you can see we also provided `g` flag because we want to search for the pattern in
the whole input string.

<pre>
"The" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

[Test the regular expression](https://regex101.com/r/dpQyf9/1)

<pre>
"/The/gi" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

[Test the regular expression](https://regex101.com/r/ahfiuh/1)

### 5.2 Global search

The `g` modifier is used to perform a global match (find all matches rather than stopping after the first match). For example, the
regular expression`/.(at)/g` means: any character except new line, followed by lowercase character `a`, followed by lowercase
character `t`. Because we provided `g` flag at the end of the regular expression now it will find every matches from whole input
string.

<pre>
"/.(at)/" => The <a href="#learn-regex"><strong>fat</strong></a> cat sat on the mat.
</pre>

[Test the regular expression](https://regex101.com/r/jnk6gM/1)

<pre>
"/.(at)/g" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> <a href="#learn-regex"><strong>sat</strong></a> on the <a href="#learn-regex"><strong>mat</strong></a>.
</pre>

[Test the regular expression](https://regex101.com/r/dO1nef/1)

### 5.3 Multiline

The `m` modifier is used to perform a multi-line match. As we discussed earlier anchors `(^, $)` are used to check if pattern is
the beginning of the input or end of the input string. But if we want that anchors works on each line we use `m` flag. For example, the
regular expression `/at(.)?$/gm` means: lowercase character `a`, followed by lowercase character `t`, optionally anything except new
line. And because of `m` flag now regular expression engine matches pattern at the end of each line in a string.

<pre>
"/.at(.)?$/" => The fat
                cat sat
                on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/hoGMkP/1)

<pre>
"/.at(.)?$/gm" => The <a href="#learn-regex"><strong>fat</strong></a>
                  cat <a href="#learn-regex"><strong>sat</strong></a>
                  on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

[Test the regular expression](https://regex101.com/r/E88WE2/1)

## Contribution

* Report issues
* Open pull request with improvements
* Spread the word
* Reach out to me directly at ziishaned@gmail.com or [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/ziishaned.svg?style=social&label=Follow%20%40ziishaned)](https://twitter.com/ziishaned)

## License

MIT © [Zeeshan Ahmed](mailto:ziishaned@gmail.com)
