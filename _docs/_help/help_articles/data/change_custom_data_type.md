---
nav_title: Changing custom attribute or event data type
article_title: Change Custom Attribute or Event Data Type
page_order: 1

page_type: solution
description: "This help article walks you through how to change the data type of a custom attribute or custom event, and the implications of doing so."
---

# Change custom attribute or event data type

> This article walks you through how to change the data type of a custom attribute or custom event, and the implications of doing so.

After a custom attribute or event is used, you may need to change its data type (for example, change a string to a boolean). The custom attribute or event must not be actively used in any campaign, Canvas, or segment filters before you can change its data type. If you try changing it while it's still referenced, Braze will display a warning modal listing where the attribute is in use.

## Prerequisites

Before changing the data type, complete the following:

- Stop any active campaigns or Canvases that use the attribute in their segments or filters.
- Remove the attribute from all segment, campaign, and Canvas filters that reference it.

## Change the data type

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

- **Existing user data is not retroactively converted.** If the attribute had a value on a user's profile before the change, that value still has the old data type. This can cause users to fall out of segments that filter on the changed attribute, because the filter now expects the new data type. To fix this, update those user profiles with values that match the new data type using the [`/users/track` endpoint]({{site.baseurl}}/api/endpoints/user_data/post_user_track/).
- **New data must match the new data type.** After the change, API calls or SDK events that send values in the old data type will be rejected. Only values matching the new data type will be accepted.

### Example

Let's say a custom attribute `loves_burritos` is originally set to **Automatically Detect**, and the string value `"Yes, I love burritos"` is logged, setting the data type to **String**.

If you change the data type to **Boolean**:
- Sending the string `"I really like burritos"` through `/users/track` will **not** update the profile because the value doesn't match the new boolean data type.
- Sending the value `true` will update successfully because it matches the **Boolean** type.
