---
title: Oh what a tangled web we weave when we … network containers!
type: post
category: talks
---

Like a lot of things in containerization, networking is built of a number of layers and abstractions based on existing technologies, with more seemingly being added all the time. As hackers we know that when abstractions are built and technologies re-used, there are going to be edge cases to exploit and assumptions to abuse.

In this talk we'll get into the details of how Kubernetes based network stacks work and detail specific places and detail specific places that are vulnerable to attack. We'll start with the low level aspect of Linux networking that have been re-purposed for container stacks, looking at how those settings can leave clusters exposed to attacks, then talk about some higher level HTTP concerns like the fact that every cluster is SSRF as a service, and demonstrate tools that allow for port-scanning via the Kubernetes API server.

We'll also talk about how Kubernetes cluster network security operates and focus on how those controls can be rendered useless by users making mistakes, or by attackers deliberately.

This talk was delivered at [44Con 2025](https://44con.com/44con-2025-schedule/)