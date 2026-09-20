# Character Frequency & Hashing in C++ — DSA

Character frequency is one of the most common patterns in string problems.

The basic idea is:

```text
Character
    ↓
Index / Key
    ↓
Count / Store information
```

It is useful for:

* Frequency counting
* Duplicate detection
* Anagrams
* Permutations
* Character presence
* First/last occurrence
* Sliding window problems
* Substring problems
* Hashing-based solutions

---

# 1. What is Frequency Counting?

Suppose:

```cpp
string s = "banana";
```

We want to know how many times each character occurs.

```text
a → 3
b → 1
n → 2
```

This is called **frequency counting**.

---

# 2. Frequency Array for `a-z`

If the problem says:

> String contains only lowercase English letters.

Use:

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}
```

For:

```text
s = "banana"
```

the array conceptually contains:

```text
a → 3
b → 1
c → 0
...
n → 2
...
z → 0
```

### Mental Model

```text
ch
 ↓
ch - 'a'
 ↓
0 ... 25
 ↓
freq[index]
 ↓
count
```

---

# 3. Why Frequency Array Instead of `unordered_map`?

If the character range is fixed and small:

```text
a-z → only 26 characters
```

then:

```cpp
vector<int> freq(26);
```

is usually simpler than:

```cpp
unordered_map<char, int> freq;
```

### Decision Rule

```text
Fixed + small character set
        ↓
Array / vector

Unknown / large / dynamic keys
        ↓
unordered_map
```

---

# 4. Frequency Array vs `unordered_map`

### Frequency Array

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}
```

For a string of length `n`:

```text
Time  : O(n)
Space : O(1)
```

Because the array always contains only 26 elements.

---

### `unordered_map`

```cpp
unordered_map<char, int> freq;

for(char ch : s) {
    freq[ch]++;
}
```

Average complexity:

```text
Time  : O(n)
Space : O(k)
```

where `k` = number of distinct characters.

For only lowercase `a-z`, `k ≤ 26`, so practically the space is bounded by a constant.

---

# 5. Check Frequency of a Character

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}

cout << freq['a' - 'a'];
```

Since:

```cpp
'a' - 'a' = 0
```

this accesses:

```cpp
freq[0]
```

You can also write:

```cpp
freq[ch - 'a']
```

---

# 6. Check Whether a Character Exists

If you only need presence:

```cpp
vector<bool> seen(26, false);

for(char ch : s) {
    seen[ch - 'a'] = true;
}
```

Then:

```cpp
if(seen['c' - 'a']) {
    // c exists
}
```

### Frequency vs Presence

```text
Need count?
    ↓
vector<int>

Need only yes/no?
    ↓
vector<bool>
```

---

# 7. Count Distinct Characters

```cpp
vector<bool> seen(26, false);

for(char ch : s) {
    seen[ch - 'a'] = true;
}

int distinct = 0;

for(bool x : seen) {
    if(x)
        distinct++;
}
```

Example:

```text
s = "banana"

distinct characters:
a, b, n

answer = 3
```

### Complexity

```text
Time  : O(n + 26) = O(n)
Space : O(1)
```

---

# 8. Find the Most Frequent Character

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}

int maxFreq = 0;
char answer = 'a';

for(int i = 0; i < 26; i++) {

    if(freq[i] > maxFreq) {
        maxFreq = freq[i];
        answer = 'a' + i;
    }
}
```

Important conversion:

```cpp
char ch = 'a' + i;
```

This converts:

```text
0 → a
1 → b
2 → c
...
25 → z
```

---

# 9. Integer Index → Character

This is the reverse of:

```cpp
ch - 'a'
```

If:

```cpp
int index = 3;
```

then:

```cpp
char ch = 'a' + index;
```

gives:

```text
d
```

### Remember

```text
Character → Index

ch - 'a'


Index → Character

'a' + index
```

This pair is extremely useful in DSA.

---

# 10. Find Duplicate Characters

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}

for(int i = 0; i < 26; i++) {

    if(freq[i] > 1) {
        cout << char('a' + i) << " ";
    }
}
```

Condition:

```cpp
freq[i] > 1
```

means that character appeared more than once.

---

# 11. Check if All Characters Are Unique

```cpp
vector<bool> seen(26, false);

for(char ch : s) {

    int idx = ch - 'a';

    if(seen[idx]) {
        return false;
    }

    seen[idx] = true;
}

return true;
```

### Important Pattern

```text
Check before inserting.

if(already_seen)
    duplicate found

mark as seen
```

This pattern is useful beyond strings too.

---

# 12. Anagram Pattern

Two strings are anagrams if they contain the same characters with the same frequencies.

Example:

```text
listen
silent
```

Both contain:

```text
e → 1
i → 1
l → 1
n → 1
s → 1
t → 1
```

### Frequency Array Solution

```cpp
vector<int> freq(26, 0);

