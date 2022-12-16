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


# Response Mode

When a mobile app desires to start an authorization flow with the
Authorization Server (AS), it specifies a new `response_mode` value
of `native_ux` which requests the AS to return the challenge
sequence in a descriptive language that can be interpreted and
displayed in a native experience by the mobile app.

## native_ux

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

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
