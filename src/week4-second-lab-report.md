# Week 4: Second Lab Report

# First Case

### Diff

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled.png)

### Failure-inducing Input

[https://github.com/ocboogie/markdown-parse/blob/main/testCases/endStartParentheses.md](https://github.com/ocboogie/markdown-parse/blob/main/testCases/endStartParentheses.md)

```makefile
)[
```

### Symptom

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%201.png)

### Explanation

The problem was opening a square bracket without ending it. After finding an open bracket, we search for the closing bracket. However when doing so, we check to see if the character preceding it is a back slash (this is done to ensure it is not an escaped bracket). Since there is no closing bracket, `indexOf` will return `-1`, subtracting `1` gives us `-2`. We then find the character at that index resulting in an index of range error.

# Second Case

### Diff

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%202.png)

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%203.png)

### Failure-inducing Input

[https://github.com/ocboogie/markdown-parse/blob/2b46ea8bb2d8c5f54f7ab99bc88d771b435be7cc/test-file.md](https://github.com/ocboogie/markdown-parse/blob/2b46ea8bb2d8c5f54f7ab99bc88d771b435be7cc/test-file.md)

```markdown
[a] link!](https://something.com)
```

### Symptom

An Infinite loop

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%204.png)

### Explanation

The condition for the while loop is `currentIndex < markdown.length()`, so an Infinite loop indicates that this condition remains true. Of course, `markdown.length()` is staying the same, so the corporate is `currentIndex`. The last search for a closing parentheses results in a `-1`, so when we update `currentIndex`, it gets incorrectly assigned `0`. An easy fix is to break if there are no more `(`.

# Third case

### Diff

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%205.png)

### Failure-inducing Input

[https://github.com/ocboogie/markdown-parse/blob/ce6dcbf6156474a0f980316be42c1bcd66fd0d52/test-file.md](https://github.com/ocboogie/markdown-parse/blob/ce6dcbf6156474a0f980316be42c1bcd66fd0d52/test-file.md)

```markdown
# Title
[fake link]extra space here(not a real link)

[a] link!](https://something.com)

[a link!](https://otherlink.com)
```

### Symptom

Incorrectly labeling `[fake link]extra space here(not a real link)` as a link. Since this is an incorrectly formatted MarkDown link (by there being characters between `]` and `(`), we don’t want to include it in our output.

![Untitled](Second%20Lab%20Report%20375f5cf5dce74b3694fe59384f402ef5/Untitled%206.png)

### Explanation

This one is different from the last two cases where it was obvious that something was wrong (i.e. throwing an exception or being stuck in an infinite loop). Nothing seems wrong here, but by the CommonMark (a formal specification of Markdown) specification ([example 510](https://spec.commonmark.org/0.30/#example-510)), a valid link must not have characters between `]` and `(`. So, we are not following specification if we include `not a real link` as a link. To fix this, we can just check if `]` directly proceeds `(` and only include the link if it does.