for(char ch : s1) {
    freq[ch - 'a']++;
}

for(char ch : s2) {
    freq[ch - 'a']--;
}

for(int x : freq) {
    if(x != 0)
        return false;
}

return true;
```

### Why `++` and `--`?

First string:

```text
character → +1
```

Second string:

```text
character → -1
```

If they have identical frequencies, everything becomes zero.

```text
s1:
a → +3

s2:
a → -3

total:
a → 0
```

### Complexity

```text
Time  : O(n + m)
Space : O(1)
```

where `n` and `m` are the lengths of the two strings.

---

# 13. Alternative Anagram Method

You can also maintain two frequency arrays:

```cpp
vector<int> freq1(26, 0);
vector<int> freq2(26, 0);
```

But the `+1 / -1` technique is often cleaner:

```cpp
freq[ch - 'a']++;
freq[ch - 'a']--;
```

### General Pattern

> If two collections should contain the same frequency, try **increment for one and decrement for the other**.

This idea is useful in many problems.

---

# 14. First Occurrence

Store the first index at which every character appears.

```cpp
vector<int> first(26, -1);

for(int i = 0; i < s.size(); i++) {

    int idx = s[i] - 'a';

    if(first[idx] == -1) {
        first[idx] = i;
    }
}
```

Why `-1`?

Because `0` is a valid index.

```text
-1 → character has not appeared
0+  → character has appeared
```

---

# 15. Last Occurrence

For the last occurrence, simply overwrite:

```cpp
vector<int> last(26, -1);

for(int i = 0; i < s.size(); i++) {
    last[s[i] - 'a'] = i;
}
```

Every new occurrence replaces the previous index.

---

# 16. Frequency + Position Together

Sometimes we need both:

```text
frequency
+
first/last position
```

We can maintain separate arrays:

```cpp
vector<int> freq(26, 0);
vector<int> first(26, -1);

for(int i = 0; i < s.size(); i++) {

    int idx = s[i] - 'a';

    freq[idx]++;

    if(first[idx] == -1) {
        first[idx] = i;
    }
}
```

This is useful when a problem asks things like:

> Find the most frequent character and its first occurrence.

---

# 17. `unordered_map` Frequency

When the character set isn't limited to `a-z`:

```cpp
unordered_map<char, int> freq;

for(char ch : s) {
    freq[ch]++;
}
```

Example:

```text
s = "aB3#aB"
```

Here the possible characters aren't just 26 lowercase letters.

So a map is more convenient.

---

# 18. General Hashing Pattern

The same idea works with many data types.

### Character

```cpp
unordered_map<char, int> mp;
```

### Integer

```cpp
unordered_map<int, int> mp;
```

### String

```cpp
unordered_map<string, int> mp;
```

General pattern:

```cpp
for(auto x : data) {
    mp[x]++;
}
```

Meaning:

> Count how many times each key appears.

---

# 19. Modern `unordered_map` Traversal

### Range-based loop

```cpp
for(auto &[key, value] : mp) {
    cout << key << " " << value << "\n";
}
```

This is useful because:

```text
key   → character/value
value → frequency
```

If you don't want to modify the values:

```cpp
for(const auto &[key, value] : mp) {
    cout << key << " " << value << "\n";
}
```

### Traditional

```cpp
for(auto it = mp.begin(); it != mp.end(); ++it) {
    cout << it->first << " " << it->second << "\n";
}
```

### Important

`unordered_map` does **not guarantee sorted order**.

If you need sorted keys, consider:

```cpp
map<char, int>
```

---

# 20. `map` vs `unordered_map`

| Feature            | `map`         | `unordered_map`  |
| ------------------ | ------------- | ---------------- |
| Internal structure | Balanced BST  | Hash table       |
| Average lookup     | O(log n)      | O(1)             |
| Average insert     | O(log n)      | O(1)             |
| Ordered keys       | Yes           | No               |
| Worst-case lookup  | O(log n)      | O(n)             |
| Use when           | Need ordering | Need fast lookup |

### DSA Rule

```text
Need sorted/order traversal?
        ↓
map

Need fast average lookup?
        ↓
unordered_map
```

---

# 21. Frequency of Words Instead of Characters

The same hashing pattern works for strings:

```cpp
unordered_map<string, int> freq;

for(string word : words) {
    freq[word]++;
}
```

So the general concept is:

```text
KEY → FREQUENCY
```

The key can be:

```text
char
int
string
...
```

---

# 22. Frequency as a Signature

Frequency can represent a string's character composition.

For example:

```text
"abbc"

