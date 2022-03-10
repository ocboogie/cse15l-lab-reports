# Week 10: Fifth Lab Report

## Finding the difference

First I saved the output of our implementation with the following command

```
bash script.sh > results-our.txt
```

Then in the review implementation, I did the same thing

```
bash script.sh > results-joe.txt
```

I moved the files into the same directory and then ran

```
diff results-our.txt results-joe.txt
```

Resulting the output,

```
212c212
< [url]
---
> []
230c230
< [baz]
---
> []
270c270
< []
---
> [/bar\* "ti\*tle"]
...
```

## First test (`577.md`)

### Test Case

```md
![foo](train.jpg)
```

Our implementation output `[]`. Whereas, the example implemenation output `[train.jpg]`. In this case, the correct output would be ours, because we are not supposed to include images as links.

### Bug

The example implementation doesn't check if the character before `[` is `!`. If it is, then it is not a link, therefore should not be included in the output.

```java
// ...
int nextOpenBracket = markdown.indexOf("[", currentIndex);
// ...
```

## Second Test (`481.md`)

### Test Case

```
[link](/uri "title")
```

Our implementation output `['/uri "title"']`. Whereas, the example implemenation output `[]`. In this case, both implementations are wrong. According the CommonMark spec, links are allowed to have titles, which is the part in quotes. However, this is not part of the link. So therefore, the correct output would be `["/url"]`.

### Bug

For our implementation, we would have to do some sort of parsing of titles and links. For example, we could split by spaces and take the first. (There is no specific code that is the problem, so I am not going to include the code snippet for our implementation)

For the example implementation, it is checking if there is a space in the link block. If there is, then don't include the link. Although, we still want to include the link if there is a space between the link and the title.

```java
// ...
String potentialLink = markdown.substring(openParen + 1, closeParen).trim();
if (potentialLink.indexOf(" ") == -1 && potentialLink.indexOf("\n") == -1) {
    toReturn.add(potentialLink);
    currentIndex = closeParen + 1;
} else {
    currentIndex = currentIndex + 1;
}
// ...
```
