---
author: Enrique Ayala
pubDatetime: 2025-01-25T17:44:29-06:00
title: Configure AWS Identity Center as a SAML Identity Provider for Twilio Flex
slug: configure-aws-identity-center-as-saml-provider-for-twilio-flex
featured: true
draft: false
tags:
  - aws
  - saml
  - idp
  - saml-identity-provider
  - sso
  - twilio
  - twilio-flex
description:
    Instructions to configure a SAML IdP for Twilio Flex with AWS Identity Center
---

In this guide, I'll walk you through configuring AWS Identity Center as a SAML Identity Provider for Twilio Flex. This setup allows Twilio Flex agents to authenticate using AWS, eliminating the need for other external identity providers. AWS Identity Center supports SAML 2.0, making it a suitable choice for organizations already using AWS as their primary cloud provider.

Note: IAM Identity Center does not support custom attributes, so the role `agent` is assigned to all users accessing Twilio Flex. For more flexibility, consider using a different Identity Provider.

## Prerequisites

Before you begin, ensure you have:
- AWS Organizations enabled.
- Access to your AWS Organizations management account to activate the [SAML 2.0 customer managed applications feature](https://docs.aws.amazon.com/singlesignon/latest/userguide/identity-center-instances.html?icmpid=docs_sso_console).
- A Twilio Flex account

## 1. Enable Identity Center

1. Login to AWS Console
2. Go to [IAM Identity Center console](https://us-east-2.console.aws.amazon.com/singlesignon).
3. Under Enable IAM Identity Center, choose `Enable with AWS Organizations`.

For more details, refer to this [guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-set-up-for-idc.html).

## 2. Create and Configure the AWS IAM Identity Center SAML application

1. In the IAM Identity Center console, go to `Applications` and select `Add Application`. 
2. Choose `I have an application I want to set up` and under Application type section , choose `SAML 2.0` .
   - Note: If these options don't appear, you need to recreate the IAM Identity Center instance with `Enable with AWS Organizations`.
3. Configure the application:
  - Display name: `twilio-flex`. 
  - Under Application metadata section:
  - Application ACS URL: Find this in Twilio Console > `Flex` > [Overview](https://console.twilio.com/us1/develop/flex/overview) > `Set up SSO`.
  - Application SAML audience: Use the `Entity Id` from the same `Setup SSO` page.

4. After creation, go to `Applications` > `Customer Managed` > `twilio-flex`.
5. Select `Actions` >  `Edit attribute mappings` and add the [obligatory attributes](https://www.twilio.com/docs/flex/admin-guide/setup/sso-configuration#mandatory-attributes) required by Twilio Flex. 
   - `email` to `${user:email}`
   - `full_name` to `${user:name}`
   - `roles` to `agent`
   - Note: IAM Identity Center does not support custom attributes, so the role `agent` is hardcoded. For more flexibility, consider another Identity Provider.

## 3. Finalize Twilio Flex Identity Provider Data

1. In Twilio Flex, go to [Set up your identity provider](https://console.twilio.com/us1/develop/flex/manage/single-sign-on) and fill in:
  - Friendly name: `aws-sso`
  - X.509 : Copy the contents of certificate file . Download this file from AWS Identity Center > `Applications` > `Customer Managed` > `twilio-flex` > `actions` > `Edit configuration` and search for Download option of `IAM Identity Center Certificate`. 
  - Single sign-on URL: Use the `IAM Identity Center sign-in URL` from the application configuration details.


## References
- [Twilio Flex - Mandatory SSO Attributes](https://www.twilio.com/docs/flex/admin-guide/setup/sso-configuration#mandatory-attributes)
- [Enabling AWS IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/get-set-up-for-idc.html)
- [AWS IAM Identity Center (successor to AWS SSO) Integration Guide for Custom SAML 2.0 application](https://static.global.sso.amazonaws.com/app-520727d4117d1647/instructions/index.htm)
- [Supported IAM Identity Center attributes](https://docs.aws.amazon.com/singlesignon/latest/userguide/attributemappingsconcept.html#supportedssoattributes)

