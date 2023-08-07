---
description: >-
  The Neon GUI Framework has a number of common methods for displaying
  standard simple content types.
---

# Show Simple Content

## Text

Display simple strings of text.

```python
self.gui.show_text(self, text, title=None, override_idle=None, override_animations=False)
```

Arguments:

- text \(str\): Main text content. It will auto-paginate.
- title \(str\): A title to display above the text content.
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely.
  - \(int\): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## Static Image

Display a static image such as a jpeg or png.

```python
self.gui.show_image(self, url, caption=None, title=None, fill=None, override_idle=None, override_animations=False)
```

Arguments:

- url \(str\): Pointer to the image
- caption \(str\): A caption to show under the image
- title \(str\): A title to display above the image content
- fill \(str\): Fill type - supports:
  - 'PreserveAspectFit',
  - 'PreserveAspectCrop',
  - 'Stretch'
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely.
  - \(int\): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## Animated Image

Display an animated image such as a gif.

```python
self.gui.show_animated_image(self, url, caption=None, title=None, fill=None, override_idle=None, override_animations=False)
```

Arguments:

- url \(str\): Pointer to the .gif image
- caption \(str\): A caption to show under the image
- title \(str\): A title to display above the image content
- fill \(str\): Fill type - supports:
  - 'PreserveAspectFit',
  - 'PreserveAspectCrop',
  - 'Stretch'
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely.
  - \(int\): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## HTML Page

Display a local HTML page.

```python
self.gui.show_html(self, html, resource_url=None, override_idle=None, override_animations=False)
```

Arguments:

- html \(str\): HTML text to display
- resource_url \(str\): Pointer to HTML resources
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely.
  - \(int\): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## Input Box

Display an input box.

```python
self.gui.show_input_box(self, title=None, placeholder=None, confirm_text=None, exit_text=None, override_idle=None, override_animations=False)
```

- title \(str\): Title to display above the input box
- placeholder \(str\): Placeholder text to display in the input box
- confirm_text \(str\): Confirmation text to display on the confirm button
- exit_text \(str\): Exit text to display on the exit button
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely
  - (int): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## Remote URL

Display a webpage.

```python
self.gui.show_url(self, url, override_idle=None, override_animations=False)
```

Arguments:

- url \(str\): URL to render
- override_idle \(boolean, int\):
  - True: Takes over the resting page indefinitely.
  - \(int\): Delays resting page for the specified number of seconds.
- override_animations \(boolean\):
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.

## Show Input Box

Display a fullscreen UI for a user to enter text and confirm or cancel

```python
self.gui.show_input_box(self, title, placeholder, confirm_text, exit_text, override_idle=True, override_animations=True)
```

Arguments:

- title (Optional[str]): title of input UI should describe what the input is
- placeholder (Optional[str]): default text hint to show in an empty entry box
- confirm_text (Optional[str]): text to display on the submit/confirm button
- exit_text (Optional[str]): text to display on the cancel/exit button
- override_idle (Union[int, bool]):
  - True: takes over the resting page indefinitely
  - int: Delays resting page for the specified number of seconds.
- override_animations (bool): disable showing all platform animations
  - True: Disables showing all platform skill animations.
  - False: 'Default' always show animations.
