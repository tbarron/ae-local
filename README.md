This is a collection of shell script functions that help me manage my local
autoenv setup (see the autoenv project at https://github.com/inishchith/autoenv).

In this file, $AELOC represents the directory where these files live. On my
machine, this is $HOME/prj/github/ae-local.

## $ source $AELOC/ae-mgr.sh

Creates function ae-mgr, which is required for the following commands to work.

## $ ae-mgr install

This command will create the following links if they don't already exist:

    $HOME/bin/autoenv_funcs -> $AELOC/autoenv_funcs
    $HOME/bin/ae-local -> $AELOC/ae-local
    $HOME/.nv/proc.d/zz.autoenv -> $AELOC/autoenv_rc

## $ ae-mgr list

List all the .env files in $HOME/.autoenv_authorized

## $ ae-mgr newenv [-f]

Will create a new .env file in the current directory if one does not already
exist.

With the -f option, rename any existing .env to env.EPOCH and create a new one.

## .env

When we have a project directory where env variables or a virtual environment
should be set up upon entry to the directory, the .env file will contain
something like the following

    #!/bin/zsh

    function ae_enter () {
        export MYVAR="something"
        export VENV="venv.3.5"
    }

    function ae_exit () {
        unset MYVAR
        deactivate
    }

    source ~/bin/ae-local $0

The file ~/bin/ae-local will set up some more commonly used functions and then,
if the current working directory is the same as the parent directory of the .env
file, it will run ae_enter(). It will also create a chpwd() function (which will
run when the current working directory changes) which will call ae_exit(). On
the way out, chpwd() will also remove all the ae-local functions from memory.

## autoenv files

The following files are involved in the autoenv functionality

    $HOME/.autoenv_authorized            # runnable .env files and hashes
    $HOME/.autoenv_authorized.tmp        # previous version of auth file?
    $HOME/.local/bin/activate.sh         # activation script from autoenv distro
    $HOME/bin/autoenv_funcs              # link to $AELOC/autoenv_funcs
    $HOME/bin/ae-local                   # link to $AELOC/ae-local
    $HOME/.nv/proc.d/zz.autoenv          # link to $AELOC/autoenv_rc
