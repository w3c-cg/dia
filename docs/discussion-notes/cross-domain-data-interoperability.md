# Cross-Domain Data Interoperability: Requirements and Open Questions

**Status:** Initial discussion note (non-normative; not an adopted Community Group position)  
**Version:** 0.1 — for review  
**Date:** 11 November 2026  
**Proposed by:** Aiyao Zhang, participant in the Data Interoperating Architecture (DIA) Community Group

> **Status of this document.** This note is an individual proposal intended to support open discussion in the DIA Community Group. It does not define a protocol, express a consensus of the group, or represent a W3C standard or endorsement. All requirements below are candidates for discussion. In particular, this document does not prescribe a universal identifier scheme, identity provider, authorization model, or implementation architecture.

## Abstract

Data interoperability across independently managed systems involves more than network connectivity and compatible data formats. A consumer may need to identify and discover a resource, interpret its description, establish who is making a request, communicate the intended use, and understand the provider's access decision. These concerns become more visible when automated workflows and AI agents interact with services across organizational boundaries, but they are not limited to agent-based applications.

This note presents two illustrative use cases, proposes a small set of candidate interoperability requirements, and identifies questions about the boundary between common interoperability conventions and existing Web technologies. Its purpose is to invite feedback on **what must be interoperable**, before proposing **how it should be implemented**.

## 1. Scope and motivation

Consider organizations that manage their data independently. They may use different applications, identifier schemes, catalogs, identity systems, and authorization policies. HTTP APIs and common serialization formats can make individual exchanges possible, but they do not by themselves establish a shared interpretation of which resource is being referenced, which party is acting, what operation has been authorized, or which conditions remain relevant when data is used.

The issue is not necessarily that every such capability requires a new standard. Existing specifications already cover substantial parts of identification, discovery, identity, and policy representation. The architectural question is which information and interactions must be understood across administrative boundaries, and whether common profiles or combinations of existing mechanisms are sufficient.

For this discussion, *data interoperability* refers to the ability of independently administered systems to identify, describe, discover, request, exchange, and use data resources through mutually interpretable interactions while preserving the provider's authority over its resources. This is a working description, not a proposed formal definition.

## 2. Illustrative use cases

### Use case A — Conventional cross-organizational data access

Organization A maintains a dataset and publishes a discoverable description. Organization B uses a conventional application to locate the dataset and request a permitted subset. The organizations do not share a resource catalog, user directory, or internal policy engine. The consumer needs to determine which resource and operation are involved and how to present the information necessary for the provider to decide. The provider remains responsible for its local decision and enforcement.

Questions raised include whether a resource reference remains meaningful when service endpoints change, what information a description should expose, and how an authorization outcome can be interpreted without revealing internal policies.

### Use case B — Automated or agent-assisted data use

Organization B instead uses an automated workflow or an AI agent to select and invoke services that access the same data. A service invocation may be carried out by software acting on behalf of a person or organization. Discovery of a tool or successful authentication to a service does not by itself establish permission to use the underlying data.

Additional questions concern how the actual caller and represented principal are distinguished, how the scope of delegation is checked, and whether relevant usage constraints can be understood when an operation spans services. These concerns extend the conventional use case; they do not imply that data authorization and agent authorization are interchangeable.

Both cases are illustrative. They are not deployment claims or evidence that a new protocol is necessary.

## 3. Candidate interoperability requirements

The following items are **discussion candidates**, not normative requirements.

**R1 — Interpretable resource references.** A consumer should be able to reference a resource without assuming that its local identifier, current service endpoint, and globally usable reference are identical. An interoperability approach should explain how an identifier's source and scheme are understood, and how references remain unambiguous across systems. Whether a common identifier scheme is necessary remains open.

**R2 — Discoverable, usable descriptions.** A resource description should provide enough information for another system to determine what is being offered, how to continue an interaction, and where applicable access conditions can be learned. Discovery should not be treated as authorization, and a description should not require the disclosure of sensitive internal state. The appropriate metadata vocabulary, visibility rules, and freshness signals warrant discussion.

