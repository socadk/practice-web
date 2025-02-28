---
Scope: System landscapes, team organization 
Phases: n/a (continuous)
Roles: All cross-project roles 
Input: Business model, IT strategy, existing systems
Output: Context Map
Abstraction/Refinement Level: Conceptual 
---

Activity/Technique: *Strategic Domain-Driven Design (DDD)*
----------------------------------------------------------

### Context
While [Tactic DDD](./DPR-TacticDDD.md) deals with implementing domain logic, Strategic DDD deals with integrating the resulting components. It attempts to manage complexity in end-to-end application landscapes, possibly over long periods of time. Small projects and businesses might not require such long-term perspective and coordination; software development projects and development organizations at large software-intensive firms usually do. <!-- TODO (v2) could indicate short discussion "how large is large enough to justify S-DDD?" -->

The interfaces between systems and teams have to be managed somehow, either centrally or decentrally. Enterprise architecture management frameworks such as [TOGAF](https://www.opengroup.org/togaf) can be leveraged to do so; [Scrum of Scrums](https://www.agilealliance.org/glossary/scrum-of-scrums/) and the portfolio level in the [Scaled Agile Framework (SAFe)](https://www.scaledagileframework.com/) address related concerns. Strategic DDD provides patterns and a simple diagram type to either support or complement such efforts.


### Goal and Purpose (When to Use and When not to Use)

> *As a domain-driven designer, I want to (de-)compose system landscape and team organization so that system parts can be developed, deployed, scaled, and changed independently.*  

According to Alberto Brandolini's [InfoQ article on strategic DDD](https://www.infoq.com/articles/ddd-contextmapping) that develops an example in several steps, there are four particular reasons to bother about model partitioning and model/context boundaries:

1. "Same term, different meaning" (homonym)
2. "Same concept, different use" (polyseme)
3. "External system differences" (heterogeneity)
4. "Scaling up the organization" (multiple teams) 

Ambiguities that are not detected can cause misunderstandings between stakeholders and create project risk. For instance, different attributes and different relations between domain concepts around people may have to be analyzed, designed, and implemented. Differences between customers, business partners, and staff members do exist and probably matter, even if all three are human beings. And legal entities/persons might have similar but not the same properties (what would the body mass index of a company be?). Finding out about such domain facets when coding or, even worse, during integration or interoperability testing causes extra effort and other pain; strategic DDD can unveil such ambiguities early, and help manage them from then on.

### Instructions (Synopsis, Definition)

The key pattern in strategic DDD is *Bounded Context*, an abstraction of a functional area (i.e., a set of related features targeting one or more stakeholder groups), system (from different viewpoints, for instance logical and physical), or team (or, more generally, an organizational unit):

* "A description of a boundary (typically a subsystem, or the work of a particular team) within which a particular model is defined and applicable" (@Evans:2003).
* "A bounded context is an explicit boundary within which a domain model exists. Inside the boundary, all terms and phrases of the Ubiquitous Language have specific meaning, and the model reflects the language with exactness" (@Vernon:2013). 
* Martin Fowler emphasizes that a Bounded Context is explicit about its interrelationships in his [bliki article on Bounded Contexts](http://martinfowler.com/bliki/BoundedContext.html).

The following domain model for Strategic DDD gives an overview of the patterns in it (as well as the connection to [Tactic DDD](./DPR-TacticDDD.md) via Aggregates):

<!-- STX4ZIO: Done! (Can comment go?) -->
![Strategic DDD Concepts and their Relations](/activities/images/StrategicDDDMetaModel.png)

The original DDD book by Eric Evans defined an initial set of relations between Bounded Contexts appearing in a [Context Map](../artifact-templates/DPR-StrategicDDDContextMap.md), e.g., the *Conformist* pattern (@Evans:2003). Later on, a few additional types were added. In ["An Introduction to Domain Driven Design"](http://www.methodsandtools.com/archive/archive.php?id=97), Dan Haywood summarizes the original six patterns as this: <!-- ZIO4STX: remove for LP? -->

* *Shared Kernel*: "Two bounded contexts use a common kernel of code (for example a library) as a common lingua-franca, but otherwise do their other stuff in their own specific way."
* *Open Host Service (OHS)*: "A bounded context specifies a protocol by which any other bounded context can use its services (for instance, a RESTful HTTP service or a SOAP Web service). This protocol exposes the Published Language."
* *Published Language (PL)*: "The interacting bounded contexts agree on a common language (for example a bunch of XML schemas over an enterprise service bus) by which they can interact with each other."
* *Customer/Supplier* (a.k.a. Customer/Supplier Teams/Development): "One bounded context uses the services of another and is a stakeholder (customer) of that other bounded context. As such it can influence the services provided by that bounded context."
* *Conformist*: "One bounded context uses the services of another but is not a stakeholder to that other bounded context. As such it uses "as-is" (conforms to) the protocols or APIs provided by that bounded context."
* *Anti-Corruption Layer (ACL)*: "One bounded context uses the services of another and is not a stakeholder, but aims to minimize impact from changes in the bounded context it depends on by introducing a set of adapters – an anti-corruption layer."

The extensions are:

* *Partnership* is defined as: "Where development failure in either of two contexts would result in delivery failure for both, forge a partnership between the teams in charge of the two contexts. Institute a process for coordinated planning of development and joint management of integration. The teams must cooperate on the evolution of their interfaces to accommodate the development needs of both systems. Interdependent features should be scheduled so that they are completed for the same release".
* *Separate Ways*, expressing that it has been decided not to collaborate and/or integrate two Bounded Contexts. Note that not show all relationship patterns (old and newer) appear in the Figure above ("Strategic DDD Concepts and their Relations").
* *Big Ball of Mud*.

*Note:* These patterns describe organizational relationships, first and foremost (not technical ones). 

Summaries of the patterns from the original DDD book as well as the extensions are available for free download in the book ["Domain-Driven Design Reference"](http://www.domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf), also by Eric Evans. 

While being organizational, the relationships can still serve as input for technical design work. They differ with respect to the following design concerns: 

* Symmetric or asymmetric relationship?
* Topology: local vs. remote interface? 
* Visibility (of collaboration/communication partners to each other)?  
* Amount of control and influence for downstream client (consumer) on upstream supplier (provider)?

Strategic DDD identifies Bounded Contexts and then answers these questions to end up at the right pattern for any given relationship. [Context Maps](../artifact-templates/DPR-StrategicDDDContextMap.md) can drive the architectural decision making in API design. Follow-on decisions then pertain the integration style and technology.

Note that the relationship patterns do not exclude, but complement each other by default. [Context Mapper](https://contextmapper.org/docs/bounded-context/) is a tool that can help doing so; it features DDD patterns and enforces additional semantic validation rules that clarify which relationships and pattern combinations make sense. 

<!-- explain FAST and link to Context Mapper paper -->
The FAST taxonomy introduced in @Kapferer:2020a proposes a classification of Bounded Contexts into *Feature*, *Application*, *System*, and *Team* contexts.

<!-- Source: https://miro.com/app/board/o9J_lIRKK0Y=/ Select the frame -> export as image -->
![Suggested Steps in Strategic DDD](/activities/images/DPR-StrategicDDDSteps.jpg)


### Example(s)
The [Context Mapper website](https://contextmapper.org/docs/examples/) provides a number of examples of DDD applied, including a model of a fictitious [microservices ecosystem in the insurance domain](https://github.com/ContextMapper/context-mapper-examples/tree/master/src/main/cml/insurance-example):

![Example Context Map: Lakeside Mutual Case Study](/activities/images/ContextMap-Illustration.png)

### Benefits vs. Effort (Expected Benefits, Skill Levels)
An [OOSPLA 2006 experience report](https://dddcommunity.org/wp-content/uploads/files/practitioner_reports/landre_einar_2006_part1.pdf) makes the case for strategic DDD in an oil company. A [second article from the same authors](https://dddcommunity.org/wp-content/uploads/files/practitioner_reports/landre_einar_2006_part2.pdf) reports on usage of strategic DDD in enterprise architecture management. 


### Hints and Pitfalls to Avoid

* Never forget whom you build a system for; do not get hung up in pattern philosophy or technical details. For instance, let the modeling results speak for themselves if stakeholders struggle with some of the DDD terminology and concepts.
* Consult Chapters 2, 3, 10, and 13 in ["Implementing Domain-Driven Design"](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577) by Vaughn Vernon, for instance for a clarification of the relationships between Subdomains and Bounded Contexts.
* Find more hints in the booklet ["The Anatomy of Domain-Driven Design"](https://leanpub.com/theanatomyofdomain-drivendesign) by Scott Millet and Samuel Knight.


### Origins and Signs of Use
Strategic DDD was introduced in Eric Evans' original DDD (@Evans:2003), but featured even more prominently later in "Implementing Domain-Driven Design" by Vaughn Vernon (@Vernon:2013).

Usage of the pattern names and presence of Context Maps, either drawn informally or modeled in a tool, indicate use.

### Related Content

### Performing Roles and Related Artifacts (Synopsis)

* Enterprise architects
* Program/portfolio architects  
* Agile teams 

* [Context Map](../artifact-templates/DPR-StrategicDDDContextMap.md)
* System [Context Diagram](../artifact-templates/DPR-ContextDiagram.md)

### Practices and Techniques (Refinements, Guides)

* [Tactic DDD](./DPR-TacticDDD.md)
* [Stepwise Service Design](./SDPR-StepwiseServiceDesign.md) 
* Event Storming, introduced [here](https://www.ibm.com/cloud/architecture/architecture/practices/event-storming-methodology-architecture) and tool-supported as described [here](https://contextmapper.org/docs/event-storming/).
* Lean [Wardley Mapping](https://learnwardleymapping.com/), capturing strategic intent.
* Enterprise Architecture Management (EAM) offerings and the Scaled Agile Framework (SAFe) may benefit from (or be complemented by) Strategic DDD. <!-- CE: add links -->
<!-- * Context Modeling in UML or DSL -->

There is a GitHub organization called [ddd-crew](https://github.com/ddd-crew) that features many business analysis-level extensions to DDD practices, for instance a [Bounded Context Canvas](https://github.com/ddd-crew/bounded-context-canvas) template.


### More Information 

Refer to presentations, articles, and books by (in alphabetical order):

* Alberto Brandolini, for instance ["Strategic Domain Driven Design with Context Mapping"](https://www.infoq.com/articles/ddd-contextmapping/)
* Ton Donker, ["The Published Language and Industry Standards in Domain-Driven Design"](https://www.linkedin.com/pulse/published-language-industry-standards-domain-driven-design-ton-donker-ttvpe/) 
* Scott Millet, for instance ["The Anatomy Of Domain-Driven Design"](https://leanpub.com/anatomy-of-DDD) on LeanPub 
* Michael Ploed, for instance ["Visualizing Sociotechnical Architectures with Context Maps"](https://speakerdeck.com/mploed/visualizing-sociotechnical-architectures-with-context-maps?slide=4)
* [Henning Schwentner](https://hschwentner.io/) and his colleagues at Workplace Solutions, for instance ["Model Pollution"](https://speakerdeck.com/hschwentner/model-pollution) features a proposal for a "Hamburger Architecture" (as well as many valuable pointers).
* An ["Introduction to Strategic DDD"](https://vaadin.com/learn/tutorials/ddd/strategic_domain_driven_design) is available on the Vaadin website.
* Vaughn Vernon, for instance his [YouTube channel](https://www.youtube.com/channel/UCdbDxsXevDLt7EhRbi2KGjg)
  
Articles about Context Mapper and its interpretation of the DDD patterns can be found on the [Context Mapper website](https://contextmapper.org/background-and-publications/), including "Domain-driven Architecture Modeling and Rapid Prototyping with Context Mapper" (@Kapferer:2020a) and "Domain-Driven Service Design — Context Modeling, Model Refactoring and Contract Generation" (@Kapferer:2020b). Experience with the tool on projects is shared in two blog posts "Domain-Driven Design in Practice — Experience with Context Mapper" ([here](https://ozimmer.ch/modeling/2022/11/23/ContextMapperInsights.html) and [here](https://blog.mimacom.com/ddd-and-context-mapper-experience/)).

### Data Provenance 

```yaml
title: "Design Practice Repository (DPR): Practice/Technique Strategic DDD"
author: Olaf Zimmermann (ZIO)
date: "02, 28, 202"5
copyright: Olaf Zimmermann, 2020-2025 (unless noted otherwise). All rights reserved.
license: Creative Commons Attribution 4.0 International License
```
