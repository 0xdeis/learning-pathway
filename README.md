<!-- @format -->

# BCSC Learning Pathway

> Programmers and hackers use the same knowledge, they just think differently.
>
> -- https://www.youtube.com/watch?v=2TofunAI6fU

Learning how to program $\iff$ learning how to hack.

Before getting into hacking things, you first have to understand how they work.
If you want to hack websites, you first need to understand how they work, then you can begin to find bugs.
Maybe even make a few websites to really see how everything fits together, and then try to break things.

## Fundamentals

### The Shell and Linux

<!-- [!NOTE] syntax -->
<!-- https://github.com/orgs/community/discussions/16925 -->
> [!NOTE]
> If you already know this stuff, move on to the next section

When hacking pretty much anything (online), there are some tools that you end up using a lot.
It's definitely worth learning these tools in and out, because

- knowing the capabilities of these tools gives you ideas when hacking
- learning how they work gives you specific knowledge about the underlying technology

The terminal (or shell or command line) is the most powerful way to use most tools.
Most GUIs (graphical user interfaces) have to be dumbed down because showing the user 200 buttons, text inputs, and sliders would be way too much information to process and get familiar with.

That's where the command line interface (CLI) comes in.

All CLI tools can do the exact same thing a GUI can, they just use a single line of text instead of many buttons and text boxes.
Most of the time, the text-only approach ends up letting you do what you want faster and with less mental overhead.

Luckily, there is a fun and interactive way to learn the most common tools. Plus, once you learn a few, the rest are super easy to pick up.

Head over to [https://overthewire.org/wargames/bandit/](https://overthewire.org/wargames/bandit/) and don't come back until you're done. If you get stuck, try one more out of the box thing, and then head to the hints page (TODO).

### Markdown

> [!NOTE]
> If you already know this stuff, move on to the next section

Markdown is a _very_ simple formatting language that makes your writing clear. (This document was written in markdown.) If you are familiar with HTML, I would suggest to take a quick look over the syntax (Markdown transpiles to HTML and all HTML is valid markdown). Otherwise, here's an interactive guide to learning the syntax.

- https://www.markdowntutorial.com/

### Scripting Languages

> [!NOTE]
> If you already know this stuff, move on to the next section

When hacking, sometimes the shell isn't enough. Bash (or Zsh) are powerful languages, but once you start needing `if` statement or `for` loops, they start to get annoying. That's where simple languages like python and javascript help out. I would recommend python because they have an awesome library `pwntools` that makes automating a lot of repetitive stuff easy.

- python https://tryhackme.com/r/room/pythonbasics

Read through the first part of the `pwntools` docs and try to automate the first levels 5 of Bandit.

- https://docs.pwntools.com/en/stable/intro.html
- https://github.com/Gallopsled/pwntools-tutorial

## Computer Networks

One of the reasons computers are as useful as they are today is because of their ability to communicate with each other.

The internet, the thing that allows computers to talk to each other, is a complex collection of technologies and concepts. The seemingly simple task of loading <https://google.com> in your browser probably took over 10 computers to complete.

> As you encounter more complex code, you’ll need a more accurate mental model to help you reason through what the programs are really doing.
>
> -- Jon Gjengset (Rust for Rustaceans, 2021)
<!-- rust mentioned !!! -->

As Jon points out, metal models are very important for understanding complex topics. By the time you complete these exercises, you should have a strong mental model of how the internet works. When thinking about how some program works, you should be able to abstract away the parts that don't matter and focus on the details that do.

- https://www.youtube.com/watch?v=d-zn-wv4Di8

Short burpsuite exercise

1. Open up burpsuite
2. go to the proxy tab
3. turn on interception and open the browser
4. go to <https://google.com>

5. What is your HTTP version?
6. What is your User Agent?

Short wireshark exercise

1. open wireshark
2. select your wireless interface
3. go to a http website (NOT HTTPS)
4. filter for `http`
5. select a packet > right click > follow > HTTP stream
6. look through the traffic

- https://www.youtube.com/watch?v=6G14NrjekLQ

short netcat exercise

1. open two terminals ([S]erver and [C]lient)
2. on S, run `nc -lvnp 9999`
  - `-l` listen
  - `-v` verbose output
  - `-n` do not perform DNS
  - `-p` listen port
3. on C, run `nc 127.0.0.1 9999`
4. type something into C, look at S
5. type something into S, look at C

- https://www.youtube.com/watch?v=VXmvM2QtuMU
- https://tryhackme.com/room/whatisnetworking
- https://tryhackme.com/room/introtolan
- https://tryhackme.com/room/introtonetworking

## Web

<!-- at the end, should be able to explain -->
<!--  - what happens when you type https://google.com into your browser? How does the response get to you? -->
<!--    - DNS -->
<!--    - client-server model -->
<!--    - http requests -->
<!--    - TCP/IP -->
<!--    - routing -->
<!---->
<!-- projects -->
<!--  - sockets in C -->
<!--  - [x] simple http server and client -->
<!--  - [x] port scanner -->
<!--  - network topology mapper -->
<!--  - http framework -->
<!---->
<!---->
<!-- ### Computer Architecture -->
<!---->
<!-- - how does code get executed on your computer -->
<!---->
<!---->
<!-- ## Essential tools / skills -->
<!---->
<!-- - nmap https://tryhackme.com/room/furthernmap -->
<!-- - url fuzzing -->
<!-- - uploading / downloading fills -->
<!-- - spawning reverse shells -->
<!-- - burb / zap -->
<!-- - -->
<!---->
<!-- ## Other -->
<!---->
<!-- - tmux https://tryhackme.com/room/tmuxremux -->
<!-- - (neo)vim ... -->
<!---->
<!---->
<!-- --- -->
<!---->
<!-- Beginner CTFs: -->
<!-- https://tryhackme.com/room/easyctf -->
<!-- https://tryhackme.com/room/rrootme -->
<!-- https://tryhackme.com/room/basicpentestingjt -->
<!---->
<!-- Beginner Educational Rooms: -->
<!-- https://tryhackme.com/room/furthernmap -->
<!-- https://tryhackme.com/room/burpsuitebasics -->
<!-- https://tryhackme.com/room/metasploitintro -->
<!---->
<!-- --- -->
<!---->
<!-- # Tomghost -->
<!---->
<!-- ## What you should know -->
<!---->
<!-- - bash -->
<!-- - nmap -->
<!-- - dirb / dirbuster / gobuster / feroxbuster -->
<!-- - ssh -->
<!-- - john / hashcat / hydra -->
<!-- - linpeas -->
<!-- - https://gtfobins.github.io/gtfobins/zip/ -->
<!-- - https://lolbas-project.github.io/ -->
<!---->
<!-- - CVEs -->
<!-- - exploit db -->
<!-- - how to find POCs for CVEs / exploits OR metasploit -->
<!---->
<!-- ## What you should be able to learn on the fly -->
<!---->
<!-- - pgp / gpg -->
<!-- - java web application architecture -->
<!-- - understand the exploit a bit -->
<!---->
<!-- ## Actual knowledge -->
<!---->
<!-- ... -->
<!---->
<!-- # Binary Exploitation -->
<!---->
<!-- -  gdb, valgrind, and objdump -->
<!---->
<!-- --- -->
<!---->
<!-- - https://learn.cantrill.io/ -->
