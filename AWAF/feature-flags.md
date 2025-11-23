## Feature Flags

Continuous Delivery (CD) is a set of practices designed to ensure that your codebase is always in a deployable state. But what does that actually mean?

If you’re using version control (and if you’re not, talk to your manager ASAP), you probably have an integration branch where all changes from all developers are merged and tested. This branch is likely connected to a sandbox as well.

At some point, most teams run into a situation where a particular change cannot be deployed further down the CI/CD pipeline and it needs to be pulled out. How do you handle this? Some teams resort to commenting out the code (an anti-pattern we discussed earlier) or using Git revert to remove the unwanted change (or whatever DevOps tool they are using).

But what if you could deploy the integration branch to production even if a feature isn’t ready? What if you could do this without impacting the business? If this were possible, your integration branch would always be deployable. Deploying would become simpler because you’d only need to push a single branch to production instead of cherry-picking individual changes.

Feature flags let you hide a specific feature behind a toggle that can be turned on or off via configuration. Here’s an example:

```apex{5}
// some of code here, and then:

FeatureFlags flags = new FeatureFlags();

if (flags.evaluate('enhancedQuoteEditor').isEnabled()) {
    // run the logic for the enhanced quote editor
}
```

In this example, all the logic for the enhanced quote editor is encapsulated within the `if` statement. The `FeatureFlags` class checks if there’s a custom metadata record (or custom permission) for the `enhancedQuoteEditor` feature and determines whether the feature should be active.

This allows you to safely deploy the changes inside the if block to production while the feature is still in progress. The logic won’t run because the feature is disabled in the configuration. Once the feature is ready, all you need to do is deploy the latest version of the integration branch and enable the feature in production.

This approach is supported by the [salesforce-feature-flags](https://github.com/pgonzaleznetwork/salesforce-feature-flags) library. The library has become popular in recent months and was also featured at TrailheadDX.

In short, **teams using AWAF actively try to keep their integration branch in a deployable state**, avoiding messy workarounds later in the pipeline. This library supports that goal.

::: tip
The salesforce-feature-flags library only supports Apex and LWC. It’s not possible to hide configuration metadata behind a feature flag, such as new fields, permission set changes, etc.
:::

You can learn more about this library in this session:

<iframe width="560" height="315" src="https://www.youtube.com/embed/-jmWi111ED0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Alternative: RFLIB Feature Switches

If you want hierarchical switches without extra code, the [Reliability Force Library (RFLIB)](https://github.com/j-fischer/rflib) is worth a look. RFLIB’s feature switches are built on Custom Metadata Types and evaluate configuration in a hierarchy (User → Public Group → Profile → Global), so you can override behavior at the right scope without redeploying code.

- **Cross-surface support**: The same switch values can be queried from Apex, LWC, Aura, Flows, and even Validation Rules, which makes it easier to coordinate rollout logic between server and client code.
- **Operational safety nets**: Switches double as circuit breakers for integrations; turn off external calls or entire trigger handlers during incidents using metadata instead of hotfixes.
- **A/B and staged rollouts**: Use the hierarchical evaluation to test features with specific groups or profiles before enabling them globally.
- **Flow and validation friendly**: Global switch values surface directly in Flow, while an Apex action handles hierarchical lookups; validation rules can call Custom Metadata API names when you need declarative control.
- **Integrated with logging**: Built on top of RFLIB's logging framework, any decision made by the feature switch library are automatically recorded in the logs captured by the library. 

For more information, check out the Wiki page on [how to get started with Feature Switches](https://github.com/j-fischer/rflib/wiki/Getting-Started-with-Feature-Switches).