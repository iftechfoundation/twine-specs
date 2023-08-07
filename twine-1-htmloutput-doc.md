# Twine 1 HTML Output Documentation (v1.0)

**Note:** This is historical documentation on a version of Twine no longer maintained. It is provided to help current and future developers understand and potentially parse HTML files created with Twine 1 or compatible Twee compilers.

Twine 1 stores its data within HTML elements.

The root of a Twine 1 story is an element with the `id` of `storeArea` or `store-area`, depending on the story format used. Inside of this are passages encoded as element children with each containing the `tiddler` attribute along with a value for the name of the passage.

**Example**

```html
<div id="storeArea">
    <div 
      tiddler="Start">
    </div>
</div>
```

**Example**

```html
<div id="store-area">
    <div 
      tiddler="Start">
    </div>
</div>
```

Some versions of Twine 1 also include a `data-size` attribute containing the number of passages in the story:

**Example**

```html
<div id="storeArea" data-size="1">
    <div 
      tiddler="Start">
    </div>
</div>
```

**Example**


```html
<div id="store-area" data-size="1">
    <div 
      tiddler="Start">
    </div>
</div>
```

## Story Data

Story data can be found across multiple elements and identifications based on different version of Twine 1 and story format.

It is recommended to begin searching for story data in elements with the following `tiddler` values:

* StoryTitle: (string). Title of story.
* StoryAuthor: (string). Author of story.
* StorySubtitle: (string). Subtitle of story.
* StorySettings: (string). Newline separated list of possible settings in the form of "option:value" per line. (See sub-section on Story Settings.)

**Example**

```html
<div 
  tiddler="StoryTitle">Title</div>
<div 
  tiddler="StoryAuthor">Author</div>
<div 
  tiddler="StorySubtitle">Subtitle</div>
<div 
  tiddler="StorySettings">jquery:off\nhash:off\nbookmark:on\nmodernizr:off\nundo:off\nobfuscate:rot13\nexitprompt:off\nblankcss:off\n</div>
```

Depending on the story format and its internal processing, story data may also be found in elements with the following `id` attribute values:

* storyTitle: (string). Title of the story.
* storyAuthor: (string). Author of the story.
* storySubtitle: (string). Subtitle of story.

**Example:**

```html
<span id="storyTitle">Title</span>
<span id="storyAuthor">Author</span>
<span id="storySubtitle">Subtitle</span>
```
### Story Settings

In some versions of Twine 1, additional options can be found in a passage with the `tiddler` value of `"StorySettings"`. Within this passage may be found a newline-separated key-value listing of settings and their values.

Most key-value pairs in the listing have either an `on` or `off` value with a major exception of the `obfuscate`, which has either `off` or `rot13` for obfuscating `tiddler` values except for `"StorySettings"`.

These settings, which may appear in any order, include the following:

* `undo`: `on` or `off`.
* `bookmark`: `on` or `off`.
* `hash`: `on` or `off`.
* `exitprompt`:`on` or `off`.
* `blankcss`: `on` or `off`.
* `obfuscate`: `off` or `rot13`.
* `jquery`: `on` or `off`.
* `modernizr`: `on` or `off`.

## Passages

Each passage is represented as an individual child element of the story element with metadata encoded within its attributes:

* tiddler: (string) Required. Name of passage. (See special passage names in the Story Data section.)
* tags: (string) Required. Space-separated list of passages tags, if any.
* twine-position: (string) Required. Comma-separated X and Y coordinates of the passage within Twine 1.
* modifier: (string) Optional. Name of the tool that last edited the passage. Generally, for versions of Twine 1, this value will be "twee". Twee compilers may place their own name (e.g. "tweego" for Tweego).
* created: (string) Optional. Datestamp of passage creation.
* modified: (string) Optional. Datestamp of last modification date of the passage.

Twine 1 translates passage content based on the following mapping of special symbols:

| Original  | Translation  |
| ----------| -------------|
| `&`       | `&amp;`      |
| `<`       | `&lt;`       |
| `>`       | `&gt;`       |
| `"`       | `&quot;`     | 
| `\\`      | `\\s`        |
| `\t`      | `\\t`        |
| `\n`      | `\\n`        |

**Example:**

```html
<div 
  tiddler="Start" 
  tags="" 
  created="202306020121" 
  modifier="twee" 
  twine-position="10,10">[[One passage]]</div>
```

**Note:** There must always be an element with the attribute-value pair of `tiddler="Start"` for a Twine 1 story format to correctly parse and present passage data. If the `tiddler="StorySettings"` element exists, and the `obfuscate`: `rot13` setting enabled, the `tiddler="Start"` name may be obfuscated.

## Story Stylesheet

The story stylesheet may be found in an element using the `id` attribute value of `"storyCSS"` or `"story-style"`, depending on the story format.

**Example:**

```html
<head>
  <style id="storyCSS"></style>
</head>
```

**Example:**

```html
<head>
  <style id="story-style"></style>
</head>
```

## Determining Tool Creator

For some story formats, the tool creator and version will be found inside an HTML comment element within the `<head>` element:

**Example:**

```html
<head>
  <!--
    Made in Twine 1.4.2
    Built on 20 Dec 2014 at 19:25:29, -0800
  -->
</head>
```

Other story formats may include the same data preceded by the words "Build Info:":

```html
<!--

Build Info:
  * "Made in Twine 1.4.2"
  * "Built on 20 Dec 2014 at 19:25:29, -0800"

-->
```
