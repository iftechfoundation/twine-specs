# Twine 2 JSON Specification (v1.0)

This specification defines a series of properties for encoding story and passage data compatible with Twine 2 and related tools using [JavaScript Object Notation (JSON)](https://www.json.org/json-en.html).

## Story Data Encoding

To maintain compatibility with the existing output formats of [Twine 2 HTML](https://github.com/iftechfoundation/twine-specs/blob/master/twine-2-htmloutput-spec.md) and [Twee 3 notation](https://github.com/iftechfoundation/twine-specs/blob/master/twee-3-specification.md), story properties encoded in JSON mirror those found in the [`StoryData` passage in Twee 3 notation](https://github.com/iftechfoundation/twine-specs/blob/master/twee-3-specification.md#storydata):

- `name`: (string) Required. The name of the story. Maps to `<tw-storydata name>`.
- `ifid`: (string) Optional. An IFID is a sequence of between 8 and 63 characters, each of which shall be a digit, a capital letter or a hyphen that uniquely identify a story (see [Treaty of Babel](https://babel.ifarchive.org/)). Maps to `<tw-storydata ifid>`.
- `format`: (string) Optional. The name of the story format used to create the story. Maps to `<tw-storydata format>`.
- `format-version`: (string) Optional. The version of the story format used to create the story. Maps to `<tw-storydata format-version>`.
- `start`: (string) Optional. The PID matching a `<tw-passagedata>` element whose content should be displayed first. Maps to `<tw-storydata startnode>`.
- `tag-colors`: (object of tag(string):color(string) pairs) Optional. Pairs map to `<tw-tag>` nodes as `<tw-tag name>:<tw-tag color>`.
- `zoom`: (decimal) Optional. Maps to `<tw-storydata zoom>`.
- `creator`: (string) Optional. The name of program used to create the file. Maps to `<tw-storydata creator>`.
- `creator-version`: (string) Optional. The version of the program used to create the file. Maps to `<tw-storydata creator-version>`.
- `style`: (string) Optional. Story Stylesheet content. Maps to `<style role="stylesheet" id="twine-user-stylesheet" type="text/twine-css"></style>`.
- `script`: (string) Optional. Story JavaScript content. Maps to `<script role="script" id="twine-user-script" type="text/twine-javascript"></script>`.
- `passages`: (array of objects) Required. Array of one or more passage objects encoded in JSON.

```json
{
  "name": "Example",
  "ifid": "D674C58C-DEFA-4F70-B7A2-27742230C0FC",
  "format": "Snowman",
  "format-version": "3.0.2",
  "start": "My Starting Passage",
  "tag-colors": {
    "bar": "Green",
    "foo": "red",
    "qaz": "blue"
  },
  "zoom": 0.25,
  "creator": "Twine",
  "creator-version": "2.8",
  "style": "",
  "script": "",
  "passages": [
    {
      "name": "My Starting Passage",
      "tags": ["tag1", "tag2"],
      "metadata": {
        "position":"600,400",
        "size":"100,200"
      },
      "text": "Double-click this passage to edit it."
    }
  ]
}
```

## Passage Data Encoding

Passage properties mirror those found in Twee 3 notation:

- `name`: (string) Required. Name of the passage.
- `tags`: (array of tag(string) ) Required. An Array of tags as string values.
- `metadata`: (object of name(string):value(string) pairs). Optional. As in Twee 3 notation, a passage can contain multiple name-value pairs. For compatibility with Twine 2, the currently supported properties include:
  - `position`: (string) Comma separated passage tile positional coordinates (e.g., "600,400").
  - `size`: (string) Comma separated passage tile width and height (e.g., "100,200").
- `text`: (string) Required. Content of the passage.

```json
{
  "name": "My Starting Passage",
  "tags": ["tag1", "tag2"],
  "metadata": {
    "position":"600,400",
    "size":"100,200"
  },
  "text": "Double-click this passage to edit it."
}
```

## Optional Partial Story Encoding

Using JSON representation, a story can be encoded in whole or in part. When directly converting from one output format to another, story metadata is suggested but optional.

Because `name` and `passages` are the only required story metadata properties, a selection of content, named "partial story encoding," is possible to represent a subset of passages and story metadata. Depending on the story format, this allows for representing in JSON additional story content, CSS, or JavaScript for including as part of a larger story compilation process.
