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

To learn how websites work as a whole, we are going to pick apart each individual part one by one, and then build something with it, running into (and fixing) security issues along the way.

### HTTP Server in Golang

Make sure you have golang 1.22 installed (check with `go version`).

Make a new directory for your project and `cd` into it.

Go is a very simple language (think C 2.0). I'm choosing to use this over python or javascript because once we start doing more complex things, go won't fall apart.

To start a new go project, we need to initialize a module.

```bash
go mod init com.github.<your github username>-web-security
git init  # optional, but you should always be using git
```

Now we can start writing some go code

```go
// main.go
package main

func main() {}
```

This is the minimal go program. The `func main` is the entry point for all go programs, which always exists in the `main` package.

To print something we can use the `fmt` module.

```go
// main.go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello world")
}
```

Now for the http server.

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	router := http.NewServeMux()
	router.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("hello world"))
	})
	server := http.Server{
		Addr:    ":8080",
		Handler: router,
	}

    // log is better than fmt for real programs
	log.Println("Starting server on port http://127.0.0.1:8080")
	server.ListenAndServe()
}
```

Run this with `go run main.go`.

If we go to <http://127.0.0.1:8080/> in our browser, we should see "hello world".

You can also use `curl http://127.0.0.1/` because both the server (go program) and the client (web browser or curl) speak HTTP.

You can even use another go program

```go
package main

import (
    "net/http"
    "io"
    "fmt"
)

func main() {
	res, err := http.Get("http://127.0.0.1:8080/")
	if err != nil {
		fmt.Println("error getting /", err)
		return
	}

	bytes, err := io.ReadAll(res.Body)
	fmt.Println(string(bytes))
}
```

(also use burpsuite, nc, and/or wireshark)

Ok, but what is a `ServeMux` and what does `ListenAndServe` do? Well, we can just look.

```go
func (srv *Server) ListenAndServe() error {
    // ...
	ln, err := net.Listen("tcp", addr)
	if err != nil {
		return err
	}
	return srv.Serve(ln)
}
```

So `ListenAndServe` create a TCP server and starts listening. Let's try and make our own TCP server. We could try and implement a full HTTP server using TCP, but that would be a lot of work. Let's only send back what the user sent for now (an echo server).

```go
package main

import (
	"log"
	"net"
)

func main() {
	ln, err := net.Listen("tcp", "127.0.0.1:8080")
	if err != nil {
		log.Fatal("err creating tcp server")
		return
	}
	defer ln.Close()

	for {
		conn, err := ln.Accept()
		if err != nil {
			log.Fatal("err accepting listener")
			return
		}

		buf := make([]byte, 1024)
		n, err := conn.Read(buf)
		if err != nil {
			log.Fatal("err reading from connection")
			return
		}

		conn.Write(buf[:n])
        conn.Close()
    }
}
```

If we run it, we can connect to it with `nc`.

```bash
$ nc 127.0.0.1 8080
```

Type something and hit enter, you should see the same message. Hit enter again and `nc` should exit because the connection is closed.

This works, but there's a bug. See if you can find it before moving on. (Hint: what does `conn.Read()` do.)

Open two terminals. In the first, connect to the server with `nc 127.0.0.1 8080` but don't do anything. In the second, connect with `nc 127.0.0.1 8080` and type something and hit enter. You don't get a response. If you type something in the first terminal, you should see a response and also get a response from the second connection.

This is because `conn.Read` blocks until it receives input from its connection. But what if the first client to connect isn't the first to send data? This is where concurrency and parallelism comes in handy, but we won't get into that right now. Just know that there is some mechanism to connect to many clients and wait for _any_ of them to give a response back.

Ok, we know what the HTTP server uses: a TCP server, but what does `net.Listen` use? Again, we can just check.

It required a bit of digging, because `net.Listen` works for more than just TCP, but we see that it uses `socket`, which makes a syscall to open a socket. A socket is OS-level way that your computer can communicate with other computers.

A ~20 min explanation for how sockets work.

- https://youtu.be/YwWfKitB8aA?t=52

Because go doesn't expose a way to make raw sockets, we are going to switch to C for this last part.

