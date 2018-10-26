# Dot Bashprompt
An easily extensible collection of Bash Prompts.

**Dot Bashprompt** is a set of Bash functions that allows you to easily
create complex Bash Prompts, and change them on the fly.  If you have the
inclination, it's also intended to be extensible and easy to build your own
prompt.  It also comes with a variety of ready-to-use prompts.


## Getting Started

Run `git clone https://github.com/gilesorr/dotbashprompt.git ~/.bashprompt`
followed by `source ~/.bashprompt/promnow` and finally `promnow` which
will give you basic usage information, including instructions on how to
immediately change your prompt to one of those available in the repository
(`promnow <prompt-name>`).  Please note that - for now - this probably
won't work if it's not checked out to the **~/.bashprompt/** folder.  Tab
completion is available on prompt names.

If you want to continue to use a particular prompt, edit your **~/.bashrc**
and add the following lines at the end:

```
source ~/.bashprompt/promnow
promnow <prompt-name> > /dev/null # replace with the name of a prompt you like
```

Sending the output to `/dev/null` squashes the prompt announcement, which
is only really useful when you're interactively changing prompts.


## Known Issues

I design prompts using a terminal with a black background.  This means the
prompts commonly look lousy with a light or white background.  This is
being addressed, but slowly.  Design a better one and send me a pull
request.

If you've aliased any Unix utility used by the prompt functions (this
includes but isn't limited to things like ``ls``, ``grep``, ``awk``, and
``sed``), there's a very good possibility it will break the prompts.  I'm
adding ``unalias -a`` to the functions to avoid this, but if you've used a
function overlay of a common utility like ``ls``, there's nothing I can do.

One of the colours available (there aren't many) for making prompts is,
reasonably enough, "black".  Under Linux, "bold black" is actually a dark
gray.  It doesn't make a lot of sense, but there it is.  And when you have
limited choices, you go with what's available.  Unfortunately, on the Mac,
"bold black" is represented as ... black.  Making prompts that use this
colour appear somewhat broken.  No fix yet.

I use Bash 4 on Linux, and Homebrew's Bash 4 on Mac, not Apple's default
(and extremely old) Bash 3.  I don't think I'm using any Bash-4-only
constructs, but that may eventually happen which could potentially break
some prompts on Mac Bash 3.


## Acknowledgements

I want to thank Srikanth Agaram: it was his pointer to [his prompt
setup](https://gitlab.com/aksrikanth/settings/tree/master/config_sources)
that started me creating this project.  If you're interested in
constructing your own prompt(s), looking at what he's done will give you a
somewhat different perspective on the same subject.


## Other Projects

- https://powerline.readthedocs.io/en/master/ - Powerline is best known as
  a plugin for Vim, but can also be used for Bash (and ZSH and Fish and
  ...) prompts.
- https://github.com/magicmonty/bash-git-prompt
- http://bashish.sourceforge.net/ (last updated 2006?)


## Bibliography

- `man bash`
- [The Bash Prompt HOWTO](http://www.gilesorr.com/bashprompt/howto/) (also
  written by me ... it's a subject I'm interested in).  It's old, but still
  technically correct.  I keep meaning to update it ...

