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

> Regular expression is a group of characters or symbols which is used to find a specific pattern from a text.

A regular expression is a pattern that is matched against a subject string from left to right. The word "Regular expression" is a
mouthful, you will usually find the term abbreviated as "regex" or "regexp". Regular expression is used for replacing a text within
a string, validating form, extract a substring from a string based upon a pattern match, and so much more.

Imagine you are writing an application and you want to set the rules for when a user chooses their username. We want to
allow the username to contain letters, numbers, underscores and hyphens. We also want to limit the number of
characters in username so it does not look ugly. We use the following regular expression to validate a username:
<br/><br/>
<p align="center">
<img src="https://i.imgur.com/ekFpQUg.png" alt="Regular expression">
</p>

Above regular expression can accept the strings `john_doe`, `jo-hn_doe` and `john12_as`. It does not match `Jo` because that string
contains uppercase letter and also it is too short.

## Table of Contents

- [Basic Matchers](#1-basic-matchers)
- [메타 문자열](#2-메타-문자열)
  - [마침표](#21-full-stop)
  - [문자 집합](#22-character-set)
    - [부정 문자 집합](#221-negated-character-set)
  - [반복](#23-repetitions)
    - [애스터리스크](#231-the-star)
    - [플러스](#232-the-plus)
    - [물음표](#233-the-question-mark)
  - [중괄호](#24-braces)
  - [문자 그룹](#25-character-group)
  - [대안](#26-alternation)
  - [이스케이프된 특수 문자](#27-escaping-special-character)
  - [앵커](#28-anchors)
    - [캐럿](#281-caret)
    - [달러](#282-dollar)
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

## 1. Basic Matchers

A regular expression is just a pattern of characters that we use to perform search in a text.  For example, the regular expression
`the` means: the letter `t`, followed by the letter `h`, followed by the letter `e`.

<pre>
"the" => The fat cat sat on <a href="#learn-regex"><strong>the</strong></a> mat.
</pre>

[Test the regular expression](https://regex101.com/r/dmRygT/1)

The regular expression `123` matches the string `123`. The regular expression is matched against an input string by comparing each
character in the regular expression to each character in the input string, one after another. Regular expressions are normally
case-sensitive so the regular expression `The` would not match the string `the`.

<pre>
"The" => <a href="#learn-regex"><strong>The</strong></a> fat cat sat on the mat.
</pre>

[Test the regular expression](https://regex101.com/r/1paXsy/1)

## 2. 메타 문자열

메타 문자열은 정규식의 구성요소이며, 그대로를 의미하지 않고 특별한 의미으로 해석됩니다. 몇몇 메타 문자들은 특별한 의미를 가지고 있고 대괄호([]) 안에서 사용됩니다. 메타 문자들은
다음과 같습니다:

메타 문자 | 설명
:---: | ------------------------------------------------------------------------------------------------------
  .   | 마침표는 줄 바꿈을 제외한 모든 단일 문자와 일치합니다.
 [ ]  | 문자 집합(Character class). 대괄호 사이에 포함 된 모든 문자와 일치합니다.
[^ ]  | 부정 문자 집합. 대괄호 사이에 포함되지 않은 문자와 일치합니다.
  \*  | 앞에서 나온 문자가 없거나 하나 이상 연속하는 문자와 일치합니다.
  \+  | 앞에서 나온 문자가 하나 이상 연속하는 문자와 일치합니다.
  ?   | 앞에서 나온 문자가 없거나 하나만 있을 경우 일치합니다.
{n,m} | 중괄호. 앞에서 나온 문자가 "n"개 이상 "m"개 이하일 경우 일치합니다.
(xyz) | 문자 그룹(Character group). 정확한 순서로 문자열 xyz와 일치합니다.
  \|  | Alternation. 기호(`|`) 앞에 있는 문자 또는 뒤에 있는 문자와 일치합니다.
  \\  | 다음 문자를 이스케이프 처리합니다. 즉, 예약문자를 매칭 `[ ] ( ) { } . * + ? ^ $ \ |`
  ^   | 입력의 시작지점과 일치합니다.
  $   | 입력의 끝지점과 일치합니다.

## 2.1 마침표

마침표 `.`은 메타 문자의 가장 간단한 예입니다. 메타 문자 `.`는 리턴 또는 개행 문자를 제외한 하나의 문자와 일치합니다. 예를 들어, 정규식 `.ar`은 임의의 문자와 그 뒤에
`a` 문자, `r` 문자가 오는 것을 의미합니다.

<pre>
".ar" => The <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[정규식 테스트](https://regex101.com/r/xc9GkU/1)

## 2.2 문자 집합

문자 집합(Character set)은 문자 클래스(character class)로도 불립니다. 대괄호는 문자 집합을 지정하는 데 사용됩니다. 문자 집합 내에서 하이픈을 사용하여 문자의 범위를
지정합니다. 대괄호 안에있는 문자 범위의 순서는 중요하지 않습니다. 예를 들어, 정규식 `[Tt]he`는 대문자`T` 또는 소문자`t` 다음에 문자`h`와 문자 `e`가 뒤 따르는 것을 의미합니다.

<pre>
"[Tt]he" => <a href="#learn-regex"><strong>The</strong></a> car parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[정규식 테스트](https://regex101.com/r/2ITLQ4/1)

그러나 문자 집합 안의 마침표는 문자 그대로의 마침표를 의미합니다. 정규식 `ar[.]`은 소문자`a`와 문자`r` 다음에 마침표`.` 문자가 오는 것을 의미합니다.

<pre>
"ar[.]" => A garage is a good place to park a c<a href="#learn-regex"><strong>ar.</strong></a>
</pre>

[정규식 테스트](https://regex101.com/r/wL3xtE/1)

### 2.2.1 부정 문자 집합

일반적으로 캐럿 기호(`^`)는 문자열의 시작을 나타내지만 대괄호 시작에 입력하면 문자 세트를 무시합니다. 예를 들어, 정규 표현식 `[^c] ar`은 `c`를 제외한 임의의 문자 다음에
`a` 문자가 오고 그 뒤에`r` 문자가 오는 것을 의미합니다.

<pre>
"[^c]ar" => The car <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[정규식 테스트](https://regex101.com/r/nNNlq3/1)

## 2.3 반복

메타 문자 `+`, `*` 또는 `?`는 얼마나 많은 하위패턴이 발생할 수 있는지를 지정하는 데 사용됩니다. 이러한 메타 문자는 상황에 따라 다르게 작동합니다.

### 2.3.1 애스터리스크

`*`는 선행 문자가 0번 이상 반복되는 것을 매치합니다. 즉, 정규식 `a*`는 선행 소문자 `a`가 0 번 이상 반복되는 것을 의미합니다. 그러나 문자 집합이나 클래스 뒤에 나타나는
경우 전체 문자 집합의 반복을 찾습니다. 예를 들어, 정규식 `[a-z]*`는 행(row)의 소문자 수를 의미합니다.

<pre>
"[a-z]*" => T<a href="#learn-regex"><strong>he</strong></a> <a href="#learn-regex"><strong>car</strong></a> <a href="#learn-regex"><strong>parked</strong></a> <a href="#learn-regex"><strong>in</strong></a> <a href="#learn-regex"><strong>the</strong></a> <a href="#learn-regex"><strong>garage</strong></a> #21.
</pre>

[정규식 테스트](https://regex101.com/r/7m8me5/1)

`*`는 메타 문자 `.`와 함께 사용되어 `.*` 문자 열과 일치시킬 수 있습니다. 또는 공백 문자 `\s`와 함께 사용되어 공백 문자의 문자열과 일치할 수 있습니다. 예를 들어,
`\s*cat\s*`표현은 0개 이상의 공백, 다음에 소문자 `c`, 소문자 `a`, 소문자 `t`, 0개 이상 공백과 매칭됩니다.

<pre>
"\s*cat\s*" => The fat<a href="#learn-regex"><strong> cat </strong></a>sat on the <a href="#learn-regex">con<strong>cat</strong>enation</a>.
</pre>

[정규식 테스트](https://regex101.com/r/gGrwuz/1)

### 2.3.2 플러스

`+`는 선행 문자가 하나 이상의 반복을 나타냅니다. 예를 들어, 정규 표현식 `c.+t`는 소문자 `c`와 그 뒤에 적어도 하나의 문자, 그리고 소문자`t`가 뒤 따르는 것을 의미합니다.
이것은 분명하게 `t`가 문장의 마지막에 있어야합니다.

<pre>
"c.+t" => The fat <a href="#learn-regex"><strong>cat sat on the mat</strong></a>.
</pre>

[정규식 테스트](https://regex101.com/r/Dzf9Aa/1)

### 2.3.3 물음표

정규식에서 메타 문자 `?`는 선행 문자를 선택적으로 만듭니다. 이 기호는 선행 문자가 없어나 혹은 한 개일 경우와 일치합니다. 예를 들어, 정규식 `[T]?he`는 옵션으로 대문자 `T`가
있거나, 그리고 소문자 `h`, 소문자 `e`가 차례로 오는 것을 의미합니다.

<pre>
"[T]he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

[정규식 테스트](https://regex101.com/r/cIg9zm/1)

<pre>
"[T]?he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in t<a href="#learn-regex"><strong>he</strong></a> garage.
</pre>

[정규식 테스트](https://regex101.com/r/kPpO2x/1)

## 2.4 중괄호

정규식에서 한정 기호(quantifiers)라고 불리는 중괄호는 문자 또는 문자 그룹을 반복 할 수있는 횟수를 지정하는 데 사용됩니다. 예를 들어, 정규식 `[0-9]{2,3}`은 다음을 의미합니다.
2자리 이상 3자리 이하의 문자(0에서 9 사이의 문자. 즉, 숫자).

<pre>
"[0-9]{2,3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

[정규식 테스트](https://regex101.com/r/juM86s/1)

우리는 두 번째 숫자를 생략 할 수 있습니다. 예를 들어, 정규식`[0-9]{2,}`는 2자리 이상의 숫자와 일치함을 의미합니다. 쉼표도 제거하면 정규식 `[0-9]{3}`은 정확히 3자리 숫자와
일치함을 의미합니다.

<pre>
"[0-9]{2,}" => The number was 9.<a href="#learn-regex"><strong>9997</strong></a> but we rounded it off to <a href="#learn-regex"><strong>10</strong></a>.0.
</pre>

[정규식 테스트](https://regex101.com/r/Gdy4w5/1)

<pre>
"[0-9]{3}" => The number was 9.<a href="#learn-regex"><strong>999</strong></a>7 but we rounded it off to 10.0.
</pre>

[정규식 테스트](https://regex101.com/r/Sivu30/1)

## 2.5 문자 그룹

문자 그룹은 괄호 `(...)` 사이에 작성된 하위 패턴의 그룹입니다. 앞서 정규식에서 설명한 것처럼 문자 뒤에 반복자를 넣으면 한정 기호를 반복할 것입니다. 그러나 문자 그룹 다음에 반복자를
입력하면 문자 그룹 전체를 반복할 것입니다. 예를 들어, 정규식 `(ab)*`는 문자 "ab"가 0개 이상의 반복과 일치합니다. 또한 문자 그룹안에 대응 문자 `|`를 사용할 수 있습니다. 예를 들어,
정규식 `(c|g|p)ar`은 소문자 `c`, `g`, `p` 중 하나, 다음에 `a`, 다음 `r`을 의미합니다.

<pre>
"(c|g|p)ar" => The <a href="#learn-regex"><strong>car</strong></a> is <a href="#learn-regex"><strong>par</strong></a>ked in the <a href="#learn-regex"><strong>gar</strong></a>age.
</pre>

[정규식 테스트](https://regex101.com/r/tUxrBG/1)

## 2.6 대안

정규식에서 수직 바 `|`는 대안을 정의하는데 사용됩니다. 대안은 여러 언어에서 조건과 비슷합니다. 문자 집합(character set)과 대안(alternation)이 같은 방식으로 동작한다고
생각할 수도 있습니다. 그러나 문자집합과 대안 사이에 큰 다른점이 있습니다. 문자 집합은 문자 수준(character level)으로 동작하지만 대안은 표현 수준(expression level)에서
동작합니다. 예를 들어, 정규식 `(T|t)he|car`는 대문자 `T` 또는 소문자 `t` 다음에 소문자 `h`, 소문자 `e` 또는 소문자 `c`, 소문자 `a`, 소문자 `r` 와 일치함을 의미합니다.

<pre>
"(T|t)he|car" => <a href="#learn-regex"><strong>The</strong></a> <a href="#learn-regex"><strong>car</strong></a> is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[정규식 테스트](https://regex101.com/r/fBXyX0/1)

## 2.7 이스케이프된 특수 문자

`\\`는 정규식에서 다음 문자를 이스케이프 처리하는데 사용됩니다. 이것으로 예약 문자 `{ } [ ] / \ + * . $ ^ | ?`를 일치하는 문자 기호로 지정할 수 있습니다. 그러므로
예약 문자를 매칭 문자로 사용하려면 앞에 `\\`를 사용하십시오. 예를 들어, 정규식 `.`은 개행 문자를 제외한 모든 문자를 매치하는 데 사용된다. 입력 문자열에서 `.`와 일치
시키기위한 정규식 `(f|c|m)at\.?`는 소문자 `f`, `c`, `m` 중 하나, 다음 소문자 `a`, 그 다음 소문자 `t` 그리고 선택적으로 `.`(특수 문자가 아닌 일반 문자)와 일치합니다.

<pre>
"(f|c|m)at\.?" => The <a href="#learn-regex"><strong>fat</strong></a> <a href="#learn-regex"><strong>cat</strong></a> sat on the <a href="#learn-regex"><strong>mat.</strong></a>
</pre>

[정규식 테스트](https://regex101.com/r/DOc5Nu/1)

## 2.8 앵커

정규식에서 앵커를 사용하여 일치하는 기호가 입력 문자열의 시작 또는 끝 기호인지 확인합니다. 앵커는 두 가지 유형이 있습니다: 첫 번째 유형 캐럿(`^`)은 입력의 시작 문자인지
확인하고, 두 번째 달러(`$`)는 마지막 문자인지 확인합니다.

### 2.8.1 캐럿

`^`은 매칭 문자가 입력문자열의 첫번째 문자인지 확인하는데 사용됩니다. 정규식 `^a`(a가 시작일 경우)는 입력 문자열 `abc`에 적용하면 `a`와 일치합니다. 그러나 `abc`를 정규식
`^b`에 적용하면 어느 것이도 매칭되지 않습니다. 왜냐하면 입력문자열 `abc`에서 `b`는 시작 기호가 아니기 때문입니다. 다음 정규식 `^(T|t)he`는 대문자 `T` 또는 소문자 `t`는
입력 문자열의 시작 기호이고 다음에 소문자 `h`, 다음에 소문자 `e`와 일치하는 것을 뜻합니다.

<pre>
"(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in <a href="#learn-regex"><strong>the</strong></a> garage.
</pre>

[정규식 테스트](https://regex101.com/r/5ljjgB/1)

<pre>
"^(T|t)he" => <a href="#learn-regex"><strong>The</strong></a> car is parked in the garage.
</pre>

[정규식 테스트](https://regex101.com/r/jXrKne/1)

### 2.8.2 달러

`$` 기호는 일치하는 문자가 입력 문자열의 마지막 문자인지 확인하는 데 사용됩니다. 예를 들어, 정규식 `(at\.)$`은 소문자 `a`, 소문자 `t`, 뒤에 `.` 문자가 오고, 반드시
문자열의 끝이어야 합니다.

<pre>
"(at\.)" => The fat c<a href="#learn-regex"><strong>at.</strong></a> s<a href="#learn-regex"><strong>at.</strong></a> on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

[정규식 테스트](https://regex101.com/r/y4Au4D/1)

<pre>
"(at\.)$" => The fat cat. sat. on the m<a href="#learn-regex"><strong>at.</strong></a>
</pre>

[정규식 테스트](https://regex101.com/r/t0AkOd/1)

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
