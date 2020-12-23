FROM [here](https://www.programmersought.com/article/60733977116/)
<center> <h1>Shell string interception</h1> </center>

1. Summary

format | Explanation
---|---
${string: start :length}|	Start at the start character from the left of the string, and intercept length characters to the right.
${string: start}    |	Start intercepting from the start character at the left of the string to the end.
${string: 0-start :length}    |	Start at the start character from the right of the string, and intercept length characters to the right.
${string: 0-start}|	Start from the start character at the right of the string, until the end.
${string#*chars}|	Starting from the position where string *chars first appears, intercept all characters to the right of *chars.
${string##*chars}|	Starting from the position where string *chars last appears, intercept all characters to the right of *chars.
${string%*chars}|	Starting from the position where string *chars first appears, intercept all characters to the left of *chars.
${string%%*chars}|	Starting from the position where string *chars last appears, intercept all characters to the left of *chars.

2. Specify location to start intercepting

2.1 Start counting from the left of the string
```
${string: start :length}
```
> Among them, string is the string to be intercepted, start is the starting position (starting from the left, counting from 0), and length is the length to be intercepted (if omitted, it means until the end of the string).  
    
Example 1  
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir:2:8}
sers/cox
```
Example 2
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir:2} #Omit length, intercept to the end of the string
sers/coxhuang/Documents/django_code/blog_code/script/app_sh/celery
```

2.2 Start counting from the right
```
${string: 0-start :length}
```
> Compared with the #1 format, the #2 format only has 0-, which is a fixed writing method, which is specifically used to indicate counting from the right side of the string.

note:
- When counting from the left, the starting number is 0 (this is in line with programmer thinking); when counting from the right, the starting number is 1 (which is in line with ordinary thinking). If the counting direction is different, the starting number is also different.
- No matter from which side the count starts, the interception direction is from left to right.

Example 3
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir: 0-30: 15}
blog_code/scrip
```
Example 4
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir: 0-30}
blog_code/script/app_sh/celery
```

3. The specified character (substring) starts to be intercepted

> This interception method cannot specify the length of the string, and can only intercept from the specified character (substring) to the end of the string. The Shell can intercept all characters to the right of the specified character (substring), or all characters to the left.

3.1  Use # to intercept the right character
```
${string#*chars}
```

> Among them, string represents the character to be intercepted, chars is the specified character (or substring), * is a kind of wildcard, which means a string of any length. *Chars are used together to mean: ignore all the characters on the left until they meet chars (chars will not be intercepted).

Example 1
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir#*script}
/app_sh/celery
```

Example 2

> You don’t need to ignore the characters to the left of chars, so you don’t have to write *

```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir#/Users/coxhuang} # Need to start from the first on the left to be effective
/Documents/django_code/blog_code/script/app_sh/celery
```

Example 3
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir#*/}
Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery
```
**The above wording ends with the first matching character (substring)**

> If you want to match until the last specified character (substring), then you can use ##, the specific format is:

```
${string##*chars}
```

```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir##*/}
celery
```

3.2 Use% to intercept the left character

> Use the% sign to intercept all characters to the left of the specified character (or substring), the specific format is as follows:

```
${string%chars*}
```
caution, Because you want to intercept the characters on the left of chars and ignore the characters on the right of chars, soIt should be to the right of chars. In other respects,% and # are used in the same way, so I won’t repeat them here, just for example:

Example 1
```
# workdir="/Users/coxhuang/Documents/django_code/blog_code/script/app_sh/celery"
# echo ${workdir%script*}
/Users/coxhuang/Documents/django_code/blog_code/
```