**R3 — Cross-domain access interaction.** A requester and provider should have a mutually interpretable way to communicate the requested resource and operation, relevant identity or delegation evidence, and the provider's decision. Interoperability need not require a common internal identity database, policy language, approval workflow, or enforcement engine. A common interaction contract may be narrower than a common authorization model.

**R4 — Continuity of applicable constraints.** When an authorized exchange is subject to usage constraints, subsequent components may need to recognize the constraints relevant to their actions. A useful distinction is between communicating a constraint, determining whether it applies, and actually enforcing it. Which guarantees can reasonably span independently managed services remains an open question.

**R5 — Understandable outcomes and changes.** Different systems should not confuse a completed access with an accepted request, a pending decision, a denial, or an unresolved outcome. Changes in permissions or resource descriptions should be handled without assuming that previously observed information remains valid indefinitely. The minimum common outcome vocabulary and update semantics require further examination.

**R6 — Testable interoperability boundaries.** Any eventual specification should make it possible to test two independently implemented systems. Such tests should address shared interpretation and behavior at their boundaries, rather than requiring identical internal designs.

## 4. Relationship to existing Web mechanisms

The group should first assess what existing mechanisms already provide and where additional conventions, profiles, or composition rules might help:

| Area                               | Examples worth examining                       | Question for DIA                                             |
| ---------------------------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| Resource references and exchange   | URIs, DNS, HTTP                                | Which references and resolution assumptions must be shared?  |
| Resource descriptions and catalogs | W3C DCAT                                       | Which descriptive fields are sufficient for interoperable discovery and access preparation? |
| Identity and access                | OAuth 2.0, OpenID Connect                      | Which information about the caller, represented principal, requested action, and decision must cross the boundary? |
| Usage conditions                   | W3C ODRL                                       | When is exchanging a policy expression sufficient, and what additional processing or enforcement assumptions are needed? |
| Automated service interactions     | Workflow systems and MCP-based tool interfaces | What changes when the immediate caller is software acting for another principal? |

Listing a technology here does not imply a deficiency in that technology or a decision to extend it. In some cases, interoperable composition of existing standards may be the appropriate answer.

## 5. Open questions for the community

1. **Resource identity:** What is the minimum shared information needed to reference a data resource across systems that use different identifier schemes? Must there be a common canonical identifier, or can explicit and verifiable mappings suffice?
2. **Discovery and description:** Which aspects of a data resource need to be interoperably described, and which can remain specific to a catalog, provider, or application?
3. **Authorization boundary:** Should a general data interoperability architecture specify a minimal access-request and decision-exchange contract, while using existing identity and access management standards for authentication and internal policy decisions? Where should that boundary lie?
4. **Usage across services:** What can reasonably be expected of systems that exchange data subject to ongoing use constraints, especially when intermediate services are independently managed?
5. **Existing standards and evidence:** Are there deployed systems or standards profiles that already meet these requirements? What concrete cases reveal gaps, and which candidate requirements may be unnecessary?

The authors particularly welcome counterexamples, pointers to existing specifications, practical deployment experiences, and proposals to simplify or remove candidate requirements.

## 6. Proposed next steps

This document is intended as an initial input to discussion, not as a decision. A useful next stage would be to:

1. Collect examples and counterexamples from different application domains.
2. Compare candidate requirements against existing Web specifications and implementation practices.
3. Record unresolved questions as public GitHub issues, with alternative approaches rather than premature protocol commitments.
4. Revise the note in response to comments, distinguishing individual proposals from any subsequent group decisions.

Feedback may be shared through the [DIA public mailing list](https://lists.w3.org/Archives/Public/public-dia/) or through an issue in the [DIA Community Group repository](https://github.com/w3c-cg/dia).

## Informative references

- W3C DIA Community Group, [Technical Progress and Upcoming Activities](https://www.w3.org/community/dia/2026/10/12/dia-community-group-technical-progress-and-upcoming-activities/), 12 October 2026.
- W3C, [Data Catalog Vocabulary (DCAT), Version 3](https://www.w3.org/TR/vocab-dcat-3/).
- W3C, [ODRL Information Model 2.2](https://www.w3.org/TR/odrl-model/).
- OpenID Foundation, [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html).

