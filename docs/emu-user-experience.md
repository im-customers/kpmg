---
layout: page
title: EMU User Experience
description: A high-level summary of user experience differences with Enterprise Managed Users
author: andyfeller
last_updated: 2022-03-14
---

# Enterprise Managed User - User Experience

## Feature Set

- Single Sign On at the GitHub enterprise account level
- User and group provisioning lifecycle management via SCIM
- User activity is auditable within the enterprise account
- GitHub user metadata conforming to external identity provider
- Enterprise managed users are invisible from outside of enterprise account
- Read-only access to OSS community on GitHub for end users
- No public repositories for EMU-enabled enterprises
- [Support for Azure AD and Okta as identity providers](https://docs.github.com/en/enterprise-cloud@latest/admin/authentication/managing-your-enterprise-users-with-your-identity-provider/about-enterprise-managed-users)
  - [Okta > SAML setup guide](https://saml-doc.okta.com/SAML_Docs/How-to-Configure-SAML-2.0-for-GitHub-Enterprise-Managed-User.html)
  - [GitHub > Okta SCIM guide](https://docs.github.com/en/enterprise-cloud@latest/admin/authentication/managing-your-enterprise-users-with-your-identity-provider/configuring-scim-provisioning-for-enterprise-managed-users-with-okta)

## Currently Unsupported Use Cases

1. No public repositories for organization or user-owned repositories
2. No public contributions for EMUs  _(for example, performing write actions outside of the enterprise account)_
3. No public projects
4. No gists
5. No outside collaborators  _(all user accounts must be backed by an identity provider)_
6. No forking from external repositories

## Miscellaneous

1. EMU-enabled enterprises have a slug that is suffixed to end users' usernames for global uniqueness
    _(for example: andyfeller_fabrikam where `fabrikam` is the slug)_
2. Public profile for end users are read-only
3. Pages are protected by SSO
4. EMU-enabled enterprises do not show up from public GitHub view

## EMU Isolation

Enterprise Managed Users (EMUs) are restricted to the enterprise account environment they are created in; they cannot perform any `write` operations outside of their enterprise account. Internally, this is referred to as a "walled garden" experience. This is the list of examples to clarify the restrictions on users created in an EMU environment.

In these examples, a "user account" is an enterprise-managed user (EMU) created within an EMU-enabled enterprise. An "external user" is any GitHub user account that is not part of the EMU-enabled enterprise.

### Searching and inviting

In short, EMUs are not publicly searchable from GitHub.com.

| Action | Allowed? (Yes/No) | If not allowed, message/error seen  |
| --- | --- | -- |
| External organization owner or enterprise account owner attempts to search for EMU to invite these users to their org or enterprise account. | No | EMUs do not show up in search results for users outside of the EMU-enabled enterprise. Admins do not have the option to invite any EMU into any external org or enterprise. |
| EMU attempts to invite an external user as an outside collaborator on a repository. | No | External users cannot be invited as an outside collaborator to a repository in an org under an EMU-enabled enterprise account, even if the external user's account appears in the search results. |
| EMU user attempts to search for an external user in the "All GitHub" search UI. | No | External users will not show up in the search results. |
| External user attempts to search for EMU in the "All GitHub" search UI | No | EMUs will not show up in the search results. |

### Gists

| Action | Allowed? (Yes/No) | If not allowed, message/error seen |
| --- | --- | --- |
| Read access to a public Gist | Yes | N/A |
| EMU attempts to create a secret or public Gist in the UI | No | When EMU clicks the `Create secret gist` or `Create public gist` button, it will result in a `404` error  |
| EMU attempts to perform a write operation on a public Gist in the UI | No | If EMU clicks `Subscribe` or `Star`, it will appear it has changed but once the EMU refreshes it will show the Gist is not subscribed or starred (does not save). If EMU clicks `Fork`, it will result in a `404`. If EMU attempts to comment in the public Gist, clicking the `Comment` button wil result in a `You can't comment at this time` error. REST API will give `403 Forbidden` `Unauthorized: As an Enterprise Managed User, you cannot access this content` error | UI Bug: https://github.com/github/external-identities/issues/780. EMU should not have the ability to click the `Comment` button. Should be fixed before Beta release. |
| EMU attempts to create a secret or public Gist via the REST API | No | `403 Forbidden` `Unauthorized: As an Enterprise Managed User, you cannot access this content` error   |

### Repository Visibility

| Action | Allowed? (Yes/No) | If not allowed, message/error seen  |
| --- | --- |--- |
| EMU attempts to create a public repository in the EMU-enabled enterprise | No | EMUs will not have the option to create a `Public` repository, or change a repository to `Public` in their enterprise. They will only have the option to create a `Private` or `Internal` repository. |
| Read access to public repos outside of the EMU's enterprise account | Yes | N/A  |
| Git clone operations on public repos outside of the EMU's enterprise account | Yes | N/A  |
| Git write operations, such as `git push`, to a public repository outside of the EMU's enterprise account | No | Error: `remote: Permissions to [owner/repository] denied to [emu username]. fatal: unable to access [repository URL]: The requested URL returned error: 403.` |
| REST or GraphQL API calls that perform write operations in public repos outside of the EMU's enterprise account | No | `403 Forbidden` `Unauthorized: As an Enterprise Managed User, you cannot access this content` |

### User Visibility

| Action  | Allowed? (Yes/No) | If not allowed, message/error seen |
| --- | --- | --- |
| Organization owners, repository admins, or team maintainers attempt to search for a user who has not been provisioned to the enterprise to add them to an org/team/repository in the EMU-enabled enterprise | No | They would not see the user as an option, because only EMU users who have been provisioned to the enterprise will be shown as an option. |
| EMU attempts to @mention other EMU user accounts that are provisioned to their enterprise account | Yes | N/A |
| Non-EMU account outside the enterprise attempts to @mention an EMU user account  | No | Non-EMU accounts outside of the enterprise will not be able to search for or mention an EMU user account inside the EMU-enabled enterprise |

### Repository Forking

| Action | Allowed? (Yes/No) | If not allowed, message/error seen |
| ---- | --- | --- |
| Fork outside (user/org) repository into EMU (user/org) namespace | No  | `403 Forbidden` `Unauthorized: As an Enterprise Managed User, you cannot access this content` |
| Fork EMU user fork into my EMU user namespace | No. For the moment we don't allow EMUs to act on other EMUs resources. If we end up allowing them, this will become a valid operation | `403 Forbidden` `Unauthorized: As an Enterprise Managed User, you cannot access this content` |
| Fork EMU org repository into my EMU user namespace | Yes  | N/A |
| Fork EMU org repository into another EMU org namespace | Yes  | N/A |
| Fork EMU user repository to EMU org namespace | Yes, because I have permissions over my repository and over the target org | N/A |

#### Forking scenarios

- Unlimited forks are permitted when forking to and from an org.

  ![alt-text](/assets/images/emu/isolationUnlimitedForks.png)

- Only 1 level deep forking is permitted when a user forks an org repository.

  ![alt-text](/assets/images/emu/isolationOneForkDeep.png)

- User-forking of another user's repository is not permitted.

### Repository Transfer

Repositories cannot be transferred between users, whether they are EMUs or not. A repository can be transferred between orgs as long as the user performing the transfer has access to both. Repositories can be transferred between an EMU user and any org they belong to. <!-- There is current [bug regarding forking + transferring](https://github.com/github/coding/issues/932) which is independent of the EMU feature. -->

| Action | Allowed for EMU? (Yes/No) |
| -- | -- |
| Transfer EMU user repository to EMU org | Yes |
| Transfer EMU user repository to any user | No |
| Transfer any user repository to EMU user | No |
| Transfer EMU org repository to EMU user | Yes |
| Transfer EMU org repository to EMU org | Yes |
