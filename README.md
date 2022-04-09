# wordpress-htaccess
A list of useful .htaccess tricks to improve security of Wordpress sites


- [Disable directory browsing](#disable-directory-browsing)
- [Protect wp-config.php file](#protect-wp-config.php-file)
- [Ban suspicious IP Addresses](#ban-suspicious-ip-addresses)
- [Disable access to XML-RPC file](#disable-access-to-xml-rpc-file)
- [Block author enumeration in Wordpress](#block-author-enumeration-in-wordpress)
- [Allow only selected IP addresses to access wp-admin](#allow-only-selected-ip-addresses-to-access-wp-admin)
- [Deny image hotlinking](#deny-image-hotlinking)
- [Redirect custom 404 page](#redirect-custom-404-page)
- [Serve WebP images](#serve-webp-images)


## Disable directory browsing 
Many WordPress security experts recommend disabling directory browsing. With directory browsing enabled, hackers can look into your site’s directory and file structure to find a vulnerable file.

```php
Options -Indexes
```

## Protect wp-config.php file
Probably the most important file in WordPress website’s root directory is wp-config.php file. It contains information about WordPress database and how to connect to it. 

```php
<files wp-config.php>
order allow,deny
deny from all
</files>
```

## Ban suspicious IP Addresses 

With some lines in .htaccess file is possible to ban specific IP address that are making high number of requests.  

```php
<Limit GET POST>
order allow,deny
deny from xxx.xxx.xx.x
allow from all
</Limit>
```

## Disable access to XML-RPC file
Each WordPress install comes with a file called xmlrpc.php. This file allows third-party apps to connect to your WordPress site. Most WordPress security experts advise that if you are not using any third party apps, then you should disable this feature.
```php
# Block WordPress xmlrpc.php requests
<Files xmlrpc.php>
order deny,allow
deny from all
</Files>
```

## Block author enumeration in Wordpress
A common technique used in brute force attacks is to run author scans on a WordPress site and then attempt to crack passwords for those usernames.

```php
# BEGIN block author scans
RewriteEngine On
RewriteBase /
RewriteCond %{QUERY_STRING} (author=\d+) [NC]
RewriteRule .* - [F]
# END block author scans 
```

## Allow only selected IP addresses to access wp-admin
The wp-admin folder contains the files required to run the WordPress dashboard. In most cases, your visitors don’t need access to the WordPress dashboard, unless they want to register an account. A good security measure is to enable only a few selected IP addresses to access the wp-admin folder. 

**Remeber to add these lines into .htaccess file located into the /wp-admin/ folder and not into the root folder**

```php
# Limit logins and admin by IP
<Limit GET POST PUT>
order deny,allow
deny from all
allow from 302.143.54.102
allow from IP_ADDRESS_2
</Limit>
```

## Deny image hotlinking
When someone uses your site’s image, your bandwidth is being consumed and most of the time, you’re not even credited for it. This code snippet eliminates that problem and sends this image when a hotlink is detected.
```php
# Prevent image hotlinking script. Replace last URL with any image link you want.
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?my_site.com [NC]
RewriteCond %{HTTP_REFERER} !^http(s)?://(www\.)?my_site.com [NC]
RewriteRule \.(jpg|jpeg|png|gif)$ http://i.imgur.com/MlQAH71.jpg [NC,R,L]
```

## Redirect custom 404 page
```php
# Custom error page for error 403, 404 and 500
ErrorDocument 404 /error.html
ErrorDocument 403 /error.html
ErrorDocument 500 /error.html
```

## Serve WebP images
If WebP images are supported and an image with a .webp extension and the same name is found at the same place as the jpg/png image that is going to be served, then the WebP image is served instead.

*If you want to convert all .png .jpg images use ImageMagick in Apache server*

```php
RewriteEngine On
RewriteCond %{HTTP_ACCEPT} image/webp
RewriteCond %{DOCUMENT_ROOT}/$1.webp -f
RewriteRule (.+)\.(jpe?g|png)$ $1.webp [T=image/webp,E=accept:1]
```
