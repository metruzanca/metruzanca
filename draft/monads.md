---
draft: true
title: "Monads"
date: 2000-01-01T00:00:00.000Z
description: Monads explained simply
tags: ["coding patterns", "monads", "typescript"]
---
<!-- Make a thumbnail with the monads symbol >>=, take inspiration from various yt videos on monads -->
<!-- Maybe include the quite "Monads are a Monoid in the category of Endofunctors" for shits and giggles -->


Put simply; Monads are a pattern for building pipelines that abstract away a common behavior, allowing you to write more declarative code.
To elaborate further on that statement, monads are a pattern that you can apply any time you've got a bunch of actions that all have a common behaviour.
One example of this can be checking for null / error after function calls.

Take this pseudo code for example:
```py
# No language in particular.
data = get_data()
if data == null
  return Error("Couldn't get data")

processed = process_data(data)
if processed == null
  return Error("Couldn't process data")

serialized = serialize_data(processed)
if processed == null
  return Error("Couldn't serialize data")

return serialized
```

Most languages have monads. My goto language, javascript(/typescript) famously has Promises, which use the Monad Pattern.