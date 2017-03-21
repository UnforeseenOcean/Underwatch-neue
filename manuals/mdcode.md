# Markdown Code Interpretor

## Overview

Instead of using C++, Lua or YML, this uses **MARKDOWN!!**

Holy shit, it's amazing, right?!

## Technical details

It's a standard do-what-the-manual-says interpreter which gets its input from lines inside Markdown documents.

You have to give it a command filter list first to teach it how to handle something, then you can use it just like Lua.

Filter list is a training data used to change the line in the document to a basic Bash or Batch command, which is also adaptable for other programs.

## Cut the shit, let's talk about programming

Training data is composed like this: 

`LineStart="Commands:", Initcmd="program.sh", Argument1="filename=", Argument2="prefix=", ArgumentSeparator=" -", EscapeChar="^", Delimiter="&"`

Let's break it down so it's easier to read.

`LineStart` sets the word in each line which the interpreter will look for commands.

`InitCmd` sets the initial command to execute (so arguments will be used).

`ArgumentN` sets various arguments and command-line options. N in the end refers to number of arguments. You don't have to put arguments in the actual document. It will just ignore the line.

`ArgumentSeparator` sets the character that will be inserted into the start of each argument.

`EscapeChar` sets the escape character for the commands so things like double comma, asterisks and minus signs are not interpreted as commands.

`Delimiter` sets the character which limits the length of the argument. After this character, the interpreter will jump to next argument.

So, if the input is `Commands: myfile&dothisthenthat`, the output will be:

`program.sh -filename=myfile -prefix=dothisthenthat`

You can use all line header indicators, text effects and even long paragraphs up to 1024 characters in length.

But `###` and `--`, on the other hand, will be ignored. This is a comment header, and if this is in front of the line, that line will be ignored.

You can use unordered and ordered list as a type of argument, or a way to enforce lines to execute in a particular order.

For example, this (using the example data from above):

Commands:
- stuff1&things3
- stuff2&eggs
- stuff3&mydoom

Will be seen as:

`program.sh -filename=stuff1 -prefix=things3`

`program.sh -filename=stuff2 -prefix=eggs`

`program.sh -filename=stuff3 -prefix=mydoom`

Oh, didn't I tell you that the order of arguments can be random as long as it is readable AND is tagged?

---

Other commands, not used in the example above or not yet implemented, but planned:

`LookFor`: Used in each argument, words starting with this set of character will be used as that argument. 

Example: `Argument1="stuff="&LookFor="!@"` Input: `u7&hj&!@nyxem&friwnf$gierwjg##dfhsaiou^^*&ghh$#` Output: `stuff=nyxem`

Caution: Delimiter argument must be used!

If `LookFor` is not used, the argument is passed sequentially from the start of the line, and if multiple instances of the header character is in the string, it will only use the first one it comes across.

Note that it will avoid reading same character twice and reads stuff from above, so if first header is `!@` and second was `!@@`, it will ignore the second one! Put spaces or strings there. (It will find the header from the string it comes across from input, which you can point to specific line with section heading indicators.)

`PassFrom`: Gets input from external module, literally. 

Example: `Argument1="seed="&PassFrom="chance.integer();"` Input: `[not needed]` Output: `-38499`

`PointTo`: Section heading to look for inputs. `---` is used as a delimiter. Use two `#` as a way to indicate the section.

`# Title Name`: Used as a title, program tag or both. Same as `<h1>` in HTML. You can put something like `# MyProgram` and set filename to `cutekittens.md` and call `MyProgram`, and it will load `cutekittens.md` instead of `myprogram.md`.

`!MD!`: Markdown File Header, used to filter out other files for quicker loading time. If first 4 bytes are not `!MD!`, that file is skipped automatically.

(More to come!)
