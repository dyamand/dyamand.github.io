--8<-- "includes/abbreviations.md"
[![Work in progress](https://img.shields.io/badge/status-wip-yellow)](https://www.repostatus.org/#wip)

Plugin developers should be aware of the concepts that form the basis of DYAMAND. All of these concepts should be mapped onto a technology-specific concept when implementing a technology in DYAMAND.

## Discovery & Description

Before anything else can happen, a device should be discovered and their functionalities should be described. Discovery is the notification of the fact that a device is available or no longer available. Description is the act of describing the functionalities of the just discovered device.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embeddedchart/7144454e-958f-4840-8c44-1cc308e9c4e9" id="~VvFNoJLlssQ"></iframe></div>

The discovery orchestration begins with a `Technology Plugin` detecting a new device using whatever mechanism the implemented technology supports.

DYAMAND will ask all `Translation Plugins` whether or not they know how to translate the just discovered device to a more generic counterpart. This part of the orchestration allows Description to happen. `Translation Plugins` offer information about the just discovered device, e.g. a plugin can indicate that the just discovered device can measure temperature.

Once DYAMAND has translated the initial technology-specific device, interested `Application Plugins` are notified of both the technology-specific device as well as its generic counterparts.

## State changes

Once a device has been discovered, the `Technology Plugin` is responsible to monitor changes in the state of the device.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embeddedchart/cb1f63ac-b1e2-404e-8321-25391ccdce1a" id="J3vFTAQ573Ip"></iframe></div>

After detecting the state change, DYAMAND will offer all interested `Translation Plugins` to translate the just altered state to its generic counterpart. In contrast to the translation in the previous section, this time the value of the state is actually translated.

## Control

Changing the state of discovered devices is done using the Control orchestration. As opposed to the orchestrations discussed above, this one actually starts by the application deciding the state of a previously discovered device should be changed.

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://lucid.app/documents/embeddedchart/acdb6d5c-cb82-48fd-b2da-d19d7e107f5f" id="I7vFR4JKfuIZ"></iframe></div>

DYAMAND will check whether all states can be applied by loaded `Technology Plugins`. If that is not the case, DYAMAND will allow `Translation Plugins` to translate the states that need to be applied (generic states) to more specific states that need to be applied. This process is repeated until all states have either been translated or no translations can be found anymore.

Once the states that should be applied have been determined, by optionally translating them, those states are offered to the `Technology Plugin` to actually apply them.

## Coming soon!

- [ ] anatomy of a technology repository
- [ ] use cases
    * [ ] simple targeted technology
    * [ ] general-purpose technology
