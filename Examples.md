# Dot Bashprompt Examples
<!-- :created: 2022-10-21 17:09 -->

To create a new prompt, simply create a new file in the `prompts/` subfolder
and add some Bash prompt code to it.  If you're not familiar with coding
Bash Prompts, you should read at least part of
[The Bash Prompt HOWTO](http://www.gilesorr.com/bashprompt/howto/) .  This
assumes you've performed the initial setup detailed in the
[README](README.md) and `promnow` is loaded in your environment and
functioning.  The code for these examples is in the `prompts/examples/`
folder.


## The Most Basic Prompt

Create a prompt file called `prompts/myprompt`.  Edit the file and add a
single line that says `PS1="$ "`.  Save and exit the file.  You should
be able to run `promnow myprompt` and your prompt should turn into a
dollar sign with a space after it.  This is the default simplest Bash prompt
(well ... no quotes and no space would work, but that's even uglier).
Just as DOS and Powershell use the ">" as a shell indicator, the csh
and ZSH use "%", Bash uses "$".

Let's improve that a bit and use a standard Bash trick.  Replace the line
with `PS1="\\\$ "` and save it.  You won't see any difference if you're a
regular user, but if you're root your prompt will now be "# ".  This lets
you know just by looking at the prompt that you're the administrator.


## Starting to use Components

Edit another file called `myprompt2` and add the following code:

```sh
source "${PROMPTDIR}/machine/_p_load3"
source "${PROMPTDIR}/machine/_p_threadcount"

threadcount="$(_p_threadcount)"

PS1="[${threadcount}]\$(_p_load3)\\\$ "
```

To use a component, you have to source it - and when you do, you have to
give a full path to it.  `${PROMPTDIR}` is automatically defined as part of
the project, so use it.  Once the function is loaded into memory (the file
and the function will always have the same name), you can run the function
and get results back by including `$(_p_threadcount)` in the code.  That
particular function produces a number which is the number of threads
available on the current machine (works on both Mac and Linux).  Because
threadcount never changes for the life of the shell, we assign it to a
variable and don't call the function again.  `$(_p_load3)` prints the one,
five, and 15 minute loads that most of us are used to from the `uptime`
command, but in a slightly more compressed format (ie. "1.33,2.50,0.56").
Left to its own devices, Bash would immediately interpret the function and
replace any occurrence of `$(_p_load3)` with the current load(s), so your
system would forever appear to have the load it had when you loaded the
prompt.  To make it wait and interpret the function each time the prompt
appears, we escape the function: `\$(_p_load3)`.  The resulting prompt
looks like this: `[8]0.50,0.48,0.57$ `.

I always include threadcount when I show load because a sane maximum load
is roughly "1" on a single-threaded system ... but if you don't know how
many threads the system has, how can you judge if "2" is a heavy load?  On
a four core, eight thread system, it's a light load.  On a single core
system, it's going to keep your processor toasty warm.


## Using Colours

This project defines multiple colour codes.  Start by loading them into
your prompt file with `source "${PROMPTDIR}/_p_colours"`.  You should take
a look at the contents of that file to see the names of the colours that
are used - and are thus available to you.  (In fact most modern terminals
support 256 colours: this file only supports the 16 standard colours from
older terminals - handling 256 colours is possible, but significantly more
complex so I'm not going to introduce it here.)  You will almost always
want to use the colours prefaced by `_pc_`.  The others exist for other
circumstances - again something I might go into later.

```sh
source "${PROMPTDIR}/_p_colours"
source "${PROMPTDIR}/machine/_p_load3"
source "${PROMPTDIR}/machine/_p_threadcount"

threadcount="$(_p_threadcount)"

PS1="${_pc_b_red}[${threadcount}]\$(_p_load3)\\\$${_pc_nocolour} "
```

This results in the same prompt as the one in the previous example, but all
the text is bright red - this makes it much more visible on scrollback,
which I find very useful.

I'm Canadian - I spell it "colours."  I've attempted to make it possible to
spell things the American way as well: the file can also be referenced as
`_p_colors` and the variable is also available as `${_pc_nocolor}`.


## Adding the directory and git status

```sh
source "${PROMPTDIR}/_p_colours"
source "${PROMPTDIR}/directory/_p_git"

PS1=""
PS1+="${_pc_b_yellow}\w  ${_pc_b_purple}\$(_p_git)${_pc_nocolour}"
PS1+="\n"
PS1+="${_pc_b_gray}\\\$${_pc_nocolor} "
```

I'm adding several things here: first, the use of the `+=` construct to add
text to an existing string (in this case, `PS1`).  Then we use `\w` which
is a Bash Prompt thing, meaning "show the working directory."  From their
point of view, that means they show something like this: `~/.bashprompt` -
notice that my home directory has been shortened to the `~` symbol, that's
part of the deal here.  Next, we add another one of the functions available
as part of the Dot Bashprompt project: `_p_git`.  This shows both the git
branch you're on and your current git status: `master+16-7+`.  In this
case, that indicates we're on the "master" branch with 16 lines added and 7
removed, and the trailing "+" indicates that there's at least one new
(untracked) file.  "Git status" is a very personal thing: maybe you don't
care about how many lines are changed, but rather how many files are
changed - the code I've written doesn't show that.  But the pieces are
available separately: `_p_git` is made of two separate functions,
`_p_git_branch_only` and `_p_git_status_only`, each of which can be loaded
and used separately if so desired.  The last new trick is the use of `\n`,
another Bash Prompt escape sequence.  This one indicates a newline, making
this a two line prompt.

Here's a slightly different version of the same prompt using the Dot
Bashprompt function `_p_pwd` instead of `\w`:

```sh
source "${PROMPTDIR}/_p_colours"
source "${PROMPTDIR}/directory/_p_git"
source "${PROMPTDIR}/directory/_p_pwd"

PS1=""
PS1+="${_pc_b_yellow}\$(_p_pwd)  ${_pc_b_purple}\$(_p_git)${_pc_nocolour}"
PS1+="\n"
PS1+="${_pc_b_gray}\\\$${_pc_nocolor} "
```

The difference this makes is that the directory shows as
`/home/giles/.bashprompt` rather than `~/.bashprompt` with the directory
path fully written out.  Again, a matter of personal taste: I usually
prefer the shorter form.


## Return Value ... and the process of Debugging Prompts

Whenever you run a command at the prompt, it has a return value - that
value is "0" if everything went as it should, or some other integer if
there was an error.  Knowing the return value can be useful.  Start a new
prompt, and put in the simplest possible version:

```sh
PS1="\$?\\\$ "
```

`$?` is the return value from the previous command.  As usual, it has to be
backslash-escaped to keep it from being immediately interpreted.

This works well:

```
0$ cat nonexistent
cat: nonexistent: No such file or directory
1$
```

When you try to `cat` a nonexistent file, you get an error.  This is an
error you weren't going to miss anyway because `cat` complains loudly, but
when running more complex commands (or shell scripts that don't do good
error reporting!) this suddenly becomes very useful.

But let's make the error more obvious by colourizing it:

```sh
# THIS HAS A BUG
source "${PROMPTDIR}/_p_colours"

PS1=""
PS1+="\$(
    if [ \"\$?\" -gt \"0\" ]
    then
        echo \"${_pc_bg_gray}${_pc_b_red}\";
    fi
)"
PS1+="\$?${_pc_nocolor}"
PS1+="${_pc_b_gray}\\\$${_pc_nocolor} "
```

What we're trying to achieve here is to show the return value in red on a
gray background if that value is more than zero.  What we get instead is a
plain "0" if the previous command exited cleanly, and a "0" in red on a
gray background if the previous command exited badly.  Wait, what just
happened?

Yes, there's a catch.  In constructing the `PS1` string, I embedded shell
code into the string.  And one of the parts of that code is an `echo`
command - WHICH EMITS A RETURN VALUE.  That return value is zero, because
it managed to print the output it intended to.  We'll fix this in a minute.

First, I wanted to address the abundance of backslashes and quotes.  We're
using code inside a prompt string, which means it gets evaluated when the
prompt is loaded, and then evaluated again when the prompt is run.  When
Bash loads the PS1 string (when you execute `promnow example5`), it will
try to evaluate variables, and it will see a double quote as the end of the
`PS1` string.  So we escape the quotes, and we escape the dollar sign in
front of the variable.  Then you can check if all this worked by typing
`echo $PS1` and examining the code you've managed to generate.  This is why
I recommend building out your `PS1` string one piece at a time and
patiently testing every feature after every single change.  This is also
why I usually use functions rather than embedding code directly in the
`PS1` string.

Here's our revised version, in which we capture the return value before
doing anything else.  We use another part of the Bash Prompt to do this,
the `$PROMPT_COMMAND` that's executed right before the prompt is displayed:

```sh
source "${PROMPTDIR}/_p_colours"

prompt_command () {
    _p_retval=$? # capture the return value before you change it w/prompt
}
PROMPT_COMMAND=" prompt_command ; ${PROMPT_COMMAND} "

PS1=""
PS1+="\$(
    if [ \"\${_p_retval}\" -gt \"0\" ]
    then
        echo \"${_pc_bg_gray}${_pc_b_red}\";
    fi
)"
PS1+="\${_p_retval}${_pc_nocolor}"
PS1+="${_pc_b_gray}\\\$${_pc_nocolor} "
```

This works as intended.

Remember I mentioned I prefer to use functions rather than embedding code
in the `PS1` string?  Because we have to capture the return value before
generating the `PS1` string, there's no good way to use a function here and
this is the simplest solution I've come up with for displaying the return
value.

