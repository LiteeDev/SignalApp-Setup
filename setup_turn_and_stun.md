# Requirements

- Coturn/Stun Servers [source built]

```bash
sudo apt-get -y update
sudo apt-get -y install build-essential sqlite libsqlite3-dev libevent-dev libssl-dev
```

```bash
wget http://turnserver.open-sys.org/downloads/v4.5.0.8/turnserver-4.5.0.8.tar.gz
tar -xzvf turnserver-4.5.0.8.tar.gz
cd turnserver-4.5.0.8
./configure
make && sudo make install
```

# Configuring the Coturn Server


```
sudo cp /usr/local/etc/turnserver.conf turnserver.cnf.bk
nano /usr/local/etc/turnserver.conf
```

Replace these values:

```
listening-port=3478
tls-listening-port=5349

listening-ip=IPV4 ADDRESS

relay-ip=IPV4 ADDRESS
external-ip=IPV4 ADDRESS

realm=domain.pointing.com
server-name=domain.pointing.com

lt-cred-mech
userdb=/var/lib/turn/turndb

oauth
user=test:test


# Lets Encrypt from NGINX
cert=/etc/letsencrypt/live/domain.pointing.com/cert.pem
pkey=/etc/letsencrypt/live/domain.pointing.com/privkey.pem


```


Then to proceed to edit the signal-server configuration with this:

```
turn: # TURN server configuration
  secret: test
  uris:
    - stun:0.0.0.0:3478 # REPLACE 0.0.0.0 with your IP.
    - stun:0.0.0.0:5349
    - turn:0.0.0.0:5349?transport=udp
    - turn:0.0.0.0:3478?transport=udp
```

# Restart Turn & Stuns and also the signal server and Calls and Video Calls will work :)
