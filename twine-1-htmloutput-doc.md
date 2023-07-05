# Twine 1 HTML Output Documentation (v1.0)

**Note:** This is historical documentation on a version of Twine no longer maintained. It is provided to help current and future developers understand and potentially parse HTML files created with Twine 1 or compatible Twee compilers.

Twine 1 stores its data within HTML elements.

The root of a Twine 1 story is an element with the `id` of `storeArea`. Inside of this are passages encoded as children with each containing the `tiddler` attribute along with a value for the name of the passage.

**Example:**

```html
<div id="storeArea">
    <div 
      tiddler="Example">
    </div>
</div>
```

Twine 1.4 also includes a `data-size` attribute containing the number of passages in the story.

```html
<div id="storeArea" data-size="1">
    <div 
      tiddler="Example">
    </div>
</div>
```

## Story Data

Story metadata is recorded across a number of HTML elements using the `id` attribute. These include:

* storyTitle: (string) Required. Title of the story, if any.
* storyAuthor: (string) Required. Author of the story, if added.
* storySubtitle: (string) Required. Subtitle of story, if any.

**Example:**

```html
<span id="storyTitle"></span>
<span id="storyAuthor"></span>
<span id="storySubtitle"></span>
```

## Passages

Each passage is represented as a child of an element with the `id` of `storeArea` with metadata encoded within its attributes:

* tiddler: (string) Required. Name of passage.
* tags: (string) Required. Comma-separated list of passages tags, if any.
* created: (string) Required. A timestamp of when the passage was created.
* modifier: (string) Required. Format of the passage. This is always "twee".
* twine-position: (string) Required. The X and Y coordinates of the passage within Twine 1 separated by a comma.

Passage content is stored within the element itself as a single text node and must contain no other child nodes; all `&`, `<`, `>`, `"`, and `'` characters should be escaped into their corresponding HTML entities.

**Example:**

```html
<div 
  tiddler="Start" 
  tags="" 
  created="202306020121" 
  modifier="twee" 
  twine-position="10,10">[[One passage]]</div>
```

**Note:** There must always be an element with the attribute-value pair of `tiddler="Start"` for a Twine 1 story format to correctly parse and present passage data.

## Story Stylesheet

Beginning with Twine 1.4, the story stylesheet is included as a child `<style>` element of the `<head>` element using the `id` attribute value of `"storyCSS"`.

**Example:**

```html
<head>
  <style id="storyCSS"></style>
</head>
```

## Determining Tool Creator

Depending on the tool used to create the HTML, there will be different text within the HTML.

For Twine 1.4 and its Twee compiler, the text will read "Made in Twine" followed by a version and the build time on the next line.

**Example:**

```html
Made in Twine 1.4.2
Built on 20 Dec 2014 at 19:25:29, -0800
```

If created using the twee compiler Tweego, the text will read "Compiled with Tweego" followed by its version number.
