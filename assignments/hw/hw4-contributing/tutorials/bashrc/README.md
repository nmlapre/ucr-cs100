Customizing .bashrc for Fun and Profit
=====================

<h2>Introduction</h2>

<code>.bashrc</code> refers to the file that allows customization of [Bash](http://en.wikipedia.org/wiki/Bash_%28Unix_shell%29), located at <code>~/.bashrc</code>. Bash, as you may know, is a powerful tool. Customizing <code>.bashrc</code> will allow you to be a more efficient programmer in the Unix environment, since you can easily create and modify your own custom commands, variables, and functions to your taste.

As it is important for a carpenter or blacksmith or accountant or meat butcher to know and, importantly, be comfortable with, her tools, so is it important for a hacker to know hers. This guide will explore the path and the goal in customizing a bash profile via the bash configuration file, located at <code>~/.bashrc</code>, in order to create not just a workable, but enjoyable programming environment.

<h2>Preliminary Information</h2>

Before we delve into the possibilities, we should go over some basics of what we can user in `.bashrc` to create the file that we want. <code>~/.bashrc</code> is written in the bash syntax. This lanugage is very powerful, and is full of simple tools that will allow you to express complex commands succinctly. In the language, we find there are some constructs that will be useful to you in understanding and, more importantly, creating your own useful tools in Bash. Note that the `#` characters denotes the comment symbol. Everything after a `#` will be ignored when entered as a command. They are used in the

    okra='wogmalumpa'         # define a delicious variable
    echo ${okra}              # prints "wogmalumpa" to stdout, $okra also valid

In the terminal, once <code>okra</code> is defined in <code>.bashrc</code>, one can check its value by typing the following into the terminal (the <code>$</code> denotes the prompt, don't type that).

    $ echo ${okra}              # prints "wogmalumpa"

Variables will be useful when we wish to work within our <code>.bashrc</code> file, as well as in the terminal itself. Variable substitution will always be of the form <code>#{var}</code>. There are many more syntactical options in Bash, which are covered quickly and succinctly [here](http://www.cheat-sheets.org/saved-copy/shellcheatsheet.pdf).

Variables in <code>.bashrc</code> are also important since some are predefined to interact with your shell. For instance, <code>echo $HOME</code> in the terminal will show you what the tilde is interpreted to - that is - your home directory. To make Bash always feel like <code>$HOME</code>, you can simply change the value of the environment variable on the command line, in this form:

    $ HOME=/your/preffered/path
    $ echo $HOME                    # prints /your/preferred/path
    $ cd ~                          # takes you to /your/preffered/path

Note that this lasts only for the duration of the shell.
To always go to your home directory on Bash startup, add the following code:

    take_me_home() { cd /your/preferred/path; }
    take_me_home

On startup, Bash will take you directly to the directory you specify in <code>take_me_home()</code>. However, home is not the only useful variable.

<h4>Particularly: </h4>

$PS1 is a Bash variable that contains the string that will be printed as the prompt each time Bash awaits a command from the user. It can be customized with some certain escape characters. They are decoded in a special way. Here, we see the use of <code>\u</code>, <code>\h</code>, and <code>\w</code>.

    me@computer:/home/ $ # here, PS1 = "\u@\h:\w", that is, user@hostname:current/working/directory  
    me@computer:/home/ $ PS1="\u $ "
    me $  # The prompt prints even less information.
    me $ PS1="$ "
    $   # a very minimal example
    
    
Also useful may be:

Escape Sequence | Meaning 
--- | --- 
`\n` | newline 
`\T` | current time (12hr format) 
`\#` | command number of this command 
`\\` | a backslash! 

<h2>Alias</h2>

After some experience with the shell, you might find yourself tiring of typing cumbersome command strings over and over. Aliases will save you from this pain. In <code>.bashrc</code>, consider adding lines such as these:

    alias ll='ls -alF'                          # long-listing format for ls
    
    alias c='clear'                             # useful, common
    
    alias ..='cd ..'
    alias ...='cd ../../../'
    alias ....='cd ../../../../'                # and so on
    alias meminfof='free -mlt'                  # memory information
    
    # print subdirectories in a tree visualization
    alias tree="ls -R | grep ":$" | sed -e 's/:$//' -e 's/[^-][^\/]*\//--/g' -e 's/^/   /' -e 's/-/|/'"
    
    # create a random password
    alias genpasswd="strings /dev/urandom | grep -o '[[:alnum:]]' | head -n 30 | tr -d '\n'; echo"

...and so on. It is certain that you will, on your journey, find yourself wishing for efficiency. Heed that urge with alias.

<h3>Case Study: Color</h3>

Bash has color features based on [ANSI escape codes](http://en.wikipedia.org/wiki/ANSI_escape_code). This is occasionally extra credit, but may more imporantly be used to expand your zen. For instance, these color codes, added to your <code>~/.bashrc</code> file, will allow you to select colors using variable substitution, as discussed earlier.

    # Normal Colors
    Black='\e[0;30m'        # Black
    Red='\e[0;31m'          # Red
    Green='\e[0;32m'        # Green
    Yellow='\e[0;33m'       # Yellow
    Blue='\e[0;34m'         # Blue
    Purple='\e[0;35m'       # Purple
    Cyan='\e[0;36m'         # Cyan
    White='\e[0;37m'        # White
    
    # Bold
    BBlack='\e[1;30m'       # Black
    BRed='\e[1;31m'         # Red
    BGreen='\e[1;32m'       # Green
    BYellow='\e[1;33m'      # Yellow
    BBlue='\e[1;34m'        # Blue
    BPurple='\e[1;35m'      # Purple
    BCyan='\e[1;36m'        # Cyan
    BWhite='\e[1;37m'       # White
    
    # Background
    On_Black='\e[40m'       # Black
    On_Red='\e[41m'         # Red
    On_Green='\e[42m'       # Green
    On_Yellow='\e[43m'      # Yellow
    On_Blue='\e[44m'        # Blue
    On_Purple='\e[45m'      # Purple
    On_Cyan='\e[46m'        # Cyan
    On_White='\e[47m'       # White
    
    NC="\e[m"               # Color Reset
    
Using these, we may more easily use colors, without having to think in terms of confusing brackets and numbers. Consider, for example, constructing this function, which reminds you, in haiku form, of your frailty:

    function _exit()
    {
        echo -e "${Blue}a crash reduces\nyour ${Red}${On_White}expensive${NC}${Blue} computer\ntosimple ${Black}stone${NC}"
    }
    trap _exit EXIT

This function, since we catch it with <code>trap</code>, will be called when we exit bash. To experience this haiku, consider this:

    $ /bin/bash
    $ exit              # enlightenment awaits
    
These colors can be similarly used on the command line, or in any scripts you may write.

<h2>Git Branch Prompt Information</h2>

stay tuned...
