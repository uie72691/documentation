# Guideline - Check and Fill OSCAR FOSS Compliance Report


## This page contains information about the OSCAR FOSS Compliance Report.

During LCR stage LCR Scan Review a preliminary version of the report (also called LCR Scan Output) is sent to the Project Team. It is expected that the team checks and fills some of the fields of the report. Afterwards, the review can be continued and a final version of the report will be provided (LCR Final Report).

## Table of contents

1. Overview

2. Tab - Summary

3. Tab - Components

  1. Review Output from OSCAR Team
      1. Column - Component Name
      2. Column - License
      2. Column - License Risk
    
  2. Details required from Dev Team/Project Team
      1. Column - Ship status
      2. Column - Usage/Integration Scenario 
      3. Provide the Usage/Integration Scenario for each component:
      4. Column - Modification 
      5. Column - Dev Team Comments

4. Tab - Source

6. Tab - Tools Summary


## Overview


Once the scan has been completed, a (preliminary) report is shared with you. This report contains the following tabs:

  - Summary
  - Components
  - Source
  - Tool Summary

In order to complete the review, we need confirmation from your end for the points below. Please provide feedback as early as possible. Once we receive this input from you, we will proceed with the legal review and provide a final report with license obligations.

  - (warning)Component Name
  - (warning)Component Version
  - (warning)Ship Status
  - (warning)Usage/Integration Scenario
  - (warning)Modification

Please refer to the information 'Tab - Components' for detailed information.

## Tab - Summary

This tab contains general information of the Review.


Image OSCAR_FOSS_ComplianceReport


## Tab - Components

This tab contains the information of components found during the review.

Image Components

## Review Output from OSCAR Team
  - Component Name: (warning) Need confirmation from your end.
  - Component Version: (warning) Need confirmation from your end if "Ship Status = Yes"
  - License
  - License Risk
  - Homepage
  - Component comments


### Column - Component Name

When a component contains different licenses, a sub-component will be listed in the 'Component Name' column as a separate component to refer a different content (Source, Licenses and Obligations).

Image Component_Name

### Component Version

The report will list the component versions detected during the analysis.

"Generic Version" is used in sub-component derived from other component but content is under a different license (dual license or multiple licenses) from the parent component.

Image Component Version

  - Confirm if the identified versions are correct for each component. 
  - (question) For components with "Unknown version", please provide version information.
  - If you have any different feedback please provide more information it in the 'Dev Team Comments' column.

### Column - License

Each component will list one license. When more licenses are present for the same component, a sub-component derived from the parent component will be listed as a separate component to indicate a different licensed content.

Image - License

### Column - License Risk

License Risk will indicate which components will need more attention.

Image License Risk

## Details required from Dev Team/Project Team

  - Ship Status: (warning)(question) Required value from your end.
  - Usage/Integration Scenario: (warning)(question) Required value from your end if "Ship Status = Yes"
  - Modification: (warning)(question) Required value from your end if "Ship Status = Yes"
  - Dev Team Comments


### Column - Ship status

Image Ship status

  - (warning) For each listed component, select 'Yes' or 'No' in the column "Ship Status(Yes/No)", based on whether the component is part of the product delivered (shipped) to the customer or not.
  - (warning) For components with License Risk = HIGH, please double-check before marking as "Ship status = No". If these components contribute to the product in any way there is high risk of license contamination.


### Column - Usage/Integration Scenario

image Usage/Integration Scenario

### Provide the Usage/Integration Scenario for each component:
Here, the project team needs to verify how the listed components in the initial reports are linking/integrated with Continental proprietary code (i.e. the component which is named as project name in the FOSS report).


Image Integrated_with_Continental_proprietary_code.

### Based on the selection of Ship Status:

If **"YES"**, then usage/integration should be one of the below 5 options:

  - Statically Linked or Dynamically Linked or Separate Work or Dev-Tool/Excluded or Pre-requisite


If **"NO"**, then usage/integration should be one of the below 2 options:

  - Dev-Tool/Excluded or Pre-requisite

### Definitions of different usage/integration scenario:

  - **Statically Linked:**

    Static Linking is a method to link individual program modules to an executable program. With static Linking, the individual modules are linked to one single file. The individual program libraries are linked to the compilation of another program by a so-called linker. Thereby the linker generates an executable file or (depending on the operating system) a loading module in a loading library, in which the called modules are integrated/ attached statically. This procedure doesn’t change the source code of the individual libraries but the linking of an accessing program with a library generates a permanently linked new program.

    In the situation of system calls (https://en.wikipedia.org/wiki/System_call) please choose scenario of "Separate work" as usage.

    In the situation of ship status as "NO", then choose scenario of "Pre-requisite" as usage.

  - **Dynamically linked:**
    Dynamic Linking is a method to link individual program modules to an executable program, too. With dynamic Linking of program libraries, the main memory loads the components only while the runtime. So dynamic Linking means the executed program and the accessed library are linked only at the moment of program execution.

    Examples: .dll or .jar files

    In the situation of system calls (https://en.wikipedia.org/wiki/System_call) please choose scenario of "Separate work" as usage.

    In the situation of ship status as "NO", then choose scenario of "Pre-requisite" as usage.

  - **Separate Work:** 
      Consider, if OS Library is a requirement for an application to run which is not available as part of the standard operating system (ex: Linux, windows). In general these packages are defined to work with OS and act as a bridge between applications and OS. It can be downloaded from OS libraries made available or contributed by the OS vendors. In this case, when this packages were used by applications, it’s usage is considered as a separate work.

       **Standalones** – The software program that can be executed by itself, without the need for other programs or files to be present. This type of application is often self-contained and does not require any installation in order to run. The connection to other software is accomplished, for example, via pipes, sockets or command line parameters.

       **Examples:** Curl, zip, unzip etc.. (these are applicable for both windows and Linux OS). Mostly these packages were sent as part of the release (i.e. build/executables).

       **System Call** is a method by which the program can execute functionalities available in the operating system. A linkage only happens temporarily when information is send from a program to the hardware, the kernel or other processes or the program is reading information from the hardware, the kernel or other processes.

       **Examples:** On Unix, Unix-like and other POSIX-compliant operating systems, popular system calls are open, read, write, close, wait, exec, fork, exit, and kill (see: https://en.wikipedia.org/wiki/System_call)

     In the situation of ship status as "NO", then choose scenario of Pre-requisite/Dev tool excluded as usage.

  - **Dev Tool/Excluded:**

      In few cases, the project team wants to ship software tools which is used for development and it is required by OEM/Customers to setup the development environment on their end. Development tools can be part of the delivery or may not be part of the delivery based on the application/software requirements.

      **Examples:** Eclipse IDE, google test, etc..

  - **Pre-Requisite:**
In general any application/software to run, it requires a platform which provide/support default inbuilt functions/system calls from application code packages. This creates a dependency for the application/software but it is not required to ship the entire pre-requisites as it is widely available or used by the end customers. But one addition to this, pre-requisite can be part of the delivery or may not be part of the delivery based on the application/software requirements.


## Column - Modification 

Image  Modification

  - If files corresponding to the component are being modified, please let us know in the “Modification (Yes/No)” column.

## Column - Dev Team Comments

You can give your comments in the 'Dev Team Comments' column in the "Components" tab.

Image Dev Team Comments

If you want to know where the corresponding files of any specific components present in the package, please refer “Source” worksheet in the reports.


## Tab - Source

Component source files will be listed in this section.

Image source 

## Tab - Tools Summary

This tab contains general information about the Tools used for Scan Review.

Image Tools Summary
