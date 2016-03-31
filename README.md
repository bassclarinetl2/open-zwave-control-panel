## INSTALLING FOR Home Assistant (HASS)

Credit given to [frinkiac7](https://thefrinkiac7.wordpress.com/node-red/build-open-zwave-control-panel-on-a-raspberry-pi/) for a lot of the steps.

- Install Z-Wave  and ensure it is working. Refer to the [Home Assistant Z-Wave component page](https://home-assistant.io/components/zwave/). Make sure that you hang onto the python-openzwave dir
- Download the libmicrohttpd library
``` bash
cd python-openzwave # The directory you made in step 1
wget ftp://ftp.gnu.org/gnu/libmicrohttpd/libmicrohttpd-0.9.19.tar.gz
```
- Install the libmicrohttpd library
``` bash
tar zxvf libmicrohttpd-0.9.19.tar.gz
mv libmicrohttpd-0.9.19 libmicrohttpd
cd libmicrohttpd
./configure && make
sudo make install
```
 Assuming no errors
``` bash
cd ..
```
- (Debian Wheezy/Ubuntu Trusty) Install libraries needed for open-zwave-control-panel
``` bash
sudo apt-get install libgnutls28 libgnutlsxx28 libgnutls-dev
```
- (Debian Jessie/Ubuntu Wily) Install libraries needed for open-zwave-control-panel
``` bash
sudo apt-get install libgnutls-deb0-28 libgnutlsxx28 libgnutls28-dev
```

- Clone the latest control panel
``` bash
# Choice 1 - clone this repository which has necessary cod and Makefile changes already applied
git clone https://github.com/bassclarinetl2/open-zwave-control-panel.git open-zwave-control-panel
# Choice 2 - clone the original repository and change all instances of the function call mktemp to mkdtemp
git clone https://github.com/OpenZWave/open-zwave-control-panel.git
```
- Edit the Makefile
``` bash
cd open-zwave-control-panel

# Toggle comment for JESSIE
GNUTLS := -lgnutls
#GNUTLS := -gnutls

# for Linux uncomment out next two lines
LIBZWAVE := $(wildcard $(OPENZWAVE)/cpp/lib/linux/*.a)
LIBUSB := -ludev
LIBS := $(LIBZWAVE) $(GNUTLS) $(LIBMICROHTTPD) -pthread $(LIBUSB)

# Also alter the path to the (compiled) openzwave folder (OPENZWAVE := ) accordingly (likely ../openzwave)
```
- Copy over the libopenzwave files we need
``` bash
cd ../openzwave
mkdir -p cpp/lib/linux
cp ./libopenzwave* ./cpp/lib/linux
cd ../open-zwave-control-panel
```

- Compile
``` bash
cd ..
make
```

- Link the config directory of our control panel to the libopenzwave one 
``` bash
# For custom packaged installs of OZW
ln -s /usr/local/lib/python3.4/dist-packages/libopenzwave-0.3.0b8-py3.4-linux-x86_64.egg/config/ config
# For directly installed versions of OZW
ln -s /usr/local/share/python-openzwave/config

```

- Ensuring that HASS isn't running (crossing the streams would be very bad), run  the control panel
``` bash
./ozwcp -p 9999
```

- Open a browser and navigate to `serverip:9999`

*Note. Stop HASS while using the control panel and make sure you save changes when you're done*
