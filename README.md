# Scrapping
Simple Web Scraping Framework, based on Curl and str* functions.
Requires PHP ^8.0 (can easily be downgraded to PHP 7)

# What you can do?
There is some functions to simplify your script, they are listed below:

### Global functions
* [upname](#upname) => Format and return string to name formats (first each word character is uppercase)
* [price](#price) => Return float value of and price string of type 'xx$: 9.999,99'
* [accents](#accents) => Replace accentuation with equivalent characters
* [strmstr](#strmstr) => Return string after start3, after start2, after start1
* [strpart](#strpart) => Return middle string between start and end strings
* [strmpart](#strmpart) => Return middle string between start2 and end string, which start2 is after start1

### Scrapping object
* [Scrapping::cache](#scrapping-cache) => can save or return an cache
* [Scrapping::cacheFolder](#scrapping-cahefolder) => you can set a custom folder to cache
* [Scrapping::json](#scrapping-json) => parse and print the response in json
* [Scrapping::isOnSession](#scrapping-inonsession) => tell if an session is set with server
* [Scrapping::load](#scrapping-load) => Reuse session of previews connections
* [Scrapping::useSession](#scrapping-usesession) => Set/Return if session setting is enabled
* [Scrapping::userAgent](#scrapping-useragent) => Set/Return userAgent
* [Scrapping::server](#scrapping-server) => Set/Return server host base URL
* [Scrapping::session](#scrapping-session) => Set/Return server session id
* [Scrapping::hasSession](#scrapping-hassession) => Returns if has a server session id set
* [Scrapping::sesionName](#scrapping-sessionname) => Set/Retrun server session cookie name
* [Scrapping::get](#scrapping-get) => Make and return an get request to the server
* [Scrapping::post](#scrapping-post) => Make and return an post request to the server
* [Scrapping::proccess](#scrapping-proccess) => Proccess the get and post request to organaze data

# How to use?
## upname
```PHP
upname(string $text);
```
Just pass the `text` string to format as parameter and the result will be the formated string. Some exemples bellow, the comment of each block represents the output:
#### Examples:
```PHP
echo upname('lara vieira');
// 'Lara Vieira'
```
```PHP
echo upname('LARA VIEIRA');
// 'Lara Vieira'
```
```PHP
echo upname('LEONARDO DE CÁPRIO');
// 'Leonardo de Cáprio'
```
```PHP
echo upname('DON PEDRO II');
// 'Don Pedro II'
```

## price
```PHP
price(string $text);
```
Just pass the `text` string to format as parameter and the result will be the float value. Some exemples bellow, the comment of each block represents the output:
```PHP
echo price('US$: 3.567,56');
// 3567.56
```
```PHP
echo price('R$: 3.456.234,45');
// 3456234.45
```
```PHP
echo price('Price is R$: 234,45');
// 234.45
```

## accents
```PHP
accents(string $text);
```
Just pass the `text` to format as parameter and the result will be the formated string. An exemple bellow, the comment represent the output:
```PHP
echo accents('Aglomeração, Apóstolo, vô, vó');
// 'Aglomeracao, Apostolo, vo, vo'
```

## strmstr
```PHP
strmstr(
    string $haystack, 
    string $start1, 
    string $start2, 
    string|null $start3=null
);
```
This function return all `haystack` string after `start3` string that is after `start2` string that is after `start1` string (if `start3` is passed) or all `haystack` string after `start2` string that is after `start1` string. The return will include the last start passed, like [strstr](https://www.php.net/manual/pt_BR/function.strstr.php).

This function is something like an stack of [strstr](https://www.php.net/manual/pt_BR/function.strstr.php) functions:
```PHP
strstr(strstr(strstr(haystack, start1), start2), start3)
```
Some exemples bellow, the comment of each block represents the output:
```PHP
echo strmstr('ABC ABC ABC', 'C');
// 'C ABC ABC'
```
```PHP
echo strmstr('ABC ABC ABC', 'B', 'A');
// 'ABC ABC'
```
```PHP
echo strmstr('ABC ABC ABC', 'B', 'B', 'A');
// 'ABC'
```

## strpart

_* This is my favorite one for web-scrapping._
```PHP
strpart(
    string $haystack,
    string|null $start = null,
    string|null $end = null,
    bool $keep_start = false
);
```
This function will return the middle string in `haystack` between the first occurence of `start` string and the first occurence of `end` string after `start` string.

* If `start` string is null, will return everything in `haystack` before the first occurence of `end` string.

* If `end` string is null, will return everything in `haystack` after the first occurence of `start` string.

* If `keep_start` boolean is set to `true`, default is `false`, the function will return as normal, but including `start` string in the retrun's begin.

Some exemples bellow, the comment of each block represents the output:
```PHP
echo strpart('ABC ABC ABC', ' ', ' ');
// 'ABC'
```
```PHP
echo strpart('ABC ABC ABC', ' ');
// 'ABC ABC'
```
```PHP
echo strpart('ABC ABC ABC', end:' ');
// 'ABC'
```
```PHP
echo strpart('ABC ABC ABC', ' ', ' ', true);
// ' ABC'
```
```PHP
echo strpart('<h2>Subtitle<h2>', '>', '<');
// 'Subtitle'
```
```PHP
echo strpart('<div><div>Content</div></div>', '<div>', '</div>');
// '<div>Content'
```

## strmpart

```PHP
strmpart(
    string $haystack,
    string $start1,
    string $start2,
    string|null $end = null,
    bool $keep_start = false
);
```
This function solve the last example of strpart.

This function will return the middle string in `haystack` between the first occurence of `start2` string, that one is after the first occurence of `start1` string, and the first occurence of `end` string after `start2` string.

* If `end` string is null, will return everything in `haystack` after the first occurence of `start2` string, after the first occurence of `start1` string.

* If `keep_start` boolean is set to `true`, default is `false`, the function will return as normal, but including `start2` string in the retrun's begin.

Some exemples bellow, the comment of each block represents the output:
```PHP
echo strmpart('<div><div>Content</div></div>', '<div>', '<div>', '</div>');
// 'Content'
```
```PHP
echo strmpart('<div><div>Content</div></div>', '>', '>', '<');
// 'Content'
```
```PHP
echo strmpart('<a id="link1"><h2 id="text1">Content</h2></a>', '<h2', 'id="', '"');
// 'text1'
```