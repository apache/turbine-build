# [Apache T U R B I N E](https://turbine.apache.org/)

Turbine Build is a collection of the main components provided as modules to build and deploy.

Find the similar Git modules for Fulcrum Turbine Fulcrum Build on GitHub [here][https://github.com/apache/turbine-fulcrum-build).

- Find the [Git Submodule Doc](https://git-scm.com/docs/git-submodule).

## G I T  S U B M O D U L E S


### Checking out 

You could use git to checkout current trunk:

     git clone https://gitbox.apache.org/repos/asf/turbine-build.git 
     
N.B. The submodules are included with a relative URL. If you are cloning with https + Github  and you want later psuh you have to switsch either to ush) or the SSH-URL or fetching from gitbox (recommended).

If you want later update the remote URLS you could use sync command to achieve this:

    git submodule sync

After cloning this repo (turbine-build) fetch the sub repos:

    git submodule update --init --remote 

Update the submodule to master (assuming default branch is "master")

   git submodule foreach "git checkout master || :"

 Turbine-core has default branch trunk.
 
   cd core
   git checkout trunk
   
#### More Examples
   
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

    git submodule foreach "git pull origin master"
    
    git submodule foreach "git checkout master"
    
    git submodule foreach "git pull"

    git submodule foreach "git diff"
    
### Other commands

Use Maven commands with git submodule, e.g. 

    git submodule foreach "mvn versions:display-property-updates || :"
    
    git submodule foreach "mvn versions:display-dependency-updates"
    

## License

Apache Turbine Components are distributed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
