# Naming things is hard

Cats and dogs are not used in OO programming.

## Mediocre Names

* UserManager
* MessagingHelper
* AppHandler

Options 

* Collapse behaviour back into the object
* Break the multiple behaviours of that object into multiple objects

"Of course, many [managers] say they are driven by quality but are more driven
by schedule...In these cases I give my more controversial advice: Don’t tell!
Subversive? I don’t think so."

Martin Fowler, Refactoring: Improving the Design of Existing Code

Naming comes in two stages.
1. The initial definition of logic. (Defining how)
2. The refactoring (Defining what)

## Break the rules

Replacing the common `i` for `indexOfLoop` is a negative change even though it's more descriptive.

Here is an example of changing `systemTimeZone` to `tz` to make scanning easier

1. Difficult to parse due to over long names for short lived variables in obvious contexts.

```java
public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone)
{
  var result  = new List<DropDownComponent>();
  var systemTimeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var systemTimeZone in systemTimeZones)
  {
    var comp = new DropDownComponent(systemTimeZone.Id, systemTimeZone.DisplayName);

    comp.Selected = (systemTimeZone.Id == selected_time_zone);

    result.Add(comp);
  }

  return result;
}
```

Simplifications:

* The systemTimeZones collection
* The systemTimeZone object scoped in the foreach loop
* The selected_time_zone parameter
* The GetSystemTimeZones() method call.

2. Shortening names to ease scanning without losing context

```java
public List<DropDownComponent> BuildTimeZonesDropDownList(string selected_time_zone)
{
  var result  = new List<DropDownComponent>();
  var timeZones = TimeZoneInfo.GetSystemTimeZones();

  foreach (var tz in timeZones)
  {
    var component = new DropDownComponent(tz.Id, tz.DisplayName);

    comp.Selected = (tz.Id == selected_time_zone);

    result.Add(component);
  }

  return result;
}
```

## Opposites require rewords

Just slapping `un` in front of every method doesn't help.
`isWorkflowUncloseable`.

Consider totally rewording the method to start with `can`

## Speaking native

Doctors do not refer to multiple patients as `Patient Lists` so `List<Patient>`
is a poor abstraction. They refer to their patients as `Patient Panels`. So
consider using those abstractions instead.


Pros:

* You remove an unnecessary layer of interpretation.
* You can have more productive discussions with your clients.
* You might even be able to show your client working code to better explain complexities and hurdles in logic they may not have thought of.
* You have a better grasp of the business concepts you have to maintain.
