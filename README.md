# virtualenv quickstart

## install

    sudo pip install pip --upgrade
    sudo pip install virtualenvwrapper

open `~/.bashrc` and add

    if [ -f  /usr/local/bin/virtualenvwrapper.sh ]; then
        source /usr/local/bin/virtualenvwrapper.sh
    else
        echo "Could not find file /usr/local/bin/virtualenvwrapper.sh. Install virtualenvwrapper, or fix path."
        echo "run 'find / -name "*virtualenvwrapper.sh" 2> /dev/null' to search for it."
    fi

Restart your terminal.

You're done!

## help finding virtualenvwrapper.sh
if you can't find the installation script, you can run

	find / -name "*virtualenvwrapper.sh" 2> /dev/null

Mac users will likely have to do this

## example usage

    mkvirtualenv proj1  # make a new virtual environment to sandbox python packages
    pip install -r requirements.txt  # install all packages in requirements.txt to the proj1 environemnt
    mkvirtualenv proj2  # make another virtual environment and switch to it
    pip install -r other_requirements.txt  # install all packages in requirements.txt to the proj2 environemnt
    pip install some_package  # install some_package to the proj2 virtual environment
    workon proj1  # use packages associated with proj1
    rmvirtualenv proj2  # remove the proj2 virtual environment (and all packages installed in it) 
    deactivate  # use only globally installed packages
    sudo pip install some_package  # globally install this package (not recommended, note 'sudo' is required)

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
