## Overview
This page explains the Artifactory  service and some basics about usage of Artifactory in general.

Most of the descriptions are not specific for the CIP team or build process but general artifactory descriptions.

The Service is based on the EA IT service of Artifactory. See also :https://confluence-it.zone2.agileci.conti.de/display/department0001/Jfrog+Artifactory

For requesting your own repo. Please check out:https://jira-it.zone2.agileci.conti.de/plugins/servlet/desk/portal/22

## Boundary Conditions
You have a registered to Coniti artifactory and have a valid user account.

### Prerequisite

> **Permissions**
To be able to access build artifacts/Docker images/Conan on Artifactory, please open a new permission request and select the "CI Tools" as the components. Please ask to be added to the "CIP SW-Developer" group to access the eu.artifactory.conti.de service. Add your line manager as the request approver. You may also make bulk requests by adding multiple IDs in the form.


> **permission** If you do not have the above permission you may still be able to log in however you will not be able to see or download the results from SV builds


## Quickstart  - read me first!

### How to get Access to Artifactory is currently described here:

These instructions are meant to integrate user’s artifactory account to Bricks build system.

  1. Use “Request Access” to request artifactory access from ADAS CIP or please create directly a IPUP-Ticket with the settings from the screenshot:(image)
  2. Wait that access is granted
  3. Run Bricks sync command and enter username (defaut: takes system user, if you want use the same user press enter) and password (user has to enter)
  4. Just for verification go to artifactory profile<https://eu.artifactory.conti.de/artifactory/webapp/#/profile>`_

     - User should have some dialog to fill out username and password
     - After logging in to artifactory there should be unlock button visible. Please type your password to input field and click “Unlock”(image)
     - Existing users and users who have requested new API tokenshould have authentication token already(image)

## Use Cases
### As a Developer...
### I want to get artifacts from a jenkins job

Getting the artifacts produced by a jenkins job is usually done by just clicking on the link. However if you only see "ait_ea_ast_chocolatey_a_v" you are possibly not logged in or don't have the required access rights. You need to log in and then click on the link again.
The following description explains how to finde the jenkins job artifacts manually.
  1. You will need 3 pieces of information: The name of the Jenkins master your build was running on (e.g. ads-cip02-zone2), the name of the Jenkins job (e.g. sv-review) and the build number for your job.
  2. Go to https://eu.artifactory.conti.de (for europe/americas) or https://ap.artifactory.conti.de/artifactory/webapp/ (Asia/Pacifc) and login(image)
  3. Click the "artifacts" link on the left navigation menu(image)
  4. Look for the c_adas_cip_generic_l repository or click here (EU) or here (Asia/Pacific). If you are looking for the artifacts for a release build, please look for c_adas_cip_release_generic_l.(image)
  5. The structure of how the artifacts are stores differs between "regular" builds and those that are triggered by relases (git tags)

      
      a. review/master build artifacts: Open the tree view and look for your build. If you are looking for the artifacts for the sv-master build that ran on https://adas-cip02-jenkins.agileci.zone2.conti.de with the build number 2411, you would look at(image)
      
      b. release artifacts: The artifacts are stored according to their release tag. svs3xx/artifactory-test-rc1 corresponds to the git tag (and release)  svs3xx/artifactory-test-rc1(image)
      
6. Right click on the folder that include you artifacts (in this case 2411/bin because there are no other sub folders in 2411) and select the Download link(image)
7. Choose your favorite archive format and press the download link(image)


### I want to create a Conan package
... for the build system 3.x

For Conan packages which shall be compiled  by the build system 3.x the creation and handling of the Conan packages are covered in the build system description please have a look there.



... for other packages

If you have some Binary or some other part which you would like to handle with Conan but its nothing which shall be build /compiled by the build system 3 then you can of course create a standard conan package.

For how to do achieve this please have a look in our 3rdparty conan documentation 124. UD - Conan tools package (3rd party conan packages)

> **Note**: The current Build system 3 release cannot handle such Conan packages out of the box.  We will add this support in future releases  of the build system 3
      
### I want to download a docker image from Artifactory


  1. If this is the first time, you want to download something from a Docker registry on Artifactory,  you will have to authenticate first:

       | docker login c-adas-cip-docker-v.eu.artifactory.conti.de |
       |----------------------------------------------------------|

  2. Download your image using docker pull. e.g.:

       | docker pull c-adas-cip-docker-v.eu.artifactory.conti.de/build/build_system_3.0_qaf:2019.02.22 |
       |-----------------------------------------------------------------------------------------------|

You will have to login again, after you change your password or if you want to access a different docker registry (e.g. eu.artifactory.conti.de:7002)





















## Advanced Use Cases
### Overview Artifactory toolchain repositories

Virtual repositories include other repositories, virtual repos should always be used to download artifacts. The type of repository can be identified by the postfix: If the name ends with v=virtual or l=local.

c_adas_cip_toolchain_generic_stage_v

  - c_adas_cip_toolchain_generic_stage_l
  - c_adas_customer_toolchain_generic_stage_l

c_adas_cip_toolchain_generic_v

  - c_adas_cip_toolchain_generic_eu_l
  - c_adas_customer_toolchain_generic_l


### I want to upload some Tool/ Artifacts....

The upload is restricted in Artifactory.  The way as a developer to upload artifacts is done via a staging process. which will described here.

Each file(s) which you want upload  shall be uploaded first to a staging repository in Artifactory. 

  - Generic project files for test (staging) – [c_adas_customer_toolchain_generic_stage_l](https://eu.artifactory.conti.de/ui/repos/tree/General/c_adas_customer_toolchain_generic_stage_l)
  - Generic project files for productive - [c_adas_customer_toolchain_generic_l](https://eu.artifactory.conti.de/ui/repos/tree/General/c_adas_customer_toolchain_generic_l)


If you have no Access to these Artifactory repositories, then please create a IPUP-Ticket with the settings from the screenshot:

image

Alternative you can also create a CIP "Request Access" Ticket.

After the file is uploaded please check if it's all fine please move it to the productive repository. Files in the productive repository can no longer be deleted.

  1. Click Deploy
   image

  2. Select your "Target Repository"
  3. Select your file which you want upload
  4. Specifiy the "Target Path" or folder structure in the repository ( image)
  5. Check your file in your target path (image)

### I want to move some Tool/ Artifacts from the stage to the productive repository

  1. Right click on the tool folder and click on "Move" ( image)
  2. Select your "Target Repository"
  3.  If needed you can change the new Path if you enable "Move to a Custom Path"(image)
  4.  Check your target path in your target repository(image)

### I want to download some Tool/Artifacts

For the download, please use the virtual Artifactory repositories. The last character from every repository define the type e.g. v = virtual, l = local.
  1. Right click on your file and click on "Download"(image)

### Point of contact
#### Service Owner
Bachmeier, Alexander
The virtual repositories includes other local repositories.
