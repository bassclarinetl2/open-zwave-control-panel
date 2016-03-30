## INSTALLING FOR Home Assistant (HASS)

Credit given to [frinkiac7](https://thefrinkiac7.wordpress.com/node-red/build-open-zwave-control-panel-on-a-raspberry-pi/) for a lot of the steps.

1. Install Z-Wave  and ensure it is working. Refer to the [Home Assistant Z-Wave component page](https://home-assistant.io/components/zwave/). Make sure that you hang onto the python-openzwave dir
2. Download the libmicrohttpd library
``` bash
cd python-openzwave # The directory you made in step 1
wget ftp://ftp.gnu.org/gnu/libmicrohttpd/libmicrohttpd-0.9.19.tar.gz
```
3. Install the libmicrohttpd library
``` bash
tar zxvf libmicrohttpd-0.9.19.tar.gz
mv libmicrohttpd-0.9.19 libmicrohttpd
cd libmicrohttpd
./configure && make
sudo make install
```
4. Assuming no errors
``` bash
cd ..
```
5. Install libraries needed for open-zwave-control-panel
``` bash
sudo apt-get install libgnutls28 libgnutlsxx28 libgnutls-dev
```
6. Clone the latest control panel
``` bash
# Choice 1 - clone this repository which has necessary cod and Makefile changes already applied
git clone https://github.com/bassclarinetl2/open-zwave-control-panel.git open-zwave-control-panel
# Choice 2 - clone the original repository and change all instances of the function call mktemp to mkdtemp
git clone https://github.com/OpenZWave/open-zwave-control-panel.git
```
7. Edit the Makefile
``` bash
cd open-zwave-control-panel

# for Linux uncomment out next two lines
LIBZWAVE := $(wildcard $(OPENZWAVE)/cpp/lib/linux/*.a)
LIBUSB := -ludev
LIBS := $(LIBZWAVE) $(GNUTLS) $(LIBMICROHTTPD) -pthread $(LIBUSB)

# Also alter the path to the (compiled) openzwave folder (OPENZWAVE := ) accordingly (likely ../openzwave/)
```
8. Copy over the libopenzwave files we need
``` bash
mkdir cpp/lib/linux
cp ./libopenzwave* ./cpp/lib/linux
```

9. Compile
``` bash
cd ..
make
```

10. Link the config directory of our control panel to the libopenzwave one 
``` bash
ln -s /usr/local/share/python-openzwave/config/ config
```

11. Ensuring that HASS isn't running (crossing the streams would be very bad), run  the control panel
``` bash
./ozwcp -p 9999
```

12. Open a browser and navigate to `serverip:9999`

*Note. Stop HASS while using the control panel and make sure you save changes when you're done*
