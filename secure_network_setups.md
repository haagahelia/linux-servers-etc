# Setting up or securing your home networks

## Just generic process:

Should be improved each time, thus, make notes for the next time.

* Use clean and secure Linux laptop for all network setups (don't use your used Windows etc. installation for this)
* Use trusted internet for downloading firmware, updates and docs (e.g. mobile phone internet with VPN from trusted VPN company)
* Do not connect to internet before you have updated and secured everything you can without internet 

## Preparing for securing the Wifi router 

1. Find out what is the model. Write it down, to the version of it, e.g. v3  v3.6
1. Make sure the router is not obsolete. It needs to support the latest secure Wifi standards
1. Use trusted internet to search for information, download the latest firmware and manuals

## Securing the Wifi router 

1. Disconnect the router physically from the network if not already done
1. Use some pin to press the Reset so long that the devices flashes at least four times
1. Then connect the router only to the secure laptop (which is not in any network)
1. Use the web address provided in the router documentation to access the router 
    * Some routers accept both http and https in the address. Others might specifically demand even http without s!
    * You might have to plug into certain specific LAN port of the Router
    * And/or you might have to put your laptop to the same subnet address
    * And/or you might have to set **manual** IP address OR obtain **DHCP** address
    * And/or do some other settings
1. The address might be different based on whether the router is set up as "Router", "Access Point" or "Repeater"
1. Do not connect the router to the network yet even if the wizard tells you. Just 'skip'.

## Inside the Wifi router admin app / web page

1. Change the admin password to a long and secure one, e.g. 25 random A-z,0-9 +_-=! characters. Write **password down on paper ONLY**, and hide it to a locked place out of guest access. Part of the password can be your own "encryption"
1. Update the Firmware of the router. E.g. Maintenance and so on... (keep laptop in powercord, and don't do anything else while firmware transferred and burned)
1. Wait, wait, wait until you are sure the firmware was saved for sure
1. while updating the firmware you might actually not get any notifications, so even if you are sure the firmware is safely burned to the network device, please wait. Otherwise you might have nonrecoverable piece of plastic and metal in your hand.
1. Give the new password from paper when login again
1. Disable WPS (at least WPS with PIN, but preferably also WPS with button)
1. Set up the 1 or 2 WLANs, e.g. 2.4GHz and 5GHz. Give a SSID that does not reveal a) the manufacturer/make/model/version of the router b) your internet provider c) your identity. And the SSID is not provoking any hackers. Best SSID/name is dull and says nothing.
1. Give **long and random** password. It can be also real words, provided that they are: a) not related and b) not from same language c) not correctly spelled d) use small and big letters and numbers e) many, altogether preferably over 33 characters
1. Have you done all you can to make the router updated and safe?
1. Only now connect the router to the network 

## Differences while working with managed wired switch/router (no wireless)

1. There is no WPS
1. "Download" and "Upload" might be used from router point of view. So you "download" the firmware image from laptop to the router. Not "upload" from laptop to router which would also make sense, from other perspective.
1. You might define also VLANs, some ports that are in same "LAN" and some that are in another separate "LAN". With untagged and tagged/trunked ports, and VLANID:s.
1. You might set up also port mirroring meaning you can copy traffic on some port to another for e.g. WireShark. **Note: ** It's illegal to listen to anybody else's internet traffic, including own children's or own spouse's without their specific permission.
1. Set the ntp time servers based on what is your network provider: https://www.digivinkit.fi/operaattorien-aikapalvelimet or use 