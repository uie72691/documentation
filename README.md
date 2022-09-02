
# Import the Confluence page of cip_bti_conan into documentation

## 1. Overview

Projects use a lot of tools (e.g. compilers) which have no installation setup, i.e. where the installation is only a simple copy function.
For these tools, we need an automated mechansim to deploy them on the CI Platform and the developer PC.
The motivation is that all developers can create Conan packages which are needed in the project by themselves .

Image1



## 2. Quickstart  - read me first!

It's only possible to create a tool package which has no installer.

Talk to team Phoenix and team Elliot how to get around that limitation.

## 3. Use Cases

### As a developer...
#### I want create a Conan package for build tools (e. g. compiler)

The following description explains how to create a Conan package for tools without any installer setup.

### Precondition

  1. The tool should be available in our Artifactory toolchain repository: (c_adas_cip_toolchain_generic_v).
If this not the case, please upload it to the stage repository: [c_adas_customer_toolchain_generic_stage_l](https://eu.artifactory.conti.de/artifactory/c_adas_customer_toolchain_generic_stage_l/)
if it's your file correct uploaded (content complete, correct structure) then you can move it to the productive repository: [c_adas_customer_toolchain_generic_l](https://eu.artifactory.conti.de/artifactory/c_adas_customer_toolchain_generic_l/)
For further Help/support with Artifactory please read the Artifactory user documentation: https://confluence-adas.zone2.agileci.conti.de/display/public/department0034/122.+UD+-+Artifactory#id-122.UDArtifactory-AdvancedUseCases
If you have no Access to these Artifactory repositories or to the GitHub repo [cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan), then please create a [IPUP-Ticket](https://jira.auto.continental.cloud/servicedesk/customer/portal/42/create/137) with the settings from the screenshot:
Image2
Alternative you can also create a CIP "[Request Access](https://jira-adas.zone2.agileci.conti.de/secure/CreateIssueDetails!init.jspa?pid=10200&issuetype=10300&priority=3&summary=User+permissions+change)" Ticket.

  2. Install conan using the python package management (pip install conan==1.34.1    # do not use the latest version e.g. 1.36.0 , because it is not backward compatible completly)
  3. To add a conan repository, go and login to the [Artifactory page](https://eu.artifactory.conti.de/).Click on c_adas_cip_conan_I and then go to "Set Me Up" and follow the first step to add the conan repository Note: Use conan remote name "conti-center".
  image3
  
  4. Delete your conan cacert.pem from your conan directory (c:\Users\<username>\.conan\) and insert the modified cacert.
This is necessary to communicate with the EU Artifactory server.cacert.pem
  5. Link the repository with your user credentials. Go to step 3 and follow the second step from **Set Me Up**.
  6. Optional you can add the conan stage repository: [c_adas_cip_conan_stage_l](https://eu.artifactory.conti.de/ui/repos/tree/General/c_adas_cip_conan_stage_l), it's useful to test your Conan package. Follow the steps from point 3 and 5.
  7. cohRemove the official conan repository from your remote list to avoid errors with that one
	
    ```PowerShell
    conan remote remove conan-center
    ```
  8. Clone the repository: [cip_bti_conan](http://github-am.conti.de/ADAS-3RDPARTY/cip_bti_conan) 

#### Create a Conan package:

1. Before you start creating a Conan package, read [cip_bti_conan/README.md at master · ADAS-3RDPARTY/cip_bti_conan (conti.de)](https://github.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan/blob/master/README.md) for the latest important news about the process.
2. Before you start creating a Conan package, you should know the following information:

    1. Package name
    2. Package version. If you want integrate your Conan package in a Bricks job, then you need a semantic version number (https://semver.org/)
    3. Location from the tool/file which you want put into the Conan package. Possible location are Artifactory (user docu), GitHub, Gerrit or IMS.
    4. How do you want use the Conan package
        1. Integrate the Conan package in the bricks build job (**preferred way**) or
        2. deploy the Conan package content to c:\cip_tools\...  in the Windows image.
        
        |  Variant |  Pro | Contra  |
        | ------------ | ------------ | ------------ |
        | Integrate Conan package in Bricks (build.yml as external package)  | 1.  Conan package is under control from project team<br/> 2. Package changes can be used immediately in the project build|1. Large packages slow down the build process <br/>2. Additional cmake file is needed to find the package in the conan cache via cmake find_package command <br/>3. Not usable with compiler which need a bricks platform file <br/> 4. Package can't installed over den dev_env installation tool, but you can find the tool with the conan search command(see chapter: I want install the conan tool package on my developer pc)<br/>5. Only  a version number with the semantic version structure is supported (it's already planned to change this limitation in bricks) |
        | Integrate Conan package in Windows image  | 1. The content of the Conan package is part of the windows image under c:\cip_tools\<br/>2. No cmake file needed.<br/>3. Usable for compiler which need a bricks platform file<br/>4. Conan package can be installed via the dev_env installation tool  |1. CIP team is also a code owner from the package<br/>2. Approx. 3 days to create a new windows image with a new package   |

3. The path structure must contain

```PowerShell
<tool | project | name>
     README.md
     <version>
           conanfile.py
           verify.py | verify.sh | verify.ps1
           cmake file for find package (optional)
           conf.yml
```
 
4. A README.md with the most important information should be created inside the tool/version directory, so that any interested person can directly see, which constraints this tool has (e.g. operating system) and maybe how additional setup steps are done.
  
5. If the package is intended to be created for Linux through Jenkins, a conf.yml file has to be created within the <version> directory with the following content. In case no conf.yml is specified, the build OS will default to Windows.

    <details>
      <summary>conf.yml Expand source </summary>
   
      ```PowerShell
       build_os:
          - linux
          - windows # omit if windows build isn't required
      ```
     </details>
 
    This configuration file is only required for Jenkins builds. To create for linux locally, the linux profile "conan_profile_linux.ini" has to be specified via the --profile flag.
  
6. Copy the checksum from Artifactory from your tool
Go to the Artifactory cip toolchain repository: [c_adas_cip_toolchain_generic_v](https://eu.artifactory.conti.de/ui/repos/tree/General/c_adas_cip_toolchain_generic_v) click on your tool and copy the checksum (see example).
Paste your checksum then in your conanfile.py (in this example it's line 39)
  Image 4
  
7. Example of conanfile.py
    > **Warning**: This example is outdated! Please use cip_bti_conan/conanfile.py at master · ADAS-3RDPARTY/cip_bti_conan (conti.de) as a template!
  
    TODO Update this example!  
    Do not ever say self.info.header_only() in a conanfile.py!!!  
    code block conanfile.py

8. Example of verify.py
    > **Warning**: This example is outdated! Please use cip_bti_conan/verify.py at master · ADAS-3RDPARTY/cip_bti_conan (conti.de) as a template!
  
    TODO Update this example!  
    code block verify.py

9. Prepare Conan Virtual Environment tests

    Example for Linux: [cip_bti_conan/verify.sh at master · ADAS-3RDPARTY/cip_bti_conan (conti.de)](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan)  
    Example for Windows: [cip_bti_conan/verify.ps1 at master · ADAS-3RDPARTY/cip_bti_conan (conti.de)](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan)

10. Set the environment variables **ARTIFACTORY_USER** (value your username) and **ARTIFACTORY_API_KEY** [value is your Artifactory API-Key](https://eu.artifactory.conti.de/ui/packages#/profile) , these are needed to create the package locally.
  
11. To create the package locally, and execute all tests that Jenkins would, run the command "powershell .\build.ps1 --name-and-version .\<package name>\<version>" under Windows and "./build.sh --name-and-version <package name>/<version>" under Linux.
  
12. If the package creation was successfully you can test it with your local package with: "conan install <package name>/<version>@3rdparty/releases"
  
13. Push the change to the GitHub [cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan) repository. The Jenkins server will create the Conan package and upload it to the Artifactory Repository: [c_adas_cip_conan_stage_l](https://eu.artifactory.conti.de/ui/repos/tree/General/c_adas_cip_conan_stage_l) if you merge your PR then jenkins will upload your package to the productive conan repository: [c_adas_cip_conan_l](https://eu.artifactory.conti.de/artifactory/c_adas_cip_conan_I/)
  
  
### I want test my Conan package from the "cip_bti_conan" branch (Staging-Area)
#### Without Bricks job
  
  Follow point 6 in the preconditions and add the "**c_adas_cip_conan_stage_l**" conan repository to your conan remote list.  
Now you can install your test package with "conan install <package name> -r **conti-center-stage**".
It's a good way to test the deploy step from the Conan package if available or to check the content in the conan cache (C:\users\<uid>\.conan\data). 

### I want to create Conan packages for a tool that has several versions

A tool can have several versions. In this case the conanfile would be almost the same for all versions. To reduce redundancy, a base package can be created that is used by all versions.

#### Example: T32 packages
Base package:
  > **Warning**: This example is outdated! Please use README.md cip_bti_conan for instructions!
  
  TODO Update this example!
  codeblock conanfile.py
  codeblock conanfile.txt
    
Tool packages:
  > **Warning**: This example is outdated! Please use README.md cip_bti_conan for instructions!

  TODO Update this example!
  codeblock conanfile.py
  codeblock conanfile.txt

Folder structure:
  
  <details>
  <summary>Folder structure Expand source</summary>
  
  ### Folder structure
  ```PowerShell
  cip_bti_conan\T32_basepkg\0.1\conanfile.py
  cip_bti_conan\T32_basepkg\0.1\conanfile.txt
  cip_bti_conan\T32\T32_R_2019_02_000108303\conanfile.py
  cip_bti_conan\T32\T32_R_2019_02_000108303\conanfile.txt
  ```
  </details>



Folder structure explanation:
  1. basepkg folder
      1. Your basepkg folder has to start with the same name as your package name which should have multiple versions in the future.
      2. This name you extend with "_basepkg".
      3. Your base package also has a version, therefore needs the subdirectory for the version.

  2. Package with multiple versions
      1. Your package is using (python_requires, etc.) the base package (see above)
      2. The folder has the package name WITHOUT "_basepkg"
      3. Subfolder with the version, which has to match the version written in the conanfile.py
    
### I want to create Conan packages (e.g. for MTS) with the content from IMS

If you take below example to create a Conan package for MTS content you should only change the conan name and version and also the IMS label (line 24) and the IMS project path (line 27). 
If you have a bundle package, please use below the second example.

Tipp:

If you want test your Conan package locally please remove the credentials parameter from the integrity client call "–user=uidt812z", and "--password=" + pwd,

> **Warning**:This example is outdated! Please use [README.md cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan/blob/master/README.md) for instructions!

TODO Update this example! 

conanfile.py Expand source


Below a example for a MTS component.

To change the MTS component, please change in line 89 the entry for "@CompName"

> **Warning**: This example is outdated! Please use [README.md cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan/blob/master/README.md) for instructions!

TODO Update this example!

conanfile.py Expand source
  
  
### I want to create Conan packages with the content from GitHub

Below a example to use GitHub as source for the Conan package:

> **Warning**: This example is outdated! Please use [README.md cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan/blob/master/README.md) for instructions!

TODO Update this example!

conanfile.py Expand source

  
### I want to create Conan packages with the content from Gerrit

Below a example to use Gerrit as source for the Conan package:

> **Warning**: This example is outdated! Please use [README.md cip_bti_conan](https://github-am.geo.conti.de/ADAS-3RDPARTY/cip_bti_conan/blob/master/README.md) for instructions!

TODO Update this example!

conanfile.py Expand source
  
### I want install the conan tool package on my developer pc

1. Search the available packages with: conan search <package name> -r <remote>.

    Example:
    ```PowerShell
    conan search diab* -r conti-center
    ```
    For more information show in the [official documentation](https://docs.conan.io/en/latest/reference/commands/consumer/search.html)

2. To install a Conan package run: "conan install <package name> -r <remote>".

    Example: 
    ```PowerShell
    conan install diab_arm/7.0.1.0-p1@3rdparty/releases -r conti-center
    ```

### I want integrate a 3rdparty Conan package in a bricks build job

see [bricks documentation](https://readthedocs.cmo.conti.de/docs/cip-bricks-user-doc/en/latest/use_cases/developer/intermodule/dev_pkg_use_external.html)

> **Note**: This will only add your tool to the build process.  You will actually call it in your cmake build.


### Known issues or Conan limitations

  - Missing some files in the Conan package, see [conan ticket](https://github.com/conan-io/conan/issues/7789)

### ToDo

  - Integrate the conan tool installation in the Bricks Build System as prerequisite, to remove the manually installation steps.

### Point of contact

  - [Service Request ticket](https://jira-adas.zone2.agileci.conti.de/secure/CreateIssueDetails!init.jspa?pid=10200&issuetype=11000&priority=3)

#### Trouble Shooting / FAQ


#### Official documentation
  
  - [Documentation](https://docs.conan.io/en/latest/devtools/create_installer_packages.html)

#### Additional Support Contact
  
  - [Service Desk](https://jira-adas.zone2.agileci.conti.de/secure/CreateIssueDetails!init.jspa?pid=10200&issuetype=11000&priority=3)

### Topics for a Training Session
 
  1. Always copy a the most recent version of your tool or at least a good example from REAMDE.md as a starting point. Beware, the most recent version of your tool will not always be sorted alphabetcally at the bottom of the list. For different files (conanfile.py, verify.py, verify.sh, verify.ps1), you might want to copy from different sources depending on your use case.

  2. If you find yourself rewriting your conanfile.py, verify.py, verify.sh or verify.ps1 then you have picked the wrong examples. Stop immediately. You are creating more work for yourself and for your reviewers who have to understan yet another way of doing the same thing.

  3. Possible variable names
      - source_version: Version of the archive => Often reflected in the self.source_file and self.source_root
      - self.source_file: File name of the archive (No path!!!)
      - self.source_root: Name of the top-level directory of the archive when it extracts.
      - self.artifactory_path: Path in Artifactory from where the archive can be downloaded: Must me a virtual repository! Cannot be a stage repository. Cannot be Conan repository.
      - self.sha256: Checksum of the archive.
 
  4. TODO: Explain the difference between archives with a top-level directory and those without.

  5. TODO: Explain all possible differences in conanfile.py, verify.ps, verify.sh, verify.ps1

  6. TODO: Explain all possible variable names

  7. TODO: Explain also all needed process actions by Conti process: e.g. security clearance , Tool owner, Licenese check, FOSS, export regulations.....

