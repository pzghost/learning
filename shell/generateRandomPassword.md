FROM [here](https://www.howtogeek.com/howto/30184/10-ways-to-generate-a-random-password-from-the-command-line/)

<center> <h1>10 Ways to Generate a Random Password from the Linux Command Line</h1> </center>

1.  This method uses SHA to hash the date, runs through base64, and then outputs the top 32 characters.
```
# date +%s | sha256sum | base64 | head -c 32 ; echo
ZDdjOTllNGYxZmMxNTRmNzQyOTY0ZmU3
```
2. This method used the built-in /dev/urandom feature, and filters out only characters that you would normally use in a password. Then it outputs the top 32.
```
# < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-32};echo;
JELOTs4ZmnH2w-i9GG1VldIho4mcy3CI
```
3. This one uses openssl’s rand function, which may not be installed on your system. Good thing there’s lots of other examples, right?
```
# openssl rand -base64 32
8irlwkMEHaLC7/EXaD6xUIQiakWbp8svOXkhUDIH0Uk=
```
4. This one works a lot like the other urandom one, but just does the work in reverse. Bash is very powerful!
```
# tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1
o9jQ70QRbYEu1n0IGKmrjkp8Gn26lE
```
5. Here’s another example that filters using the strings command, which outputs printable strings from a file, which in this case is the urandom feature.
```
# strings /dev/urandom | grep -o '[[:alnum:]]' | head -n 30 | tr -d '\n'; echo
x3kZ4Oov7AKhQ6KL0VUVClh6wz1beE
```
6. Here’s an even simpler version of the urandom one.
```
# < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c6
suDI7X
```
7. This one manages to use the very useful dd command.
```
# dd if=/dev/urandom bs=1 count=32 2>/dev/null | base64 -w 0 | rev | cut -b 2- | rev
vyKf5Cr/H3EHIsj49LzKm/Fjj8YxDo39TaPjFv/xBgA
```
8. You can even create a random left-hand password, which would let you type your password with one hand.
```
# </dev/urandom tr -dc '12345!@#$%qwertQWERTasdfgASDFGzxcvbZXCVB' | head -c8; echo ""
3d$ZA$fr
```
9. If you’re going to be using this all the time, it’s probably a better idea to put it into a function. In this case, once you run the command once, you’ll be able to use randpw anytime you want to generate a random password. You’d probably want to put this into your ~/.bashrc file.
```
# randpw(){ < /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-16};echo;};randpw
npg2PEbvooGETnKR
```
You can use this same syntax to make any of these into a function—just replace everything inside the { }
10. And here’s the easiest way to make a password from the command line, which works in Linux, Windows with Cygwin, and probably Mac OS X. I’m sure that some people will complain that it’s not as random as some of the other options, but honestly, it’s random enough if you’re going to be using the whole thing.
```
# date | md5sum
6e1be5057d71adb9cb1dd8316769a901  -
```
Yeah, that’s even easy enough to remember.

- There’s loads of other ways that you can create a random password from the command line in Linux—for instance, the mkpasswd command, which can actually assign the password to a Linux user account. So what’s your favorite way?

