---
layout: needle-post
title: "One Small Step for MDM Assessments"
description: Needle for iOS MDM Assessments.
categories:
  - needle
  - blog
tags:
---

# One Small Step for MDM Assessments

## Intro

Mobile Device Management (MDM) is a solution commonly used for the administration of mobile devices. When a corporate device is provided to a new employee, and the device is powered on for the first time, a request will be made to an MDM server which will enforce a specific set configuration settings onto the device. These settings will then be used to manage the device by enabling feature restrictions, configuring email accounts, installing applications, etc.

MDM solutions are commonly used to centrally administer large pools of devices, removing the need to individually configure each device on a case-by-case basis. Although this is convenient, it implies that if there are any security weaknesses within the MDM configuration itself, these weaknesses will be enforced on every device subject to the MDM solution. This greatly increases the risk of a security incident occuring if the solution is not subject to regular security assessments.

## MDM Assessments

Performing MDM assessments can be a lengthy and laborious process. Given a managed device subject to a specific set of restrictions, the assessor is required to create a template for what they believe is a secure and effective configuration based on the clients’ security requirements. The assessor must then ensure that the appropriate protections are in place to prevent users from bypassing these restrictions. This is commonly accomplished through the repetitive task of inspecting and verify that each restriction applied by the MDM complies with this “secure template”. After assessing each restriction and manually verifying the ability to bypass restrictions or exploit weak configuration settings, the assessor will provide recommendations on how to better secure the device and its content. These recommendations would then be implemented by altering the configuration applied by the MDM solution. The overall findings on how the device is configured will define the effectiveness and security the posture of the MDM solution in place. 

After researching into how a device’s configuration is tracked and stored on iOS, it was discovered that MDM configurations are ultimately enforced by pushing and installing a configuration profile on the managed device. This is similar to how a regular user manages their configuration, by creating and installing profiles.

Given one or more restrictions and/or configuration profiles enforced, an `“EffectiveUserSettings.plist"` file is generated and stored in one of the following directories, depending on what version of iOS is in use:

+ `“/var/mobile/library/ConfigurationProfiles/"` (iOS 9 and below)
+ `“/var/mobile/Library/UserConfigurationProfiles/"` (As of iOS 10)

The `“EffectiveUserSettings.plist"` file defines what restrictions are enforced on the device, taking into account any configuration profiles that are applied, as well as individual user-controlled settings. This shows that a lot of the information required to perform an MDM assessment is stored within this single file. With this in mind it should be possible to compare the `“EffectiveUserSettings.plist"` file from a device configured to be secure, based on the client’s security requirements, with the `“EffectiveUserSettings.plist"` file from the test device subject to the MDM assessment. This is precisely what the `“mdm/effective_user_settings"` module in MWR’s Needle does:

![Image: Module Info Preview Image.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_01.png "Module Info Preview.")

The module has two main functions:

1. Pull the Effective User Configuration file from a device.
2. Perform a template-driven assessment of a device’s Effective User Configuration.

The module’s default configuration is set to pull the effective configuration file from a device. The module can be configured to perform a template-driven assessment by setting the `“PULL_ONLY"` option to “False”, and defining the full path to an `“EffectiveUserSettings.plist"` file within the `“TEMPLATE"` option. 

## Pulling the Effective Configuration File

The module’s default configuration can be used to pull the `“EffectiveUserSettings.plist"` file from a device, as can be seen below: 

![Image: Pull Effective Configuration Image.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_02.png "Pull Effective Configuration.")

This configuration will pull the `“EffectiveUserSettings.plist"` file from the device and store it within the `“/root/.needle/output/"` directory. This file can then renamed and moved to a more permanent location:

![Image: Rename Configuration File.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_03.png "Rename Configuration File.")

The file can be used as a guide for performing an assessment of the MDM configuration applied to the device being tested.

## Performing a Template-Driven Assessment

Once a template `“EffectiveUserSettings.plist"` file is stored locally, it can be used by the `“mdm/effective_user_settings"` module to perform a template-driven assessment of a device’s configuration. This is done by setting the modules `“PULL_ONLY"` option to `“False"` and supplying the full path to the `“template"` file within the `“TEMPLATE"` option:

![Image: Configure Template-Driven Assessment.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_04.png "Configure Template-Driven Assessment.")

Once these options have been configured, the module can be run to perform an automated assessment:

![Image: Run Template-Driven Assessment.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_05.png "Run Template-Driven Assessment.")

The module will then output a summary of what settings are not compliant with the template, along with recommended changes where appropriate:

![Image: Template-Driven Assessment Output.](http://mobiletools.mwrinfosecurity.com/assets/images/needle_mdm_module/Image_06.png "Template-Driven Assessment Output.")

The template used in the above example was configured to highlight configuration issues that could allow a user to exfiltrate data from the device. The output from the module shows that configuration applied to the device by the MDM solution supports a number of ways to exfiltrate sensitive data, such as screenshots, screen recording, and iCloud backup. The output has also highlighted that the configuration supports insecure passwords and a weak screen lock grace period.

## Summary

Although Needle’s `“mdm/effective_user_settings"` module is not a fully automated solution to MDM assessments, it is a step in the right direction towards supporting consultants throughout testing. MDM solutions are an effective way of administrating and securing large pools of mobile devices, however the implications of an insecure MDM configuration can result in a range of security incidents. As the number of devices subject to a specific MDM solution grows, the likelihood of an incident occuring will increase if these configurations are not subject to thorough security assessments.

## Demo

[![asciicast](https://asciinema.org/a/4xafsfnd22qfiop87w68bsjfw.png)](https://asciinema.org/a/4xafsfnd22qfiop87w68bsjfw?theme=tango)