#TYPO3 Neos Install
====

Installing TYPO3 Neos on domainfactory - managed server

<br/>
###get composer
<pre>$:curl -s https://getcomposer.org/installer | /usr/local/bin/php5-53STABLE-CLI
</pre>

<br/>

###get TYPO3 Neos alpha 4
```
$:/usr/local/bin/php5-53LATEST-CLI composer.phar create-project --dev --stability alpha typo3/neos-base-distribution TYPO3-Neos-1.0-alpha4
```
<br/>

###point your domain to /Web via domainfactory domain settings
or Set up a virtual host inside your apache.conf, and then restart apache

<br/>
###edit environment settings
```
$:cd TYPO3-Neos-1.0-alpha4
$:vi flow
```
```
change the first line from -> #!/usr/bin/env php
to -> #!/usr/local/bin/php5-53STABLE-CLI
    
:wq!
```

```
$:cd Configuration/
$:cp Settings.yaml.example Settings.yaml
$:vi Settings.yaml
```
```
//set database host
db: 'mysql5.<yourdomain.com>

//uncomment core: & phpBinaryPathAndFilename
//and change phpBinaryPathAndFilename to: 
core:
    phpBinaryPathAndFilename: '/usr/local/bin/php5-53STABLE-CLI'

:wq!
```

```
$:cd Development/
$:cp Settings.yaml.example Settings.yaml
$:vi Settings.yaml
```
```
//set dbname, dbuser, dbpassword:
dbname: '<dbname>'
user: '<dbuser>'
password: '<password>'

:wq!
```
<br/>
###run flow
```
$:./flow help
```

###migrate database
```
$:./flow doctrine:migrate 
```

###kickstart site package
```
./flow site:kickstart Your.Demopage Your.Demopage    
```

###import site package 
```
$:./flow site:import --package-key My.Demopage 

```

###list your sites
```
$:./flow site:list
```

###create neos backend user
```
$:./flow user:create <username> <password> <firstname> <lastname> 
```

###add administrator role to backenduser
```
$:./flow user:addrole <username> Administrator
```

<br/>
###thats it!
========
###now visit<br/>

http://yourdomain.com/ -> frontend<br/>
http://yourdomain.com/neos -> backend
