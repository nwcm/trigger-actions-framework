This fork provides a single custom setting option for org level disabling of all trigger actions accross sobjects. 

This provides the below additional functionality:

1. Trigger actions become opt-in for apex tests. Trigger actions will not run on DML in apex test context, this is useful for existing source with apex tests with unmocked DML.
2. Org admin can disable all trigger actions (all objects) in a single place. While the framework provides "bypass permission" this needs to be configured per sobject or trigger action and leaves room for error or additional effort.

To enable a trigger in a apex test. Simply upsert the custom setting in setup or the test. All other bypass mechanisms are intact and can be used as documented.

```apex
@TestSetup
	static void setup() {
		upsert new Trigger_Action_Framework_Settings__c(IsEnabled__c = true);
	}
```


# Trigger Actions Framework

#### [Unlocked Package Installation (Production)](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tKY000000R0yHYAS)

#### [Unlocked Package Installation (Sandbox)](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tKY000000R0yHYAS)

---

The Trigger Actions Framework allows developers and administrators to partition, order, and bypass record-triggered automations for applications built on Salesforce.com.

The framework supports both **Apex and Flow** - empowering developers and administrators to define automations in the tool of their choice, then plug them together harmoniously. All trigger logic is driven by custom metadata, providing a consolidated "Automation Studio" view of every action configured for a given SObject.

## Documentation

Full documentation is available at **[mitchspano.com/trigger-actions-framework](https://mitchspano.com/trigger-actions-framework/)**, including:

- [Getting Started](https://mitchspano.com/trigger-actions-framework/#/getting-started)
- [Apex Actions](https://mitchspano.com/trigger-actions-framework/#/apex-actions)
- [Flow Actions](https://mitchspano.com/trigger-actions-framework/#/flow-actions)
- [Entry Criteria Formula](https://mitchspano.com/trigger-actions-framework/#/entry-criteria)
- [Bypass Mechanisms](https://mitchspano.com/trigger-actions-framework/#/bypass-mechanisms)
- [Recursion Prevention](https://mitchspano.com/trigger-actions-framework/#/recursion-prevention)
- [Avoid Repeated Queries](https://mitchspano.com/trigger-actions-framework/#/avoid-repeated-queries)
- [DML-Less Testing](https://mitchspano.com/trigger-actions-framework/#/dml-less-testing)
- [DML Finalizers](https://mitchspano.com/trigger-actions-framework/#/dml-finalizers)

## Contributing

Contributions are welcome! See [CONTRIBUTING](docs/contributing.md) and [CODE OF CONDUCT](docs/code-of-conduct.md) for guidelines.
