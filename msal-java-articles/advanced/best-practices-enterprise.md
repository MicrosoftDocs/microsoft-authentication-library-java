---
title: Best practices for enterprises
description: "To build robust, enterprise-ready applications, you will need to follow some of the best patterns and practices in using MSAL Java."
---

# Best practices for enterprises

To build robust, enterprise-ready applications, you will need to ensure that you implement a few additional guardrails. We recommend developers to:

- Handle exceptions, both when acquiring a token, but also when calling a protected web API. In particular, if an application runs in a Microsoft Entra tenant where the tenant admins have set [Conditional Access](/azure/active-directory/conditional-access/overview) policies to enforce Multiple Factor Authentication (MFA), you will need to handle a claim challenge which is described in [Exceptions](./exceptions.md).
- Enable [Logging](/azure/active-directory/develop/msal-logging?tabs=java) to troubleshoot applications, while respecting user privacy and remain compliant with privacy regulations, such as GDPR.
