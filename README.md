# Dot Bashprompt
An easily extensible collection of Bash Prompts.

**Dot Bashprompt** is a set of Bash functions that allows you to easily
create complex Bash Prompts, and change them on the fly.  If you have the
inclination, it's also intended to be extensible and easy to build your own
prompt.


## Getting Started

Run `git clone https://github.com/gilesorr/dotbashprompt.git ~/.bashprompt`
followed by `source ~/.bashprompt/promnow` and finally `promnow` which
will give you basic usage information, including instructions on how to
immediately change your prompt to one of those available in the repository
(`promnow <prompt-name>`).  Please note that - for now - this probably
won't work if it's not checked out to the **~/.bashprompt/** folder.

If you want to continue to use a particular prompt, edit your **~/.bashrc**
and add the following lines at the end:

```
source ~/.bashprompt/promnow
promnow <prompt-name> # replace with the name of a prompt you like
```


## Known Issues

I design prompts using a terminal with a black background.  This means the
prompts commonly look lousy with a light or white background.  Design a
better one and send me a pull request.

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


## Acknowledgements

I want to thank Srikanth Agaram: it was his pointer to [his prompt
setup](https://gitlab.com/aksrikanth/settings/tree/master/config_sources)
that started me creating this project.  If you're interested in
constructing your own prompt(s), looking at what he's done will give you a
somewhat different perspective on the same subject.


## Bibliography

- `man bash`
- [The Bash Prompt HOWTO](http://www.gilesorr.com/bashprompt/howto/) (also
  written by me ... it's a subject I'm interested in).  It's old, but still
  technically correct.  I keep meaning to update it ...

