---
layout: post
title: Dissecting TikTok Share Links
date: 2025-12-15 10:40 +0000
updated: 2025-12-18 15:09 +0000
categories: [Rambles]
tags: [Unfinished, Cyber, TikTok]
---

You may have noticed that when you open a shared TikTok via link from a friend, it may show "[username] shared this with you".
But they sent you the link through a different platform -- how does TikTok know this?

{: .d-flex .justify-content-center }
![](/assets/img/tiktok-1.png){: .mw-50 .normal}
![](/assets/img/tiktok-2.png){: .mw-50 .normal}

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

These URLs follow the format `https://vm.tiktok.com/[id]]/`, where `id` appears to be a sequential id, encoded into alphanumeric characters.

On further examination, these seem to be TikTok's internal way of shortening URLs in general.
I initially thought these were sequential because the first few characters didn't change as I generated multiple links, but the other characters did.
This is the same when I've generated a link after receiving one from someone else, so would appear to be a server-side feature.

To test this, I took a link generated on my phone and decremented the last character (`Q` -> `P`).
This redirected to what seems to be a parcel tracking link, confirming my theory.

<details markdown="1">
<summary>Click to show a redacted and formatted version of this link.</summary>

```
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
```

</details>

### WWW
The `www` subdomain links seem to be the normal web URLs.
These embed tracking data through the use of URL query parameters (see RFC3986,[^RFC3986] sec. 3.4.), in plaintext.

When a video is 'shared via link' from a computer, the short URL system doesn't appear to be used; the URL generated when not signed in was: `https://www.tiktok.com/@tiktok/video/7574204555320102175?is_from_webapp=1&sender_device=pc`.
When signed in, and spoofing a mobile User-Agent, this only changed slightly to: `https://www.tiktok.com/@tiktok/video/7574204555320102175?is_from_webapp=1&sender_device=mobile&sender_web_id=[integer]`.

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

## Other Network Traffic
When looking at network traffic, I saw some interesting logging (and just a lot of network traffic in general).

Stuff like the following was logged pretty frequently, with a *lot* of duplicated data.
To me, this just seems like bad programming/API design.

I tried to run TikTok on an emulator, but it wasn't happy.

<details markdown="1">
<summary>Click to show a redacted version of the logged data</summary>

