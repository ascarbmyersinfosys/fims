![FIMS logo](./FIMS_logo.png) ![EBU logo](./EBU_logo.svg.png) __EBU Tech 3356__ ![AMWA_logo](./AMWA_logo.png)
# Specification of the FIMS Media SOA Framework
### General Description - Version 1.3

_Previous_: [Overview](./overview.md) | _Up_: [Contents](./introduction.md) | _Next_: [High-level architectures](./mediaServiceManagement.md)

## High-level architectures

### What does it mean to be 'FIMS Compliant'?

Compliance with FIMS requires that the following conditions shall be met:

1. Messages shall be implemented as described in the FIMS service description framework.
2. Message names or defined schemas shall not be modified.
3. Messages shall not be processed in a means that is dependent on schema extensions.
4. In the case of SOAP implementations, each service interface shall comply with the FIMS WSDL. For REST implementations, 
   messages shall comply with the header and body REST mappings defined by the specification for each service. Message 
   payloads shall validate with the FIMS schema.
5. In the case of SOAP implementations, each service shall implement at least the mandatory parts of the FIMS WSDL 
   definitions. Similarly, REST implementations of services shall implement at least the mandatory parts defined in 
   the common FIMS schemas. Definitions shall not be extended except for fields that explicitly allow vendor extensions.

### Reference architecture(s)

The following figure shows he overall reference model of the FIMS framework, including areas not specified in FIMS.

<img src="./FIMS_ref_model.png" alt="FIMS framework reference model" width="100%"/>

To take account of the specific needs of audiovisual media, the FIMS framework has to consider aspects of media services 
that may differ from conventional IT-based SOA such as:

1. Asynchronous communication to properly handle the very large amounts of data associated with AV media in a timely manner.
2. Media Container/Media Descriptor to associate AV metadata with AV essence.
3. Media Infrastructure Service (Resource Manager) for appropriate media handling.
4. Media Bus for AV data exchange, in addition to the conventional ESB for XML message exchange.
5. Media Bus M-SLA (Media-Service Level Agreement), in addition to the conventional SLA in ESB.
6. Security guidelines for secure media handling.

Not all of these aspects are addressed in this version of the specification. It is a goal of the FIMS Task Force to address 
all aspects in future versions.

[Media-centric issues](#meida-centric-issues) are discussed below.

The reference architecture is capable of working with different SOA architectural approaches, supporting SOAP/WSDL and 
RESTful interaction styles.

### Media-centric issues

* * *

_Previous_: [Overview](./overview.md) | _Up_: [Contents](./introduction.md) | _Next_: [High-level architectures](./mediaServiceManagement.md)

Copyright 2015 European Broadcasting Union

Copyright 2015 Advanced Media Workflow Association
