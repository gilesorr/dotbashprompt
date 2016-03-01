# Dot Bashprompt
An easily extensible collection of Bash Prompts.

**Dot Bashprompt** is a set of Bash functions that allows you to easily
create complex Bash Prompts, and change them on the fly.  If you have the
inclination, it's also intended to be extensible and easy to build your own
prompt.


## Getting Started

Run `git clone https://github.com/gilesorr/dotbashprompt.git ~/.bashprompt`
followed by `source ~/.bashprompt/promnow` and finally `promnow` which
should give you basic usage information, including instructions on how to
immediately change your prompt to one of those available in the repository
name (`promnow <prompt-name>`).  If you want to continue to use a
particular prompt, edit your **~/.bashrc** and add the following lines at
the end:

```
source ~/.bashprompt/promnow
promnow your-favourite-prompt # replace with the name of a prompt you like
```


## Known Issues

None yet, but this is the first public release ...


## Bibliography

- `man bash`
- [The Bash Prompt HOWTO](http://www.gilesorr.com/bashprompt/howto/) (also
  written by me ... it's a subject I'm interested in).  It's old, but still
  technically correct.  I keep meaning to update it ...

