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


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
