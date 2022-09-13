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


