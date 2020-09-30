# How to structure technology support in DYAMAND

Technology support is a multi-faceted problem. DYAMAND offers a structure to break down this support into manageable pieces. To be able to do this, some best practices should be taken into account. 

## Repository layout

In general, all projects related to a particular technology should reside in one git repository named _dyamand-**technologyname**_[^1]. In this repository different projects that together offer the technology support for this technology are put together.

### Technology API

There should be a project that defines the DYAMAND concepts used for this technology, so it can be used by any other project, it should be named _org.dyamand.**technologyname**.api_[^1]. It should include a component that implements an interface named _org.dyamand.**technologyname**.**TechnologyName**_[^1][^2]. This component should make sure all necessary concepts (State prototypes, service prototypes, etc.) are registered when this component is activated, to indicate that everything is ready for other plugins to start doing their work.

[^1]: the technology name should be all lower case in this instance

[^2]: the technology name should be camel case in this instance
