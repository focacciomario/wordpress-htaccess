# wordpress-htaccess
A list of useful .htaccess tricks to improve security of Wordpress sites


- [](#)
- [](#)



## Disable directory browsing 
Many WordPress security experts recommend disabling directory browsing. With directory browsing enabled, hackers can look into your site’s directory and file structure to find a vulnerable file.

```php
Options -Indexes
```
