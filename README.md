# ez-pedro

A start-to-finish PedroPath v3 authoring, visualization, and deployment tool

## Goals

Primarily: shift from code-centric (write a bunch of Java syntax code that
represents values, poses, paths, interpolations, paths) pathing to a
data-centric (write a bunch of JSON syntax values, poses, paths, interpolations,
paths) model.

This editor (or raw JSON) should be the _only_ way users create and/or modify
paths.

Installation should be a single command (`bunx @freik/ez-pedro`?). The other
capabilities should all be in the web interface. If folks want to use the
commands directly, that should _also_ be doable.

## Usage

JSON files contain lists of named values, poses, curves/lines, interpolations,
and paths. Those names can refer to other items in the same file, or in other
files. If the first character of the name is ':' (colon) then the item
references is in the file named before the second colon. For example `myHeading`
refers to the value in the same file. But `:values:other-heading` refers to the
value `other-heading` in the file `values.json`.

These files are included as part of the build. The JSON files are stored in the
assets location, normally. The java part of this is to enable path parsing &
pedro path generation, as well as have the 'by name' interface: If you have a
single file, it's just `PathFile.get("myPath")`. If you have multiple files,
it's `PathFile.get("file", "myPath")`.

At the end of the day, this feels like maybe it should be a quick-start setup? I
don't really like quick-starts, because they preclude less technically capable
teams of using multiple quick-starts. That's why I prefer the
`bunx @freik/ez-pedro install` angle. It just requires that you have Bun already
installed (not a high bar, but still a bar...)

## Code Structure

- The back-end reads from & writes to the JSON files in the `TeamCode` module.
- The front-end is the viewer & editor of the JSON files.
- The code-deployment module produces Java code for loading from the JSON files.
  I prefer this as generated code instead of a library, so that it's not as
  complicated.
- The path-deployment module can update the .JSON files directly on a connected
  control hub, via `adb`. It should _also_ update the JSON file in the
  `TeamCode` module, so you'll never lose anything, and timestamps can be used
  to determine which one is most recent.
