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

So, if the input is `Commands: program.sh&myfile&dothisthenthat`, the output will be:

`program.sh -filename=myfile -prefix=dothisthenthat`

You can use all line header indicators, text effects and even long paragraphs up to 1024 characters in length.

But `###`, on the other hand, will be ignored. This is a comment header, and if this is in front of the line, that line will be ignored.
