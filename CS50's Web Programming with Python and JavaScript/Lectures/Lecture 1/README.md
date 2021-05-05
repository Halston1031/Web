# Lecture 1
-  <a href= "#1">hello, world</a>
-  <a href= "#2">Compilers</a>
-  <a href= "#3">String</a>
-  <a href= "#4">Memory, imprecision, and overflow</a>


<h2 id="1">hello, world</h2>
- The “when green flag clicked” block in Scratch starts the main program; clicking the green flag causes the right set of blocks underneath to start. In C, the first line for the same is `int main(void)`, which we’ll learn more about over the coming weeks, followed by an open curly brace `{`, and a closed curly brace `}`, wrapping everything that should be in our program.

- The “say (hello, world)” block is a function, and maps to `printf("hello, world");`. In C, the function to print something to the screen is `printf`, where `f` stands for “format”, meaning we can format the printed string in different ways. Then, we use parentheses to pass in what we want to print. We have to use double quotes to surround our text so it’s understood as text, and finally, we add a semicolon `;` to end this line of code.

- To make our program work, we also need another line at the top, a header line `#include <stdio.h>` that defines the `printf` function that we want to use. Somewhere there is a file on our computer, `stdio.h`, that includes the code that allows us to access the `printf` function, and the `#include` line tells the computer to include that file with our program.


<h2 id="2">Compilers</h2>
- Once we save the code that we wrote, which is called **source code**, we need to convert it to **machine code**, binary instructions that the computer understands directly.

- We use a program called a **compiler** to compile our source code into machine code.

- To do this, we use the **Terminal** panel, which has a **command prompt**. The $ at the left is a prompt, after which we can type commands.

- We type `clang hello.c` (where `clang` stands for “C languages”, a compiler written by a group of people). But before we press enter, we click the folder icon on the top left of CS50 Sandbox. We see our file, `hello.c`. So we press enter in the terminal window, and see that we have another file now, called `a.out` (short for “assembly output”). Inside that file is the code for our program, in binary. Now, we can type `./a.out` in the terminal prompt to run the program `a.out` in our current folder. We just wrote, compiled, and ran our first program!

<h2 id="3">String</h2>
- But after we run our program, we see `hello, world$`, with the new prompt on the same line as our output. It turns out that we need to specify precisely that we need a new line after our program, so we can update our code to include a special newline character, `\n`:

```c
#include <stdio.h> 
int main(void)
{
    printf("hello, world\n");
}
```

-  Now we need to remember to recompile our program with `clang hello.c` before we can run this new version.

- Line 2 of our program is intentionally blank since we want to start a new section of code, much like starting new paragraphs in essays. It’s not strictly necessary for our program to run correctly, but it helps humans read longer programs more easily.

- We can change the name of our program from `a.out` to something else, too. We can pass **command-line arguments**, or additional options, to programs in the terminal, depending on what the program is written to understand. For example, we can type `clang -o hello hello.c`, and `-o hello` is telling the program `clang` to save the compiled output as just hello. Then, we can just run `./hello`.

- In our command prompt, we can run other commands, like `ls` (list), which shows the files in our current folder:

```c
$ ls a.out* hello* hello.c
```

- The asterisk, `*`, indicates that those files are executable, or that they can be run by our computer.

- We can use the `rm` (remove) command to delete a file:

```c
$ rm a.out
rm: remove regular file 'a.out'?
```

- We can type `y` or `yes` to confirm, and use `ls` again to see that it’s indeed gone forever.

- Now, let’s try to get input from the user.

```c
string answer = get_string("What's your name?\n");
printf("hello, %s\n", answer);
```

- First, we need a **string**, or piece of text (specifically, zero or more characters in a sequence in double quotes, like `""`, `"ba"`, or "bananas"), that we can ask the user for, with the function `get_string`. We pass the prompt, or what we want to ask the user, to the function with `"What is your name?\n"` inside the parentheses. On the left, we want to create a variable, `answer`, the value of which will be what the user enters. (The equals sign `=` is setting the value from right to left.) Finally, the type of variable that we want is `string`, so we specify that to the left of `answer`.
  
- Next, inside the `printf` function, we want the value of `answer` in what we print back out. We use a placeholder for our string variable, %s, inside the phrase we want to print, like `"hello, %s\n"`, and then we give `printf` another argument, or option, to tell it that we want the variable `answer` to be substituted.
  
- If we made a mistake, like writing `printf("hello, world"\n);` with the `\n` outside of the double quotes for our string, we’ll see an errors from our compiler:

