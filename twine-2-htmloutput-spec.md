# Twine 2 HTML Output (Draft)

Twine 2 stores its data within HTML elements.

The root of a Twine 2 story is the `<tw-storydata>` element with attributes encoding metadata about the story itself. Inside of this element are the `<tw-passagedata>` (Passages), `<tw-tag>` (Passage Tag Colors), `<style>` (Story Stylesheet), and `<script>` (Story JavaScript) elements.

**Example:**
```
<tw-storydata>
  <style
    role="stylesheet"
    id="twine-user-stylesheet"
    type="text/twine-css">
  </style>
  <script
    role="script"
    id="twine-user-script"
    type="text/twine-javascript">
  </script>
  <tw-tag
    name="tagName"
    color="orange">
  </tw-tag>
  <tw-passagedata></tw-passagedata>
</tw-storydata>
```

## Story Data

Story metadata is stored as attributes on the `<tw-storydata>` element.

* name: (string) Required. The name of the story.
* ifid: (string) Required. An IFID is a sequence of between 8 and 63 characters, each of which shall be a digit, a capital letter or a hyphen that uniquely identify a story (see [Treaty of Babel](https://babel.ifarchive.org/)).
* format: (string) Optional. The story format used to create the story.
* format-version: (string) Optional. The version of the story format used to create the story.
* startnode: (string) Optional. The PID matching a `<tw-passagedata>` element whose content should be displayed first.
* zoom: (string) Optional. The decimal level of zoom (i.e. 1.0 is 100% and 1.2 would be 120% zoom level).

**Example:**
```
<tw-storydata
  name="DocumentationExample"
  startnode="1"
  creator="Twine"
  creator-version="2.3.3"
  ifid="6D509890-1CA5-49DF-BFB6-5CA35B8DE2AC"
  zoom="1"
  format="Harlowe"
  format-version="3.0.2">
  ...
</tw-storydata>
```

## Passages

Each passage is represented as a `<tw-passagedata>` element with its metadata stored as its attributes.

* pid: (string) Required. The Passage ID (PID). (Note: This is subject to change during editing with Twine 2.)
* name: (string) Required. The name of the passage.
* tags: (string) Optional. Any tags for the passage separated by spaces.
* position: (string) Optional. Comma-separated X and Y position of the upper-left of the passage when viewed within the Twine 2 editor.
* size: (string) Optional. Comma-separated width and height of the passage when viewed within the Twine 2 editor.

Passage content is stored within a text node of the `<tw-passagedata>` element itself; all `&`, `<`, `>`, `"`, and `'` characters should be escaped into their corresponding HTML entities.

`<tw-passagedata>` should have no other children elements than a single text node.

**Example:**
```
<tw-passagedata
  pid="1"
  name="Start"
  tags="tag1 tag2"
  position="102,99"
  size="100,100">
Some content
</tw-passagedata>
```

##  Story JavaScript

Any JavaScript code saved from the Story JavaScript window of the Story menu in Twine 2 is kept within a single `<script>` element with its `type` attribute set to `text/twine-javascript`.

This will be executed upon story start.

**Example:**
```
<script id="twine-user-script" type="text/twine-javascript">window.setup = {};</script>
```

## Story Stylesheet

Any CSS rules saved from the Story Stylesheet window from the Story menu in Twine 2 are kept within a single `<style>` element with its `type` attribute set to `text/twine-css`.

Any CSS rules found within the element will applied to the document upon story start.

**Example:**
```
<style id="twine-user-stylesheet" type="text/twine-css">body {font-size: 1.5em;}</style>
```

## Passage Tag Colors

If any passage tags have been assigned colors within the Twine 2 editor, they will appear as `<tw-tag>` elements. The `name` attribute will be the name of the tag and the `color` attribute will be one of seven named colors.

Named Colors:
* `gray`
* `red`
* `orange`
* `yellow`
* `green`
* `blue`
* `purple`

**Example:**
```
<tw-tag name="one" color="gray"></tw-tag>
<tw-tag name="two" color="purple"></tw-tag>
```
