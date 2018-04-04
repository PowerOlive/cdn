# Server side configuration for video download

Instructions for configuring and verifying CORS settings for different web servers are below. 

The HolaCDN FAQ includes a [section dedicated to CORS] (http://holacdn.com/faq#imp-cors). If you have additional questions, email cdn-help@hola.org, or skype:holacdn

### CORS test method #1: using develeopr console commands
First enable hola_debug. Either by adding url param `?hola_debug=true` when accessing the page,  or type in console `hola_cdn.debug_mode()`.

Now enter in console:
```
hola_cdn.test_cors()
```

The current status of CORS settings will be displayed.

### CORS test method #2: using HolaCDN Control Panel 'Integration' tool
The HolaCDN Control Panel features an easy way to check your HolaCDN deployment, including CORS status. Simply go to the [Integration tool] (http://holacdn.com/cp/tools/integration), enter the URL of page that contains the video, select 'CDN mode' from the drop down box and start the test.

The current status of CORS settings will be displayed in the test results. You can click on the summary to see detailed test results.

### Interpreting the CORS test results

If you see ```CORS status: full support```, your CORS settings are correct.

Otherwise, continue following this guide to configure and verify your settings using one of the following guides:
- [CORS setup for most web servers](#configMostServers)
- [CORS setup for Amazon AWS S3](#configAWS)
- [CORS setup for MaxCDN](#configMaxCDN)
- [CORS setup for Fastly](#configFastly)
- [CORS setup for LeaseWeb](#configLeaseweb)
- [CORS setup for Wowza](#configWowza)

### <a name="configMostServers"></a>CORS configuration for most web servers

For step by step instructions regarding how to enable CORS on different web servers, see the [[original CORS documentation](http://enable-cors.org/server.html)] (http://enable-cors.org/server.html). Make sure you add all the required headers (**header fields are comma separated**), not just '*' referenced in the generic instructions:

- Access-Control-Allow-Origin: * 
- Access-Control-Allow-Methods: HEAD, GET, OPTIONS
- Access-Control-Expose-Headers: Content-Range, Date, Etag, Cache-Control, Last-Modified
- Access-Control-Allow-Headers: Content-Type, Origin, Accept, Range, Cache-Control
- Access-Control-Max-Age: 600
- Timing-Allow-Origin: *

It is also recommended to add Cache-Control header with max-age directive, otherwise the default of 600 seconds will be used:

- Cache-Control: max-age=86400

After committing the configuration changes, [verify it works](https://docs.google.com/document/d/1aLFKeQIbs3EnSMXKjXQrseVf_XkeOlDNMDYbxnbH2N8/edit?usp=sharing)

**Finished with CORS configuration? You can [go back to enabling HolaCDN] (https://github.com/spark-mv/cdn/blob/master/install.md#4-allow-holacdn-to-download-content)**

### <a name="configApache"></a>CORS configuration for Apache
To add the CORS authorization to the header using Apache, simply add the following line inside either the `<Directory>`, `<Location>`, `<Files>` or `<VirtualHost>` sections of your server config (usually located in a *.conf file, such as httpd.conf or apache.conf), or within a .htaccess file:

```
Header set Access-Control-Allow-Origin "*"
Header set Access-Control-Allow-Methods "HEAD, GET, OPTIONS"
Header set Access-Control-Expose-Headers "Content-Range, Date, Etag, Cache-Control, Last-Modified"
Header set Access-Control-Allow-Headers "Content-Type, Origin, Accept, Range, Cache-Control"
Header set Access-Control-Max-Age "600"
Header set Timing-Allow-Origin "*"
```

After this passes, you may need to reload Apache to make sure your changes are applied by running the command

`sudo service apache2 reload`

Altering headers requires the use of mod_headers. Mod_headers is enabled by default in Apache, however, you may want to ensure it's enabled by run

`a2enmod headers`

### <a name="configNginx"></a>CORS configuration for Nginx
A sample conf statements is given below. Note that they have to be in the same nested level
```
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'HEAD, POST, GET, OPTIONS';
  add_header 'Access-Control-Expose-Headers' 'Content-Range, Date, Etag, Cache-Control, Last-Modified';
  add_header 'Access-Control-Allow-Headers' 'Content-Type, Origin, Accept, Range, Cache-Control';
  add_header 'Access-Control-Max-Age' '600';
  add_header 'Timing-Allow-Origin' '*';
  add_header 'Cache-Control' 'max-age=86400';
  if ($request_method = 'OPTIONS') {
      return 204;
  }
```

### <a name="configAWS"></a>CORS configuration for Amazon AWS

Please refer to [these instructions](https://docs.google.com/document/d/1RrWA7gfCLDXcjgCQOQRGs0A3IRSlvn3Ktu9zHp9j-po/edit?usp=sharing) for a step-by-step guide on how to configure and verify CORS for Amazon AWS S3.

For Amazon's official document on setting CORS on S3, see [here](http://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html).

If you're using CloudFront, please refer to [Amazon's documentation](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/header-caching.html#header-caching-web-cors) for to how to configure CloudFront to respect cross-origin resource sharing (CORS).

### <a name="configMaxCDN"></a>CORS configuration for MaxCDN

Please refer to [these instructions] (https://docs.google.com/document/d/12wQr79BXaTJ4WF9_-oueEW9Cx0FMvsNOCcMBifzfzIE/edit?usp=sharing) for a step-by-step guide on how to setup CORS on MaxCDN.

### <a name="configFastly"></a>CORS configuration for Fastly

Please refer to [these instructions] (https://docs.google.com/document/d/1zIMMQe4lSy7ubcWv9FDxzsWw6acjv9_o6yZrhgoyit0/edit?usp=sharing) for a step-by-step guide on how to setup CORS on Fastly.

### <a name="configLeaseweb"></a>CORS configuration for LeaseWeb

Please refer to [these instructions] (https://docs.google.com/document/d/16jw0ORZr7i1ZfumzVcnGF1mua8J48IuVPCNm_7jhoAE/edit?usp=sharing) for a step-by-step guide on how to setup CORS on LeaseWeb.

### <a name="configWowza"></a>CORS configuration for Wowza

Please refer to [these instructions] (https://www.wowza.com/docs/how-to-enable-cross-origin-resource-sharing-cors-for-http-based-streams) for a step-by-step guide on how to setup CORS on Wowza.

**Finished with CORS configuration? You can [go back to enabling HolaCDN] (https://github.com/spark-mv/cdn/blob/master/install.md#4-allow-holacdn-to-download-content)**

