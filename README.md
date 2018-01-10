# Set up IMS managed ACBs

## Overview

With the IMS Managed ACBs workflow you can rapidly enable [IMS managed ACBs](https://www.ibm.com/support/knowledgecenter/en/SSEPH2_14.1.0/com.ibm.ims14.doc.sdg/ims_catalog_acb_mgmt_enabling_catg_exists.htm) using the IBM® z/OS® Management Facility (z/OSMF) 

IMS can manage the runtime application control blocks (ACBs) for databases and program views for you. When IMS manages ACBs, IMS no longer requires DBD, PSB, and ACB libraries

The workflow will enable IMS managed ACBs to an existing IMS with these steps:
* Create the IMS DFSDFxxx proclib members to enable IMS catalog with managed ACBs.
* Update the catalog database if needed. 
* Create the IMS catalog directory data sets.

The workflow will also optionally take an image copy of the HALDB catalog database.

## Pre-requisites
* An SMP/E installation of IMS is done and the IMS load libraries are available.
* Identify the z/OS system parameters
* IMS SVC modules are installed on the system.
* The Common Service Layer must be started.
* z/OSMF must be started. Both the angel and server z/OSMF address spaces must be started. 
* The IMS catalog has been enabled.

## Security requirements  
To run the workflow, you need the following authority:
* RACF read authority on SMP/E installed IMS libraries
* RACF update authority on the high level qualifiers (HLQs) you are using for the IMS instance libraries
* Authority to ADD/DELETE APF authorizations

## Package structure  
The repository includes the following files:
* setUpManagedACB.xml
  * This is the file that enables IMS managed ACBs. You should not modify the workflow XML.
* workflow_variables.properties
  * This properties file contains values from the variables referenced in the setUpManagedACB.xml workflow. Edit the workflow_variables.properties file to specify the system specific information for the variables in the file. 

## Installation  
* FTP the setUpManagedACB.xml workflow and the workflow_variables.properties file to USS on the z/OS host in binary mode.
* The files need to be made visible to the z/OSMF application.  Do this by changing the access permissions of the files using the chmod command
* Example chmod commands: 
```Java
chmod 755 setUpManagedACB.xml
```
* Or if the file is in a folder with the name of workflows:
```Java 
chmod -R 755 workflows
```

## To run the workflow using the z/OSMF web interface
1. Log into the IBM z/OS Management Facility web interface
1. Select "Workflows" from the left menu
1. Select the "Actions" drop down menu
1. Select "Create Workflow"
1. In the Create Workflow dialog, specify:
    *	Workflow definition file: 
    *	Workflow variable input file:
    *	System:
1. Select "Next"
1. Select “Assign all steps to owner user ID” if you are going to run all of the workflow steps with the current user ID.
1. Select "Finish"
1. Right-click on the first action and select "Perform"

For more information about running a workflow see [Create a workflow](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.3.0/com.ibm.zosmfworkflows.help.doc/izuWFhpCreateWorkflowDialog.html)

## Troubleshooting
* IZUWF0105E   Workflow property file file-name is either not found or cannot be accessed
  * Typically this error comes from the file not existing at the path given, or the file exists, and chmod needs to be done on this file.
* If there is no "Workflows" menu option in the z/OSMF web interface configure the IZUPRMxx member in the SYS.PARMLIB specifying the WORKLOAD_MGMT in the PLUGINS statement. For more information see [creating a IZUPRMxx](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.izua300/izuconfig_IZUPRMxx.htm) in the IBM Knowledge Center.
  * Example: 
  
  PLUGINS(INCIDENT_LOG  
        COMMSERVER_CFG
        CAPACITY_PROV 
        SOFTWARE_MGMT 
        ISPF          
        RESOURCE_MON  
        WORKLOAD_MGMT)

## z/OSMF documentation

Visit the IBM Knowledge Center for more information on [IBM z/OS Management Facility](https://www.ibm.com/support/knowledgecenter/search/IBM%20z%2FOS%20Management%20Facility?scope=SSLTBW_2.2.0)
