# The Bash Prompt Layout Manifesto
<!-- :created: 2022-10-22 16:32 -->

I originally wrote this for my
[blog](http://localhost/blog/bash-prompt-layout-manifesto.html) - that
text has been copied to here.

------------------------------------------------------------------------

I've been tinkering with the Bash Prompt since 1998, or maybe earlier - and
in that time I've developed some opinions about what should be displayed in
a prompt and how it should be displayed.  I'm not laying down the law here:
these are *opinions*, but reading them may help you clarify how you think a
prompt should look - even if it's totally different from what I suggest.

Back around 1999 I knew a guy whose entire prompt was `0$` - the last
return value was the only thing he figured he needed to see.  "If I need to
know where I am, I can type `pwd`."  I think the world has changed since
then: certainly I spend too much time on remote machines with different
accounts to get by with just that.  But still ... respect.  It's all about
personal taste.

Keep in mind that this is being written by an old-school command line guy
who makes a living building, logging in to, and debugging servers.  How we
use Bash influences what we want to see in the prompt.

- if you work on more than one machine, the hostname should be displayed
- if you work with more than one account, the user name should be displayed
- prompts on different machines should look different, so there's a visual
  change when you `ssh` to another machine
- there should be a visual cue when you're the root user because commands
  run as root can be devastating if mis-handled (`$` vs `#` is
  good, but I usually add more - usually changing the user name to some
  intrusive colour)
- the directory you're in is useful information.  I've experimented with
  many methods of shortening, but other than `~` (short form for
  `$HOME`) I've never been entirely satisfied
- the return value from the last command is a GOOD THING: it should be
  displayed, certainly if it's non-zero
- a colourful prompt is a GOOD THING: when the prompt is the same colour as
  regular text, it's very hard to find in thousands of lines of scrollback
- if the current folder is in a VCS (Version Control System), the current
  status of the VCS should be appended (there are caveats to this one:
  centralized VCS's like SVN have to make a network call to get status, and
  that takes too long in a prompt)
- battery percentage should be displayed if you're on a laptop (some people
  prefer this conditional, ie. it shows up only if below 25%, but I prefer
  to see it all the time)
- an indicator of the writeability of the current directory is more useful
  than you'd think.  If you're in `/usr/bin/` as a regular user, it's
  barely news that it's not writeable.  But if you followed a **link** to a
  system folder, you might not even know you've ended up there.  This
  indicator has also saved me more than once when I deleted the folder from
  under myself: I `cd` to a folder in one terminal, then delete it in
  another.  Doesn't happen often, but it's terribly confusing when it does.
  The directory suddenly changing to [ro] is a big clue.
- I like a warning when the system load is high
- I like a notice when I'm in `tmux`, `screen`, or `typescript`
  (for those thinking "`tmux` and `screen` always have status
  lines," ... that's actually incorrect, it's "usually" not "always")
- I've been experimenting with what I call "ephemeral" data provided by the
  prompt: this is less important but still useful information (which often
  varies by machine, depending on what it's used for) that's printed AFTER
  the `$`-or-`#`.

If you're counting, you may notice my prompt is already fairly long.  I'm
suggesting something along the lines of
`user@host:~/.bashprompt:master+~[3]$` (where "3" is your return value, and
"master+~" is a git branch and status.  That's 36 characters, and we're
half way across an 80 character terminal without even changing into a deeply
buried directory.  For this reason, I mostly use two line prompts - I
know that doesn't suit some people, but hey, it's my call.  (And again: a
prompt that takes up a lot of width is easier to spot in scroll-back).  The
top line should have `user@host:<dir> <VCS-status>`, the second line should
remain very short: `<battery-percent><notifications><$-or-#>`.

I've revised most of my notifications down to a single character, for
example if load is below 50% it doesn't show at all.  Above 50% but below
100% I get a caret on a yellow background.  Above 100%, the caret is on a
red background.  That area is also where I indicate if you're in
screen/tmux.  I use "[s]" or "[t]" for that - reducing it to a single
specialty character isn't off the table though.

Another useful (again, depending on the machine) notification is disk
usage.  You can set the levels, but there are good speciality characters
for this: "○", "◐", and "●".  On my systems, they change at 70% (from empty
circle to half full), and 90% (from half to entirely full) and each is
printed in a matching colour of progressively green, yellow, red.  If you
have several disks or partitions attached, this can be long even though
it's a single character as it prints one for each disk, and it's very
likely to be garish - it's not for everyone.

Another way to display that same information (I hinted at the idea earlier)
is to print the stats after the `$`.  For example:
`$       [8] 0.56,0.46,0.46  (69%)(75%)`.
The text after the dollar sign is usually printed in dark gray (on a black
background).  In this case it shows "[number-of-cores] <1,5,15 load> (disk0
usage)(disk1 usage)".  The user's cursor appears just after the dollar sign
- and yes, that means you type over top of that "ephemeral" information.
Again, probably not for everybody.

