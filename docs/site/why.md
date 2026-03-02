# Why the Trigger Actions Framework?

Trigger frameworks have been around on the Salesforce platform for a long time. The idea is simple: keep the trigger file thin and push the logic into a handler class.

That works for a while... but then the org grows.

## The Problem with Existing Frameworks

A standard trigger framework gives you a handler class you override per context:

```java
public class OpportunityTriggerHandler extends TriggerHandler {

  public override void afterInsert() {
    for (Opportunity o : (List<Opportunity>) Trigger.new) {
      // do something
    }
  }
  // override additional contexts as necessary
}
```

The trigger file stays thin. The handler owns the logic. This is a great step.

To add an additional automation, you'll need to modify the handler:

```java
public class OpportunityTriggerHandler extends TriggerHandler {

  public override void afterInsert() {
    this.handleWholesaleInsert((List<Opportunity>) Trigger.new);
    this.handleRetailInsert((List<Opportunity>) Trigger.new);
  }

  private void handleWholesaleInsert(List<Opportunity> newList) {
    for (Opportunity o : newList) {
      // do something
    }
  }

  private void handleRetailInsert(List<Opportunity> newList) {
    for (Opportunity o : newList) {
      // do something
    }
  }
}
```

Every new automation means opening this file, adding a private method, and wiring it up in the override. _The handler is never done being modified_.

After a few years on a real project, the handler looks something like this:

```java
public class OpportunityTriggerHandler extends TriggerHandler {

  public override void afterInsert() {
    this.handleWholesaleInsert((List<Opportunity>) Trigger.new);
    this.handleRetailInsert((List<Opportunity>) Trigger.new);
    this.recalculateAccountValues((List<Opportunity>) Trigger.new);
    this.notifyAMEASales((List<Opportunity>) Trigger.new);
    this.setDefaultAccountType((List<Opportunity>) Trigger.new);
    this.defineOpportunityContactRoles((List<Opportunity>) Trigger.new);
    // ...
  }

  private void handleWholesaleInsert(List<Opportunity> newList) {
    for (Opportunity o : newList) {
      // do something
    }
  }

  // More methods implemented and called for every action...
}
```

The handler is now a god class. It has too much responsibility, too many contributors, and no clear ownership. Any time someone on the team ships a new Opportunity automation, they touch this file — which means merge conflicts are a regular occurrence. And keep in mind: this example is unusually clean; handlers in the real world are rarely this tidy.

## The Root Cause

Traditional trigger frameworks violate two foundational design principles:

- **[Open-closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)** - software entities should be open for extension but closed for modification. The handler is never closed; every new automation reopens it.
- **[Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)** - a class should have one reason to change. The handler has as many reasons to change as there are automations on the object.

## Modularity in the Trigger Actions Framework

The Trigger Actions Framework removes the handler from the equation. Each automation is its own class:

```java
public class TA_Opportunity_HandleWholesaleInsert implements TriggerAction.AfterInsert {
  public void afterInsert(List<Opportunity> newList) {
    // focused, single-responsibility logic
  }
}
```

The order of execution, the trigger context, and whether an action is active are all controlled through custom metadata records — not code. Shipping a new automation means creating a new class and a new metadata record. No existing file is touched, so there are no merge conflicts by design.

Each action class can be tested by instantiating it directly and passing in a list of records — no DML required. And because each action is its own unit, it can be bypassed independently without affecting anything else.

[Get Started](getting-started.md) to enable the framework on your first object.
