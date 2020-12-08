# DYAMAND-Bluetooth plugin

## Context
Bluetooth is a short range wireless technology standard  using UHF radio waves in the ISM band. Bluetooth Low Energy (BLE) has been part of the standard since version 4.0. This plug-in makes no distinction between the two.

### GATT
The DYAMAND Bluetooth plugin is capable of discovering Bluetooth (and BLE) devices. Data acquisition is done through the device's provided GATT attributes.
A device may serve as a GATT server that hosts GATT attributes. GATT servers organize data in services, which hold characteristics. Some services have been standardized so the service determines which characteristics should be present. Standardized GATT profiles determine which services should be present.

Attribute UUIDs determine whether the service / characteristic is standardized or not (note that the UUID determines WHAT the attribute is but does not serve as an ID for which attribute it is). Attributes have a 16 byte UUID. In case of standardized attributes this UUID takes the form of a 16 or 32 bit UUID which is extended by the default Bluetooth Base UUID. Specifically: 

> xxxxxxxx-0000-1000-8000-00805F9B34FB

Where the xxxxxxxx is replaced by the standardized 16 or 32 bit (including leading 0s) UUID. The full list of standardized 16 UUID numbers can be found [here](https://btprodspecificationrefs.blob.core.windows.net/assigned-values/16-bit%20UUID%20Numbers%20Document.pdf)

For instance the following UUID specifies a PLX Spot-Check Measurement:

> 00002a5e-0000-1000-8000-00805F9B34FB

This characteristic is part of the pulse oximeter service:

> 00001822-0000-1000-8000-00805F9B34FB

Services and characteristics are described in [xml format](https://www.bluetooth.com/wp-content/uploads/Sitecore-Media-Library/Gatt/Xml/Characteristics/org.bluetooth.characteristic.plx_spot_check_measurement.xml)

### BlueZ over DBus

This plug-in uses BlueZ over DBus for all discovery and data access. DBus is an inter-process communication and remote procedure call mechanism and BlueZ is a Bluetooth library registered on DBus.

## Discovery

GATT services and characteristics can be discovered when a device is connected. When starting from a 'clean slate' scenario (i.e. no devices, services or characteristics are known) an adapter needs to be powered on and a device needs to be discovered. Once discovered the plug-in needs to connect to it. Its services can then be resolved. A device has a flag (ServicesResolved) that indicates if the device's services have been resolved. This way it is possible to distinguish between a device with unresolved services and a device without services.

The BlueZ ObjectManager keeps track of all found devices, services, characteristics etc and is used to do discovery of new devices, services and characteristics. It also knows when devices, services and characteristics are removed.

No heartbeat mechanism is used and services and characteristics persist in BlueZ even when the device has disconnected. However, you are guaranteed that the services and characteristics are unchanged when the device is reconnected (even if it power cycled).

### Identification
Bluetooth devices are (globally) uniquely identified through their MAC address (format= xx:xx:xx:xx:xx:xx). The device services and characteristics are (per device) uniquely identified through their handle, which is a 16-bit identifier. Therefore the combination of the device MAC address and service handle uniquely identifies a service.

## Data model

DYAMAND models GATT services as DYAMAND services and groups them by Bluetooth device. The GATT characteristics are the states of the DYAMAND services. The services are identified by an identifier generated as '{mac_address}/service{service_handle}' (e.g. '00:1C:05:FE:44:6B/service0006'). Characteristics are identified through their characteristic handle (e.g. 'char0007').

## Control

## Health information

## Available libraries

### BlueZ
BlueZ is a Bluetooth stack for Linux kernel-based family of operating systems. Its goal is to program an implementation of the Bluetooth wireless standards specifications for Linux. As of 2006, the BlueZ stack supports all core Bluetooth protocols and layers. It was initially developed by Qualcomm, and is available for Linux kernel versions 2.4.6 and up. In addition to the basic stack, the bluez-utils and bluez-firmware packages contain low level utilities such as dfutool which can interrogate the Bluetooth adapter chipset to determine whether its firmware can be upgraded.

hidd is the Bluetooth human interface device (HID) daemon.

BlueZ is licensed under the GNU General Public License (GPL), but reported to be on its way toward switching to the GNU Lesser General Public License (LGPL). 

### Visualization
The d-feet program (available through aptitude) allows users to visualize all d-bus objects. Searching for 'bluez' will show all discovered bluez objects and allow users to read data and call methods.

