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

normative:

informative:


--- abstract

This specification provides trusted applications to request an authorization
interaction that is rendered in a way that is native to the client. This
enables a more seamless authorization experience.


--- middle

# Introduction

For mobile apps the user experience can be a little jarring when starting the
authentication/authorization flow as the user is transitioned from a native
user experience to a browser context in order to login or perform other
identity related functions. This specification defines a mechanisms that
allows a client to request an interaction with the Authorization server
that allows for a fully native authorization experience.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Overview
The Native Interaction Request is used to obtain an authorization code which can be 
used with the Authorization Code Grant flow defined in [@RFC6749] to obtain an access
and refresh token. 

It allows a native client (e.g. a mobile phone application) to implement the user 
experience for authenticating the user in a native application, without opening a 
browser. 

The native authentication SHOULD make use of Direct Interaction flows and MAY be 
extended to other mechnisms to authenticate the user before issuing and Authorization Code. 

The Authorization Code may be exchanged for an access and a refresh token.

~~~ ascii-art
     +----------+
     | Resource |
     |   Owner  |
     |          |
     +----------+
          ^
          |
         (B)
     +----|-----+         Native Interaction      +---------------+
     |         -+----(A)------- Request --------->|               |
     |  Native  |                                 | Authorization |
     |  Client  |                                 |    Server     |
     |          |          User Authenticates     |               |
     |          |        with Direct Interaction  |               |
     |         -+----(B)-------- Flow ----------->|               |
     |          |                                 |               |
     |          |                                 |               |
     |         -+----(C)-- Authorization Code ---<|               |
     |          |                                 +---------------+     
     |          |                                      ^      v
     |          |                                      |      |
     |          |                                      |      |
     |          |>---(D)-- Authorization Code ---------'      |
     |          |          & Redirection URI                  |
     |          |                                             |
     |          |<---(E)----- Access Token -------------------'
     +----------+       (w/ Optional Refresh Token)
~~~
Figure 1: Native Client Initiated Direct Interaction Grant

The flow illustrated in Figure 1 includes the following steps:

   (A)  The native client initiates the flow by initiating an authorization
        request with response mode "native_ux" to the authorization endpoint.  
        The native client includes its client identifier, requested scope, 
        and local state similar to the Authroization Code Flow. Note the 
        redirect URI is not sent.

   (B)  The authorization server authenticates the resource owner and obtains 
        the resource owners consent via the native client UX and 
        grants or denies the native client's authorization request.

   (C)  Once the resource owner grants access, the authorization
        server returns an authorization code and any local state 
        provided by the native client earlier.

   (D)  At this point the native client completes the authroization code flow
        by sending the authorization code received in the previous step to the
        Authroization Server.
        
   (E)  The authorization server authenticates the native client and validates the
        authorization code. If valid, the authorization server responds back with
        an access token and, optionally, a refresh token.
        
TODO: How should the redirection URI be used? I assumed it won't be neccesary, 
since no redirection is happening. Should it be set to null, rather than just omitting it
to avoid too big a delta with the authroization code flow?

# Authorization Request

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

# Authorization Response

# Error Response

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
