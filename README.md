# [Apache T U R B I N E](https://turbine.apache.org/)

Turbine Build is a collection of the main components provided as modules to build and deploy.

## U S A G E 

Apache Turbine components are:

- Core: The Turbine Web Framework 
- Parent: The Parent POM for all Turbine/Fulcrum components
- Site: The Turbine Wrapper Site
- Archetypes: The Turbien Archetype Generator Source

You can find more here: [Apache Turbine](https://turbine.apache.org/).

## B U I L D I N G

You must have Maven 3.x

Building the Turbine from GIT is very easy.  Turbine has been
Maven-enabled.  Please refer to the Maven Getting Started document for
instructions on building.  This document is available here:

https://maven.apache.org/guides/getting-started/

### GIT 

Turbine Build does use (Git Submodules)[https://git-scm.com/book/en/v2/Git-Tools-Submodules].

Find more detailed information (here)[README_TECHNICAL_GITMODULES.md].

You could use git to checkout current trunk:

     git clone --recurse-submodules https://github.com/apache/turbine-build.git 
     
If you did a normal clone you have to init the submodules from the root folder with

    git submodule update --init --recursive
    
This will register and clone all submodules in one step.
     
Hint: To limit history e.g. to 10 commits add --depth 10.
     
To update all submodules: 

     git pull --recurse-submodules 
     
or equivalently:

    git submodule update --init --remote 
    
    git submodule sync
    
or:
    git submodule foreach git pull
     
To init or merge and fast-forward a single module:

    git submodule update --init --remote <module>
    
or later
    
    git submodule update --remote --merge <module>
    
or change into submodule

    cd <submodule>
    git checkout master
    git pull
    
After having done this, check with

    git status
    
and add the changed submodules (untracked contents, new commits" :

    git add <submodule>
    git commit "updated submodule <>"
    
## Documentation

Turbine Core documenttion is found [here](https://turbine.apache.org/).

## Requirements

Turbine requires Java 11 since v5.2 and Java 8 (since v4). 
  
### Contributing

Please feel free to contribute. We are always happy to encourage new committers to the project. 

The Apache Software foundation requires that any contributor has signed the ICLA (Individual Contributor License Agreement).

Find an overview, more information and how-tos [here](http://www.apache.org/licenses/contributor-agreements.html#clas).

### Publishing Workflow

#### Prerequisites

Deploy jars
 
    mvn deploy -Papache-release

More Information:
  - [Maven Component Reference](http://maven.apache.org/developers/website/deploy-component-reference-documentation.html)
  - [Apache Release Publishing](https://infra.apache.org/publishing-maven-artifacts.html)
  
##### Steps


1. Local Testing

Verify gpg.homedir, gpg.useagent, gpg.passphrase. Check, if -Dgpg.useagent=false is needed,  see below comment to pinentry.
You may need to add additional profiles, e.g. -Papache-release,java8 or add -Dgpg.passphrase=<xx> 

    
    mvn clean site install -Papache-release 
  

If dependency check is skipped by default, do mvn org.owasp:dependency-check-maven:check -Ddependency.check.skip=false
Since Turbine Parent 8 security check is enabled by default.

    mvn release:prepare -DdryRun=true -Papache-release -Dtag=<project.artifact>-<version>-candidate
    
Here the tag is already set.

And finally:

    mvn release:clean    

2. Remote Testing


As we using GIT as SCM, be sure that you execute the steps in the *master|trunk|main* branch as GIT commands are applied to the current branch.

Find more Information in [Turbine Release Manual](https://cwiki.apache.org/confluence/display/TURBINE/Publishing+a+Release).

If you have not set ssh-key or gpg authentication,  the tasks may require that you explicitely authenticate with -Dusername=<username> -Dpassword=<pw>. 


Important: Success will be on the master build, the others are skipped.


    mvn release:prepare -Papache-release -Dtag=<project.artifact>-<version>-candidate

  
3. Release Preparing

If you get a 401 error on the upload to repository.apache.org, make sure
that your mvn security settings are in place ~/.m2/settings.xml and ~/.m2/settings-security.xml 

For more information on setting up security see the encryption guide:
 - [GUIDE ENCRYPTION](http://maven.apache.org/guides/mini/guide-encryption.html).

This performs an upload to https://repository.apache.org/#stagingRepositories

Hint: Add -Dgpg.useagent=false helps, if running from a windows machine to avoid hanging while gpg plugin signing process 
 .. this may happen, if you do not define the pinentry-program in gpg-agent.conf correctly ..

    mvn release:perform
  
You could find more information here: [Book Reference Staging](http://www.sonatype.com/books/nexus-book/reference/staging.html)
  
4. Close the staging

Login and close in Nexus Repo:

 - [Staging](https://repository.apache.org/index.html#stagingRepositories)
 
 More information available: [CLOSE STAGE](https://www.apache.org/dev/publishing-maven-artifacts.html#close-stage).
 
 Fetch the URL for the tagged Repo from target/checkout with
  
    git pull --prune --tags
    git tag -l 
    git checkout <tag>
    
You will be in detached mode.    
  
5. Prepare Voting Information and Voting
 
 ....
  
6. Either Promote / Publish or Drop and Restage

  - [PROMOTE INFO](http://www.apache.org/dev/publishing-maven-artifacts.html#promote)
  - [DROP INFO](http://www.apache.org/dev/publishing-maven-artifacts.html#drop)
  
6a Promote / Release

- Release staged repository in nexus

- Rename (replace) the GIT tag <xyz>-candidate to <xyz>. 
Find more Information in [Turbine Release Manual](https://cwiki.apache.org/confluence/display/TURBINE/Publishing+a+Release).

- Proceed with next step.

  
6b Revert

- Drop "reverse merge" the release prepare. If backup files (from release:prepare) are still there:

    mvn release:rollback

which will delete the tag in git repo tag and revert to the pre-release state.

Otherwise reset the commit in master in your checked out trunk/master/main branch 

    git reflog
    // find commit previous to release
    git reset –hard <shacommit>
    git push -f origin master

and update master repo and delete the tag manually.

    git push origin :refs/tags/<project.artifact>-<version>-candidate

- Drop staged repository in nexus and start again with step 1.

- Don't forget to refer to the failed vote Message-ID in the commit messages (git, nexus).

After changing, what was missing, refer to the reason in the commit message.

    git commit "Reset RC to state previous RC due to <Message-ID>"


 
7. Distribution 

  - (http://www.apache.org/dev/release#upload-ci),
  - http://www.apache.org/dev/release.html#host-GA and 
  - http://www.apache.org/dev/release-publishing.html#distribution
  
  
  - Update branch asf-site in Turbine component root: Copy files from target/staging to the root of branch asf-site and commit.
  This will trigger (with .asf.yaml configuration) an update to the distribution site.
  
  - SVN checkout target distribution from https://dist.apache.org/repos/dist/release/turbine/<...>/<...>
  - GIT checkout GIT_REPO <foo>-<version>-candidate
    Find more onformation in [Wiki](https://cwiki.apache.org/confluence/display/TURBINE/Publishing+a+Release)
  - Generate artifacts (check local repo and target for artifacts) from released version
 
Checkout the tagged released release and run:
 
    mvn clean install package -Papache-release 

 
Generate checksums with UNIX tool shasum (2022 at least sha512), Windows certutil or other tools and 
copy artifacts and sha-files to the distribution folder dist source/binaries folder.

If not all jars are included (assembly plugin _SHOULD_ run after jar generation), run a second time without clean.
If no sha512 files are in the target folder, check local repo.
      
- SVN Add <binaries>, <sources> artifacts (jar/zip/tar.gz,asc,sha512 files) to target repo.
- SVN Remove old releases binaries and sources 

 After repos/dist is updated an automatic email will be generated, if no update of the release database is done:
  
 - [ADD TO TURBINE RELEASE APACHE](https://reporter.apache.org/addrelease.html?turbine)
   

 8.  Stage the latest documentation 

- [DEPLOY DOCU](http://maven.apache.org/developers/website/deploy-component-reference-documentation.html)

## Git Checkout <tagged release version> source. Generate and Publish Site

Find an overview about the technical structure of the sites (here)[README_SITE_STRUCTURE.md].

### Short description of the process using asf-site branch using GIT commands

Hint: If checking out the branch asf-site you find an .asf.yaml file, where the exact configuration is found, what happens after site generation.

Find more information [here](https://cwiki.apache.org/confluence/display/INFRA/git+-+.asf.yaml+features).

- Generate the site in master branch and optionally save it somewhere


    mvn site


- Save target/site or target/staging into another folder (target may be ignored, but to be sure)

- checkout branch asf-site. Verify proper settings in .asf.yaml providing at least this line:

    whoami: asf-site

- Copy content of saved copy or target/site (single module) to *the root of the branch*

- Commit and push (this triggers the site update, if not contact INFRA)

### ~~Maven Publishing~~

The second steps are not yet tested with GIT repos (2021). Use instead the workflow above (using GIT commands). 

Omit site:stage, which reqires site element definition in distributionManagement

    mvn site scm-publish:publish-scm -Dscmpublish.dryRun=true
    mvn clean site scm-publish:publish-scm -Dusername=<username> -Dpassword=<pw>

### Deployment 
   
To deploy the site execute

    mvn site-deploy

IMPORTANT: You may have to clean up the checkoutDirectory of maven-scm-publish-plugin plugin after doing a dry run!
This directory is configured in turbine-parent by default outside target folder:

    turbine.site.cache: ${user.home}/turbine-sites
    
Check pom.xml configuration of properties.

## License

Apache Turbine Components are distributed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
