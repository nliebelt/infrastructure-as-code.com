---
layout: pattern
title:  "Multi-Service Stack Pattern"
date: 2019-03-27 08:00:00 +0000
category: Stack Structural Patterns
order: 14
published: false
---

A Multi-Service Stack hosts multiple applications in a single instance of the [infrastructure stack](/patterns/stack-concept/).


<figure>
  <img src="images/multi-service-stack.png" alt="A Multi-Service Stack hosts multiple applications in a single instance of the infrastructure stack" width="50%"/>
  <figcaption>A Multi-Service Stack hosts multiple applications in a single instance of the infrastructure stack.</figcaption>
</figure>


For example, a search application might involve running several services - a front end UI, an indexing service, a search API service, and a data store. 


## Also Known As

- Combined stack
- Service group stack


## Motivation

Defining the infrastructure for multiple related services together may make it easier to manage the application as a whole.


## Applicability

This can work well when a single team owns the infrastructure and deployment of all of the pieces of the application, so the boundaries of the stack match the boundaries of the team.

Multi-service stacks are sometimes a useful part of an incremental strategy of splitting a monolithic stack into [smaller stacks](micro-stack.html), with an aim of ending up with [single service stacks](single-service-stack.html) or even [cross-stack services](cross-stack-service.html) where appropriate. In this case, the multi-service stack is a stepping stone towards a more fully decomposed architecture.


## Consequences

A drawback with this pattern is that the stack can become unwieldy, tending towards a [monolith](monolithic-stack.html). It can be difficult to make a small change to a single service without risking disruption to other applications running on the stack.

