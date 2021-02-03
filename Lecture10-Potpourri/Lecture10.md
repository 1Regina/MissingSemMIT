1. The lecture covers the topics below but not seems pertinent. For full details, turn to the [lecture](https://missing.csail.mit.edu/2020/potpourri/)
    * Keyboard remapping
    * Daemons
    * FUSE
    * Backups -- see last year's [lecture](https://missing.csail.mit.edu/2019/backups/)
    * APIs -- to get data on the website
    * [Common command-line flags/patterns](#Common-command-line-flags/patterns)
    * Window managers -- to tile or position and size the window without using the mouse
    * VPNs -- not necessary if everything I am doing is already encrypted thru HTTPS or TLS since VPN servers can also be vulnerable
    * Markdown -- already using. Google for markdown cheatsheet anytime if required.
    * [Hammerspoon (desktop automation on macOS)](#Hammerspoon-(desktop-automation-on-macOS))
    * Booting + Live USBs --a live USB to recover data or fix the operating system if the existing OS installation is broken.
    * [Docker, Vagrant, VMs, Cloud, OpenStack](#Docker,-Vagrant,-VMs,-Cloud,-OpenStack)
    * Notebook programming -- programming environment e.g [Jupyter](https://jupyter.org/) for Python and [Wolfram Mathematica](https://www.wolfram.com/mathematica/) for math-oriented programming.
    * [GitHub](#GitHub) 
  
2. ### Common command-line flags/patterns
   1. ```--help``` for brief info for the tool
   2. "dry run" for a rehearsal demo if the process is performed without actuall doing it
   3. ``` --version``` or ```-V``` to show version
   4. ``` --verbose``` or ```-v``` flag or  multiple times (```-vvv```) to get more verbose output, which can be handy for debugging.  ```--quiet``` flag for making it only print something on error.
   
3. ### Hammerspoon (desktop automation on macOS)
   1. allow for mute speaker when you enter lab (bcos wifi network detection) or warning alert for mistaking a friend power supply. Configuration files available but instructors discouraged plucking wholesale. Instead look through what is required.
   2. [Getting Started with Hammerspoon](https://www.hammerspoon.org/go/)
   3. [Sample configurations](https://github.com/Hammerspoon/hammerspoon/wiki/Sample-Configurations)
   4. [Anish’s Hammerspoon config](https://github.com/anishathalye/dotfiles-local/tree/mac/hammerspoon)
   
4. ### Docker, Vagrant, VMs, Cloud, OpenStack
   1. [Virtual Machine](https://en.wikipedia.org/wiki/Virtual_machine) - useful for creating an isolated environment for testing, development, or exploration (e.g. running potentially malicious code).
   2. Cloud Benefits
      1. A cheap always-on machine that has a public IP address, used to host services
      2. A machine with a lot of CPU, disk, RAM, and/or GPU
      3. Many more machines than you physically have access to (billing is often by the second, so if you want a lot of computing for a short amount of time, it’s feasible to rent 1000 computers for a couple of minutes)
      4. Popular services include [Amazon AWS](https://aws.amazon.com/), [Google Cloud](https://cloud.google.com/), [Microsoft Azure](https://azure.microsoft.com/en-us/), [DigitalOcean](https://www.digitalocean.com/).
   
5.  ### GitHub
    1.  One of the most popular platforms for open-source software development.
    2.  2 ways to contribute:
        1.  Creating an [issue](https://docs.github.com/en/github/managing-your-work-on-github/creating-an-issue). This can be used to report bugs or request a new feature.
        2.  Contribute code with [pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)
        3.  You can fork (ie create a copy of) a repository on GitHub, aka clone your fork, create a new branch, make some changes (e.g. fix a bug or implement a feature), push the branch, and then create a [pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request). After generally some back-and-forth with the project maintainers, who for feedback/query on your patch. Finally, if relevant your patch will be merged into the upstream repository. Often times, larger projects will have a contributing guide, tag beginner-friendly issues, and some, mentorship programs to help first-time contributors become familiar with the project.