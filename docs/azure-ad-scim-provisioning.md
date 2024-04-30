---
layout: page
title: Configure Azure SCIM on GHEC Playbook
description: Playbook to configure SCIM provisioning in GHEC with Azure Active Directory
author: droidpl
toc: true
---

> This guide describes how to configure Azure SCIM on GitHub Enterprise Cloud (GHEC).

## Affected products and versions

GitHub Enterprise Cloud
{: .label }

## Enablement of Azure System for Cross-domain Identity Management (SCIM) on GHEC

### What is SCIM?

SCIM is a protocol supported by different providers to auto provision/remove users from one or multiple platforms according to the permissions users have in a single identity provider.

GitHub supports Azure Active Directory (AD) natively, and we can leverage this functionality for the auto-provisioning of certain Azure groups.

Find more about SCIM in the [GitHub documentation](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-scim)

### Requirements

In order to enable SCIM you will need:

- An Azure AD configured using [SAML](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/github-tutorial) in your organization
- The access/credentials of a user that is an administrator on the GitHub organization (it's recommended to use a machine account with administrator access)
- The access/credentials of a user that has permission to edit the GitHub enterprise application on Active Directory
- A license subscription for GitHub Enterprise Cloud

### Steps

[The official documentation](https://docs.microsoft.com/en-us/azure/active-directory/saas-apps/github-provisioning-tutorial) to enable SCIM has these steps:

- Enter your Azure AD configuration
- Go to `Enterprise applications`
- Go to your GitHub application configured for SAML
- In the left menu, open `Provisioning`
- Enable the provisioning as `automatic` and authorize Azure to access GitHub.
- Set the tenant url parameter to `https://api.github.com/scim/v2/organizations/:org` and click on `Authorize`
- A window on GitHub will appear to authorize the Azure SCIM OAuth application. We recommend using a machine account for the approval, as this is an OAuth app and would be approved by a certain user.
- Once the request is approved, you will get the authorization and SCIM will start working

![SCIM provisioning 1](/assets/images/organization-administration/ghec-scim-provisioning.png)

- Check `all users and groups` if you want to sync all the users in the directory, or use `only assigned users and groups` to use only the ones that are assigned to the app in the menu `users and groups`
- Click `save`

### What you will see

Once SCIM is correctly configured, the Azure SCIM schedule will run the synchronization with the organization every 45 minutes. Using the emails and usernames set in Azure, it will try to find the users in GitHub and send invitations to those users to join the organization.

- **If the user does not exist**: an invitation will be sent to their email address asking them to create a GitHub account and then they will be prompted to join the organization
- **If the user exists**: it will match it and send an email with the invitation to the organization

You can check the current state of the sync:

- **Using the GitHub SCIM API**: Our [REST API documentation](https://developer.github.com/v3/scim/) has information on how to check it. You can also find these invitations in the [GraphQL model](https://developer.github.com/v4/object/externalidentity/).
- **Using the provisioning logs in Azure**: Go to the `enterprise application`, and in the left menu click on `Provisioning logs`. You may need additional permissions from your administrator to see these logs.
- **In the GitHub UI**: You can check the current SCIM identity linked to the users in the Organization's `People` tab. You can go directly to `https://github.com/orgs/:org/people/:user/sso` to see the linked identity of a user.

### Troubleshooting

#### When clicking authorize on azure provisioning, the button doesn't work

If the authorize button in the Azure provisioning page doesn't work, refresh the page without cache (shift+cmd+R on mac or ctrl+R on Windows) and try again with and without the url set. Sometimes it fails authorizing when one attempt was wrong.

#### How can I see my synced users

Check the API and the provisioning logs to see which users are synced and any reason that a user doesn't sync. Keep in mind that a user identity is created when:

- The invitation is accepted
- When the user logs in using the SSO url `https://github.com/orgs/:org/sso`

For users having a problem with the identity, use link from above directly to trigger the synchronization again.

#### I have inconsistencies in the synchronization

If you find inconsistencies with the synchronization you can reset to start again:

- Go to Azure's GitHub enterprise application
- Go to `Provisioning`
- Mark the tick `Clear current state and restart synchronization`
- Click `Save`

This will recreate the synchronization and fix any inconsistencies of users not being SCIM provisioned

## References

- [GitHub SCIM documentation](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-scim)
