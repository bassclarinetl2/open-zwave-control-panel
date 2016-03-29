INSTALLING FOR HASS
(Mostly taken from https://thefrinkiac7.wordpress.com/node-red/build-open-zwave-control-panel-on-a-raspberry-pi/)

1. Install and make sure that zwave is working. https://home-assistant.io/components/zwave/ Make sure that you hang onto the python-openzwave dir
2. cd into the python-openzwave directory and run: wget ftp://ftp.gnu.org/gnu/libmicrohttpd/libmicrohttpd-0.9.19.tar.gz
3. run tar zxvf libmicrohttpd-0.9.19.tar.gz
4. run mv libmicrohttpd-0.9.19 libmicrohttpd
5. cd libmicrohttpd
6. run ./configure, make and sudo make install
7. assuming no errors, cd ..
8. run sudo apt-get install  libgnutls28 libgnutlsxx28 libgnutls-dev
9. git clone https://github.com/bassclarinetl2/open-zwave-control-panel.git open-zwave-control-panel
   or git clone https://github.com/OpenZWave/open-zwave-control-panel.git, and edit webserver.cpp and change all instances of mktemp to mkdtemp
10. cd openzwave-control-panel
11. edit the Makefile and (for linux) ensure that the following lines are uncommented:

# for Linux uncomment out next two lines
LIBZWAVE := $(wildcard $(OPENZWAVE)/cpp/lib/linux/*.a)
LIBUSB := -ludev
LIBS := $(LIBZWAVE) $(GNUTLS) $(LIBMICROHTTPD) -pthread $(LIBUSB)

also alter the path to the (compiled) openzwave folder (OPENZWAVE := )accordingly (likley ../openzwave/)

12. under this folder mkdir the path cpp/lib/linux and cp ./libopenzwave* ./cpp/lib/linux

13. cd back to the open-zwave-control-panel folder and run make.

14. now, link the config directory with ```ln -s /usr/local/share/python-openzwave/config/ config```

15. ensuring that HASS isnt running (crossing the streams would be very bad), run ```./ozwcp -p 9999```
16 open a browser and navigate to serverip:9999

