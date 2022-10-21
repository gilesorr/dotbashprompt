# Dot Bashprompt Examples
<!-- :created: 2022-10-21 17:09 -->

To create a new prompt, simply create a new file in the `prompts/` subfolder
and add some Bash prompt code to it.  If you're not familiar with coding
Bash Prompts, you should read at least part of
[The Bash Prompt HOWTO](http://www.gilesorr.com/bashprompt/howto/) .  This
assumes you've performed the initial setup detailed in the
[README](README.md) and `promnow` is loaded in your environment and
functioning.


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

