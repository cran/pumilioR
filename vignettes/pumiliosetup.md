<!--
%\VignetteEngine{knitr::knitr}
%\VignetteIndexEntry{Setting up Pumilio for pumilioR and usage}
-->

# Setting up Pumilio and Usage of the *pumilioR* Package #

## Introduction ##

The *pumilioR* package has functions to get data and sound files from the web-based sound archive system Pumilio (http://ljvillanueva.github.io/pumilio). This introduction will provide a brief overview of setting up Pumilio and examples of how to query the system and extract both data and soundfiles.

*pumilioR* requires R version 2.14, or newer, and depends on the packages *XML* and *RCurl*. 

The source of the package is kept at Github: https://github.com/ljvillanueva/pumilioR

The website of the package is http://ljvillanueva.github.io/pumilioR/

## Pumilio Setup ##

To allow the use of the package, log in as an administrator to the Pumilio system. Click the Admin menu link (on the top right edge of the page). On the main settings page of the Admin menu, under "System behavior", there is an option:

 - Allow access using XML: Yes/No
 
Set it to Yes. Now *pumilioR*, and other programs, can read data from the archive using XML.

To limit the access to valid users registered in the Pumilio system, set the option:

 - Who can access via XML: Any/Users

to Users. To allow anyone to query the system set it to Any.

## Check access ##

Before querying the system, first check that it is properly configured and that you can access it, for example:

```
#Load the package
library(pumilioR)

#Check that the system can use pumilioR, returns TRUE if it is allowed and the version supports it
access <- checkVersion(pumilio_URL = "http://pumilio.coquipr.com/pumilio/")

access
```





## Querying the system ##

To query the system, use the functions *getCollections()* or *getSites()* to get a data frame with the collections and the sites, respectively, in the system. Then, you can get the sounds for that collection or site using the function *getSounds()*. For example:

```
#Get all collections
cols <- getCollections(pumilio_URL = "http://pumilio.coquipr.com/pumilio/")

#Show the first row
cols[1,]

#Get all sites
sites <- getSites(pumilio_URL = "http://pumilio.coquipr.com/pumilio/")	

#Show the first row
sites[1,]

#Get all the sounds in the site with the SiteID = 102
sounds <- getSounds(pumilio_URL="http://pumilio.coquipr.com/pumilio/", SiteID=102, type="site")

#Show the first row
sounds[1,]
```

## Download files ##

Once a result is obtained from the system, the audio file can be downloaded using the function *getFile()*. This function needs the result of either *getCollections()* or *getSites()* and the SoundID of the desired file.

```
#First, get a list of results
results <- getSounds(pumilio_URL="http://pumilio.coquipr.com/pumilio/", SiteID=111, type="site")

results[1,]
```

Then, get the file from the first row, identified with the SoundID 4078. The audio file is saved in the working directory.

```
getFile(results, SoundID = 4078)
```




## Apache Basic Authentication ##

Basic authentication can be used on the Apache webserver as a way to limit access to the data by requiring a username and password. This is outside the scope of this vignette. Since version 1.2 of *pumilioR* these sites can be accessed providing the username and password, separated with a colon, to the argument *credentials* in each function.

```

sites <- getSites(pumilio_URL = "http://pumilio.coquipr.com/pumilio2/", credentials = "pumilio:pumilio")

sites[1,]
```

## Pumilio User Authentication ##

Since version 1.2 of *pumilioR* and version 2.7.0 of Pumilio, the system can be accessed providing a valid username and password, separated with a colon, already registered in the system to the argument *pumiliologin* in each function.

```
library(pumilioR)

sites <- getSites(pumilio_URL = "http://pumilio.coquipr.com/pumilio3/", pumiliologin = "pumilio:pumilio")

sites[1,]
```



## Package website ##

Please visit the package website for more information and updates: http://ljvillanueva.github.io/pumilioR/

For suggestions or to report bugs or problems: http://github.com/ljvillanueva/pumilioR/issues

The package page in CRAN is http://cran.r-project.org/web/packages/pumilioR/

- - -

Vignette "Setting up Pumilio and Usage of the *pumilioR* Package" by LJ Villanueva-Rivera

*Version 1.0 (1 February 2014)*
