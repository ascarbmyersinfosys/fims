![FIMS logo](./FIMS_logo.png) ![EBU logo](./EBU_logo.svg.png) __EBU Tech 3356__ ![AMWA_logo](./AMWA_logo.png)
# Specification of the FIMS Media SOA Framework
### General Description - Version 1.3

## Executive Summary

This document describes a vendor-neutral common framework for implementing Interoperable 
Media Services using a Service Oriented Architecture (SOA) based system, supporting interoperability, 
interchangeability and reusability of media specific services.

## Contents

* [Scope](./scope.md)
* [Conformance language](./conformanceLanguage.md)
* [Reference documents](./referenceDocuments.md)
* [Overview](./overview.md)
* [High-level architectures](./high-levelArchitectures.md)
  * [What does it mean to be 'FIMS compliant'?](./high-levelArchitectures.md#what-does-it-mean-to-be-fims-compliant)
  * [Reference architectures](./high-levelArchitectures.md#reference-architectures)
  * [Service Taxonomy](,/high-levelArchitectures.md#service-taxonomy)
  * [Scenarios: Mediations, dynamic resource allocation](./high-levelArchitectures.md#scenarios-mediations-dynamic-resource-allocation)
  * [Media-centric issues](./high-levelArchitectures.md#media-centric-issues)
    * [Asynchronous operations for long running process](./high-levelArchitectures.md#asynchronous-operations-for-long-running-process)
    * [Process scheduling and resource management](./high-levelArchitectures.md#process-scheduling-and-resource-management)
    * [Media Bus](./high-levelArchitectures.md#media-bus)
    * [Security](./high-levelArchitectures.md#security)
* [Media service management](./mediaServiceManagement.md)
  * [Media service management](./mediaServiceManagement.md#media-service-management)
    * [Service lifecycle](./mediaServiceManagement.md#service-lifecycle)
    * [Deployment](./mediaServiceManagement.md#deployment)
    * [Decomissioning](./mediaServiceManagement.md#decomissioning)
    * [Replacement / upgrade](./mediaServiceManagement.md#replacement--upgrade)
    * [FIMS Interface Versionning](./mediaServiceManagement.md#fims-interface-versioning)
  * [Job management](./mediaServiceManagement.md#job-management)
    * [Lifecycle of a job](./mediaServiceManagement.md#lifecycle-of-a-job)
    * [Management commands](./mediaServiceManagement.md#management-commands)
    * [Resource-oriented data model](./mediaServiceManagement.md#resource-oriented-data-model) 
* [Media service awareness](./mediaServiceAwareness.md)
  * [Service registry](./mediaServiceAwareness.md#service-registry)
    * [Listing registered services](./mediaServiceAwareness.md#listing-registered-services) 
  * [Service description](./mediaServiceAwareness.md#service-description)
* [Media service behaviour](./mediaServiceBehaviour.md)
* [Annex 1: Future visions - Pipelined media services](./pipelined.md)

## Notes

The user’s attention is called to the possibility that implementation and compliance with this 
specification may require use of subject matter covered by patent rights. By publication of this 
pecification, no position is taken with respect to the existence or validity of any claim or of 
any patent rights in connection therewith. The AMWA, including the AMWA Board of Directors, shall 
not be responsible for identifying patents for which a license may be required by an AMWA specification 
or for conducting inquiries into the legal validity or scope of those patents that are brought to 
its attention.


Copyright 2015 European Broadcasting Union

Copyright 2015 Advanced Media Workflow Association