a → 1
b → 2
c → 1
```

Two strings with the same frequency array have the same character composition.

This idea is the foundation of:

* Anagram checking
* Group Anagrams
* Permutation checking
* Finding anagrams inside a string
* Frequency-based substring problems

---

# 23. Sliding Window + Frequency

Frequency arrays become especially powerful with sliding windows.

Example idea:

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}
```

When a character enters the window:

```cpp
freq[ch - 'a']++;
```

When it leaves:

```cpp
freq[ch - 'a']--;
```

Mental model:

```text
Character enters window
        ↓
      +1

Character leaves window
        ↓
      -1
```

This is extremely common in:

* Longest substring problems
* Permutation in string
* Find all anagrams
* Minimum window style problems

---

# 24. Fixed-Size Character Frequency

If the problem only contains lowercase English letters:

```cpp
array<int, 26> freq{};
```

is also a good choice.

Example:

```cpp
for(char ch : s) {
    freq[ch - 'a']++;
}
```

### Why `array<int,26>`?

The size is fixed at compile time.

It communicates clearly:

> "I know there are exactly 26 possible characters."

---

# 25. Common Mistakes

### Mistake 1: Forgetting to initialize

Wrong:

```cpp
int freq[26];
```

For local variables, values are uninitialized.

Better:

```cpp
int freq[26] = {};
```

or:

```cpp
vector<int> freq(26, 0);
```

or:

```cpp
array<int, 26> freq{};
```

---

### Mistake 2: Wrong index

For lowercase:

```cpp
freq[ch - 'a']
```

Not:

```cpp
freq[ch]
```

---

### Mistake 3: Mixing uppercase and lowercase

These are different:

```text
'a' != 'A'
```

If input can contain both, normalize:

```cpp
ch = tolower(ch);
```

or maintain separate handling.

---

### Mistake 4: Assuming `unordered_map` is sorted

This is NOT guaranteed:

```cpp
unordered_map<char, int> mp;
```

Traversal order can be arbitrary.

---

### Mistake 5: Forgetting to decrease frequency

In sliding-window problems:

```cpp
freq[s[right] - 'a']++;
```

When moving left:

```cpp
freq[s[left] - 'a']--;
```

If you forget the decrement, your frequency no longer represents the current window.

---

# 26. Complexity Cheat Sheet

For string length `n` and `k` distinct characters:

### Fixed alphabet frequency array

```text
Time  : O(n)
Space : O(1)
```

### `unordered_map`

Average:

```text
Time  : O(n)
Space : O(k)
```

### `map`

```text
Time  : O(n log k)
Space : O(k)
```

because every insertion/update costs `O(log k)`.

---

# 27. Most Important Patterns to Memorize

### Count

```cpp
freq[ch - 'a']++;
```

### Remove / decrease

```cpp
freq[ch - 'a']--;
```

### Check presence

```cpp
if(freq[ch - 'a'] > 0)
```

### Mark seen

```cpp
seen[ch - 'a'] = true;
```

### Character → index

```cpp
int idx = ch - 'a';
```

### Index → character

```cpp
char ch = 'a' + idx;
```

### First occurrence

```cpp
if(first[idx] == -1)
    first[idx] = i;
```

### Last occurrence

```cpp
last[idx] = i;
```

---

# 🧠 How to Think During a DSA Problem

When you see a string problem, ask:

### 1. Do I need to count something?

```text
YES
 ↓
Frequency
```

### 2. Is the key range small and fixed?

```text
YES → array/vector
NO  → unordered_map
```

### 3. Do I need only presence?

```text
YES → seen[]
```

### 4. Do I need positions?

```text
YES → first[] / last[]
```

### 5. Is it a sliding window?

Think:

```text
enter → +1
leave → -1
```

### 6. Are two strings being compared?

Think:

```text
frequency difference
```

For example:

```cpp
freq[s1[i] - 'a']++;
freq[s2[i] - 'a']--;
```

---

# 🔥 Final Mental Model

Most character-hashing problems reduce to:

```text
                STRING
                   ↓
             Take character
                   ↓
          Convert to index/key
                   ↓
        ┌──────────┴──────────┐
        ↓                     ↓
   Small fixed range       Unknown range
        ↓                     ↓
   array / vector         unordered_map
        ↓                     ↓
     count/store             count/store
```

### The core operations

```cpp
// Add
freq[key]++;

// Remove
freq[key]--;

// Check
freq[key];

// Character → index
ch - 'a';

// Index → character
'a' + index;
```

### ⭐ Golden Rule

> **Hashing is not only `unordered_map`. An array/vector can also act as a hash table when the key range is small and fixed.**

Before using `unordered_map`, ask:

> **"Can I represent my keys as a small integer range?"**

If yes, an array/vector may give you a simpler and often more efficient solution.
