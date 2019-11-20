# Requirements

- KeyStore Explorer = https://keystore-explorer.org/

# Enter the directory for the Signal-Android and proceed to the /src/raw/. You will fine whisper.store, proceed to open this file with keystore and you will be prompted for the password which is whisper.

# The next step requires, you to be on the Signal Server

# Enter SSL Certificate Location
```
sudo su
cd /etc/letsencrypt/live/{domain name}
```

# convert certificate chain + private key to the PKCS#12 file format
```
openssl pkcs12 -export -out keystore.pkcs12 -in fullchain.pem -inkey privkey.pem
```

# Download keystore PKCS12 file from the server and procced to keystore explorer.

# Press the import key pair and procced with selecting the file, you've just downloaded and fill the in the required information.

# Now, rebuild the application and your SSL key issues will be fixed.
