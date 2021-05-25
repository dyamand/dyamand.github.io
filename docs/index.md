--8<-- "includes/abbreviations.md"
[![Work in progress](https://img.shields.io/badge/status-wip-yellow)](https://www.repostatus.org/#wip)

# Welcome to DYAMAND

In the last decades, the world has made significant technological advances. When we talk about the use of digital technology, it all started with the introduction of the PC, making it possible for the general public to use computing power, that was previously reserved for big research centers, in the comfort of their own home. After the introduction of the Internet, people could get connected with each other to exchange information all around the world. Gradually this evolved into a world in which nearly every human being is continuously connected with the rest of the world using a device that is significantly smaller and more powerful than PCs originally were.

A similar evolution has been happening in the world of devices. Traditionally referred to as IoT, it has a significant impact on multiple application domains, ranging from home automation over eHealth, industry 4.0 and many more to smart cities. What all of these application domains have in common is that they face similar problems when bringing new applications to the market. Integration of devices is complex since they not only differ in what they enable, the way an application needs to connect to each and every device is potentially different, how devices model the data they sent is diverse and how they can be controlled is yet another source of variation. This heterogeneity is a source of frustration for application developers and inhibits innovation.

DYAMAND simplifies the world of connected devices to make the creation of new, innovative applications easier.

## Flexibility

As discussed above, there are a lot of factors to take into account when developing an application that makes use of connected devices. To tackle these challenges, DYAMAND is completely plugin-based. If you want to create a plugin, please have a look at the [documentation for plugin developers](./plugindevelopers).

### Device types

Devices can measure some aspect of the environment or control the environment around them in some way. If this sounds vague, it is. A simple example is a temperature sensor: a device that measures the temperature of the environment it is deployed in and that does not allow controlling anything. A light on the other hand can be switched on and off, can optionally be dimmed or can even change color. These simple examples show how DYAMAND looks at devices, devices have state that can or can not be changed. DYAMAND in and of itself is not aware of any of these types (temperature, color, etc.), this information is provided at runtime by so-called _Type Plugins_.

### Technologies

Every application domain has a particular set of technologies that are dominant. Again, DYAMAND in and of itself is not aware of any technology out there, _Technology Plugins_ help DYAMAND in understanding the ins and outs of every technology out there. A _Technology Plugin_ is responsible to discover a device, extract technology-specific data out of the device and implement the mechanisms to control the device.

### Abstraction

In previous sections, _Type Plugins_ and _Technology Plugins_ were introduced. However, these two types of plugins alone do not simplify interacting with devices. To achieve this, DYAMAND introduces _Translation Plugins_. Based on knowledge about the technology on the one hand and the generic type(s) it supports on the other hand, a _Translation Plugin_ makes sure that the technology-specific data as extracted by a _Technology Plugin_ can be translated to a generic concept provided by a _Type Plugin_.

### Applications

Once again, DYAMAND is not directly aware of what it should do with the data it receives or what to control, applications are basically another type of plugins, surprisingly called _Application Plugins_. Another way to develop applications is to use the provided GraphQL API. If you want to create an application using the external GraphQL API, please have a look at the [documentation for application developers](./applicationdevelopers).

## Coming soon!

Based on the abstraction DYAMAND creates, a number of generic services can be offered. More information on this will be provided soon.