```c
#include <arpa/inet.h>
#include <asm-generic/socket.h>
#include <netinet/in.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <unistd.h>

int main(void) {

  int socket_fd = socket(AF_INET,     // internet socket
                         SOCK_STREAM, // tcp
                         0            // auto detect protocol
  );
  if (socket_fd < 0) {
    printf("Error creating socket\n");
    return EXIT_FAILURE;
  }

  int opt = 1;
  int err = setsockopt(socket_fd,
                       SOL_SOCKET, // set this option at the socket level
                       SO_REUSEADDR | SO_REUSEPORT, &opt, sizeof opt);
  if (err) {
    printf("Error setting socket options\n");
    return EXIT_FAILURE;
  }

  struct sockaddr_in address;
  address.sin_family = AF_INET;
  address.sin_port = htons(8080);
  address.sin_addr.s_addr = INADDR_ANY;

  err = bind(socket_fd, (struct sockaddr *)&address, sizeof address);
  if (err) {
    printf("Error binding socket\n");
    return EXIT_FAILURE;
  }

  err = listen(socket_fd, 4);
  if (err) {
    printf("Error listening to socket\n");
    return EXIT_FAILURE;
  }

  socklen_t addrlen = sizeof(address);
  int conn = accept(socket_fd, (struct sockaddr *)&address, &addrlen);
  if (conn < 0) {
    printf("Error accepting new connection\n");
    return EXIT_FAILURE;
  }

  char buffer[1024] = {0};
  int n = read(conn, buffer, 1024 - 1);

  n = send(conn, buffer, n, 0);
  if (n < 0) {
    printf("Error sending data\n");
    return EXIT_FAILURE;
  }

  close(conn);
  close(socket_fd);

  return EXIT_SUCCESS;
}
```

We could see how sockets work at an OS level, but it would depend on the OS that you are using (to some extent).

---

Back to the go server, let's actually make something. To showcase the possible security bugs that might pop up when writing web applications, we are going to make an app that has

- authentication
- authorization
- user submitted data

A todo app has all of those while also being simple enough so that the details of the application aren't the focus.

Let's see some of the capabilities that the HTTP server in go's std lib has.

```go
package main

import (
	"log"
	"net/http"
)

func Logger(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		requestPath := r.URL.Path
		log.Println(r.Method, requestPath)
		next.ServeHTTP(w, r)
	})
}

func main() {
	router := http.NewServeMux()
    // 1. only /, not matching anything
	router.HandleFunc("/{$}", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("hello world"))
	})

    // 2. path parameters
    // GET /1 => id: 1
    // GET /asdf => id: asdf
	router.HandleFunc("/{id}", func(w http.ResponseWriter, r *http.Request) {
		id := r.PathValue("id")
		w.Write([]byte("id: " + id))
	})

    // 3. specific http verbs
    // GET /submit => 404 not found / 405 method not allowed
	router.HandleFunc("POST /submit", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("submitted"))
	})

    // 4. subrouting
    authRouter := http.NewServeMux()
    authRouter.HandleFunc("POST /register", ...)
    authRouter.HandleFunc("POST /login", ...)

	v1 := http.NewServeMux()
	v1.Handle("/v1/", http.StripPrefix("/v1", router))
	v1.Handle("/v1/auth/", http.StripPrefix("/v1/auth", authRouter))

	server := http.Server{
		Addr:    "127.0.0.1:8080",
        // 5. middleware
		Handler: Logger(v1),
	}

	log.Println("Starting server on port http://127.0.0.1:8080")

	server.ListenAndServe()
}
```

Other than a database (and the front end), this is all we need to make a good todo app.

Lets start with reading and creating todos

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func Logger(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		requestPath := r.URL.Path
		log.Println(r.Method, requestPath)
		next.ServeHTTP(w, r)
	})
}

type Todo struct {
	Task string
	Done bool
}

func NewTodo(task string) Todo {
	return Todo{
		Task: task,
		Done: false,
	}
}

func main() {
	todos := []Todo{}
	todos = append(todos, NewTodo("make slides"))
	todos = append(todos, NewTodo("do laundry"))

	router := http.NewServeMux()

	router.HandleFunc("GET /api/todo", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "%+v\n", todos) // + to print struct field names
	})

	router.HandleFunc("POST /api/todo", func(w http.ResponseWriter, r *http.Request) {
		err := r.ParseForm()
		if err != nil {
			http.Error(w, "bad formdata", 400)
			return
		}

		task := r.Form.Get("task")
		todos = append(todos, NewTodo(task))

		w.WriteHeader(201)
		fmt.Fprintf(w, "%+v\n", todos) // + to print struct field names
	})

	server := http.Server{
		Addr:    "127.0.0.1:8080",
		Handler: Logger(router),
	}

	log.Println("Starting server on port http://127.0.0.1:8080")

	server.ListenAndServe()
}
```

```bash
curl http://localhost:8080/api/todo

curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "task=hi" http://localhost:8080/api/todo

curl http://localhost:8080/api/todo
```

Ok, now lets handle updating and deleting. If we want to change a todos `Done` field from `false` to `true`, we don't have a way of addressing a specific todo item. One way is to assign an ID to each todo item when it is created.

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
