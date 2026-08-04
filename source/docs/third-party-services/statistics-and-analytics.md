---
title: Statistics and Analytics
description: NexT User Docs – Third-party Plugins Integration – Statistics and Analytics
---

{% note warning %}
NexT will not send record to analytics service provider as long as the page's host name does not match `url` option set in {% label info@Hexo config file %}. This will prevent local debugging from polluting analytics. Make sure you have configured `url` correctly, otherwise these statistics tools may not work as expected.
{% endnote %}

### Analytics Tools

#### Google Analytics

1. Create an account and log into [Google Analytics](https://analytics.google.com). [More detailed documentation](https://support.google.com/analytics/?hl=en#topic=3544906)
2. Edit {% label primary@NexT config file %} and fill `tracking_id` under section `google_analytics` with your Google track ID. Google track ID always starts with `UA-`.

    ```yml NexT config file
    # Google Analytics
    google_analytics:
      tracking_id: UA-XXXXXXXX-X
      only_pageview: false
      # only needed if you are using `only_pageview` mode, https://developers.google.com/analytics/devguides/collection/protocol/ga4
      measure_protocol_api_secret:
    ```

3. When field `only_pageview` is set to true, NexT will only send `pageview` event to Google Analytics.
The benefit of using this instead of `only_pageview: false` is reduce a external script on your site, which will give you better performance but no complete analytics function.

#### Baidu Analytics (China)

{% tabs baidu-analytics %}
<!-- tab Login → -->
Login to [Baidu Analytics](https://tongji.baidu.com) and locate to site code getting page.
<!-- endtab -->

<!-- tab Script ID → -->
Copy the script ID after `hm.js?`, like the following picture:
![NexT Baidu Analytics](/images/analytics-baidu-id.png)
<!-- endtab -->

<!-- tab NexT Config -->
Edit {% label primary@NexT config file %} and change the value of `baidu_analytics` to your script ID.

```yml NexT config file
# Baidu Analytics ID
baidu_analytics: your_id
```
<!-- endtab -->
{% endtabs %}

#### Cloudflare Web Analytics

Official documentation: https://www.cloudflare.com/web-analytics/

Edit {% label primary@NexT config file %} and change the value of `cloudflare_analytics` to your project ID.

```yml NexT config file
# Cloudflare Web Analytics
cloudflare_analytics:
```

#### Microsoft Clarity Analytics

Official documentation: https://clarity.microsoft.com/

Edit {% label primary@NexT config file %} and change the value of `clarity_analytics` to your project ID.

```yml NexT config file
# Microsoft Clarity Analytics
clarity_analytics: # <project_id>
```

#### Matomo Analytics (Self-managed)

Edit {% label primary@NexT config file %} and fill `server_url` and `site_id` under section `matomo` with the url of your backend server and your customized site ID.

```yml NexT config file
# Matomo Analytics
# See: https://matomo.org/
matomo:
  enable: false
  server_url: # https://www.example.com/
  site_id: # <your site id>
```

#### Umami Analytics (Self-managed)

Umami is a self-hosted web analytics solution. Official documentation: https://umami.is/

Edit {% label primary@NexT config file %}. Fill `script_url` under section `umami` with your tracking script URL, and change the value of `website_id` to your website ID.

```yml NexT config file
# Umami Analytics
umami:
  enable: true
  script_url: # https://umami.example.com/script.js
  website_id: # <your website id>
  host_url: # <your umami site url>
```

#### Plausible Analytics (Self-managed)

Opt for paid managed hosting or self-host it on your server. Official documentation: https://plausible.io/

Edit {% label primary@NexT config file %}. Fill `script_url` under section `plausible` with your tracking script URL, and change the value of `site_domain` to your website domain.

```yml NexT config file
# Plausible Analytics
plausible:
  enable: true
  script_url: # https://plausible.io/js/script.js
  site_domain: # www.example.com
```

### Counting Tools

#### Firebase

Cloud Firestore provides the functionality of visitor statistics without loading the Firebase JavaScript SDK.

{% tabs firestore %}
<!-- tab Create Firestore Database → -->
1. Create a Firestore database in Firebase console.

![Firestore](/images/firestore-1.png)
![Firestore](/images/firestore-2.png)

1. Apply these rules in Firestore Database Rules:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /articles/{document} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasOnly(['count'])
                    && request.resource.data.count is int
                    && request.resource.data.count == 1;
      allow update: if request.resource.data.diff(resource.data).affectedKeys().hasOnly(['count'])
                    && request.resource.data.count is int
                    && request.resource.data.count == resource.data.count + 1;
      allow delete: if false;
    }
  }
}
```

Replace `articles` if you use a different collection name. These rules allow anonymous reads and count increments while preventing changes to other fields and document deletion. They cannot prevent artificial increments or quota abuse; use a trusted backend if stronger protection is required.

![Firestore](/images/firestore-3.png)
<!-- endtab -->

<!-- tab NexT Config -->
Find your Project ID in Firebase console under `Project settings → General`, then edit {% label primary@NexT config file %} and add or change the `firestore` section:

![Firebase Project ID](/images/firebase.png)

Only the Project ID is required. The Web API Key shown on the same page is not used by NexT.

```yml NexT config file
firestore:
  enable: true
  collection: articles #required, a string collection name to access firestore database
  projectId: #required
```

An API key is not required. NexT uses the [Cloud Firestore REST API](https://firebase.google.com/docs/firestore/use-rest-api), and anonymous requests are authorized by your Firestore Security Rules.

Note: Firestore visitor counting records at most one visit per article and browser profile because it uses the browser's localStorage. It is not a reliable unique-visitor metric. To test the counter:

- Clear localStorage in Developer Tools
- Use incognito/private mode
- Use different browsers
<!-- endtab -->
{% endtabs %}

#### Busuanzi Counting (China)

{% tabs busuanzi-counting %}

<!-- tab Global Settings → -->
Edit `busuanzi_count` option in {% label primary@NexT config file %}.
When `enable: true`, global setting is enabled. If `total_visitors`, `total_views`, `post_views` are all `false`, Busuanzi only counts but never shows.
<!-- endtab -->

<!-- tab Site UV Settings → -->
When `total_visitors: true`, it will show site UV in footer. You can also use font-awesome by setting `total_visitors_icon` to the name of the icon.

```yml NexT config file
busuanzi_count:
  total_visitors: true
  total_visitors_icon: fa fa-user
```
<!-- endtab -->

<!-- tab Site PV Settings → -->
When `total_views: true`, it will show site UV in footer. You can also use font-awesome by setting `total_views_icon` to the name of the icon.

```yml NexT config file
busuanzi_count:
  total_views: true
  total_views_icon: fa fa-eye
```
<!-- endtab -->

<!-- tab Per-page PV Settings -->
When `post_views: true`, it will show page PV in post meta. You can also use font-awesome by setting `post_views_icon` to the name of the icon.

```yml NexT config file
busuanzi_count:
  post_views: true
  post_views_icon: far fa-eye
```
<!-- endtab -->
{% endtabs %}
