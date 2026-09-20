# Character Indexing in C++ — DSA

Character indexing means converting a character into a useful integer value so that we can use it as an **array/vector index** or perform calculations on it.

This is one of the most common small tricks in string problems.

---

## 1. Character → 0-Based Alphabet Index

For lowercase English letters:

```cpp
int index = ch - 'a';
```

Mapping:

```text
a → 0
b → 1
c → 2
d → 3
...
z → 25
```

### Why does it work?

Characters have numeric character codes, and alphabet characters are consecutive.

```cpp
'b' - 'a' = 1
'c' - 'a' = 2
'd' - 'a' = 3
```

Think:

```text
'a' = starting/base character

ch - 'a'
    ↓
distance from 'a'
    ↓
0 ... 25
```

### Example

```cpp
char ch = 'd';

int index = ch - 'a';

cout << index;  // 3
```

### Complexity

```text
Time  : O(1)
Space : O(1)
```

---

# 2. Character → 1-Based Alphabet Position

If we want the normal alphabet position:

```text
a → 1
b → 2
c → 3
...
z → 26
```

use:

```cpp
int position = ch - 'a' + 1;
```

Example:

```cpp
char ch = 'd';

int position = ch - 'a' + 1;

cout << position;  // 4
```

### Remember

```text
ch - 'a'       → 0-based
ch - 'a' + 1   → 1-based
```

---

# 3. Reverse Alphabet Index

Sometimes a problem uses:

```text
a → 26
b → 25
c → 24
...
z → 1
```

Use:

```cpp
int value = 'z' - ch + 1;
```

Example:

```cpp
char ch = 'd';

int value = 'z' - ch + 1;

cout << value;  // 23
```

### Reverse 0-Based Index

If:

```text
z → 0
y → 1
x → 2
...
a → 25
```

use:

```cpp
int index = 'z' - ch;
```

---

# 4. Uppercase Characters

For uppercase letters:

```cpp
int index = ch - 'A';
```

Mapping:

```text
A → 0
B → 1
C → 2
...
Z → 25
```

For 1-based position:

```cpp
int position = ch - 'A' + 1;
```

---

# 5. Digit Character → Integer

This is another very important DSA trick.

A character digit and an integer digit are different:

```text
'7' ≠ 7
```

Convert:

```cpp
int digit = ch - '0';
```

Example:

```cpp
char ch = '7';

int digit = ch - '0';

cout << digit;  // 7
```

### Mapping

```text
'0' → 0
'1' → 1
'2' → 2
...
'9' → 9
```

### Remember

```text
ch - '0' → character digit to integer digit
```

### Complexity

```text
Time  : O(1)
Space : O(1)
```

---

# 6. Integer Digit → Character

The reverse operation:

```cpp
char ch = digit + '0';
```

Example:

```cpp
int digit = 7;

char ch = digit + '0';

cout << ch;  // '7'
```

So:

```text
'7' - '0' → 7
7 + '0'   → '7'
```

---

# 7. Character → Character Mapping

Sometimes we need to transform one alphabet character into another.

Example:

```text
a → z
b → y
c → x
d → w
...
z → a
```

Formula:

```cpp
char mapped = 'z' - (ch - 'a');
```

Example:

```cpp
char ch = 'c';

char mapped = 'z' - (ch - 'a');

cout << mapped;  // x
```

### Think in two steps

```text
ch
 ↓
ch - 'a'
 ↓
0-based index
 ↓
reverse index from 'z'
```

---

# 8. Useful Character Checks

C++ provides functions in:

```cpp
#include <cctype>
```

Common functions:

```cpp
isdigit(ch)   // digit?
isalpha(ch)   // alphabet?
islower(ch)   // lowercase?
isupper(ch)   // uppercase?
tolower(ch)   // convert to lowercase
toupper(ch)   // convert to uppercase
```

Example:

```cpp
if(isdigit(ch)) {
    // ch is a digit
}
```

These are useful when a problem contains **mixed characters**.

---

# 9. ASCII Ranges You Should Know

You don't need to memorize every ASCII value.

Just remember the important ranges:

```text
'0' - '9'   → digits
'A' - 'Z'   → uppercase
'a' - 'z'   → lowercase
```

The characters inside each range are consecutive.

That's why these work:

```cpp
ch - 'a'
ch - 'A'
ch - '0'
```

---

# 10. Character as an Array Index

This is where character indexing becomes really useful in DSA.

Suppose the problem says:

> String contains only lowercase English letters.

Instead of:

```cpp
unordered_map<char, int> freq;
```

we can use:

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}
```

Now:

```text
freq[0]  → a
freq[1]  → b
freq[2]  → c
...
freq[25] → z
```

### The mental conversion

```text
character
    ↓
ch - 'a'
    ↓
0 ... 25
    ↓
