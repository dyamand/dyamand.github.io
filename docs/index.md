--8<-- "includes/abbreviations.md"
[![Work in progress](https://img.shields.io/badge/status-wip-yellow)](https://www.repostatus.org/#wip)

- [ ] Add problem statement
- [ ] Discuss the different extension points DYAMAND offers
    * [ ] Types
    * [ ] Technologies
    * [ ] Applications

# Welcome to DYAMAND

DYAMAND is a software component that enables developers to integrate connected devices into their application.

## Writing an application

Writing an application on top of DYAMAND consists of different steps. First of all, as an application, you are typically interested in the [lifecycle of services](./applications/services/).

## Extending DYAMAND

DYAMAND can be extended in various ways. First and foremost, SDP plugins extend DYAMAND by adding support for a particular protocol. This is necessary since DYAMAND in and of itself is not able to communicate with any device. The very first thing an SDP plugin should do is [discover new services](./plugins/discovery/).



