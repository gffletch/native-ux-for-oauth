---
title: "Native User Experience for OAuth 2.0"
abbrev: "oauth-native-ux"
docname: draft-gffletch-native-ux-for-oauth-latest
category: info

ipr: trust200902
area: General
workgroup: oauth
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: G. Fletcher
    name: George Fletcher
    organization: Capital One Financial
    email: george.fletcher@capitalone.com
(TODO: add other authors)

normative:

informative:


--- abstract

This specification provides trusted applications to request an authorization
interaction that is rendered in a way that is native to the client. This
enables a more seamless authorization experience.


--- middle

# Introduction

Today, both OAuth and OpenID Connect leave the full authentication flow
out of scope and the full responsibility of the Authorization Server (AS).
The client just directs the user to the AS /authorization endpoint and the
user interacts with the AS via some web experience (system browser or webview).

For mobile apps this user experience can be a little jarring when starting the
authentication/authorization flow as the user is transitioned from a native
user experience to a browser context in order to login or perform other
identity related functions.

This specification defines a mechanisms that
allows a client to request an interaction with the Authorization server
that allows for a fully native authorization experience. The requirements
of the flow are managed by the AS and the client with the client rendering
the UX similar to how the browser renders the UX in OAuth flows today.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Description

The goal of this mechanism is to be a lightweight way for the client
to signal to the AS that it would like to use a native protocol rather
than a browser to complete the authorization or authentication (OpenID Connect)
flow. In order to accomplish this a new `response_mode` value is defined along
with a corresponding `protocol` parameter and required messages to start
and complete the flow.

A basic flow is as follows:
1. The client constructs its normal authorization request setting the `response_mode` to `native_ux` and specifying a`protocol` value
2. The client opens an HTTP GET request to the /authorization endpoint passing the authorization request
3. The Authorization Server (AS) recognizes the requested `response_mode` and `protocol` and returns the `initiation` response (JSON)
4. The client processes the `initiation` response and starts the flow
5. When the client has met all the AS criteria for the authorization request, the AS returns the `completion` message
6. The client extracts the `code` and optional `state` message from the `completion` message
7. The client constructs a call to the AS /token endpoint as per normal OAuth and receives back requested tokens

## Response Mode

When a mobile app desires to start an authorization flow with the
Authorization Server (AS), it specifies a new `response_mode` value
of `native_ux` which requests the AS to return the challenge
sequence in a descriptive language that can be interpreted and
displayed in a native experience by the mobile app.

### native_ux

This specification defines the `response_mode` value of `native_ux`

`native_ux`
: In this mode, the authorization challenge sequence, including
consent, is managed via a domain specific language (DSL) with the
last response containing the `code` and `state` parameters.

The native application uses the challenges described by the DSL
to obtain the necessary authorization credentials and complete
the flow required by the AS. At this point the AS returns the
`code` and `state` parameters via the DSL and the native app
completes the OAuth flow with the AS by submitted the required
/token endpoint request.

## Protocol Parameter
Given that there may be multiple defined mechanisms defining the
challenge response protocol, the client indicates which protocol
it supports via the `protocol` parameter. This parameter contains
one or more previously registered (IANA???) protocol names
identifying which protocol the client supports. The AS may choose
any of the supported protocols or return a message indicating
it does not support any of the protocols and the client must use
the standard web browser based experience.

`protocol`
: A space delimited set of protocol values that the client supports.
(TODO: what about versions? Is this too complicated?)

## Initiation Message
When the AS agrees to start the native UX protocol, it returns a JSON
message containing an `mfa_token` and the HTTPS `endpoint` to contact
to start the flow. The `mfa_token` is only valid for this instance of
the mobile app originating on this device. Protcol specifications
may add additional elements to this JSON response.

{
"mfa_token" : "asdfasdfasd",
"endpoint" : "https://idp.example/native"
}

If the AS determines that the risk is too high to complete the flow
via the native experience the AS may return a JSON response that
instructs the client to open a web browser to the specified URL.

{ "error" : "browser_required", "url" : "https://idp.example/8sdfgs34t9sdgfesr" }

The URL returned by the AS MUST be unique to this authentication request
and contain all the semantics of the original /authorization request.

## Completion Message
When the client and AS complete the authorization sequence as defined by
the protocol specification, the AS returns the standard OAuth parameters
via a JSON message. This message contains the OAuth `code` value and optional
`state` value. This claims present in this message may contain required claims
as required by additional OAuth/OpenID Connect specifications and/or the
protocol specification.

{ "code" : "asdfadsfasdf", "state" : "8ckasd24rsadf" }

If the client does not successfully complete the authorization flow, the AS
may return the message directing the client to the web experience or it can
return the following error response. If the client recieves an error it
must inform the user of the error and exit the authorization attempt. The `mfa_token`
will no longer be valid once this message is received.

{ "error" : "failed", "error_description" : "reason for failure" }

## Authentication endpoint (???)
In order to support this flow, the AS defines a new `/native` endpoint
where the client directs requests as defined by the protocol specification.
The authorization mechanisms for this endpoint is the `mfa_token` returned
as part of the `initiation` message.

## Protocol specifications
The actual mechanism as defined by the specified protocol is out of scope for
this specification which just standardizes the framework for establishing
the connection between the client and the AS.

# Security Considerations

## 1st party clients


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
