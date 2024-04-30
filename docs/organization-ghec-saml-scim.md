---
layout: page
title: SAML/SCIM for GHEC (organization level)
description: Organization level implementation information regarding SAML and SCIM features for GitHub Enterprise Cloud
author: nicklegan
---

## Available/verified features and documentation

### Azure Active Directory (Azure AD)

- ✅ SAML organization member authentication
  - [Enabling and testing SAML single sign-on for your organization](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/enabling-and-testing-saml-single-sign-on-for-your-organization) (GitHub Docs)
  - [Tutorial: Azure Active Directory single sign-on (SSO) integration with a GitHub Enterprise Cloud Organization](https://docs.microsoft.com/azure/active-directory/saas-apps/github-tutorial) (MSFT Docs)
- ✅ SCIM organization membership provisioning
  - [Tutorial: Configure GitHub for automatic user provisioning](https://docs.microsoft.com/azure/active-directory/saas-apps/github-provisioning-tutorial) (MSFT Docs)
  - [About SCIM](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/about-scim) (GitHub Docs)
- ✅ SCIM organization team member sync (Azure AD security groups as teams)
  - [Managing team synchronization for your organization](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/managing-team-synchronization-for-your-organization) (GitHub Docs)
  - [Synchronizing a team with an identity provider group](https://docs.github.com/organizations/organizing-members-into-teams/synchronizing-a-team-with-an-identity-provider-group) (GitHub Docs)
  - [Azure Active Directory team synchronization, now available with Enterprise Cloud](https://github.blog/2019-09-24-azure-active-directory-team-synchronization-now-available-with-enterprise-cloud) (GitHub blog)
- 🛍️ [Azure Marketplace: GitHub Enterprise Cloud - Organization](https://azuremarketplace.microsoft.com/marketplace/apps/aad.githubcom)

### Okta

- ✅ SAML organization member authentication
  - [Configuring SAML single sign-on and SCIM using Okta](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/configuring-saml-single-sign-on-and-scim-using-okta) (GitHub Docs)
- ✅ SCIM organization membership provisioning
  - [Configuring access provisioning with SCIM in Okta](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/configuring-saml-single-sign-on-and-scim-using-okta#configuring-access-provisioning-with-scim-in-okta) (GitHub Docs)
- ✅ SCIM organization team member sync (Okta groups as teams) __(limited beta)__
  - [Enabling team synchronization for Okta](https://docs.github.com/organizations/managing-saml-single-sign-on-for-your-organization/managing-team-synchronization-for-your-organization#enabling-team-synchronization-for-okta) (GitHub Docs)
  - [Synchronizing a team with an identity provider group](https://docs.github.com/organizations/organizing-members-into-teams/synchronizing-a-team-with-an-identity-provider-group) (GitHub Docs)
  - [Team sync for Okta now in beta](https://github.blog/changelog/2020-05-07-team-sync-for-okta-now-in-beta/) (GitHub blog)
  - [Customer beta sign-up page](https://github.com/features/okta-team-sync/signup)
  - [Internal beta sign-up issue](https://github.com/github/group-syncer/issues/823)
  - 🔒 [Feature flag](https://admin.github.com/devtools/feature_flags/okta_team_sync) `okta_team_sync`
- 🛍️ [Okta Integration Network: GitHub Enterprise Cloud - Organization](https://www.okta.com/integrations/github-enterprise-cloud-organization)

## Azure AD/Okta SAML and SCIM organization level diagram

<img width="900" alt="Azure AD/Okta SAML and SCIM organization level diagram" src="https://user-images.githubusercontent.com/60080580/122581516-69c23700-d057-11eb-8569-037a5e9f9ef6.png">

## Azure AD/Okta SAML authentication flow diagram

<img width="1000" alt="Azure AD/Okta SAML authentication flow diagram" src="https://user-images.githubusercontent.com/60080580/122583043-1cdf6000-d059-11eb-9bd5-440d93f7b5d6.png">
