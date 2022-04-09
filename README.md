# wordpress-htaccess
A list of useful .htaccess tricks to improve security of Wordpress sites


- [Disable directory browsing](#disable-directory-browsing)
- [Protect wp-config.php file](#protect-wp-config.php-file)
- [Ban suspicious IP Addresses](#ban-suspicious-ip-addresses)
- [Disable access to XML-RPC file](#disable-access-to-xml-rpc-file)
- [Block author enumeration in Wordpress](#block-author-enumeration-in-wordpress)


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

