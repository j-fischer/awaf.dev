## Logging and Observability

Using `System.debug()` is fine for quick troubleshooting or Apex performance profiling during development. However, for production support, it’s the worst possible solution. It requires setting up debug logs for the affected user, retrieving the log, and dealing with potential size limits where the part you actually care about gets trimmed.

For a better solution, we recommend [Nebula Logger](https://github.com/jongpie/NebulaLogger) by Jonathan Gillespie. Nebula Logger is an observability framework that integrates with Apex, Lightning Components, Flow, OmniStudio, and external integrations. The library has strong community support and has been featured at many events, including Dreamforce.

You can learn more about Nebula Logger in this session:

<iframe width="560" height="315" src="https://www.youtube.com/embed/RYUz7Y9i0Sk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Alternative: RFLIB Logging

[Reliability Force Library (RFLIB)](https://github.com/j-fischer/rflib/wiki/Getting-Started-with-Logging) provides an observability framework that supports Apex, Lightning Web Components, Aura, Flow, and OmniStudio. It takes a slightly different approach to logging than other frameworks, with a focus on optimizing interoperability with an org's business logic.

- **Hierarchical logger settings**: Highly customizable via Custom Settings (per user, profile, or org) to control whether messages stay in memory, are emitted as Platform Events, appear in Debug Logs, or are persisted to a Big Object archive.
- **Structured aggregation**: Log events roll into Ops Center dashboards, providing real-time counts, trend charts, and drill-down context so support teams can spot spikes instead of combing through raw logs.
- **Highly optimized**: Fully configurable to reduce and manage consumption of governor limits (transaction management, use of Platform Events, and more), limiting the footprint of logging within your business logic.

You can learn more about RFLIB in the Apex Hour session:

<iframe width="560" height="315" src="https://www.youtube.com/embed/zHEBDRstsZU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>