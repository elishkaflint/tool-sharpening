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
