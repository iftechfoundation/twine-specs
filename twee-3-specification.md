# Twee 3 Specification (v3.0.1)

## Introduction

Twee is a text format for marking up the source code of Twine stories.  It is the equivalent of Twine HTML files, which contain story and passage data, but in a more human-readable format.

Twee notation was the original way to write and compile Twine stories—via the original, and now obsolete, compiler of the same name.  Later, when Twine 1 was introduced, it was used alongside Twine 1's native format as an import/export option.  With the introduction of Twine 2, Twee was separated from Twine.  In time, other compilers were written to take advantage of the format to generate Twine compatible HTML files.

Over several years, multiple extensions to Twee and its possible options were created and used by different software.  This specification seeks to unify these options into a single document that best represents existing community practices and properly encapsulates the data stored within the `<tw-storydata>` and `<tw-passage>` elements of Twine 2-style HTML files.

## File Type

It is recommended Twee files have a `.tw` or `.twee` file extension.

## Twee Notation

It is recommended Twee files are written in UTF-8.

Twee files are composed of one or more passages.  Passages consist of a header and content section.

### Passage Header

Each header must be a single line and is composed of the following components (in order):

1. Required start token, a double colon (`::`), that must begin the line.
2. Required passage name.
3. Optional tag block that must directly follow the passage name.
4. Optional metadata block, an inline JSON chunk, that must directly follow either the tag block or, if the tag block is omitted, the passage name.

NOTE: Each component after the start token may be preceded by one or more spaces for readability.

To prevent ambiguity during parsing, passage and tag names that include the optional tag and metadata block opening and closing metacharacters (i.e. `[`, `]`, `{`, `}`) must escape them.

- Encoding: The escapement mechanism is to prefix the escaped characters with a backslash (`\`).  To avoid ambiguity, non-escape backslashes must also be escaped via the same mechanism (`foo\bar` yields `foo\\bar`).
- Decoding: To make decoding more robust, any escaped character within a chunk of encoded text yields the character minus the backslash (`\q` yields `q`).

**Examples:**

Minimal working example:

```
:: An overgrown path
```

With only optional tags:

```
:: An overgrown path [forest spooky]
```

With only optional metadata:

```
:: An overgrown path {"position":"600,400","size":"100,200"}
```

With both optional tags and metadata:

```
:: An overgrown path [forest spooky] {"position":"600,400","size":"100,200"}
```

#### Passage Name

It is recommended that passage names should not contain link markup metacharacters like `[`, `]`, or `|`.

#### Tag Block

The optional tag block is a space separated list of tag names enclosed between opening and closing square brackets (i.e. `[`, `]`). Tags themselves must not contain spaces.

#### Metadata Block

The optional metadata block is an inline JSON chunk.  The currently supported properties include:

- `position`: (string) Comma separated passage tile positional coordinates (e.g., `"600,400"`).
- `size`: (string) Comma separated passage tile width and height (e.g., `"100,200"`).

COMPILERS: It is recommended that the outcome of a decoding error should be to: emit a warning, discard the metadata, and continue processing of the passage.

### Passage Content

The content section begins on the next line after the passage header and continues until the next passage header or the end of file.

COMPILERS: Trailing blank lines must be ignored/omitted.

## Special Passages

Special passages are simply passage names that have special meaning to compilers, story formats, or both.  The following have special meaning to all Twee 3 compilers.

NOTE: This is not an exhaustive list of special passage names.

### `StoryTitle`

The project's name.  Maps to `<tw-storydata name>`.

### `StoryData`

A JSON chunk encapsulating various Twine 2 compatible details about your project.  The currently supported properties include:

- `ifid`: (string) Required.  Maps to `<tw-storydata ifid>`.
- `format`: (string) Optional.  Maps to `<tw-storydata format>`.
- `format-version`: (string) Optional.  Maps to `<tw-storydata format-version>`.
- `start`: (string) Optional.  Maps to `<tw-passagedata name>` of the node whose `pid` matches `<tw-storydata startnode>`.
- `tag-colors`: (object of tag(string):color(string) pairs) Optional.  Pairs map to `<tw-tag>` nodes as `<tw-tag name>`:`<tw-tag color>`.
- `zoom`: (decimal) Optional.  Maps to `<tw-storydata zoom>`.

COMPILERS:

- An IFID (Interactive Fiction IDentifier) uniquely identifies compiled projects—see [The Treaty of Babel](https://babel.ifarchive.org/).  Twine 2 uses v4 (random) [UUIDs](https://en.wikipedia.org/wiki/Universally_unique_identifier), using only capital letters, and Twee 3 compilers must follow suit to ensure maximum compatibility.
- For readability, it is recommended that the JSON be pretty-printed (line-broken and indented).
- It is recommended that the outcome of a decoding error should be to: emit a warning, discard the metadata, and continue processing the file.

**Example:**

```
:: StoryData
{
	"ifid": "D674C58C-DEFA-4F70-B7A2-27742230C0FC",
	"format": "SugarCube",
	"format-version": "2.28.2",
	"start": "My Starting Passage",
	"tag-colors": {
		"bar": "green",
		"foo": "red",
		"qaz": "blue"
	},
	"zoom": 0.25
}
```

### `Start`

The default starting passage.  May be overridden by the story metadata or compiler command.

## Special Tags

Special tags are simply tag names that have special meaning to compilers, story formats, or both.  The following have special meaning to all Twee 3 compilers.

NOTE: This is not an exhaustive list of special tag names.

### `script`

Signifies that the passage's contents are JavaScript code.  Maps to the `<script type="text/twine-javascript">` node.

### `stylesheet`

Signifies that the passage's contents are CSS style rules.  Maps to the `<style type="text/twine-css">` node.

## Processing Different Versions of Twee

It is recommended that any software accepting Twee notation test for and attempt to parse data sections officially part of this specification or informally part of other Twee versions.  How to handle data from other informal versions of Twee, however, is left up to software authors.

## Requirements and Recommendations

For all sections using the word **must**, software authors are required to implement and follow the requirements therein.

For all sections using the word **recommended**, software authors are encouraged, but not required, to implement and follow the guidelines therein.
