# Discovering the Value of IBM API Connect

##### Firmware Version:  5.0.0.1

![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/api-connect.png)

## Table of Contents

[Lab 1 - Introduction to IBM API Connect](./Lab%201%20-%20Introduction%20to%20IBM%20API%20Connect)

[Lab 2 - Create a LoopBack Application](./Lab%202%20-%20Create%20a%20LoopBack%20Application)

[Lab 3 - Customize and Deploy an Application](./Lab%203%20-%20Customize%20and%20Deploy%20an%20Application)

[Lab 4 - Configure and Secure an API](./Lab%204%20-%20Configure%20and%20Secure%20an%20API)

[Lab 5 - Advanced API Assembly](./Lab%205%20-%20Advanced%20API%20Assembly)

[Lab 6 - Working with API Products](./Lab%206%20-%20Working%20with%20API%20Products)

[Lab 7 - Consumer Experience](./Lab%207%20-%20Consumer%20Experience)

[Lab 8 - Analytics in API Connect](./Lab%208%20-%20Analytics%20in%20API%20Connect)

## Overview

This IBM API Connect (APIC) Proof of Technology (PoT) provides insight to a complete solution which helps you:

* Quickly configure your API management solution and rapidly create new APIs.
* Manage your APIs with business-level controls by setting varying levels of consumption/entitlements and the ability to manage an application developer portal.
* Transform and grow your business with insights through detailed analytics with structured filtered searches and navigation capabilities.
* Acquire partners and innovative application developers, both internally and externally; through an attractive developer portal which can be tailored to your brand.
* Use leading standard industry practices to help secure, control, and optimize access to your APIs.
* Reduce resolution time with supplied operational monitoring and debugging capabilities and investments.

## Introduction

To be a successful business and provide increased value to clients, employees, citizens, members or patients, one needs to continue to innovate while achieving cost optimization. To innovate while optimizing costs, you have an opportunity to expose key business services to business partners to drive new forms of collaboration, increase revenue opportunities, and provide higher value services to your customers. You can also expose your internal services to your internal developer communities, to drive application development consistency, reduce IT costs, and promote standard use of enterprise-approved services across your organization.

The expansion of the number of mobile devices, such as smart phones and tablets, as well as the growth of social media presents new and additional business opportunities to create new channels with customers and partners. To address these emerging opportunities, you require easy-to-use tools to define, assemble, secure, limit and monitor services, while providing a self-service experience for rapid developer on-boarding.

Business services are exposed with APIs. An API management solution is imperative to the success of externalizing the core services by providing easy assembly of new APIs, enabling security and different levels of service, providing management and insight to developers and business users, and socializing those APIs to developers, through communities and portals. This enables organizations to participate in the API economy, with rapid, highly secure connections between API providers and API consumers.

This new release of IBM API Connect delivers a complete solution across the Create + Run + Secure + Manage set of capabilities that can be deployed on-premise in an enterprise's data center, or in the cloud. API Connect allows businesses as API providers to integrate the most important features that are necessary to enable the three most significant players in the API economy: application developers, business owners, and IT personnel.

API Connect provides capabilities for creating new applications and APIs, SDLC management, proxying, assembling, securing and scaling the APIs. This solution also provides detailed analytics and operational metrics to the business owner and a customizable developer portal to socialize the APIs and manage applications that can be used by the developers. The developer portal enables self-service registration and provides links to social communities.

## Requirements

To complete the labs in this workbook, you’ll need the following:

+ A network attached workstation with sufficient memory (16GB min).
+ VMware Workstation v9 for Windows or VMWare Fusion v8 for OSX to run the supplied student VMware images.
+ Access to the API Connect POT Stack – which includes
  - IBM DataPower Gateway Appliance (Physical or virtual) with Firmware 7.5.0.0 or greater
  - IBM API Management Hypervisor, version 5 or higher.
  - Developer Portal, version 5.0.0.0 or higher
  - Primary host OS VM for the POT running on Xubuntu Linux

The following symbols appear in lab guides at places where additional guidance is available.

| Icon | Purpose | Explanation |
|------|---------|-------------|
|![][important]|  Important!  |  This symbol calls attention to a particular step or command. For example, it might alert you to type a command carefully because it is case sensitive. |
|![][info]|  Information  |  This symbol indicates information that might not be necessary to complete a step, but is helpful or good to know. |
|![][troubleshooting]|  Troubleshooting  |  This symbol indicates that you can fix a specific problem by completing the associated troubleshooting information. |

Proceed to [Lab 1 - Introduction to IBM API Connect](./Lab%201%20-%20Introduction%20to%20IBM%20API%20Connect)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"