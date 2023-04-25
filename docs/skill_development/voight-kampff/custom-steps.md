---
description: >-
  Some Skills require their own unique Steps to test functionality specific to
  that Skill.
---

# Adding Custom Steps

Custom Steps can be added to your Skills repository within the `test/behave/steps` directory.

The Mycroft Timer Skill for example provides [test/behave/steps/timer.py](https://github.com/MycroftAI/mycroft-timer/blob/20.02/test/behave/steps/timer.py). This has a range of custom Steps to ensure the system is in an appropriate state before the test is run, and that a timer is stopped correctly when requested.

Let's use an example from this Skill to see how we can define our own custom Steps.

```python
from behave import given

from test.integrationtests.voight_kampff import wait_for_dialog, emit_utterance


@given('a {timer_length} timer is set')
@given('a timer is set for {timer_length}')
def given_set_timer_length(context, timer_length):
    emit_utterance(context.bus, 'set a timer for {}'.format(timer_length))
    wait_for_dialog(context.bus, ['started.timer'])
    context.log.info('Created timer for {}'.format(timer_length))
    context.bus.clear_messages()
```

## Packages

First we import some packages.

```python
from behave import given

from test.integrationtests.voight_kampff import wait_for_dialog, emit_utterance
```

From `behave` we can get the behave decorators - `given`, `when`, or `then`. For this Step we also need some helper functions from the Voight Kampff module.

Like any Python script, you can import other packages from Mycroft or externally as needed.

### Voight Kampff Tools

The `test.integrationtests.voight_kampff` module provides a number of common tools that may be useful when creating Step files. 

#### then\_wait\(msg\_type, criteria\_func, context, timeout=10\)

Wait for a specified time for criteria to be fulfilled.

Arguments:

`msg_type` - Message type to watch  
`criteria_func` - Function to determine if a message fulfilling the test case has been found.  
`context` - Behave context object  
`timeout` - Time allowance for a message fulfilling the criteria, defaults to 10 sec

Returns:

`tuple (bool, str)` - test status, debug output

**mycroft\_responses\(context\)**

Collect and format mycroft responses from context.

Arguments:

`context` - Behave context to extract messages from.

Returns: 

`(str)` - Mycroft responses including skill and dialog file

#### emit\_utterance\(bus, utt\)

Emit an utterance on the bus.

Arguments:

`bus (InterceptAllBusClient)` - Bus instance to listen on  
`dialogs (list)` - List of acceptable dialogs

Returns:

`None`

#### wait\_for\_dialog\(bus, dialogs, timeout=10\)

Wait for one of the dialogs given as argument.

Arguments:

`bus (InterceptAllBusClient)` - Bus instance to listen on  
`dialogs (list)` - list of acceptable dialogs  
`timeout (int)` - Time allowance to wait, defaults to 10 sec

Returns:

`None`

## Decorators

Now we can use the `@given()` decorator on our function definition.

```python
@given('a {timer_length} timer is set')
@given('a timer is set for {timer_length}')
def given_set_timer_length(context, timer_length):
```

This decorator tells the system that we are creating a `Given` Step. It takes a string as it's first argument which defines what phrase we can use in our tests. So using the first decorator above means that in our tests we can then write `Given` Steps like:

```text
Given a 10 minute timer is set
```

A handy feature of decorators is that they can be stacked. In this example we have two stacked decorators applied to the same function. This allows us to use variations of natural language, and both versions will achieve the same result. So now we could write another Step phrased differently:

```text
Given a timer is set for 10 minutes
```

Either way, it will ensure a 10 minute timer exists before running the test Scenario.

## Function arguments

When we define a Step function, the first argument will always be a [Behave `Context` Object](https://behave.readthedocs.io/en/latest/api.html#behave.runner.Context). All remaining arguments will map to variables defined in the decorators.

```python
@given('a {timer_length} timer is set')
@given('a timer is set for {timer_length}')
def given_set_timer_length(context, timer_length):
```

In our current example, we have only one variable "`timer_length`". This corresponds to the second argument of our function. Additional variables can be added to the argument list such as:

```python
@given('a timer named {timer_name} is set for {timer_length}')
def given_set_timer_named(context, timer_name, timer_length):
```

### Context Object

The first argument of each Step function is always a [Behave `Context` Object](https://behave.readthedocs.io/en/latest/api.html#behave.runner.Context), with some additional properties added by Voight Kampff. These are:

`context.bus` - an instance of the Mycroft `MessageBusClient` class. `context.log` - an instance of the Python standard library `logging.Logger` class. `context.msm` - a reference to the Mycroft Skills Manager

## Step logic

Now we have the structure of our Step function in place, it's time to look at what that Step does.

```python
def given_set_timer_length(context, timer_length):
    emit_utterance(context.bus, 'set a timer for {}'.format(timer_length))
    wait_for_dialog(context.bus, ['started.timer'])
    context.log.info('Created timer for {}'.format(timer_length))
    context.bus.clear_messages()
```

In this example we have four lines:

1. Emitting an utterance to the Mycroft MessageBus to create a timer for the given length of time. 
2. Waiting for dialog to be returned from the MessageBus confirming that the timer has been started. 
3. Logging an `info` level message to confirm we created a timer. 
4. Clearing any remaining messages from the MessageBus to prevent interference with the test.

_Note: the log message in this Step isn't really necessary. Voight Kampff will confirm that each Step is completed successfully. It just serves as a useful example to show how messages can be logged._

## Help

For further assistance with Skill testing, please post your question on the [Community Forums](https://community.mycroft.ai/) or in the [Skills channel on Mycroft Chat](https://chat.mycroft.ai/community/channels/skills).

See our [tips for how to ask the best questions](../../using-mycroft-ai/troubleshooting/getting-more-support.md). This helps you get a more complete response faster.

