## The noisy screen with 1000000000000000s errors a second, which isn't needed and made the server actually work.

First, you need to enter the directory of the server.

Proceed to edit this file:

``` /service/src/main/java/org/whispersystems/textsecuregcm/WhisperServerService.java ```

Find this:

``` environment.lifecycle().manage(accountDatabaseCrawler); ```

And replace with this:

``` // environment.lifecycle().manage(accountDatabaseCrawler); ```

And then rebuild the signal server from the main directory with

``` mvn install -DskipTests	 ```

And restart the server and you should see nice happy console.
