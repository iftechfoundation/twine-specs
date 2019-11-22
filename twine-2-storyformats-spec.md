# Twine 2 Story Formats (Draft)

Story formats are JavaScript files used by Twine or other compiling tools to convert story and passage data into an HTML file.  The story and passage data is usually stored internally by Twine 2, but can also be stored in a Twee3 text file.

## Introduction

Twine and other tools combine the story format's "source" and the story and passage data into a new HTML file.  This is done by replacing placeholders in the source with the particular story's passages and metadata, wrapped in custom HTML elements or inserted into custom attributes.  (See the Twine 2 HTML spec for more details about the final HTML output.)

### Playable Story Formats

Playable story formats include code to extract the passage contents from the custom HTML and display it as the story "runs."  The allowed passage content varies from story format to story format.  For example, some story formats expect the story's text to be in Markdown, while others use wiki-style formatting.  (In either case, the story format's code converts the text into HTML in order to display it in a browser.)

Playable story formats usually include their own unique macro and scripting functionality, and each one only understands its own programming "language."  The built-in Twine 2 playable story formats (Harlowe, Snowman, and SugarCube) differ significantly from one other both in their included scripting functionality and in the extent to which they also allow JavaScript to be used for scripting.

### Proofing Story Formats

Proofing and other utility formats do not "run" the story, so they tend to be shorter, simpler, and more agnostic than the playable story formats.  Instead of interpreting the contents of passages, proofing story formats merely reformat or analyze the raw passage data.  For example, [Paperthin](https://twinery.org/2/story-formats/paperthin-1.0.0/format.js), the proofing format built into Twine 2, is only 1200 bytes, yet can be used to proofread any Twine 2 story regardless of the story format used to play the story.

Story format output is always HTML.  Some utility formats' intended output is not HTML (for example, those that export to Twee or JSON), and such formats must use a workaround to extract the desired data type from the HTML created by Twine or another compiling tool.

## Installation

Twine 2 story formats are generally hosted online, but they can also be loaded from the local filesystem into most desktop Twine 2 versions.

### Adding Story Formats

To add a story format to Twine 2, use its full URL, e.g., `https://twinery.org/2/story-formats/snowman-1.3.0/format.js`, or, for a local file, the file URI scheme, e.g., `file:///Users/user/path/to/snowman-1.3.0/dist/format.js` or `file:///C:/path/to//snowman-1.3.0/dist/format.js`. Spaces and other special characters should be escaped using [percent encoding](https://en.wikipedia.org/wiki/Percent-encoding).

For compiling tools other than Twine 2, story formats are either built-into the tool or are loaded from the file system normally (without the need for file URIs).

## Structure

In Twine 2 a story format is a single JavaScript file, usually called `format.js`.

### Wrapper

The story format JavaScript file consists of a single function call which takes the story format details as an argument (a JavaScript object):

```
window.storyFormat({
	"name": "Snowman",
	"version": "1.3.0",
	...
});
```

While ideally the object should be formatted as JSON, this is not currently required by compilers.

### Keys

The following keys are found in most or all story formats:

* name: (string) *Optional.* The name of the story format. (Omitting the name will lead to an Untitled Story Format.)
* version: (string) *Required*, and semantic version-style formatting (*x.y.z*, *e.g.*, 1.2.1) of the version is also required.
* author: (string) *Optional.*
* description: (string) *Optional.*
* image: (string) *Optional.* The filename of an image (ideally SVG) served from the same directory as the `format.js` file.
* url: (string) *Optional.*  The URL of the directory containing the `format.js` file.
* license: (string) *Optional.*
* proofing: (boolean) *Optional* (defaults to false). True if the story format is a "proofing" format.  The distinction is relevant only in the Twine 2 UI.
* source: (string) *Required*. An adequately escaped string containing the full HTML output of the story format, including the two placeholders `{{STORY_NAME}}` and `{{STORY_DATA}}`. (The placeholders are not themselves required.)

Harlowe also includes a deprecated *setup* key used to install its syntax highlighter into the Twine 2 UI.  Proprosed replacements include *codeMirrorSyntax* and *editorToolbar*, but these have not yet been implemented.

In Twine 2, a new story format is considered different from other installed story formats if (A) the name is not the same as any currently installed format or (B) the name is the same as an installed format but the version is not.  Twine 2 does not allow installed story formats of the same version and name to be overwritten.

## Twine 1 Story Formats

In Twine 1.4, story formats were local, contained in a single HTML file (analogous to the source section of a Twine 2 story format) called `header.html`.  The file used two wildcards `"STORY"`, for the set of passages, and `"STORY_SIZE"`, for the number of passages.  The story format name was derived from the containing folder.  License info and other supporting files could also be included in the same folder.  (Twine 1 versions prior to 1.4 were somewhat different.)

The story formats built into Twine 1 (Jonah, Sugarcane, and later Responsive) were much more closely related to one another and thus easier to switch between than the major Twine 2 formats are.  Twine 1 story formats should no longer be used due to bugs and other incompatibilities with modern browsers.  Instead, if Twine 1 must be used for some reason, a modern story format that also supports Twine 1.4 (most notably [SugarCube](https://www.motoslave.net/sugarcube/2/#downloads)) should be installed and used instead of the built-in formats.