array index
```

---

# 11. `vector` vs `unordered_map`

### Known small character set

If the problem guarantees:

```text
'a' to 'z'
```

use:

```cpp
vector<int> freq(26);
```

or:

```cpp
array<int, 26> freq{};
```

### Unknown / dynamic keys

Use:

```cpp
unordered_map<char, int> freq;
```

### Decision Rule

```text
Fixed + small range
        ↓
array / vector

Unknown / large / dynamic range
        ↓
unordered_map
```

### Complexity

For a string of length `n`:

```cpp
for(char ch : s)
    freq[ch - 'a']++;
```

```text
Time  : O(n)
Space : O(1)
```

Why O(1) space?

Because the frequency array always has only **26 positions**.

---

# 12. `vector<bool>` for Presence

If we only need to know:

> "Have I seen this character?"

We don't need an integer frequency.

```cpp
vector<bool> seen(26, false);

for(char ch : s) {
    seen[ch - 'a'] = true;
}
```

Then:

```cpp
if(seen[ch - 'a']) {
    // character was already seen
}
```

Use:

```text
frequency needed → vector<int>
only presence    → vector<bool>
```

---

# 13. First / Last Occurrence

Character indexing can also be used to store positions.

### First occurrence

```cpp
vector<int> first(26, -1);

for(int i = 0; i < s.size(); i++) {

    int idx = s[i] - 'a';

    if(first[idx] == -1) {
        first[idx] = i;
    }
}
```

### Last occurrence

```cpp
vector<int> last(26, -1);

for(int i = 0; i < s.size(); i++) {
    last[s[i] - 'a'] = i;
}
```

This pattern is useful in:

* String problems
* Sliding window
* Substring problems
* Greedy problems

---

# 14. Common Mistakes

### Mistake 1: `'7'` vs `7`

```cpp
'7'  // character
7    // integer
```

Convert:

```cpp
'7' - '0'  // 7
```

---

### Mistake 2: Forgetting `+1`

```cpp
ch - 'a'
```

gives:

```text
a → 0
```

not:

```text
a → 1
```

For 1-based:

```cpp
ch - 'a' + 1
```

---

### Mistake 3: Using `ch - 'a'` for uppercase

This assumes lowercase.

For uppercase:

```cpp
ch - 'A'
```

---

### Mistake 4: Using a map unnecessarily

If the keys are only:

```text
a-z
```

don't automatically reach for:

```cpp
unordered_map<char, int>
```

Think:

```cpp
vector<int> freq(26);
```

first.

---

# 15. Complexity Cheat Sheet

| Operation      | Time | Space |
| -------------- | ---: | ----: |
| `ch - 'a'`     | O(1) |  O(1) |
| `ch - 'a' + 1` | O(1) |  O(1) |
| `'z' - ch + 1` | O(1) |  O(1) |
| `ch - '0'`     | O(1) |  O(1) |
| `digit + '0'`  | O(1) |  O(1) |
| `tolower(ch)`  | O(1) |  O(1) |
| `toupper(ch)`  | O(1) |  O(1) |

For processing a string of length `n`:

```text
Character conversion per character → O(1)

Entire string → O(n)
```

---

# 16. The Most Important Formulas

### Lowercase

```cpp
ch - 'a'
```

```text
a → 0 ... z → 25
```

### Lowercase 1-based

```cpp
ch - 'a' + 1
```

```text
a → 1 ... z → 26
```

### Reverse alphabet

```cpp
'z' - ch + 1
```

```text
a → 26 ... z → 1
```

### Reverse 0-based

```cpp
'z' - ch
```

```text
z → 0 ... a → 25
```

### Digit character → integer

```cpp
ch - '0'
```

### Integer → digit character

```cpp
digit + '0'
```

---

# 🧠 DSA Problem-Solving Checklist

Whenever you see a character/string problem, ask:

### 1. What is the character set?

```text
a-z?
A-Z?
0-9?
mixed?
```

### 2. Can the character become an index?

```cpp
ch - 'a'
```

### 3. Is the range small and fixed?

If yes:

```cpp
array / vector
```

instead of automatically using:

```cpp
unordered_map
```

### 4. Do I need frequency or just presence?

```text
frequency → vector<int>
presence  → vector<bool>
```

### 5. Do I need first/last position?

Use:

```cpp
vector<int>(26, -1)
```

### 6. Is this a character digit?

Remember:

```cpp
ch - '0'
```

---

# 🔥 Final Mental Model

Don't memorize 20 isolated formulas.

Remember these **3 bases**:

```text
'a' → lowercase alphabet base
'A' → uppercase alphabet base
'0' → digit base
```

Then:

```text
ch - base
     ↓
relative index/value
```

Examples:

```cpp
ch - 'a'   // lowercase index
ch - 'A'   // uppercase index
ch - '0'   // digit value
```

And for 1-based indexing:

```cpp
ch - base + 1
```

### ⭐ Golden Rule

> **If the input has a small fixed character range, think "array index" before thinking "hash map".**

That single habit can simplify many string DSA problems.