```c
$ clang -o hello hello.c
hello.c:5:26: error: expected ')'
    printf("hello, world"\n);
                         ^
hello.c:5:11: note: to match this '('
    printf("hello, world"\n);
          ^
1 error generated.
```

   - The first line of the error tells us to look at `hello.c`, line 5, column 26, where the compiler expected a closing parentheses, instead of a backslash.
   
   - To simplify things (at least for the beginning), we’ll include a library, or set of code, from CS50. The library provides us with the `string` variable type, the `get_string` function, and more. We just have to write a line at the top to `include` the file `cs50.h`:
   
```c
#include <cs50.h> #include <stdio.h> 
int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, name\n");
}
```

- So let’s make a new file, `string.c`, with this code:

```c
#include <stdio.h> 
int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

- Now, if we try to compile that code, we get a lot of lines of errors. Sometimes, one mistake means that the compiler then starts interpreting correct code incorrectly, generating more errors than there actually are. So we start with our first error:

```c
$ clang -o string string.c
string.c:5:5: error: use of undeclared identifier 'string'; did you mean 'stdin'?
  string name = get_string("What's your name?\n");
  ^~~~~~
  stdin
/usr/include/stdio.h:135:25: note: 'stdin' declared here
extern struct _IO_FILE *stdin;          /* Standard input stream.  */
```

- We didn’t mean `stdin` (“standard in”) instead of `string`, so that error message wasn’t helpful. In fact, we need to import another file that defines the type `string` (actually a training wheel from CS50, as we’ll find out in the coming weeks).
  
- So we can include another file, `cs50.h`, which also includes the function `get_string`, among others.

```c
#include <cs50.h> #include <stdio.h> 
int main(void)
{
    string name = get_string("What's your name?\n");
    printf("hello, %s\n", name);
}
```

- Now, when we try to compile our program, we have just one error:

```c
$ clang -o string string.c
/tmp/string-aca94d.o: In function `main': string.c:(.text+0x19): undefined reference to `get_string'
clang-7: error: linker command failed with exit code 1 (use -v to see invocation)
```

- It turns out that we also have to tell our compiler to add our special CS50 library file, with `clang -o string string.c -lcs50`, with `-l` for “link”.
  
- We can even abstract this away and just type `make string`. We see that, by default in the CS50 Sandbox, `make` uses`clang` to compile our code from `string.c` into `string`, with all the necessary arguments, or flags, passed in.


<h2 id="4">Memory, imprecision, and overflow</h2>
- Our computer has memory, in hardware chips called RAM, random-access memory. Our programs use that RAM to store data as they run, but that memory is finite. So with a finite number of bits, we can’t represent all possible numbers (of which there are an infinite number of). So our computer has a certain number of bits for each float and int, and has to round to the nearest decimal value at a certain point.
- With `floats.c`, we can see what happens when we use floats:

- With `%50f`, we can specify the number of decimal places displayed.
- Hmm, now we get …
```C
x: 1
y: 10
x / y = 0.10000000149011611938476562500000000000000000000000
```
- It turns out that this is called floating-point imprecision, where we don’t have enough bits to store all possible values, so the computer has to store the closest value it can to 1 divided by 10.
- We can see a similar problem in `overflow.c`:
```c
#include <stdio.h> #include <unistd.h> 
int main(void)
{
    for (int i = 1; ; i *= 2)
    {
        printf("%i\n", i);
        sleep(1);
    }
}
```
- In our `for` loop, we set `i` to `1`, and double it with `*= 2`. (And we’ll keep doing this forever, so there’s no condition we check.)
- We also use the `sleep` function from `unistd.h` to let our program pause each time.
- Now, when we run this program, we see the number getting bigger and bigger, until:
```c
1073741824
overflow.c:6:25: runtime error: signed integer overflow: 1073741824 * 2 cannot be represented in type 'int'
-2147483648
0
0
...
```
- It turns out, our program recognized that a signed integer (an integer with a positive or negative sign) couldn’t store that next value, and printed an error. Then, since it tried to double it anyways, `i` became a negative number, and then 0.
- This problem is called __integer overflow__, where an integer can only be so big before it runs out of bits and “rolls over”. We can picture adding 1 to 999 in decimal. The last digit becomes 0, we carry the 1 so the next digit becomes 0, and we get 1000. But if we only had three digits, we would end up with 000 since there’s no place to put the final 1!
- The Y2K problem arose because many programs stored the calendar year with just two digits, like 98 for 1998, and 99 for 1999. But when the year 2000 approached, the programs would have stored 00, leading to confusion between the years 1900 and 2000.
- A Boeing 787 airplane also had a bug where a counter in the generator overflows after a certain number of days of continuous operation, since the number of seconds it has been running could no longer be stored in that counter.
- So, we’ve seen a few problems that can happen, but now understand why, and how to prevent them.
