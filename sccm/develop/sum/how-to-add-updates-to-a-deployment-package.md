---
title: "Add Updates to a Deployment Package | Microsoft Docs"
ms.custom: ""
ms.date: "09/20/2016"
ms.prod: "configuration-manager"
ms.reviewer: ""
ms.suite: ""
ms.technology:
  - "configmgr-other"
ms.tgt_pltfrm: ""
ms.topic: "article"
applies_to:
  - "System Center Configuration Manager (current branch)"
ms.assetid: bc094f2a-47a5-4a39-8d28-696676b64cbd
caps.latest.revision: 8
author: "shill-ms"
ms.author: "v-suhill"
manager: "mbaldwin"
---
# How to Add Updates to a Deployment Package
You add updates to a software updates deployment package, in System Center Configuration Manager, by obtaining an instance of the [SMS_SoftwareUpdatesPackage](assetId:///SMS_SoftwareUpdatesPackage?qualifyHint=False&autoUpgrade=True) class and by using the [AddUpdateContent](assetId:///AddUpdateContent?qualifyHint=False&autoUpgrade=True) method.  

### To create a software updates deployment package  

1.  Set up a connection to the SMS Provider.  

2.  Obtain an existing package object by using the assetId:///SMS_SoftwareUpdatesPackage?qualifyHint=False&autoUpgrade=True class.  

3.  Add update content to the existing package using the assetId:///AddUpdateContent?qualifyHint=False&autoUpgrade=True method.  

## Example  
 The following example method shows how to add updates to a software updates deployment package by using the assetId:///SMS_SoftwareUpdatesPackage?qualifyHint=False&autoUpgrade=True class and the assetId:///AddUpdateContent?qualifyHint=False&autoUpgrade=True method.  

> [!NOTE]
>  The updates must be available in the content source path (as part of the dictionary object `addUpdateContentParameters` in C#). If the updates exist in a package source, that package source cannot be used for more than one deployment package.  

> [!IMPORTANT]
>  No VBScript example was included, as the assetId:///AddUpdateContent?qualifyHint=False&autoUpgrade=True method does not return from the method call on failure. This is a known issue and is being investigated.  

 For information about calling the sample code, see [Calling Configuration Manager Code Snippets](../../develop/core/understand/calling-code-snippets.md).  

 Example of the method call in C#:  

```  

// PREWORK FOR AddUpdatesToSUMDeploymentPackage  

// Define the array of Content Ids to load into addUpdateContentParameters.  
int[] newArrayContentIds = new int[] { 82 };  

// Define the array of source paths (these must be UNC) to load into addUpdateContentParameters.  
string[] newArrayContentSourcePath = new string[] { "\\\\ServerOne\\source1" };  

// Load the update content parameters into an object to pass to the method.  
Dictionary<string, object> addUpdateContentParameters = new Dictionary<string, object>();  
addUpdateContentParameters.Add("ContentIds", newArrayContentIds);  
addUpdateContentParameters.Add("ContentSourcePath", newArrayContentSourcePath);  
addUpdateContentParameters.Add("bRefreshDPs", false);  

AddUpdatestoSUMDeploymentPackage(WMIConnection,  
                                 "ABC00001",  
                                 addUpdateContentParameters);  

```  

```c#  

public void AddUpdatestoSUMDeploymentPackage(WqlConnectionManager connection,  
                                            string existingSUMPackageID,  
                                            Dictionary<string, object> addUpdateContentParameters)  
{  
    try  
    {  
        // Get the specific SUM Deployment Package to change.  
        IResultObject existingSUMDeploymentPackage = connection.GetInstance(@"SMS_SoftwareUpdatesPackage.PackageID='" + existingSUMPackageID + "'");  

        // Add updates to the existing SUM Deployment Package using the AddUpdateContent method.  
        // Note: The method will throw an exception, if the method is not able to add the content.  
        existingSUMDeploymentPackage.ExecuteMethod("AddUpdateContent", addUpdateContentParameters);  

        // Output a success message that the content was added.  
        Console.WriteLine("Added content to the SUM deployment package. ");                  
    }  
    catch (SmsException ex)  
    {  
        Console.WriteLine("Failed to add content to the SUM deployment package.");                  
        Console.WriteLine("Error: " + ex.Message);        
        throw;  
    }  
}  

```  

 The example method has the following parameters:  

||||  
|-|-|-|  
|Parameter|Type|Description|  
|`connection`|-   Managed: [WqlConnectionManager](assetId:///WqlConnectionManager?qualifyHint=False&autoUpgrade=True)|A valid connection to the SMS Provider.|  
|`existingSUMPackageID`|-   Managed: `String`|The package ID for an existing software updates deployment package.|  
|`addUpdateContentParameters`|-   Managed: `dictionary` object|The set of parameters (`ContentIDs`, `ContentSourcePath`, `bRefreshDPs`) that is passed into the method and used with the `AddUpdateContent` method call.|  

## Compiling the Code  
 This C# example requires:  

### Namespaces  
 System  

 System.Collections.Generic  

 System.Text  

 Microsoft.ConfigurationManagement.ManagementProvider  

 Microsoft.ConfigurationManagement.ManagementProvider.WqlQueryEngine  

### Assembly  
 adminui.wqlqueryengine  

 microsoft.configurationmanagement.managementprovider  

## Robust Programming  
 For more information about error handling, see [About Configuration Manager Errors](../../develop/core/understand/about-configuration-manager-errors.md).  

## .NET Framework Security  
 For more information about securing Configuration Manager applications, see [Securing Configuration Manager Applications](../../develop/core/understand/securing-configuration-manager-applications.md).  

## See Also  
 [System Center Configuration Manager Software Development Kit](../../develop/core/misc/system-center-configuration-manager-sdk.md)   
 [Configuration Manager Software Updates](../../develop/sum/software-updates.md)   
 [Software Updates Scheduled Deployment](../../develop/sum/software-updates-deployments.md)   
 [How to Assign a Package to a Distribution Point](../../develop/core/servers/configure/how-to-assign-a-package-to-a-distribution-point.md)   
 [SMS_SoftwareUpdatesPackage](../../develop/reference/sum/sms_softwareupdatespackage-server-wmi-class.md)   
 [AddUpdateContent Method in Class SMS_SoftwareUpdatesPackage](../../develop/reference/sum/addupdatecontent-method-in-class-sms_softwareupdatespackage.md)