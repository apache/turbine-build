# [Apache T U R B I N E](https://turbine.apache.org/)

Turbine Build is a collection of the main components provided as modules to build and deploy.

Find the similar Git modules for Fulcrum Turbine Fulcrum Build on GitHub [here][https://github.com/apache/turbine-fulcrum-build).

- Find the [Git Submodule Doc](https://git-scm.com/docs/git-submodule).

## G I T  S U B M O D U L E S

Show all changes for submodules (assuming your default branch is "master")

   git submodule foreach "git checkout master || :"

   (core has default branch trunk):
   
   git submodule update core
   
   git submodule foreach "git diff"  > all.diff.patch

Commit all changes for submodules with default message

    git submodule foreach "git commit -am 'xxxxxx' || :"
    
Check and commit with message all files matching */site.xml

    git submodule foreach "git diff */site.xml"  > site.diff.patch
    git submodule foreach "git commit -m 'xxxxxx' */site.xml || :"
    
Adapt Commit Message for each submodule:

    git submodule foreach "git commit --amend || :"
    
    git submodule foreach "git push || :"
    
Check, that all is committed (no diffs anymore):

    git submodule foreach "git diff"
    Entering 'archetypes'
    Entering 'core'
    Entering 'parent'
    Entering 'site'
    
### Submodule fulcrum 

This is a Git module hierarchy in itself, which requires to fetch it the same way as the parent  

    cd fulcrum   
    git submodule update --init --remote 
    
Then you could just use the same commands like

    git submodule foreach "git diff"

## License

Apache Turbine Components are distributed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
