## Setup SGX Driver

First, you need to clone `linux-sgx-driver` repository to your computer:
```bash
git clone https://github.com/intel/linux-sgx-driver.git
```

Change directory to local repository:
```bash
cd linux-sgx-driver
```

Then you need to use `sgx2` branch:
```
git checkout sgx2
```

You can follow guide provided in the repository: [Build and Install the Intel(R) SGX Driver](https://github.com/intel/linux-sgx-driver/#build-and-install-the-intelr-sgx-driver)

But I will provide example to build and install in Ubuntu 18.04, maybe it will be useful for you to get the general idea.

Check if matching kernel headers are installed:
```bash
$ dpkg-query -s linux-headers-$(uname -r)
```

To install matching headers: 
```bash
$ sudo apt-get install linux-headers-$(uname -r)
```

Build the driver from source code:
```
$ make
```

To install the Intel(R) SGX driver, enter the following command with root privilege:
```
$ sudo mkdir -p "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"    
$ sudo cp isgx.ko "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"    
$ sudo sh -c "cat /etc/modules | grep -Fxq isgx || echo isgx >> /etc/modules"    
$ sudo /sbin/depmod
$ sudo /sbin/modprobe isgx
```


## Setup SGX PSW

[You want to contribute? Please submit GitHub issue and Pull Request.]


## Get Intel SGX PCK Certificate

Environment:
1. fresh Azure Confidential Compute VM (remove checkmark in "Install OpenEnclave things...", using West Europe region)
1. Ubuntu 18.04


First, install DKMS:
```
$ sudo apt install dkms
```


Then installing Intel compiled ready-to-use SGX driver:
```
$ wget https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/sgx_linux_x64_driver_1.12_c110012.bin
$ chmod +x sgx_linux_x64_driver_1.12_c110012.bin

$ sudo ./sgx_linux_x64_driver_1.12_c110012.bin
```


Proceed to install SGX SDK:
```
$ wget https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/sgx_linux_x64_sdk_2.6.100.51363.bin

$ chmod +x sgx_linux_x64_sdk_2.6.100.51285.bin

$ sudo ./sgx_linux_x64_sdk_2.6.100.51285.bin
```


Activate SGX environment (I guess we don't need it):
```
$ source ~/sgxsdk/environment
```


Continue to install SGX PSW:
```
$ sudo apt install libprotobuf10

$ wget https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/libsgx-enclave-common_2.6.100.51363-bionic1_amd64.deb

$ sudo dpkg -i libsgx-enclave-common_2.6.100.51285-bionic1_amd64.deb
```


Alright, please install SGX DCAP:
```
$ wget https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/libsgx-dcap-ql_1.2.100.51313-bionic1_amd64.deb

$ sudo dpkg -i libsgx-dcap-ql_1.2.100.51313-bionic1_amd64.deb
```


Finally, download and run PCKIDRetrieval Tool:
```
$ wget https://download.01.org/intel-sgx/dcap-1.2/linux/dcap_installers/ubuntuServer18.04/PCKIDRetrievalTool_v1.2.100.51313.tar.gz

$ tar xzf PCKIDRetrievalTool_v1.2.100.51313.tar.gz

$ cd PCKIDRetrievalTool_v1.2.100.51313

$ ./PCKIDRetrievalTool
```


In my Azure Confidential Compute VM, it was successfully generated a CSV file with required values. And I can get a PCK Certificate from Intel API. StdOut:
```
Intel(R) Software Guard Extensions PCK ID Retrieval Tool Version 1.2.0

pckid_retrieval.csv has been generated successfully!
```


## Setup Signal CDS (Contact Discovery Service)

You can see sample of YML configuration file for Signal CDS: [config-signal-cds.yml](config-signal-cds.yml)

`spid` is "Service Provider ID" assigned by Intel for you. You can get it by sign-up for an Intel account, and start service subscription in [Intel's SGX self-service portal](https://api.portal.trustedservices.intel.com/EPID-attestation)

Then you will need X.590 certificate and RSA private key. You can generate one by using this command:
```bash
openssl req -x509 -nodes -newkey rsa:4096 -keyout server.key -out server.crt -days 365
```

Please check your `server.key` file value, is it started with string below:
```
-----BEGIN PRIVATE KEY-----
```

If so, we need to convert the key in PKCS#8 format to old PKCS#1, format expected by the CDS program using this command:
```bash
openssl rsa -in server.key -out server_new.key
```

Copy-and-paste value of `server.crt` to `certificate` field inside YML configuration file. For `key` field, you need to copy value from `server.key` or `server_new.key` which started with string below:
```
-----BEGIN RSA PRIVATE KEY-----
```

Then please build your `enclave` using this command:
```
make -C <repository_root>/enclave
```

It will place a file (your compiled CDS SGX enclave) inside this directory:
```
services/src/main/resources/enclave/
```

Your SGX enclave binary file will be named 64-chars long, with ".so" suffix like this:
```
services/src/main/resources/enclave/aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.so
```

