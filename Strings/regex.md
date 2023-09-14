# Regular Expressions

**Priority: 1/5**

## Introduction

​	RegEx is a component of string parsing that uses a series of characters to match patterns in a body of text. In order to effectively use RegEx in C++, you have to learn both the functionality of the library functions and the language of RegEx itself. Although it's rare to find RegEx in LeetCode-style problems, it can be a very useful tool for your day-to-day and is a concept that can be very hard to master on the fly.

## Components

- `.`: Matches any character
- `X?`: Matches 0 or 1 of `X`
- `X*`: Matches 0+ `X`
- `X+`: Matches 1+ `X`
- `[]`: Groups characters together or creates a range using `-`
  - `[abc]` matches either `a`, `b`, or `c`
  - `[0-9]` matches all digits
- `()`: Defines a sequence
  - `(abc)` matches the substring `"abc"` 
- `{}`: Defines a numerical quantifier range
  - `[0-9]{2,3}` matches any sequence of 2 or 3 digits
- `^`: Asserts that the match must occur at the beginning of the line
- `$`: Asserts that the match must occur at the end of the line
- `\b`: Matches whitespace
- `|`: Logical OR
- `\`: Used to escape any of the preceding characters so that they can be matched
  - For some reason, in C++, you have to escape the escape character (i.e., to match `"."`, the regular expression must be `\\.`)
- All other characters only match themselves

## Tools

- `regex`

  - The class used to define a regular expression

  - A `regex` object can be constructed from a string as follows:

    - ```cpp
      regex r("[a-z]");
      ```

- `smatch`

  - The class used to handle match results for strings
  - `prefix()`: A member function that returns the substring preceding the matched sequence
  - `suffix()`: A member function that returns the substring following the matched sequence
  - `length()`: A member function that returns the length of the matched sequence
  - `position()`: A member function that returns the position of the matched sequence in the string
  - `str()`: A member function that returns the matched sequence as a string

- `bool regex_match(string s, regex expr)`

  - A function that returns `true` if the string `s` matches the regular expression `expr`, or `false` otherwise

  - Ex) Determine whether or not a string begins with an alphanumeric character

    - ```cpp
      bool beginsWithAlnum(string s) {
          regex r("^[a-zA-z0-9]");
          return regex_match(s, r);
      }
      ```

- `string regex_replace(string s, regex expr, string fmt)`

  - A function that returns a string created by replacing all matches of `expr` in `s` with the format specified by `fmt`

  - `fmt` can either be a typical string, or a string that contains one or more of the following format specifiers:

    - `$n`: A copy of the `n`th group (as defined by `()`) matched in `expr`
    - `$&`: A copy of the entire matched substring
    - `$$`: The character `$`

  - Ex) Place parentheses around each digit in a string

    - ```cpp
      string formatDigits(string s) {
      	regex r("[0-9]");
      	return regex_replace(s, r, "($&)")
      }
      ```

- `bool regex_search(string s, smatch m, regex expr)`

  - A function that returns if the string `s` matches the regular expression `expr`, and populates `m` with the first instance of the matched sequence

  - `m` is iterable, with each group in `expr` (as defined by `()`) being its own element

  - Ex) Return the number of matches for a given regular expression in a string

    - ```cpp
      bool countMatches(string s, string e) {
      	smatch m;
      	regex r(e);
      	int count = 0;
      	
      	// Find the next match
      	while (regex_search(s, m, r)) {
      		count++;
      		s = m.suffix().str();
      	}
      	
      	return count;
      }
      ```

## Example Problems

[1108. Defanging an IP Address](https://leetcode.com/problems/defanging-an-ip-address/description/)

<details>
    <summary>Solution</summary>

```cpp
string defangIPaddr(string address) {
    regex r("\\.");
    return regex_replace(address, r, "[$&]");
}
```

</details>

