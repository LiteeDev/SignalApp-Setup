## Requirements

* Ubuntu 18.04 (Tested)

If you're planning to setup the Contact Discovery, I personally recommend setting that up first.


To be sure to have the latest version of the software.

```
    sudo apt-get update
    sudo apt-get -y install maven build-essential  
  ```

### Install Java

  You will need to create an oracle account to download the actual software.

  ```
  Oracle Java 11 can’t be directly downloaded from Oracle website any more! Now you HAVE to log in and manually download Oracle Java 11 .tar.gz, and place the archive in /var/cache/oracle-jdk11-installer-local/
  ```

  ```bash
    sudo add-apt-repository ppa:linuxuprising/java
    sudo apt-get update
    sudo apt-get install oracle-java11-installer-local
    sudo apt-get install oracle-java11-set-default-local
  ```

### Install Redis
  ```bash
    sudo apt-get install -y redis-server redis-sentinel
    sudo systemctl start redis
    sudo systemctl enable redis
    sudo systemctl start redis-sentinel
    sudo systemctl enable redis-sentinel
  ```


### Install database
	```bash
    sudo apt-get install postgresql postgresql-contrib -y
    sudo -i -u postgres
  	createdb accountdb
  	createdb messagedb
  	createuser --interactive
  	psql
  	ALTER USER "Signal" WITH PASSWORD 'Signal!!';
  ```


## Remotely access to the PostgreSQL database

    To open the port 5432 edit your ``/etc/postgresql/10/main/postgresql.conf``` and change

  	```bash
      listen_addresses='localhost' // listen_addresses='*'
    ```

    To modify the access configuration edit your ``/etc/postgresql/10/main/pg_hba.conf``` and add this to the bottom of the file

    ```bash
      host all all * md5
    ```

    And restart or restart you DBMS

    ```bash
      invoke-rc.d postgresql restart
    ```
