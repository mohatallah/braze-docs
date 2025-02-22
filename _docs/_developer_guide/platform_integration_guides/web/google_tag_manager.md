---
nav_title: Google Tag Manager
article_title: Google Tag Manager for Web
platform: Web
page_order: 20
description: "This article covers how to use Google Tag Manager to deploy Braze to your website."

---

# Google Tag Manager

This article provides a step-by-step guide on how to add the Braze Web SDK to your website using the Google Tag Manager.

[Google Tag Manager][2] lets you remotely add, remove, and edit tags on your website without requiring a production code release or engineering resources.

## Braze tag templates

There are two Google Tag Manager templates built by Braze, the [Initialization Tag](#initialization-tag) and the [Actions Tag](#actions-tag).

Both tags can be added to your workspace from [Google's community gallery][15] or by searching for Braze when adding a new tag from the Community Templates.

![image of gallery search][gtm-community-gallery-search]

### Initialization Tag template {#initialization-tag}

Use the Initialization Tag to add the Braze Web SDK to your website.

#### Step 1: Select the Initialization Tag

Search for Braze in the community template gallery, and select the **Braze Initialization Tag**.

![A dialog box showing the Braze Initialization Tag configuration settings. Settings included are "tag type", "API key", "API endpoint", "SDK version", "external user ID", and "Safari web push ID".][gtm-initialization-tag]

#### Step 2. Configure settings

Enter your Braze API app identifier key and SDK endpoint, which can be found in your dashboard's **Manage Settings** page.

#### Step 3. Choose initialization options

Choose from the optional set of additional initialization options described in the [Initial setup][7] guide.

#### Step 4: Verify and QA

Once you've deployed this tag, there are two ways you can verify a proper integration:

1. Using Google Tag Manager's [debugging tool][gtm-debugging-tool], you should see the Braze Initialization Tag has been triggered on your configured pages or events.
2. You should see network requests made to Braze, and the global `window.braze` library should now be defined on your webpage.

### Actions Tag template {#actions-tag}

The Braze Actions Tag template lets you trigger custom events, track purchases, change user IDs, and stop or resume tracking for privacy requirements.

![][gtm-actions-tag]

#### Changing user external ID {#external-id}

The **Change User** tag type calls the [`changeUser` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/braze.html#changeuser). 

Use this tag whenever a user logs in or is otherwise identified with their unique `external_id` identifier.

Be sure to enter the current user's unique ID in the **External User ID** field, typically populated using a data layer variable sent by your website.

![A dialog box showing the Braze Action Tag configuration settings. Settings included are "tag type" and "external user ID".][gtm-change-user]

#### Log custom events {#custom-events}

The **Custom Event** tag type calls the [`logCustomEvent` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/braze.html#logcustomevent).

Use this tag to send custom events to Braze, optionally including custom event properties.

Enter the **Event Name** by either using a variable or typing an event name.

Use the **Add Row** button to add event properties.

![A dialog box showing the Braze Action Tag configuration settings. Settings included are "tag type"(custom event), "event name" (button click), and "event properties".][gtm-custom-event]

#### Track purchase {#purchases}

The **Purchase** tag type calls the [`logPurchase` method](https://js.appboycdn.com/web-sdk/latest/doc/modules/braze.html#logpurchase).

Use this tag to track purchases to Braze, optionally including Purchase properties.

The **Product ID** and **Price** fields are required.

Use the **Add Row** button to add Purchase properties.

![A dialog box showing the Braze Action Tag configuration settings. Settings included are "tag type", "external ID", "price", "currency code", "quanitity", and "purchase properties".][gtm-purchase]

#### Stop and resume tracking {#stop-tracking}

Sometimes, you might be required to disable or re-enable Braze tracking on your website, for example, after a user indicates they've opted out of web tracking for privacy reasons.

Use the **Disable Tracking** or **Resume Tracking** tag type to disable or re-enable web tracking, respectively.

#### Custom user attributes {#custom-attributes}

Custom user attributes are not available due to a limitation in Google Tag Manager's scripting language. To log custom attributes, create a Custom HTML tag with the following content:

```html
<script>
window.braze.getUser().setCustomUserAttribute("attribute name", "attribute value");
</script>
```

{% alert important %}
The GTM template does not support nested properties on events or purchases. You can use the preceding HTML to log any events or purchases that require nested properties.
{% endalert %}

#### Default user attributes {#standard-attributes}

Default user attributes, such as a user's first name, should be logged in the same manner as custom user attributes. Ensure the values you're passing in for default attributes match the expected format specified in the [User class][16] documentation.

For example, the gender attribute can accept any of the following as values: `"m" | "f" | "o" | "u" | "n" | "p"`. Therefore to set a user's gender as female, create a Custom HTML tag with the following content:

```html
<script>
window.braze.getUser().setGender("f")
</script>
```

## Upgrading and updating templates {#upgrading}

To upgrade to the latest version of the Braze Web SDK, take the following three steps in your Google Tag Manager dashboard:

1. **Update tag template**<br>Go to the **Templates** page within your workspace. Here you should see an icon indicating an update is available.<br><br>![Templates page showing an update is available][gtm-update-available]<br><br>Click that icon and after reviewing the change, click to **Accept Update**.<br><br>![A screen comparing the old and new tag templates with a button to "Accept Update"][gtm-accept-update]<br><br>
2. **Update version number**<br>Once your tag template has been updated, edit the Braze Initialization Tag, and update the SDK version to the most recent `major.minor` version. For example, if the latest version is `4.1.2`, enter `4.1`. You can view a list of SDK versions in our [changelog][changelog].<br><br>![Braze Initialization Template with an input field to change the SDK Version][gtm-version-number]<br><br>
3. **QA and publish**<br>Verify the new SDK version is working using Google Tag Manager's [debugging tool][gtm-debugging-tool] prior to publishing an update to your tag container.

## Troubleshooting steps {#troubleshooting}

### Enable tag debugging {#debugging}

Each Braze tag template has an optional **GTM Tag Debugging** checkbox which can be used to log debug messages to your webpage's Javascript console.

![Google Tag Manager's Debug tool][gtm-tag-debugging]

### Enter debug mode

Another way to help debug your Google Tag Manager integration is using Google's [Preview mode][14] feature.

This will help identify what values are being sent from your webpage's data layer to each triggered Braze tag and will also explain which tags were or were not triggered.

![The Braze Initialization Tag summary page provides an overview of the tag, including information on which tags were triggered.][gtm-tag-debug-mode]


[2]: https://support.google.com/tagmanager/answer/6103696
[gtm-community-gallery-search]: {% image_buster /assets/img/web-gtm/gtm-community-gallery-search.png %}
[gtm-initialization-tag]: {% image_buster /assets/img/web-gtm/gtm-initialization-tag.png %}
[gtm-actions-tag]: {% image_buster /assets/img/web-gtm/gtm-actions-tag.png %}
[6]: {{ site.baseurl }}/user_guide/administrative/app_settings/manage_app_group/app_group_management/#app-group-management
[7]: {{ site.baseurl }}/developer_guide/platform_integration_guides/web/initial_sdk_setup/#step-2-initialize-braze
[gtm-change-user]: {% image_buster /assets/img/web-gtm/gtm-change-user.png %}
[gtm-custom-event]: {% image_buster /assets/img/web-gtm/gtm-custom-event.png %}
[gtm-purchase]: {% image_buster /assets/img/web-gtm/gtm-purchase.png %}
[gtm-tag-debugging]: {% image_buster /assets/img/web-gtm/gtm-tag-debugging.png %}
[gtm-tag-debug-mode]: {% image_buster /assets/img/web-gtm/gtm-debug-mode.png %}
[14]: https://support.google.com/tagmanager/answer/6107056
[15]: https://tagmanager.google.com/gallery/#/?filter=braze
[16]: https://js.appboycdn.com/web-sdk/latest/doc/classes/braze.user.html
[gtm-update-available]: {% image_buster /assets/img/web-gtm/gtm-update-available.png %}
[gtm-accept-update]: {% image_buster /assets/img/web-gtm/gtm-accept-update.png %}
[gtm-version-number]: {% image_buster /assets/img/web-gtm/gtm-version-number.png %}
[changelog]: https://github.com/braze-inc/braze-web-sdk/blob/master/CHANGELOG.md
[gtm-debugging-tool]: https://support.google.com/tagmanager/answer/6107056?hl=en
