---
description: A statement is any information spoken by Neon to the User.
---

# Statements

**Note:** The links to the various NeonSkill methods in this documentation point to the `mycroft-core` documentation. There is no material difference between them for the methods referenced and, since there is not publicly available `neon_core` documentation, we will reference `mycroft-core` instead.

## Speaking a statement

One of Neon's most important core capabilities is to convert text to speech, that is, to speak a statement.

Within a Skill's Intent handler, you may pass a string of text to Neon and Neon will speak it. For example: [`self.speak('this is my statement')`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.speak) That's cool and fun to experiment with, but passing strings of text to Neon doesn't help to make Neon a multilingual product. Rather than hard-coded strings of text, Neon has a design pattern for multilingualism.

### Multilingualism

To support multilingualism, the text that Neon speaks must come from a file. That file is called a dialog file. The dialog file contains statements (lines of text) that a listener in a particular language would consider to be equivalent. For instance, in USA English, the statements "I am okay" and "I am fine" are equivalent, and both of these statements might appear in a dialog file used for responding to the USA English question: "How are you?".

By convention, the dialog filename is historically formed by _dot connected_ _words_ and must end with ".dialog". The dialog filename should be descriptive of the contents as a whole. Sometimes, the filename describes the question being answered, and other times, the filename describes the answer itself. For the example above, the dialog filename might be: **how.are.you.dialog** or **i.am.fine.dialog**.

Newer skills may use _underscore connected_ _words_ ending with ".dialog". For example. **how_are_you.dialog** or **i_am_fine.dialog**. The older convention is still supported, but the newer convention is preferred.

Multilingualism is accomplished by translating the dialog files into other languages, and storing them in their own directory named for the country and language. The filenames remain the same. Using the same filenames in separate language dependent directories allows the Skills to be language agnostic; no hard-coded text strings. Adjust the language setting for your Device \*\*\*\* and Neon uses the corresponding set of dialog files. If the desired file does not exist in the directory for that language, Neon will use the file from the USA English directory.

As an example of the concept, the contents of **how.are.you.dialog** in the directory for the French language in France (fr-fr) might include the statement: "Je vais bien".

### The Tomato Skill Revisited