```json
[
    {
        "events": [
            {
                "event":"api_request",
                "params":{
                    "page_name":"video_detail",
                    "enter_from":"video_detail",
                    "time_from_origin":20368,
                    "is_landing_page":1,
                    "page_url":"https://www.tiktok.com/@tiktok/video/7574204555320102175",
                    "userAgent":"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:146.0) Gecko/20100101 Firefox/146.0",
                    "user_type_alias":"user",
                    "domain_name":"www.tiktok.com",
                    "page_path":"/@tiktok/video/7574204555320102175",
                    "vidab":"70508271,72437276,73547759,73720540,74444736,74446914,74757744,74780476,74782563,74813746,74852654,74869500,74879361,74879784,74894691,74907604,74926160,74930475,74957748,74970062,74976255,74983392,74983940,74987204,74998869,75001422,75011882,75014049,75026333,75036738,75038625,75041558,75045553,75045730,75045801,75045921,75048550,75074404,75077940,75078721,75081926,75081995,75088453,75093989,75094426,75103454,75108293,75112496,75113530,75114547,75122538,75125466,75144608,75154302,75158103,75160651,75176961,75176984,70138197,70156809,70405643,70772958,71057832,71200802,71381811,71516509,71803300,71962127,72360691,72408100,72854054,72892778,73171280,73208420,73989921,74276218,74844724",
                    "seo_vidab":"73675307",
                    "is_non_personalized":"0",
                    "data_collection_enabled":1,
                    "api_path":"https://www.tiktok.com/api/feedback/v1/newest_reply/?WebIdLastTime=1744733835&aid=1284&app_key=tiktok-web&app_language=en&app_name=tiktok_web&appkey=tiktok-web&browser_language=en-US&browser_name=Mozilla&browser_online=true&browser_platform=Linux%20x86_64&browser_version=5.0%20%28X11%3B%20Ubuntu%29&channel=tiktok_web&clear_unread=false&cookie_enabled=true&data_collection_enabled=true&device_id=[redacted]&device_platform=web_pc&focus_state=true&from_page=video&history_len=1&iid=0&is_fullscreen=false&is_page_visible=true&odinId=[redacted]&os=linux&priority_region=GB&referer=&region=GB&screen_height=1080&screen_width=1920&tz_name=Europe%2FLondon&user_is_login=true&webcast_language=en",
                    "method":"GET",
                    "event_index":1766007997980
                },
                "local_time_ms":1766007430640,
                "is_bav":1,
                "ab_sdk_version":"121663296,121885509",
                "session_id":"[redacted]"
            },
            {
                "event":"api_request",
                "params":{
                    "page_name":"video_detail",
                    "enter_from":"video_detail",
                    "time_from_origin":20356,
                    "is_landing_page":1,
                    "page_url":"https://www.tiktok.com/@tiktok/video/7574204555320102175",
                    "userAgent":"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:146.0) Gecko/20100101 Firefox/146.0",
                    "user_type_alias":"user",
                    "domain_name":"www.tiktok.com",
                    "page_path":"/@tiktok/video/7574204555320102175",
                    "vidab":"70508271,72437276,73547759,73720540,74444736,74446914,74757744,74780476,74782563,74813746,74852654,74869500,74879361,74879784,74894691,74907604,74926160,74930475,74957748,74970062,74976255,74983392,74983940,74987204,74998869,75001422,75011882,75014049,75026333,75036738,75038625,75041558,75045553,75045730,75045801,75045921,75048550,75074404,75077940,75078721,75081926,75081995,75088453,75093989,75094426,75103454,75108293,75112496,75113530,75114547,75122538,75125466,75144608,75154302,75158103,75160651,75176961,75176984,70138197,70156809,70405643,70772958,71057832,71200802,71381811,71516509,71803300,71962127,72360691,72408100,72854054,72892778,73171280,73208420,73989921,74276218,74844724",
                    "seo_vidab":"73675307",
                    "is_non_personalized":"0",
                    "data_collection_enabled":1,
                    "api_path":"https://www.tiktok.com/api/user/following/request/list/?WebIdLastTime=1744733835&aid=1988&app_language=en&app_name=tiktok_web&browser_language=en-US&browser_name=Mozilla&browser_online=true&browser_platform=Linux%20x86_64&browser_version=5.0%20%28X11%3B%20Ubuntu%29&channel=tiktok_web&cookie_enabled=true&count=20&data_collection_enabled=true&device_id=[redacted]&device_platform=web_pc&focus_state=true&from_page=video&history_len=1&is_fullscreen=false&is_page_visible=true&max_time=0&odinId=[redacted]&os=linux&priority_region=GB&referer=&region=GB&screen_height=1080&screen_width=1920&tz_name=Europe%2FLondon&user_id=[redacted]&user_is_login=true&webcast_language=en",
                    "method":"GET",
                    "event_index":1766007997979
                },
                "local_time_ms":1766007430626,
                "is_bav":1,
                "ab_sdk_version":"121663296,121885509",
                "session_id":"[redacted]"
            },
            {
                "event":"api_request",
                "params":{
                    "page_name":"video_detail",
                    "enter_from":"video_detail",
                    "time_from_origin":20346,
                    "is_landing_page":1,
                    "page_url":"https://www.tiktok.com/@tiktok/video/7574204555320102175",
                    "userAgent":"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:146.0) Gecko/20100101 Firefox/146.0",
                    "user_type_alias":"user",
                    "domain_name":"www.tiktok.com",
                    "page_path":"/@tiktok/video/7574204555320102175",
                    "vidab":"70508271,72437276,73547759,73720540,74444736,74446914,74757744,74780476,74782563,74813746,74852654,74869500,74879361,74879784,74894691,74907604,74926160,74930475,74957748,74970062,74976255,74983392,74983940,74987204,74998869,75001422,75011882,75014049,75026333,75036738,75038625,75041558,75045553,75045730,75045801,75045921,75048550,75074404,75077940,75078721,75081926,75081995,75088453,75093989,75094426,75103454,75108293,75112496,75113530,75114547,75122538,75125466,75144608,75154302,75158103,75160651,75176961,75176984,70138197,70156809,70405643,70772958,71057832,71200802,71381811,71516509,71803300,71962127,72360691,72408100,72854054,72892778,73171280,73208420,73989921,74276218,74844724",
                    "seo_vidab":"73675307",
                    "is_non_personalized":"0",
                    "data_collection_enabled":1,
                    "api_path":"https://www.tiktok.com/api/notice/multi/?WebIdLastTime=1744733835&aid=1988&app_language=en&app_name=tiktok_web&browser_language=en-US&browser_name=Mozilla&browser_online=true&browser_platform=Linux%20x86_64&browser_version=5.0%20%28X11%3B%20Ubuntu%29&channel=tiktok_web&cookie_enabled=true&data_collection_enabled=true&device_id=[redacted]&device_platform=web_pc&focus_state=true&from_page=video&group_list=%5B%7B%22count%22%3A20%2C%22is_mark_read%22%3A0%2C%22group%22%3A500%2C%22max_time%22%3A0%2C%22min_time%22%3A1766002430%7D%5D&history_len=1&is_fullscreen=false&is_page_visible=true&odinId=[redacted]&os=linux&priority_region=GB&referer=&region=GB&screen_height=1080&screen_width=1920&tz_name=Europe%2FLondon&user_is_login=true&webcast_language=en",
                    "method":"GET",
                    "event_index":1766007997978
                },
                "local_time_ms":1766007430617,
                "is_bav":1,
                "ab_sdk_version":"121663296,121885509",
                "session_id":"[redacted]"
            }
        ],
        "user":{
            "user_unique_id":"[redacted]",
            "user_type":12,
            "user_id":"[redacted]",
            "user_is_login":true,
            "device_id":"[redacted]"
        },
        "header":{
            "app_id":1988,
            "os_name":"Linux",
            "os_version":"",
            "device_model":"",
            "network_type":"",
            "language":"en-US",
            "region":"GB",
            "platform":"web",
            "sdk_version":"5.3.9_oversea",
            "sdk_lib":"js",
            "timezone":0,
            "tz_offset":0,
            "resolution":"1920x1080",
            "browser":"Firefox",
            "browser_version":"146.0",
            "referrer":"",
            "referrer_host":"",
            "width":1920,
            "height":1080,
            "screen_width":1920,
            "screen_height":1080,
            "custom":{
                "session_id":"[redacted]",
                "webid_created_time":"[redacted]",
                "app_language":"en",
                "page_name":"video_detail",
                "vv_avg_per_day":1,
                "ff_duration_avg":792,
                "device":"pc",
                "launch_mode":"direct",
                "traffic_type":"no_referrer",
                "source":"",
                "referer_url":"direct",
                "browserName":"firefox",
                "hevcSupported":1,
                "cpu_core":16,
                "hardware_score":8.12,
                "total_score":8.12,
                "release":"1.5.0.588",
                "user_is_login":1,
                "priority_region":"GB",
                "landing_page":"video_detail",
                "is_kids_mode":0,
                "region":"GB",
                "extraData":"{}",
                "is_downgrade":false,
                "is_sharing":0,
                "vidab":"70508271,72437276,73547759,73720540,74444736,74446914,74757744,74780476,74782563,74813746,74852654,74869500,74879361,74879784,74894691,74907604,74926160,74930475,74957748,74970062,74976255,74983392,74983940,74987204,74998869,75001422,75011882,75014049,75026333,75036738,75038625,75041558,75045553,75045730,75045801,75045921,75048550,75074404,75077940,75078721,75081926,75081995,75088453,75093989,75094426,75103454,75108293,75112496,75113530,75114547,75122538,75125466,75144608,75154302,75158103,75160651,75176961,75176984,70138197,70156809,70405643,70772958,71057832,71200802,71381811,71516509,71803300,71962127,72360691,72408100,72854054,72892778,73171280,73208420,73989921,74276218,74844724",
                "seo_vidab":"73675307",
                "b_c":"0",
                "max_touch_points":0
            }
        },
        "local_time":1766007430,
        "verbose":1
    }
]
```

</details>

## Notes and References
[^video]: [www.tiktok.com/@tiktok/video/7574204555320102175](https://www.tiktok.com/@tiktok/video/7574204555320102175)

[^url-decoded]: 
    URLs can only contain a certain set of characters, so -- to work around this -- a technique called "URL encoding" is used.
    Essentially, non-allowed characters (such as a space (` `) or curly braces (`{`, `}`)) are replaced by a percent symbol (`%`) and their hexadecimal representation in US-ASCII, as per RFC3986.[^RFC3986]

[^RFC3986]: Berners-Lee, T., Fielding, R.T. and Masinter, L.M. (2005) Uniform Resource Identifier (URI): Generic Syntax. RFC 3986. Internet Engineering Task Force. doi: [10.17487/RFC3986](https://doi.org/10.17487/RFC3986).
