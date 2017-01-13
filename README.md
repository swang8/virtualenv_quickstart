# virtualenv quickstart

## install

    sudo pip install pip --upgrade
    sudo pip install virtualenvwrapper
    # one time installation
    source /usr/local/bin/virtualenvwrapper.sh (if not at that path, search for virtualenvwrapper.sh)


## use

    mkvirtualenv myproject
    pip install something
    mkvirtualenv myotherproject
    pip install something different
    deactivate

## other options

### auto-restore last used virtual env in new terminals windows

This is my preferred workflow.

Place in your ~/.bashrc file:

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