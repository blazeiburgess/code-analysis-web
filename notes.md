# Notes

These are notes to code changes made for the code-analysis-web portion of this code challenge. The notes will explain the reasoning behind various commits I made on a forked repository.

---

[commit 1: Fix getLastName function output to avoid trailing numbers and hyphens](https://github.com/blazeiburgess/code-analysis-web/commit/e66f7816deed8d3387bbb6462ba041977edb4072)

The original `getLastName` function was matching names like 'Raines2' or 'Ashby-'. This was because the regex's parameters were to broad. I changed this to be exclusionary of known problems rather than inclusionary of any alphanumerics, which cleaned the output.

---

[commit 2:  Change shuffleList function to copy entire array, only modify copied array, and simplify syntax](https://github.com/blazeiburgess/code-analysis-web/commit/705c944654b8456c972c29d6fa7cb7afcc9ff5ab)

The most obvious change was to make the copy `result` array a `slice(0)` of the passed in `list` array. As it was originally `slice(1)`, this meant that shuffling reduced the array by one person every time.

The second big change was that the function intended to only modify the copied array, but it actually modified the original list, which meant that the search function would default to the suffled search rather than sorted.

The last change was stylistic. It seemed convoluted and difficult to read to have `i = list.length - 1`, then get a random index in the array from `i + 1` in a for loop that would repeatedly perform `i -= 1`. This could be simplified to a while loop that ends when i no longer has value and having `i--` everytime the random index is generated.

---

[commit 3:  Change search filter for case insensitive, partial matches](https://github.com/blazeiburgess/code-analysis-web/commit/59314beb0332a757e62897b415271212e13738e1)

This was a simple fix. The original search was determined by a `===` operator. This required case sensitive, complete matches. My modified function compares `.toLowerCase()` strings, so case insensitive, and checks the `indexOf` the user input against the person's name. So long as the index is above 0 there is a partial match and that person is returned in the filter.

---

[commit 4: Remove redundant conditionals, change slice to copy entire array, simplify sort logic](https://github.com/blazeiburgess/code-analysis-web/commit/dd082f4f1d8633dd2b40885e95a3625865eeb90a)

First fix, as with the shuffle function, was to change `slice(1)` to `slice(0)` so the entire array would be copied. The rest was just simplifying the logic of the sort. The original had two conditionals, one to check true, the other to check false. It also had an unnecessary trailing `return 1` if neither condition was met. This could be reduced to a single line: `result.sort((a, b) => a[prop] > b[prop])`

This handles all those conditions, but is much easier to read/comprehend.

---

[commit 5: Change sortObjListByProp function to accurately sort last names](https://github.com/blazeiburgess/code-analysis-web/commit/c1fc0ce6e56298bac5b81e7e9943f0683e5f05fe)

The last name sort function was entirely broken. It literally just reversed the first name sort. In order to fix this without writing a massive amount of redundant code, I decided to modify the `sortObjListByProp` function to accept a `modifier` as well as the `objList` parameter. The modifier would be a function that would then be called during the sort to change what was being sorted.

As `lastName` was not a property of the person object it was necessary to perform this operation during the sort. To maintain the `const sortByLastName` structure, the modifier was then just passed in as a second parameter and that was all it took.
