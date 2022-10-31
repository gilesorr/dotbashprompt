# Docker Image for dotbashprompt Project

This is very much a work-in-progress.  As I write (2022-10-30), it's
functioning, but the image isn't published anywhere (and may never be).

The idea behind creating a Docker image with the dotbashprompt project
inside it is two-fold: 1) create an isolated test environment for myself
free from my standard scripts (which sometimes share names with functions
inside the project!), and 2) create an image that will allow people who
want to test the project and its prompts without having to install it on
their own system.

Run with something like:
  docker container run -it --hostname dockerDBP --rm gilesorr/dotbashprompt:latest

If you want to build it yourself:
  docker image build -t dotbashprompt .
OR (if you're me):
  docker image build -t gilesorr/dotbashprompt:0.15 .
Caching routinely ignores pulling git:
  docker image build --build-arg CACHEBUST="$(date +%s)" -t gilesorr/dotbashprompt:0.25 .
Usually (again, if you're me) after a build you'll want to tag it as latest:
  docker tag gilesorr/dotbashprompt:0.29 gilesorr/dotbashprompt:latest

