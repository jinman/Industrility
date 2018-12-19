==============================================================================================
Getting started with Multi-tech Conduit mLinux Programmable Gateway to connect to AWS IoT Core
==============================================================================================
MultiConnect® ConduitTM is the industry’s most configurable, manageable gateway for industrial IoT applications. It provides a range of network connectivity choices to your preferred data management platform including carrier approved 4G-LTE, 3G, 2G and Ethernet. 

Following are the steps to connect your Multi-tech Conduit mLinux gateway (MTCDT-LAT1-247L-US) to AWS IoT Core using the AWS IoT Python SDK. 

 
------------------
Set up the gateway
------------------ 
1.	Open the box
2.	Connect the power supply, antennas etc.

----------------------
Connect to the gateway
----------------------
1.	Unscrew the front panel of the gateway
2.	User the MicroUSB cable to connect the debug port to your laptop
3.	Use the following command

    $ screen /dev/tty.usbmodem14101 115200
    Login: mtadm
	  Password: root
    
4.	Connect the ethernet cable from your gateway to your router 
5.	Configure the network connections so the gateway can connect to Internet, use the following command

    $ sudo nano /etc/network/interfaces

    Change the file to
    # The loopback interface
    auto lo iface lo inet loopback

    # Wired interface
    auto eth0 
    iface eth0 inet dhcp
    address 192.168.2.100 
    netmask 255.255.255.0 
    gateway 192.168.1.1
    iii.	sudo shutdown -r now
 
------------------
Set up the gateway
------------------


1.	Check for python (2.7+) and openssl (1.0.1+) version
    $ python --version
    $ python
	  >>>> import ssl
	  >>>> ssl.OPENSSL_VERSION
2.	Update packages
    $ sudo opkg update
  	$ sudo opkg install python-pip
    $ sudo pip install --upgrade pip
3.	Install AWS IoT Phyton SDK
    $ sudo pip install AWSIoTPythonSDK

----------------------------------------
Configure AWS IoT Core using AWS Console
----------------------------------------
 
1.	Create an AWS account, if you don’t have one
2.	Login to AWS Console and "IoT Core"
3.	Onboard> Register a Thing > Linux/OSX and Python
4.	Create a Thing and provide a name
    a.	mThing
5.	Download Linux connection kit to your laptop
6.	Unzip the files
7.	Transfer the files to mLinux gateway
    $ scp /path/to/local/file mtadm@192.168.2.100:/path/to/remote/file
8.	Get the Custom endpoint
    a.	AWS IoT Console> Settings (bottom left)
    b.	something like azrsdf173t-ats.iot.us-west-2.amazonaws.com

-----------------------
Connect to AWS IoT Core
-----------------------

1.	Download the root certificate in the same folder
    $ curl https://www.amazontrust.com/repository/AmazonRootCA1.pem > root-CA.crt
2.	Get one of the AWS IoT Python SDK Sample
    $ wget https://raw.githubusercontent.com/aws/aws-iot-device-sdk-python/master/samples/basicPubSub/basicPubSub.py
3.	Run the command
    $ python basicPubSub.py -e azrsdf173t-ats.iot.us-west-2.amazonaws.com -r root-CA.crt -c mthing.cert.pem -k mthing.private.key
 
    2018-12-13 00:16:14,986 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
    2018-12-13 00:16:15,963 - AWSIoTPythonSDK.core.protocol.mqtt_core - INFO - Performing sync publish...
    2018-12-13 00:16:15,981 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Filling in custom puback (QoS>0) event callback...
    2018-12-13 00:16:16,002 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [puback] event
    2018-12-13 00:16:16,006 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [puback] event
    2018-12-13 00:16:16,014 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - Invoking custom event callback...
    2018-12-13 00:16:16,023 - AWSIoTPythonSDK.core.protocol.internal.clients - DEBUG - This custom event callback is for pub/sub/unsub, removing it after invocation...
    2018-12-13 00:16:16,040 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Dispatching [message] event
    2018-12-13 00:16:16,045 - AWSIoTPythonSDK.core.protocol.internal.workers - DEBUG - Produced [message] event
    Received a new message: 
    {"message": "Hello World!", "sequence": 3}
    from topic: 
    sdk/test/Python

Congratulations, you successfully run the official AWS IoT Python SDK sample on mLinux platform using an Ethernet connection

---------------------------------
Configure the cellular connection
---------------------------------

1.	Disconnect Ethernet as Ethernet typically gets priority over PPP
2.	Insert the SIM card (just above the debug port on front panel)

^^^^^^^^^^^^^^^^
Configure Twilio
^^^^^^^^^^^^^^^^
1.	Create a Twilio account if you don’t have one
2.	Use the following command
a.	mlinux-set-apn “wireless.twilio.com”
b.	pppd call gsm

^^^^^^^^^^^^^^^^^
Configure Soracom
^^^^^^^^^^^^^^^^^
1.	Create a Soracom account if you don’t have one
2.	Use the following command
    $ mlinux-set-apn “soracom.io”
  	$ pppd call gsm
3.	Verify ppp0 is up
  	$ route
	  $ Ifconfig ppp0

-----------------------
Connect to AWS IoT Core
-----------------------
    $ python basicPubSub.py -e azrsdf173t-ats.iot.us-west-2.amazonaws.com -r root-CA.crt -c mthing.cert.pem -k mthing.private.key
 

---------- 
References
----------

1.	http://www.multitech.net/developer/software/mlinux/using-mlinux/mlinux-cellular-connection/
2.	http://www.multitech.net/developer/software/mlinux/getting-started-with-conduit-mlinux/ 
  
Congratulations, you have now successfully published messages using the official AWS IoT Python SDK sample using a cellular connection.

Let the fun begin!
