#TYPO3 NEOS Install
====

Installing TYPO3 Neos on domainfactory - managed server

<br/>
###get composer
<pre>$:curl -s https://getcomposer.org/installer | /usr/local/bin/php5-54LATEST-CLI
</pre>
<br/>

###get TYPO3 NEOS
```
$:/usr/local/bin/php5-54LATEST-CLI composer.phar create-project typo3/neos-base-distribution TYPO3-Neos
```
or update it.

```
$:/usr/local/bin/php5-54LATEST-CLI composer.phar -d=TYPO3-Neos update
```
<br/>

###point your domain to /TYPO3-Neos/Web via domainfactory domain settings
or Set up a virtual host inside your apache.conf, and then restart apache

<br/>
###edit environment settings

```
$: cd TYPO3-Neos
$: vim flow
```

change like the following lines:

```
- #!/usr/bin/env php
+ #!/usr/local/bin/php5-54LATEST-CLI
```

set configuration file:

```
$:cd Configuration/
$:cp Settings.yaml.example Settings.yaml
```
and change like the following (use spaces instead of tabs for indentation):

```
TYPO3:
  Flow:
    persistence:
      backendOptions:
        host: mysql5.<yourdomain.com>
        driver: pdo_mysql

    core:
      phpBinaryPathAndFilename: /usr/local/bin/php5-54LATEST-CLI
    resource:
      publishing:
        fileSystem:
          mirrorMode: copy
```

###open index.php in /Web folder
and change it like the following

```
...
elseif (substr($rootPath, -1) !== '/') {
   $rootPath .= '/';
}
 
putenv ("FLOW_REWRITEURLS=1");
putenv ("FLOW_CONTEXT=Production");
 
require($rootPath . 'Packages/Framework/TYPO3.Flow/Classes/TYPO3/Flow/Core/Bootstrap.php');
... 
```

<br/>

###PHP Settings
set "magic_quotes_gpc" to Off in your php.ini <br>
set "date.timezone" e.g. "Europe/Berlin" <br>
set "memory_limit" to 256M
____

<br/><br/>

###thats it!
========
go to

```
http://yourdomain.com/setup 
```
and follow the on-screen instructions!
