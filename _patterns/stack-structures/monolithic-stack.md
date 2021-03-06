---
layout: pattern
title:  "Monolithic Stack Antipattern"
date: 2019-03-27 08:00:00 +0000
category: Stack Structural Patterns
order: 11
published: false
---

A Monolithic Stack is an [infrastructure stack](/patterns/stack-concept/) that includes too many elements, so that it is difficult to work with.


<figure>
  <img src="images/monolithic-stack.png" alt="A Monolithic Stack is an infrastructure stack that includes too many elements, so that it is difficult to work with" />
  <figcaption>A Monolithic Stack is an infrastructure stack that includes too many elements, so that it is difficult to work with.</figcaption>
</figure>


## Also Known As

- Spaghetti stack


## Motivation

Infrastructure often grows organically, each new piece being naturally added into the existing codebase as we go. Over time, the codebase can become messy and unwieldy. 


## Consequences

Whether a given infrastructure stack is a monolith is a matter of judgement. The symptoms of a monolithic stack include:

- It's difficult to understand how the pieces of the stack fit together,
- New people take a while learning the stack's codebase,
- Debugging problems with the stack is hard,
- Changes to the stack frequently cause issues,
- Significant effort is spent maintaining systems and processes to manage the complexity of the stack.


A key indicator of whether a stack is becoming monolithic is how many people are working on changes to it at any given time. The more often a stack is being worked on by multiple people, the more pressure there is to create processes and tooling to cope with conflicts. If your team is introducing heavier processes to manage change and release in response to frequent failures on deployment of a single stack, then this is symptom of a monolithic stack.

[Feature branching](https://martinfowler.com/bliki/FeatureBranch.html) is a strategy for coping with this, but it can add friction and overhead to delivery. Habitual use of feature branches to work on a stack suggests that the stack has become monolithic.

[Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) (CI) is a more sustainable way to make it safer for multiple people to work on a single stack. But as a stack grows more monolithic, the CI build takes longer to run, and it becomes difficult to maintain good build discipline. If your team's CI is sloppy, it's another sign that your stack is a monolith.

These describe issues within a single team working on an infrastructure stack. When multiple teams need to make changes to a single stack project this is a clear sign of a monolith. Enabling different teams to work easily on clearly separated parts of a system is a key goal for good system architecture.


## Related patterns

Dividing a monolithic stack into multiple smaller [micro stacks](micro-stack.html) can make it easier for multiple people, and multiple teams, to maintain and evolve the system. It's possible that related services can be cleanly maintained in a single [multi-service stack](multi-service-stack.html), with strong engineering discipline. It's often more natural to split a system into [single-service stacks](single-service-stack.html). In some situations, breaking services down into [multiple stacks](cross-stack-service.html) has value.

Splitting monolithic stacks does not guarantee to fix the issues described above with messy, fragile stacks. With smaller stacks, design challenges are pushed out to the integration between the stacks. As with many architectural decisions, how far to go between small, micro-stacks and large, monolithic stacks, involves tradeoffs.

The benefit of a single stack is that deployment is a single operation. When a system is comprised of multiple stacks which must integrate with one another, managing the delivery of changes is more complex. Delivery requires versioning of stack code, maintaining integration contracts between stacks, and more sophisticated testing.

But small, cleanly defined stacks can make the design of the overall system more clear. Having each stack easy to change and apply on its own should reduce barriers to making further improvements to the system. Particularly for infrastructure, which can be very slow and risky to change, breaking a monolith is a useful first step.

