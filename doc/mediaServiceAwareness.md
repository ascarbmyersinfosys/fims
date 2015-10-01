![FIMS logo](./FIMS_logo.png) ![EBU logo](./EBU_logo.svg.png) __EBU Tech 3356__ ![AMWA_logo](./AMWA_logo.png)
# Specification of the FIMS Media SOA Framework
### General Description - Version 1.3

_Previous_: [High-level architectures](./mediaServiceManagement.md) | _Up_: [Contents](./introduction.md) | _Next_: [Media service awareness](./mediaServiceBehaviour.md)

## Media service awareness

### Service registry

A registry is useful for tracking deployed services throughout an infrastructure. By making this registry machine-readable 
and well defined the overall system can make intelligent decisions. The registration mechanism is out of scope of version 1 
of the FIMS specification, it may be a complex data exchange mechanism or a human editing a file. 
  
#### Listing registered services

A FIMS system may provide a registry, in which case it shall contain a line-delimited list of URLs. This list shall be made 
available via an HTTP GET.

Each URL within the list shall return a response compliant to the service description document defined in [the following 
section](#service-description).

The URL within the registration file may provide a hint as a `Service` query string indicating the Service Description that
will be discovered when the URL is queried.

For example, the registry could contain URLs such as the following, including those that assist in the discovery of, say, a 
Capture service:

* http://some.dns.entry/
* http://some.dns.entry/AnyURL
* http://192.168.1.1:8888/SomeURL?Service=Capture
* http://192.168.1.1:8888/SomeOtherURL?Service=Transform

A single description discovery URL shall only describe a single service endpoint and a single capability .

For example, the following is invalid:

* http://192.168.1.1:1234/YetANotherURL?Service=Transform,Capture

### Service description

Services should support a mechanism for publishing their capabilities and configuration to systems (and humans). Without 
understanding what a service will accept or can process, orchestration choices become constrained and out-of-band decisions 
are required. Whenever information is moved out-of-band, the ability of a system to automatically react is compromised. 
Either the system must wait for human intervention, or the information must be brought back into the scope of the system 
via extension.

__CHECK THE FOLLOWING STATEMENT__

For this reason, the FIMS specification defines a machine-readable model for providing information about operations a 
service will accept along with the current configuration of the service. This model references EBU Core Metadata.

The [service registry](#service-registry) defines URL locations that shall be used to return a service description XML 
document. When requested, a service shall return a document describing the capabilities and configuration of that service. 
Such attributes of a service may change over time, and the document returned may vary during the lifetime of a deployed 
service, for example, due to system faults, licensing restrictions, software upgrades or system administration restrictions 
to operations. 

> Note: As of FIMS v1.3, the service description may be structured as a SMPTE Media Device and Control Framework service 
  capability description. See the definition of `serviceInformation` as part of the `ServiceType` resource.
  
A single service instance may implement multiple classes of operation (such as a service that can perform transformation as 
well as plain transfer services) the service shall not register these on a single endpoint. Such a service shall register 
multiple endpoints.

* * *

_Previous_: [High-level architectures](./mediaServiceManagement.md) | _Up_: [Contents](./introduction.md) | _Next_: [Media service awareness](./mediaServiceBehaviour.md)

Copyright 2015 European Broadcasting Union

Copyright 2015 Advanced Media Workflow Association
