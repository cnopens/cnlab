#nodejs-install-config.md

## nvm install 

If you have git installed (requires git v1.7.10+):

    clone this repo in the root of your user profile

    cd ~/ from anywhere then git clone https://github.com/nvm-sh/nvm.git .nvm

    cd ~/.nvm and check out the latest version with git checkout v0.35.3
    activate nvm by sourcing it from your shell: . nvm.sh

Now add these lines to your ~/.bashrc, ~/.profile, or ~/.zshrc file to have it automatically sourced upon login: (you may have to add to more than one of the above files)

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

## nvm usage

---

To download, compile, and install the latest release of node, do this:

nvm install node # "node" is an alias for the latest version

To install a specific version of node:

nvm install 6.14.4 # or 10.10.0, 8.9.1, etc

The first version installed becomes the default. New shells will start with the default version of node (e.g., nvm alias default).

You can list available versions using ls-remote:

nvm ls-remote

And then in any new shell just use the installed version:

nvm use node

Or you can just run it:

nvm run node --version

Or, you can run any arbitrary command in a subshell with the desired version of node:

nvm exec 4.2 node --version

You can also get the path to the executable to where it was installed:

nvm which 5.0

In place of a version pointer like "0.10" or "5.0" or "4.2.1", you can use the following special default aliases with nvm install, nvm use, nvm run, nvm exec, nvm which, etc:

    node: this installs the latest version of node
    iojs: this installs the latest version of io.js
    stable: this alias is deprecated, and only truly applies to node v0.12 and earlier. Currently, this is an alias for node.
    unstable: this alias points to node v0.11 - the last "unstable" node release, since post-1.0, all node versions are stable. (in SemVer, versions communicate breakage, not stability).

Long-term Support

Node has a schedule for long-term support (LTS) You can reference LTS versions in aliases and .nvmrc files with the notation lts/* for the latest LTS, and lts/argon for LTS releases from the "argon" line, for example. In addition, the following commands support LTS arguments:

    nvm install --lts / nvm install --lts=argon / nvm install 'lts/*' / nvm install lts/argon
    nvm uninstall --lts / nvm uninstall --lts=argon / nvm uninstall 'lts/*' / nvm uninstall lts/argon
    nvm use --lts / nvm use --lts=argon / nvm use 'lts/*' / nvm use lts/argon
    nvm exec --lts / nvm exec --lts=argon / nvm exec 'lts/*' / nvm exec lts/argon
    nvm run --lts / nvm run --lts=argon / nvm run 'lts/*' / nvm run lts/argon
    nvm ls-remote --lts / nvm ls-remote --lts=argon nvm ls-remote 'lts/*' / nvm ls-remote lts/argon
    nvm version-remote --lts / nvm version-remote --lts=argon / nvm version-remote 'lts/*' / nvm version-remote lts/argon

Any time your local copy of nvm connects to https://nodejs.org, it will re-create the appropriate local aliases for all available LTS lines. These aliases (stored under $NVM_DIR/alias/lts), are managed by nvm, and you should not modify, remove, or create these files - expect your changes to be undone, and expect meddling with these files to cause bugs that will likely not be supported.
Migrating Global Packages While Installing

If you want to install a new version of Node.js and migrate npm packages from a previous version:

nvm install node --reinstall-packages-from=node

This will first use "nvm version node" to identify the current version you're migrating packages from. Then it resolves the new version to install from the remote server and installs it. Lastly, it runs "nvm reinstall-packages" to reinstall the npm packages from your prior version of Node to the new one.

You can also install and migrate npm packages from specific versions of Node like this:

nvm install 6 --reinstall-packages-from=5
nvm install v4.2 --reinstall-packages-from=iojs

Note that reinstalling packages explicitly does not update the npm version — this is to ensure that npm isn't accidentally upgraded to a broken version for the new node version.

To update npm at the same time add the --latest-npm flag, like this:

nvm install lts/* --reinstall-packages-from=default --latest-npm

or, you can at any time run the following command to get the latest supported npm version on the current node version:

nvm install-latest-npm

If you've already gotten an error to the effect of "npm does not support Node.js", you'll need to (1) revert to a previous node version (nvm ls & nvm use <your latest _working_ version from the ls>, (2) delete the newly created node version (nvm uninstall <your _broken_ version of node from the ls>), then (3) rerun your nvm install with the --latest-npm flag.
Default Global Packages From File While Installing

If you have a list of default packages you want installed every time you install a new version, we support that too -- just add the package names, one per line, to the file $NVM_DIR/default-packages. You can add anything npm would accept as a package argument on the command line.

# $NVM_DIR/default-packages

rimraf
object-inspect@1.0.2
stevemao/left-pad

io.js

If you want to install io.js:

nvm install iojs

If you want to install a new version of io.js and migrate npm packages from a previous version:

nvm install iojs --reinstall-packages-from=iojs

The same guidelines mentioned for migrating npm packages in node are applicable to io.js.
System Version of Node

If you want to use the system-installed version of node, you can use the special default alias "system":

nvm use system
nvm run system --version

Listing Versions

If you want to see what versions are installed:

nvm ls

If you want to see what versions are available to install:

nvm ls-remote

Suppressing colorized output

nvm ls, nvm ls-remote and nvm alias usually produce colorized output. You can disable colors with the --no-colors option (or by setting the environment variable TERM=dumb):

nvm ls --no-colors
TERM=dumb nvm ls

To restore your PATH, you can deactivate it:

nvm deactivate

To set a default Node version to be used in any new shell, use the alias 'default':

nvm alias default node

To use a mirror of the node binaries, set $NVM_NODEJS_ORG_MIRROR:

export NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist
nvm install node

NVM_NODEJS_ORG_MIRROR=https://nodejs.org/dist nvm install 4.2

To use a mirror of the io.js binaries, set $NVM_IOJS_ORG_MIRROR:

export NVM_IOJS_ORG_MIRROR=https://iojs.org/dist
nvm install iojs-v1.0.3

NVM_IOJS_ORG_MIRROR=https://iojs.org/dist nvm install iojs-v1.0.3

nvm use will not, by default, create a "current" symlink. Set $NVM_SYMLINK_CURRENT to "true" to enable this behavior, which is sometimes useful for IDEs. Note that using nvm in multiple shell tabs with this environment variable enabled can cause race conditions.
.nvmrc

You can create a .nvmrc file containing a node version number (or any other string that nvm understands; see nvm --help for details) in the project root directory (or any parent directory). Afterwards, nvm use, nvm install, nvm exec, nvm run, and nvm which will use the version specified in the .nvmrc file if no version is supplied on the command line.

For example, to make nvm default to the latest 5.9 release, the latest LTS version, or the latest node version for the current directory:

$ echo "5.9" > .nvmrc

$ echo "lts/*" > .nvmrc # to default to the latest LTS version

$ echo "node" > .nvmrc # to default to the latest version

Then when you run nvm:

$ nvm use
Found '/path/to/project/.nvmrc' with version <5.9>
Now using node v5.9.1 (npm v3.7.3)

nvm use et. al. will traverse directory structure upwards from the current directory looking for the .nvmrc file. In other words, running nvm use et. al. in any subdirectory of a directory with an .nvmrc will result in that .nvmrc being utilized.

The contents of a .nvmrc file must be the <version> (as described by nvm --help) followed by a newline. No trailing spaces are allowed, and the trailing newline is required.




    安装nvm后可切换node版本，但npm版本还是最新的，npm不变为什么？

    node 如何降级npm 版本?

    npm install npm@3.8.6 -g

    npm install npm@5.8.0 -g

    npm install npm@latest -g


