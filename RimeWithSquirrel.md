> Build instructions for Squirrel.

## Build Squirrel from Scratch

0.  You should already have installed cmake, git and Xcode with Command Line Tools.

1.  Install dependencies with [Homebrew](http://mxcl.github.com/homebrew/).

        brew install boost

2.  Checkout the code.

        git clone git@github.com:rime/squirrel.git
        # for brise & librime
        cd squirrel
        git submodule update --init

3.  Build dependencies.

    Build librime's dependencies:

        make -C librime -f Makefile.xcode thirdparty

    Note: you can also `brew install` all the dependent libraries instead of the above.

    Then build librime and data files that Squirrel depends on:

        make deps

3.  Build Squirrel.

        make
        # or:
        #make debug

## Test it on your Mac

    sudo make install
    # or:
    #sudo make install-debug

That's it. Thanks for reading.