---
layout: post
title: Dissecting TikTok Share Links
date: 2025-12-15 10:40 +0000
categories: [Rambles]
tags: [Unfinished]
---

You may have noticed that when you open a shared TikTok via link from a friend, it may show "[username] shared this with you".
But they sent you the link through a different platform -- how does TikTok know this?

I think that when you click "share by link", the client sends certain data to the server, which creates a corresponding link.
The opener's client can then receive this data when they load the video.

## Why do I think this?
I was going through a tunnel on a train, watching TikToks, and wanted to send one to a friend.
As I was in a tunnel, I didn't have immediate internet access, so when I clicked "share by link", I got a very long URL.
This is what leads me to think that the short URLs are server-generated.

## Methods
So, I did some testing.

I chose the pinned video on the official TikTok account,[^video] and generated multiple share links on my phone (signed-in to my account).
I then opened these links on my laptop (not signed-in) to inspect HTTP traffic, as well as generating other links.

## What's in the URLs?
I identified two types of URLs: `vm.` and `www.`.

### VM
URLs for the subdomain `vm.tiktok.com` appear to be the "proper" share links.
When opened, they return a HTTP 301 redirect to a `www.` URL.

These URLs follow the format `https://vm.tiktok.com/[id]]/`, where `id` appears to be a sequential id, encoded into alphabetic characters.

On further examination, these seem to be TikTok's internal way of shortening URLs in general.
I initially thought these were sequential because the first few characters didn't change as I generated multiple links, but the other characters did.
This is the same when I've generated a link after receiving one from someone else, so would appear to be a server-side feature.

To test this, I took a link generated on my phone and decremented the last character (`Q` -> `P`).
This redirected to what seems to be a parcel tracking link, confirming my theory.

<details>
<summary>Click to show a redacted and formatted version of this link.</summary>

{% highlight plain %}
https://www.tiktok.com/
    ?gd_label=edm_platform_mercurius_out_for_delivery_logistics
    &msg_id=out_for_delivery_logistics_Email_20251215[redacted]
    &redirect_url=aweme://ec/mall
        ?after_landing_draw=1
        &bcm_crm_id=[redacted]
        &crm_scenario=out_for_delivery_logistics
        &crm_type=email
        &diversion_config={
            "diversion_id":"shop_tab_order_push_sparkview",
            "diversion_target_name":"mall",
            "diversion_target_resources":[
                {
                    "name":"mall_sparkview",
                    "type":"sparkview",
                    "uuid":"mall_sparkview_order_detail",
                    "config":{
                        "schema":"aweme://webcast_webview_popup
                            ?__status_bar=true
                            &awe_falcon=sh
                            &container_color_auto_dark=1
                            &disableBounces=1
                            &disable_ttnet_proxy=0
                            &enable_pull_down_close=1
                            &height=78%
                            &hide_nav_bar=1
                            &hide_source=1
                            &lock_variable_height_expanded=1
                            &mask_bg_color=00000080
                            &radius=12
                            &should_full_screen=1
                            &show_mask=1
                            &url=https://lf16-gecko-source.tiktokcdn.com/obj/byte-gurd-source-sg/tiktok/fe/live/fe_tiktok_ecommerce_order_detail_pia/pia_home.html
                                ?__status_bar=true
                                &_pia_=1
                                &container_color_auto_dark=1
                                &disableBounces=1
                                &hide_nav_bar=1
                                &hide_source=1
                                &main_order_id=[redacted]
                                &mask_page_name=mall
                                &page_type=popup
                                &should_full_screen=1
                                &trackParams={
                                    "enter_from":"email",
                                    "enter_from_info":"email",
                                    "enter_method":"email_retargeting_[redacted]_0_out_for_delivery_logistics_E",
                                    "previous_page":"deeplink_ug_retargeting_GB_email_mallhalforderdetail_out_for_delivery_logistics_E",
                                    "source_previous_page":"deeplink_ug_retargeting_GB_email_mallhalforderdetail_out_for_delivery_logistics_E",
                                    "template_id":"out_for_delivery_logistics_E"
                                }
                                &use_forest=1
                                &use_new_container=1
                                &use_preload_resource_h5=1
                                &use_spark=1
                                &variable_height=100%",
                            "image":{}
                        }
                    },
                    {
                        "name":"mall_orders_base_tooltip",
                        "type":"base_tooltip",
                        "config":{
                            "text":"Check your orders here",
                            "image":{},
                            "duration":3,
                            "action":"openOrderDetail",
                            "order_id":"[redacted]"
                        }
                    }
                ]
            }
            &enter_method=click_push
            &gd_label=edm_platform_mercurius_out_for_delivery_logistics
            &landing_deeplink=aweme://webcast_webview_popup
                ?__status_bar=true
                &awe_falcon=sh
                &container_color_auto_dark=1
                &disableBounces=1
                &disable_ttnet_proxy=0
                &enable_pull_down_close=1
                &height=78%
                &hide_nav_bar=1
                &hide_source=1
                &lock_variable_height_expanded=1
                &mask_bg_color=00000080
                &radius=12
                &should_full_screen=1
                &show_mask=1
                &url=https://lf16-gecko-source.tiktokcdn.com/obj/byte-gurd-source-sg/tiktok/fe/live/fe_tiktok_ecommerce_order_detail_pia/pia_home.html
                    ?__status_bar=true
                    &_pia_=1
                    &container_color_auto_dark=1
                    &disableBounces=1
                    &hide_nav_bar=1
                    &hide_source=1
                    &main_order_id=[redacted]
                    &mask_page_name=mall
                    &page_type=popup
                    &should_full_screen=1
                    &trackParams={
                        "enter_from":"email",
                        "enter_from_info":"email",
                        "enter_method":"email_retargeting_[redacted]_0_out_for_delivery_logistics_E",
                        "previous_page":"deeplink_ug_retargeting_GB_email_mallhalforderdetail_out_for_delivery_logistics_E",
                        "source_previous_page":"deeplink_ug_retargeting_GB_email_mallhalforderdetail_out_for_delivery_logistics_E",
                        "template_id":"out_for_delivery_logistics_E"
                    }
                    &use_forest=1
                    &use_new_container=1
                    &use_preload_resource_h5=1
                    &use_spark=1
                    &variable_height=100%
                    &mall_extra_info={
                        "campaign_id":"0",
                        "mall_homepage_visited_type":"2",
                        "mall_landing_page":"mallhalforderdetail",
                        "mall_out_source":"email.out_for_delivery_logistics_E.out_of_app",
                        "material_id":"out_for_delivery_logistics_E",
                        "template_id":"out_for_delivery_logistics_E",
                        "track_id":"out_for_delivery_logistics_Email_20251215[redacted]"
                    }
                    &msg_id=out_for_delivery_logistics_Email_20251215[redacted]
                    &previous_page=deeplink_ug_retargeting_GB_email_mallhalforderdetail_out_for_delivery_logistics_E
                    &utm_campaign=email_mercurius_out_for_delivery_logistics
                    &utm_source=edm_platform_mercurius_out_for_delivery_logistics
                    &scene=ecom_email_redesign_order_details_page
                    &utm_campaign=email_mercurius_out_for_delivery_logistics
                    &utm_source=edm_platform_mercurius_out_for_delivery_logistics
{% endhighlight %}
</details>

