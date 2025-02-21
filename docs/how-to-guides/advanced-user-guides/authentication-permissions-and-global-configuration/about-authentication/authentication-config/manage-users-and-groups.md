---
title: Users and Groups
---

Rancher relies on users and groups to determine who is allowed to log in to Rancher and which resources they can access. When you configure an external authentication provider, users from that provider will be able to log in to your Rancher server. When a user logs in, the authentication provider will supply your Rancher server with a list of groups to which the user belongs.

Access to clusters, projects, multi-cluster apps, and global DNS providers and entries can be controlled by adding either individual users or groups to these resources. When you add a group to a resource, all users who are members of that group in the authentication provider, will be able to access the resource with the permissions that you've specified for the group. For more information on roles and permissions, see [Role Based Access Control](../../../../../pages-for-subheaders/manage-role-based-access-control-rbac.md).

## Managing Members

When adding a user or group to a resource, you can search for users or groups by beginning to type their name. The Rancher server will query the authentication provider to find users and groups that match what you've entered. Searching is limited to the authentication provider that you are currently logged in with. For example, if you've enabled GitHub authentication but are logged in using a [local](create-local-users.md) user account, you will not be able to search for GitHub users or groups.

All users, whether they are local users or from an authentication provider, can be viewed and managed. In the upper left corner, click **☰ > Users & Authentication**. In the left navigation bar, click **Users**.

:::note SAML Provider Caveats:

- SAML Protocol does not support search or lookup for users or groups. Therefore, there is no validation on users or groups when adding them to Rancher.
- When adding users, the exact user IDs (i.e. `UID Field`) must be entered correctly. As you type the user ID, there will be no search for other  user IDs that may match.
- When adding groups, you must select the group from the drop-down that is next to the text box. Rancher assumes that any input from the text box is a user.
- The group drop-down shows only the groups that you are a member of. You will not be able to add groups that you are not a member of.

:::

## User Information

Rancher maintains information about each user that logs in through an authentication provider. This information includes whether the user is allowed to access your Rancher server and the list of groups that the user belongs to. Rancher keeps this user information so that the CLI, API, and kubectl can accurately reflect the access that the user has based on their group membership in the authentication provider.

Whenever a user logs in to the UI using an authentication provider, Rancher automatically updates this user information.

### Automatically Refreshing User Information

Rancher will periodically refresh the user information even before a user logs in through the UI. You can control how often Rancher performs this refresh.

Two settings control this behavior:

- **`auth-user-info-max-age-seconds`**

    This setting controls how old a user's information can be before Rancher refreshes it. If a user makes an API call (either directly or by using the Rancher CLI or kubectl) and the time since the user's last refresh is greater than this setting, then Rancher will trigger a refresh. This setting defaults to `3600` seconds, i.e. 1 hour.

- **`auth-user-info-resync-cron`**

    This setting controls a recurring schedule for resyncing authentication provider information for all users. Regardless of whether a user has logged in or used the API recently, this will cause the user to be refreshed at the specified interval. This setting defaults to `0 0 * * *`, i.e. once a day at midnight. See the [Cron documentation](https://en.wikipedia.org/wiki/Cron) for more information on valid values for this setting.

To change these settings,

1. In the upper left corner, click **☰ > Global Settings**.
1. Go to the setting you want to configure and click **⋮ > Edit Setting**.

:::note

Since SAML does not support user lookup, SAML-based authentication providers do not support periodically refreshing user information. User information will only be refreshed when the user logs into the Rancher UI.

:::
### Manually Refreshing User Information

If you are not sure the last time Rancher performed an automatic refresh of user information, you can perform a manual refresh of all users.

1. In the upper left corner, click **☰ > Users & Authentication**.
1. On the **Users** page, click on **Refresh Group Memberships**.

**Results:** Rancher refreshes the user information for all users. Requesting this refresh will update which users can access Rancher as well as all the groups that each user belongs to.

:::note

Since SAML does not support user lookup, SAML-based authentication providers do not support the ability to manually refresh user information. User information will only be refreshed when the user logs into the Rancher UI.

:::

## Session Length

The default length (TTL) of each user session is adjustable. The default session length is 16 hours.

1. In the upper left corner, click **☰ > Global Settings**.
1. Go to **`auth-user-session-ttl-minutes`** and click **⋮ > Edit Setting**.
1. Enter the amount of time in minutes a session length should last and click **Save**.

**Result:** Users are automatically logged out of Rancher after the set number of minutes.
