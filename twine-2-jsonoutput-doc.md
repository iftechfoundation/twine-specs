# Twine 2 JSON Specification (v1.0)

This specification defines a series of properties for encoding story and passage data compatible with Twine 2 and related tools using [JavaScript Object Notation (JSON)](https://www.json.org/json-en.html).

## Story Encoding

To maintain close compatibility with the existing formats of [Twine 2 HTML](https://github.com/iftechfoundation/twine-specs/blob/master/twine-2-htmloutput-spec.md) and [Twee 3 notation](https://github.com/iftechfoundation/twine-specs/blob/master/twee-3-specification.md), story properties encoded in JSON mirror those found in the [`StoryData` passage in Twee 3 notation](https://github.com/iftechfoundation/twine-specs/blob/master/twee-3-specification.md#storydata):

- `ifid`: (string) Required. Maps to `<tw-storydata ifid>`.
- `format`: (string) Optional. Maps to `<tw-storydata format>`.
- `format-version`: (string) Optional. Maps to `<tw-storydata format-version>`.
- `start`: (string) Optional. Maps to `<tw-passagedata name>` of the node whose pid matches `<tw-storydata startnode>`.
- `tag-colors`: (object of tag(string):color(string) pairs) Optional. Pairs map to `<tw-tag>` nodes as `<tw-tag name>:<tw-tag color>`.
- `zoom`: (decimal) Optional. Maps to `<tw-storydata zoom>`.
- `passages`: (array of objects) Required. Array of one or more passage objects encoded in JSON.

```json
{
  "ifid": "D674C58C-DEFA-4F70-B7A2-27742230C0FC",
  "format": "Snowman",
  "format-version": "3.0.2",
  "start": "My Starting Passage",
  "tag-colors": {
    "bar": "green",
    "foo": "red",
    "qaz": "blue"
  },
  "zoom": 0.25,
  "passages": [
    {
      "name": "My Starting Passage",
      "tags": ["tag1", "tag2"],
      "metadata": {
        "position":"600,400",
        "size":"100,200"
      },
      "content": "Double-click this passage to edit it."
    }
  ]
}
```

## Passage Encoding

Passage properties mirror those found in Twee 3 notation:

- `name`: (string) Required. Name of the passage.
- `tags`: (array of tag(string) ) Required. An Array of tags as string values.
- `metadata`: (object of name(string):value(string) pairs). Optional. As in Twee 3 notation, a passage can contain multiple name-value pairs. For compatibility with Twine 2, the currently supported properties include:
  - `position`: (string) Comma separated passage tile positional coordinates (e.g., "600,400").
  - `size`: (string) Comma separated passage tile width and height (e.g., "100,200").
- `content`: (string) Required. Content of the passage.

```json
{
  "name": "My Starting Passage",
  "tags": ["tag1", "tag2"],
  "metadata": {
    "position":"600,400",
    "size":"100,200"
  },
  "content": "Double-click this passage to edit it."
}
```
