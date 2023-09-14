# String Parsing

**Priority: 2/5**

## Introduction

​	String parsing is the process of breaking down a string into meaningful chunks, where "meaningful" is defined by the problem statement. Common problems related to string parsing may involve extracting words or patterns from a string, and then doing some other processing on them. Although string parsing doesn't have too large of a presence in LeetCode-style questions, it is extremely important for real-world problems, and thus, is a valuable tool to have. Please note that RegEx is handled separately.

​	With that said, one of the best arguments to *not* use C++ for interviews is that its standard library is severely lacking in string parsing functionality when compared to the likes of Python/JavaScript/etc. This makes solving string parsing problems in C++ much more time consuming, more verbose, less reader-friendly, and more error-prone.

## Tools

- `stringstream`

  - `stringstream` is a class defined in the `<sstream>` header

  - `stringstream` allows for the conversion of string data into stream form, allowing for much more flexibility in string manipulation

  - The more specific classes of `istringsteam` and `ostringstream` exist to handle input and output data, respectively, and can be used in place of `stringstream` to make your solution more readable/less error-prone, but this is overkill in most cases

  - A `stringstream` can be constructed from a string as follows:

    - ```cpp
      string str = "Hello, World!";
      stringstream ss(str);
      ```

  - `stringstream`s can be read from using the extraction operator `>>`, which will extract data from the stream until the next whitespace, and will return a falsy value when the end of the stream is reached:

    - ```cpp
      // Prints every word from the stream ss
      string word;
      while (ss >> word) {
          cout << word << endl;
      }
      ```

- `istream& getline(istream& is, string& str, char delim)`

  - `getline` is a function defined in the `<string>` header
  - Takes data from the stream `is` until the end of steam or until `delim` and places it into the string `str`
  - Very useful for parsing through a `stringstream` when non-standard delimiters are needed

## Approaches

- Brute force

  - Unlike most cases, brute forcing string parsing typically can still lead to an optimal solution from a time/space complexity perspective

  - The base logic of this approach is incredibly straightforward

  - The biggest issues with brute force approaches are readability (due to verbose code), vulnerability to bugs (off-by-one errors, out-of-bounds accesses, extensive edge-cases, etc.), and a lack of flexibility with changing requirements

  - Ex) Extracting words from a string

    - ```cpp
      // Assume words are separated by ' '
      vector<string> extractWordsFromString(string& str) {
          vector<string> words;
          
          // Iterate through string word-by-word
          int i = 0;
          while (i < str.size()) {
              
              // Collect the next word
              string word;
              while (str[i] != ' ' && i < str.size()) {
                  word += str[i];
              }
              
              // Add the word to the list
              words.push_back(word);
              
              // Iterate past the space
              i++;
          }
          
          return words;
      }
      
      /*
      	Time: O(N)
      	Space: O(M), where M is the length of the longest word
      */
      ```

- `stringstream`/`getline`

  - A useful alternative to brute force **if you have time to learn it**

  - With proper use, should help to make solutions cleaner and *much* less error-prone

    - This becomes even more true as the problem statement gets more and more complex

  - However, using `stringstream` introduces memory requirements linear in the size of the original string, which means a brute force approach is likely to be more space efficient if the problem is simple

  - Ex) Extracting words from a string

    - ```cpp
      // Assume words are separated by ' ' => no longer a necessary assumption
      vector<string> extractWordsFromString(string& str) {
          vector<string> words;
          
          // Construct stringstream from input
          stringstream ss(str);
          string word;
          
          // Extract words until none are left
          while (ss >> word) {
              words.push_back(word);
          }
          
          return words;
      }
      
      /*
      	Time: O(N)
      	Space: O(N)
      */
      ```

## Example Problems

**[557. Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii/description/)**

<details>
  <summary>Solution</summary>

  ```cpp
  string reverseWords(string s) {
      string reversed;
  
      // Construct stringstream from input
      stringstream ss(s);
      string word;
  
      // Parse word-by-word until end-of-stream
      while (ss >> word) {
          // Handle whitespace
          if (!reversed.empty()) {
              reversed += " ";
          }
  
          // Reverse word and add to result
          reverse(word.begin(), word.end());
          reversed += word;
      }
  
      return reversed;
  }
  
  /*
  	Time: O(N)
  	Space: O(N)
  */
  ```
</details>



