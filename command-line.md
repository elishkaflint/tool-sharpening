## Command Line

### Folder navigation

Find a file in a directory:<br>
`$ find . -name testfile.txt`

### Installing dependencies

brew install => Adds binaries to /usr/local/bin

### JSON

Use `jq` to process json from the command line

### History

Get history with timestamp, in zsh:<br>
`$ history -f`<br>
(for bash,  you set a specific environment variable HISTIMEFORMAT)

### Environment Variables

Display all environment variables<br>
`$ env`

Display a specific environment variable<br>
`$ echo $VARIABLE_NAME`

Set an environment variable (in current terminal session only)<br>
`$ export VARIABLE_NAME=variable`<br>
To persist across sessions, add to zsh profile via .zshrc and run source .zshrc

Remove an environment variable (in current terminal session only)<br>
`$ unset VARIABLE_NAME`<br>
To remove across all sessions, remove from .zshrc and run source .zshrc

### MacOS

In finder type shift + ⌘ commmd + . to make invisible files visible

### Commands

`which kubectl` => /usr/local/bin/kubectl
`ls -la /usr/local/bin/kubectl` => tells you where this command is aliased to, you can override this in .zshrc I think
`lrwxr-xr-x  1 eflint  admin  43  7 Aug 15:21 /usr/local/bin/kubectl -> ../Cellar/kubernetes-cli/1.15.2/bin/kubectl`

### Brew

`brew info kubectl`
Find out which version is running (then used commands above to check I was running the version of kubectl I'd installed via Brew, if I'd downloaded it via a Docker image it might be using another bin programme ie. not in Cellar as above)
