# PHP-Specific Quirks

This section will focus on PHP in particular and a variety of quirks that might come in useful when attacking a PHP based site.  Whilst the previous pages have dealt with some PHP vulnerabilities, in this instance we will be attacking features of the language, or vulnerable files directly, and vulnerabilities here won't fit as easily into other sections.

## Login Bypasses

### Type Juggling

| **Hash** | **Magic String** | **Magic Hash** |
| :--- | :--- | :--- |
| MD5 | 240610708 | 0e462097431906509019562988736854 |
| SHA1 | 10932435112 | 0e07766915004133176347055865026311692244 |

[https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/PHP%20juggling%20type](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/PHP%20juggling%20type)

[https://github.com/lucyoa/ctf-wiki/tree/master/web/php-quirks](https://github.com/lucyoa/ctf-wiki/tree/master/web/php-quirks)