To demonstrate the multilingualism design pattern, we examine the usage of the [`speak_dialog()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.speak_dialog) method in the [Tomato Skill](intents/padatious-intents.md) .

The Tomato Skill has two Intents: one demonstrates simple, straightforward statements, and the other demonstrates the use of variables within a statement.

### Simple statement

The first Intent within the Tomato Skill, **what.is.a.tomato.intent**, handles inquiries about tomatoes, and the dialog file, **tomato.description.dialog**, provides the statements for Neon to speak in reply to that inquiry.

Sample contents of the Intent and dialog files:

<details>
  <summary>what.is.a.tomato.intent</summary>

```
what is a tomato
what would you say a tomato is
describe a tomato
what defines a tomato
```

</details>

  <summary>tomato.description.dialog</summary>

```
The tomato is a fruit of the nightshade family
A tomato is an edible berry of the plant Solanum lycopersicum
A tomato is a fruit but nutrionists consider it a vegetable
```

</details>

Observe the statements in the tomato.description.dialog file. They are all acceptable answers to the question: "What is a tomato?" Providing more than one statement in a dialog file is one way to make Neon to seem less robotic, more natural. Neon will randomly select one of the statements.

The Tomato Skill code snippet:

```python
@intent_handler('what.is.a.tomato.intent')
def handle_what_is(self, message):
    """Speaks a statement from the dialog file."""
    self.speak_dialog('tomato.description')
```

With the Tomato Skill installed, if the User utters \*\*\*\* "Hey Neon, what is a tomato?", the Intent handler method `handle_what_is()` will be called.

Inside `handle_what_is()`, we find: `self.speak_dialog('tomato.description')`

As you can probably guess, the parameter `'tomato.description'` is the dialog filename without the ".dialog" extension. Calling this method opens the dialog file, selects one of the statements, and converts that text to speech. Neon will speak a statement from the dialog file. In this example, Neon might say "The tomato is a fruit of the nightshade family".

Remember, Neon has a language setting that determines from which directory to find the dialog file.

#### File locations

The [Skill Structure](../skill-structure/) section describes where to place the Intent file and dialog file. Basically, you put both files in `locale/en-us`. Other structures are deprecated.

### Statements with variables

The second Padatious Intent, **do.you.like.intent**, demonstrates the use of variables in the Intent file and in one of the dialog files:

<details>
  <summary>do.you.like.intent</summary>

```text
do you like tomatoes
do you like {type} tomatoes
```

</details>

  <summary>like.tomato.type.dialog</summary>

```text
I do like {type} tomatoes
{type} tomatoes are my favorite
```

</details>

  <summary>like.tomato.generic.dialog</summary>

```text
I do like tomatoes
tomatoes are my favorite
```

</details>

Compare these two dialog files. The **like.tomato.generic.dialog** file contains only simple statements. The statements in the **like.tomato.type.dialog** file include a variable named `type`. The variable is a placeholder in the statement specifying where text may be inserted. The `speak_dialog()` method accepts a dictionary as an optional parameter. If that dictionary contains an entry for a variable named in the statement, then the value from the dictionary will be inserted at the placeholder's location.

Dialog file variables are formed by surrounding the variable's name with curly braces. In Neon parlance, curly braces are known as a _mustache_.

For multi-line dialog files, be sure to include the **same** variable on **all** lines.

The Tomato Skill code snippet:

```python
 @intent_handler('do.you.like.intent')
    def handle_do_you_like(self, message):
        tomato_type = message.data.get('type')
        if tomato_type is not None:
            self.speak_dialog('like.tomato.type',
                              {'type': tomato_type})
        else:
            self.speak_dialog('like.tomato.generic')
```

When the User utters "Hey Neon, do you like RED tomatoes?", the second of the two Intent lines "do you like {type} tomatoes" is recognized by Neon, and the value 'RED' is returned in the message dictionary assigned to the 'type' entry when `handle_do_you_like()` is called.

The line `tomato_type = message.data.get('type')` extracts the value from the dictionary for the entry 'type'. In this case, the variable `tomato_type` will receive the value 'RED', and `speak_dialog()`will be called with the 'like.tomato.type' dialog file, and a dictionary with 'RED' assigned to 'type'. The statement "i do like {type} tomatoes" might be randomly selected, and after insertion of the value 'RED' for the placeholder variable {type}, Neon would say: "I do like RED tomatoes".

Should the User utter "Hey Neon, do you like tomatoes?", the first line in the Intent file "do you like tomatoes" is recognized. There is no variable in this line, and when `handle_do_you_like()` is called, the dictionary in the message is empty. This means `tomato_type` is `None`,`speak_dialog('like.tomato.generic')` would be called, and Neon might reply with "Yes, I do like tomatoes".

## Waiting for speech

By default, the `speak_dialog()` method is non-blocking. That is any code following the call to `speak_dialog()` will execute whilst Neon is talking. This is useful to allow your Skill to perform actions while it is speaking.

Rather than telling the User that we are fetching some data, then going out to fetch it, we can do the two things simultaneously providing a better experience.

However there are times when we need to wait until the statement has been spoken before doing something else. We have two options for this.

### Wait Parameter

We can pass a `wait=True` parameter to our `speak_dialog()` method. This makes the method blocking and no other code will execute until the statement has been spoken.

```python
@intent_handler('what.is.a.tomato.intent')
def handle_what_is(self, message):
    """Speaks a statement from the dialog file.
       Waits (i.e. blocks) within speak_dialog() until
       the speaking has completed. """
    self.speak_dialog('tomato.description', wait=True)
    self.log.info("I waited for you")
```

### wait_while_speaking

The [`neon_core.audio.wait_while_speaking()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.audio.html#mycroft.audio.wait_while_speaking) method allows us to execute some code, then wait for Neon to finish speaking.

```python
@intent_handler('what.is.a.tomato.intent')
def handle_what_is(self, message):
    """Speaks a statement from the dialog file.
       Returns from speak_dialog() before the
       speaking has completed, and logs some info.
       Then it, waits for the speech to complete. """
    self.speak_dialog('tomato.description')
    self.log.info("I am executed immediately")
    wait_while_speaking()
    self.log.info("But I waited for you")
```

Here we have executed one line of code immediately. Our Skill will then wait for the statement from `i.do.like.dialog` to be spoken before executing the final line of code.

## Using translatable resources

There may be a situation where the dialog file and the `speak_dialog()` method do not give the Skill enough flexibility. For instance, there may be a need to manipulate the statement from the dialog file before having it spoken by Neon.

The NeonSkill class provides four multilingual methods to address these needs. Each method uses a file, and multilingualism is accomplished using the country/language directory system.

The [`translate()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.translate) method returns a random string from a ".dialog" file (modified by a data dictionary).

The [`translate_list()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.translate_list) method returns a list of strings from a ".list" file (each modified by the data dictionary). Same as translate_template() just with a different file extension.

The [`translate_namedvalue()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.translate_namedvalue) method returns a dictionary formed from CSV entries in a ".value" file.

The [`translate_template()`](https://mycroft-core.readthedocs.io/en/latest/source/mycroft.html#mycroft.MycroftSkill.translate_template) method returns a list of strings from a ".template" file (each modified by the data dictionary). Same as translate_list() just with a different file extension.
