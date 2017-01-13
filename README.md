# virtualenv quickstart

## install

    sudo pip install pip --upgrade
    sudo pip install virtualenvwrapper

open `~/.bashrc` and add

    if [ -f  /usr/local/bin/virtualenvwrapper.sh ]; then
        source /usr/local/bin/virtualenvwrapper.sh
    else
        echo "Could not find file /usr/local/bin/virtualenvwrapper.sh. Install virtualenvwrapper, or fix path."
    fi

Restart your terminal.

You're done!

## example usage

    mkvirtualenv proj1
    pip install pkg-a
    mkvirtualenv proj2
    pip install pkg-b
    workon proj1
    rmvirtualenv proj2
    deactivate

## other options

### auto-restore last used virtual env in new terminals

This is my preferred workflow.

Place in your `~/.bashrc` file:

    if [ -f  /usr/local/bin/virtualenvwrapper.sh ]; then
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
    fi
