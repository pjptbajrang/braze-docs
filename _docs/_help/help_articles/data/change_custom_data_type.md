---
nav_title: Changing custom attribute or event data type
article_title: Change Custom Attribute or Event Data Type
page_order: 1

page_type: solution
description: "This help article walks you through how to change the data type of a custom attribute or custom event, and the implications of doing so."
---

# Change custom attribute or event data type

> This article walks you through how to change the data type of a custom attribute or custom event, and the implications of doing so.

After a custom attribute or event is in use, you may need to change its data type (for example, changing a string to a boolean). The custom attribute or event must not be actively used in any campaign, Canvas, or segment filters before you can change its data type. If you attempt to change it while it's still referenced, Braze will display a warning modal listing where the attribute is currently in use.

## Prerequisites

Before changing the data type, make sure you've completed the following:

- Stop any active campaigns or Canvases that use the attribute in their segments or filters.
- Remove the attribute from all segment, campaign, and Canvas filters that reference it.

## Changing the data type

1. Go to **Data Settings** and select **Custom Attributes** or **Custom Events**.
2. Find the attribute or event you want to change, and select the edit icon to update its data type to the desired value.
3. Update the attribute values on existing user profiles to match the new data type. You can do this with the [`/users/track` endpoint]({{site.baseurl}}/api/endpoints/user_data/post_user_track/). User profile data is not retroactively converted to the new data type, so any users with the old data type value will no longer match segment filters expecting the new type.
4. Reapply filters to the relevant segments, campaigns, and Canvases.
5. Reactivate any campaigns or Canvases you stopped in the prerequisites.

![Custom Attributes tab to edit attribute or data type]({% image_buster /assets/img/change_custom_attribute.png %})

{% alert important %}
The ability to prevent automatic detection from updating the custom attribute data type is currently in early access. Contact your customer success manager if you're interested in participating in this early access.
{% endalert %}

## What to expect

After you update the data type:

- **The change takes effect immediately.** In most cases, the new data type is active within seconds to a few minutes.
- **Existing user data is not retroactively converted.** If the attribute had a value on a user's profile before the change, that value still has the old data type. This can cause users to fall out of segments that filter on the changed attribute, because the filter now expects the new data type. To fix this, update those user profiles with values that match the new data type using the [`/users/track` endpoint]({{site.baseurl}}/api/endpoints/user_data/post_user_track/).
- **New data must match the new data type.** After the change, API calls or SDK events that send values in the old data type will be rejected. Only values matching the new data type will be accepted.

### Example

A custom attribute `loves_burritos` is originally set to **Automatically Detect**, and the string value `"Yes, I love burritos"` is logged, setting the data type to **String**.

If you change the data type to **Boolean**:
- Sending the string `"I really like burritos"` through `/users/track` will **not** update the profile, because the value doesn't match the new boolean data type.
- Sending the value `true` will update successfully, because it matches the **Boolean** type.

## Limitations

- **Nested custom attributes:** You cannot change the data type of a [nested custom attribute]({{site.baseurl}}/user_guide/data/custom_data/custom_events/nested_objects/) through the dashboard. Contact your customer success manager or [Braze Support]({{site.baseurl}}/braze_support/) for assistance.
- **Cross-workspace attributes:** If the same custom attribute exists in multiple workspaces within your company, changing the data type in one workspace may cause unexpected behavior. Contact [Braze Support]({{site.baseurl}}/braze_support/) before changing the data type of attributes that are shared across workspaces.
