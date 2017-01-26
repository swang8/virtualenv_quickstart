# virtualenv/virtualenvwrapper quickstart

## install

    sudo pip install pip --upgrade
    sudo pip install virtualenvwrapper

Find where the shell script was installed to

    find / -name "*virtualenvwrapper.sh" 2> /dev/null

Source that file in your `~/.bashrc` or `~/.bash_profile` (assumed location here was `/usr/local/bin/virtualenvwrapper.sh` but your location may vary):

    source /usr/local/bin/virtualenvwrapper.sh

Restart your terminal.

You're done!

## example usage
You now have the power to sandbox and switch your available python packages with a single command!

    # take a look at your globally installed python packages
    pip freeze

    # make a new virtual environment to sandbox python packages, and immediately start working on it
    mkvirtualenv proj1

    # now look at your python packages available, they should be
    # different from the global packages! This is a big deal!
    # You have just sandboxed your python packages!
    pip freeze

    # install all packages in requirements.txt to the proj1 env
    pip install -r requirements.txt

    # make another virtual environment and switch to it
    mkvirtualenv proj2

    # install all packages in requirements.txt to the proj2 env
    pip install -r other_requirements.txt

    # install some_package to the proj2 virtual environment
    pip install some_package

    # use packages associated with proj1
    workon proj1

    # remove the proj2 virtual environment (and all packages installed in it)
    rmvirtualenv proj2

    # use only globally installed packages
    deactivate

    # globally install this package (not recommended, note 'sudo' is required)
    sudo pip install some_package

## Specify Python binary

    mkvirtualenv proj --python=python3
    mkvirtualenv proj --python=python3.4
    mkvirtualenv proj --python=python3.6
    mkvirtualenv proj --python=python2
    mkvirtualenv proj --python=<some other binary>

## other options

### auto-restore last used virtual env in new terminals

This is my preferred workflow.

Place in your `~/.bashrc` file:

    source /usr/local/bin/virtualenvwrapper.sh

    # Set up hooks to automatically enter last virtual env
    export LAST_VENV_FILE=${WORKON_HOME}/.last_virtual_env
    echo -e "#!/bin/bash\necho \$1 > $LAST_VENV_FILE" > $WORKON_HOME/preactivate
    echo -e "#!/bin/bash\necho '' > $LAST_VENV_FILE" > $WORKON_HOME/predeactivate
    chmod +x $WORKON_HOME/preactivate
    chmod +x $WORKON_HOME/predeactivate
    if [ -f  $LAST_VENV_FILE ]; then
        LAST_VENV=$(tail -n 1 $LAST_VENV_FILE)
        if [ ! -z $LAST_VENV ]; then
            # Automatically re-enter virtual environment
            workon $LAST_VENV
        fi
    fi

## official documentation

[https://media.readthedocs.org/pdf/virtualenvwrapper/latest/virtualenvwrapper.pdf](https://media.readthedocs.org/pdf/virtualenvwrapper/latest/virtualenvwrapper.pdf)