### WWW
The `www` subdomain links seem to be the normal web URLs.
These embed tracking data through the use of URL query parameters (see RFC3986,[^RFC3986] sec. 3.4.), in plaintext.

When a video is 'shared via link' from a computer, the short URL system doesn't appear to be used; the URL generated when not signed in was: `https://www.tiktok.com/@tiktok/video/7574204555320102175?is_from_webapp=1&sender_device=pc`.
When signed in, and spoofing a mobile User-Agent, this only changed slightly to: `https://www.tiktok.com/@tiktok/video/7574204555320102175?is_from_webapp=1&sender_device=mobile&sender_web_id=[integer]`

However, when shared from my (signed-in) phone without internet connection, the following link was generated (redacted and formatted).

```
https://www.tiktok.com/@tiktok/video/7574204555320102175
    ?_r=1
    &u_code=[alphanumeric string]
    &preview_pb=0
    &sharer_language=en
    &_d=[alphanumeric string]
    &share_item_id=[integer]
    &source=h5_m×tamp=[integer]
    &user_id=[integer]
    &sec_user_id=[probable base 62 or 64 string]
    &item_author_type=2
    &social_share_type=0
    &utm_source=copy
    &utm_campaign=client_share
    &utm_medium=android
    &share_iid=[integer]
    &share_link_id=[UUID]
    &share_app_id=1233
    &ugbiz_name=MAIN
    &ug_btm=b5836,b2878
    &link_reflow_popup_iteration_sharer={"click_empty_to_play":1,"dynamic_cover":1,"follow_to_play_duration":-1.0,"profile_clickable":1}
    &enable_checksum=1
```

## Notes and References
[^video]: [www.tiktok.com/@tiktok/video/7574204555320102175](https://www.tiktok.com/@tiktok/video/7574204555320102175)

[^url-decoded]: 
    URLs can only contain a certain set of characters, so -- to work around this -- a technique called "URL encoding" is used.
    Essentially, non-allowed characters (such as a space (` `) or curly braces (`{`, `}`)) are replaced by a percent symbol (`%`) and their hexadecimal representation in US-ASCII, as per RFC3986.[^RFC3986]

[^RFC3986]: Berners-Lee, T., Fielding, R.T. and Masinter, L.M. (2005) Uniform Resource Identifier (URI): Generic Syntax. RFC 3986. Internet Engineering Task Force. doi: [10.17487/RFC3986](https://doi.org/10.17487/RFC3986).
