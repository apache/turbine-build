# [Apache T U R B I N E](https://turbine.apache.org/)

    
## T U R B I N E  S I T E  S T R U C T U R E  AND P U B L I S H I N G

Assumptions:
- asf-site branch exists for each GIT repo.
- Base Git-Repo: https://gitbox.apache.org/repos/asf/

The table shows the subfolder assignments (in .asf.yaml) from subdir to the web site:

turbine.apache.org 

| URL context        |  GIT REPO           | .asf.yaml  subdir | Explanation |
| ------------- |:-------------:| -----:|----:|
| /     | turbine--site.git  |  .asf.yaml has no subdir set  | the site content is published to the root |
| /turbine/development/turbine-5-1      | turbine-core.git      |    subdir: turbine/development/turbine-5-1 | development, usually a SNAPSHOT version  |
| /turbine/turbine-2-3-3      | turbine-core.git      |   subdir: turbine/turbine-2-3-3 |  previous release 2.3.3, (no dots allowed in subdir!) |
| /turbine/turbine-5-0      | turbine-core.git      |   subdir: turbine/turbine-5.0 |  previous release 5.0, (no dots allowed in subdir!) |
| /fulcrum/     | turbine-fulcrum-site.git     |  subdir: fulcrum | wrapper for context path fulcrum/  |
| /fulcrum/fulcrum-intake/    | turbine-fulcrum-intake.git     |  subdir: fulcrum/fulcrum-intake |  Fulcrum component intake |
| /fulcrum/fulcrum-json/     | turbine-fulcrum-site.git     |  subdir: fulcrum/fulcrum-json | Fulcrum component json*  |
| /fulcrum/fulcrum-&lt;COMPONENT&gt;/     | turbine-fulcrum-&lt;COMPONENT&gt;.git     |  subdir: fulcrum/fulcrum-&lt;COMPONENT&gt; | Fulcrum component |

This mechanism is possible, because once a subdir (including the root) is published, publishing this and any nested subdir is performed as an git update, not as a clean clone.

TODO: Save previous versions of Fulcrums in the Turbine /turbine/turbine-<VERSION>?

* A multi modules component requires building the site with mvn site:stage.

The only turbine component with variable subdir/contexts is *turbine-core*. 

### Details about Turbine Core Subdir / Release Mapping

You have to edit .asf.yaml in branch .asf-site after a release. 

Change in the tagged release

    git checkout turbine-5.1
    
and generate the site. Save it into an external tmp-folder.
    
    git checkout asf-site
    
Cleanup

    git ls-files | grep -v "^\\." | xargs  rm -f
    
and copy the content from the tmp-folder to the root.
    
 and change

    subdir: turbine/development/turbine-5-1

to
    subdir: turbine/turbine-5-1
    
Prepare commit and push

        git add -A
        git commit -am "New site .." 
        git push
        
This will put the content into a new node turbine/turbine-5-1.
    
After the new site is deployed, change subdir to the next version

    subdir: turbine/development/turbine-5-2

After this Jenkinsfile could be used to publish the site in [Apache Build](https://builds.apache.org/).
(WORK IN PROGRESS this has to tested more, but seems to be working).


## License

Apache Turbine Components are distributed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).
