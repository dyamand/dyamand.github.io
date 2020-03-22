## Prototypes

!!! failure "_1.0.0_ Creating n States prototype each with a unique name and any class should result in n States with the corresponding name and class, retrievable through the PrototypeManager. A State returned through the StateRegistration should equal the State retrieved through the PrototypeManager"
!!! failure "_1.0.1_ Creating n States prototype each with a unique name and any class should result in n newStatePrototype events, each containing the corresponding State prototype. A State returned through the StateRegistration should equal the State retrieved through the PrototypeManager"
!!! failure "_1.0.2_ Creating a State prototype with a name of an already existing State prototype should result in an IllegalStateException"
!!! failure "_1.0.3_ Creating n State prototypes with unique names and disposing all of them should result in 0 State prototypes"
!!! failure "_1.0.4_ The State returned by the StateRegistration should be a prototype"

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/3ebf2abe-7866-4f87-baa7-262bee96ab1d" id="TFD2.WngGQ_c"></iframe></div>

## Services

!!! failure "_1.1.0_ Creating n Service prototypes each with a unique name, any number of states and any number of child Services, should result in n new Services with the corresponding name and class, retrievable through the PrototypeManager. A Service returned through the ServiceRegistration should equal the Service retrieved through the PrototypeManager"
!!! failure "_1.1.1_ Creating n Service prototypes each with a unique name, any number of states and any number of child Services, should result in n newServicePrototype events, each containing the corresponding Services prototype. A Service returned through the ServicesRegistration should equal the Service retrieved through the PrototypeManager"
!!! failure "_1.1.2_ Creating a Service prototype with a name of an already existing Service prototype should result in an IllegalStateException"
!!! failure "_1.1.3_ After creating n Service prototypes with unique names, any number of States and any number of child Services, disposing all of the parent services should result in the original number of Service and State prototypes"
!!! failure "_1.1.4_ Creating a Service prototype with any unregistered State should result in an IllegalStateException. Creating a Service prototype with any unregistered child Service should result in an IllegalStateException"
!!! failure "_1.1.5_ The Service returned by the ServiceRegistration should be a prototype"
!!! failure "_1.1.6_ When disposing a child Service, the parent Service should be disposed as well"
!!! failure "_1.1.7_ When disposing a State prototype, any Service prototype using that State prototype should be disposed as well"
!!! failure "_1.1.8_ When creating a Service prototype with States and/or chlid Services, the returned prototype should contain those exact States and child Services"

<div style="width: 640px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:640px; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/33432a5a-3f57-4c24-9773-bacf5b6f3358" id="FGD2PQciDqT4"></iframe></div>

