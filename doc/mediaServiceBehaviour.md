![FIMS logo](./FIMS_logo.png) ![EBU logo](./EBU_logo.svg.png) __EBU Tech 3356__ ![AMWA_logo](./AMWA_logo.png)
# Specification of the FIMS Media SOA Framework
### General Description - Version 1.3

_Previous_: [Media service awareness](./mediaServiceAwareness.md) | _Up_: [Contents](./introduction.md) | _Next_: [Annex 1: Future visions](./pipelined.md)

## Media service behaviour

### Common service behaviour

#### Resource-oriented dialogue

Communication in FIMS consists of a dialogue of messages about the FIMS-defined resources (job, queue, service, profile, 
BMObject etc.) between a service and its client. An operation of a media service is executed through a dialogue about a 
job resource between the service provider and the operation requester.

FIMS defines a set of well-known operation implementation patterns that are supported through the SOAP/WSDL and RESTful 
service definitions provided, as described in [the next section](#operation-implementation-patterns). The corresponding 
messages embed representations of resources and/or resource references by identifiers as parameters to operations. The WSDL 
and RESTful service definitions provide for synchronous and asynchronous requests, responses, faults and notifications.

The RESTful approach to FIMS uses standard HTTP verbs with URI paths to achieve the same dialogue, with the resource 
description embedded directly as the message body. The specification of specific bindings between the HTTP verbs, headers, 
status codes, the FIMS-defined operation patterns and a RESTful event mechanism for notifications is provided with this 
version of the FIMS specification.

> Note that one service provider could simultaneously support both SOAP/WSDL and RESTful interaction styles for the same 
  resources.
  
The following figure shows an example sequence diagram of Capture Request/Ack using the SOAP approach.

<img src="./soap_rpc.png" alt= Service Request/Ack (Capture) - SOAP-RPC" width="100%"/>

The RESTful equivalent carries the part of the Capture Request message coloured in green as the body of a message POSTed 
to a service. If successful, the synchronous HTTP response has a status code of 201 Created with a body similar to the 
green part of the Capture Ack message.

The content type of a RESTful FIMS message body shall be represented in either XML or JSON format. Definition of the 
mapping of the FIMS XML schema definitions to JSON format messages is provided in __Section 8.3 FIX LINK__.

The body of an example JSON Capture Request message is shown in the figure below.

<img src="rest_json.png" alt="Service Request (Capture) – REST JSON" width="100%"/>


#### Operation implementation patterns

Service operations defined by FIMS interfaces provide different types of interactions between the service provider and 
the service requester. Each one specifies how the result of an operation is made available to the requester. The 
interface definition along with the input parameters of the operation determines how the service should return the 
response of the operation.

##### Synchronous request/response

In this interaction mode the service client (e.g. a business process) invokes the service to perform an operation passing 
the input parameters (_par1_, ... _parN_) and receives the response in the same communication session as the request. Operations 
that implement this mode of interaction should not be long-running processes to avoid blocking the service client for a long 
period of time and to prevent timeouts that may occur in the communication session. See the figure below.

<img src="./sync_req_res.png" alt="Synchronous request/response" width="80%"/>

Examples of operations that use this type of interaction are the job and queue management operations.

##### Asynchronous request/response with notification

This interaction pattern should be used for long running processes. The request and response associated to the operation 
are exchanged in two different communication sessions. The request session includes the invocation by the client of an 
operation passing the input parameters (_par1_, ... _parN_, _jobGUID_, _notifyAt_) and the acknowledgement by the service 
that the request was received. A service shall return an acknowledgement when it is ready to respond to any further actions 
for that request from the orchestration system. See the figure below.

<img src="./async_rep_res_notify.png" alt="Asynchronous request/response with notification" width="80%"/>

The _jobGUID_ parameter identifies uniquely the job request from the business process (the service client) point of view. The 
_notifyAt_ parameter specifies to the service provider where to send the response message when the operation execution 
completes. It also specifies where to send an error notification message if the service fails during its execution. The 
_notifyAt_ parameter shall be provided for the service to operate in this mode (see `AsyncEndpointType` definition).

A separate communication session is used to send the response message to the address specified by the _replyTo_ element of 
the _notifyAt_ parameter. If an error occurs during the process of the operation an error notification should be issued to 
the endpoint specified by the _faultTo_ element of the _notifyAt_ parameter.

An example of operation that employs this interaction mode is the transform operation of the Transform Media service. See the 
figure below.

<img src="./async_example.png" alt="Example of asynchronous request/response with notification: Transform service" width="60%"/>

WSDL/SOAP requests shall have WSDL/SOAP notifications. RESTful requests shall have RESTful notifications.

For a RESTful asynchronous request, notifications shall be sent to the replyTo and faultTo endpoints provided by the defined
by the RESTful notifications table provided with each service.

The body of a RESTful notification message shall be the service-specific notification type (e.g. `CaptureNotificationType`) 
for a reply and the service-specific fault type (e.g. `CaptureFaultType`) for a fault, as defined by the RESTful notifications 
table.

A RESTful notifications table shall have the following columns:

* __Description__ - An informative description of the reason for the notification.
* __HTTP method__ - The HTTP method that shall be used to send the notification (normally POST).
* __Body__ - Type of the body of the notification that shall be defined by reference to the service-specific XML schema or 
base XML schema.
* __Generation events__ - The events that cause a notification to be sent, such as job creation or job failure.

##### Asynchronous request/response with polling – WSDL/SOAP

This is another interaction pattern for long running processes, where the preferred interaction pattern is not possible 
(e.g. a firewall preventing a service to call back a client). It is similar to the [asynchronous request/response with 
notification](#asynchronous-request/response-with-notification) interaction mode with the exception that a notification 
is not sent by the service when the service completes the operation. The _notifyAt_ parameter shall not be provided for 
the service to operate in this mode. See the following figure.

<img src="async_polling.png" alt="Asynchronous request/response with polling" width="80%"/>

When the client issues the request, it receives an acknowledgement message that contains the ID of the job as identified 
by the service.

In this mode of interaction, it is the responsibility of the client to poll the service, using the _queryJob_ operation, 
to retrieve its queue status, running status and the result of the operation once it is completed. This is achieved using 
the _resourceID_ of the job (that is part of the acknowledgment message). Once the job completes its execution the response 
message for the status request brings back the result of the operation.

The result of the job execution is contained in the jobs element. This element shall be present when retrieving the job 
information after the service completes the operation. The same information that would be part of the notification message 
in the pattern specified in the [previous section](#asynchronous-request/response-with-notification) shall be present in 
this field.

A transform operation of the Transform Media service may be implemented using this type of interaction.

WSDL/SOAP implementation of FIMS services shall support either Notification or Polling.

##### Asynchronous request/response with polling - RESTful

Every resource created by a service has a unique identity through which it can be addressed with a URI. This URI will have 
been returned in the `Location` header of the response message synchronously following the successfully created the resource. 
At any time between when a resource is created and deleted such that it has an addressable endpoint, the status of that 
resource shall be made available through a HTTP GET request to the URI for that resource.

* * *

_Previous_: [Media service awareness](./mediaServiceAwareness.md) | _Up_: [Contents](./introduction.md) | _Next_: [Annex 1: Future visions](./pipelined.md)

Copyright 2015 European Broadcasting Union

Copyright 2015 Advanced Media Workflow Association
