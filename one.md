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
  5. Check your file in your target path
