gitrack
=======

## Implementation concepts

Any traceability, build numbers, versions, promotions, validations, etc., is stored in a [Git Note](https://www.kernel.org/pub/software/scm/git/docs/git-notes.html), stored at _commit_ level in the `gitrack` namespace.

The format of the note is a YAML content, event oriented, like:

```YAML
- of: build
  name: 11.8.0-345
  at: ...
  by: ...
- of: validation
  name: CI
  at: ...
  by: jenkins
  with: PASSED
- of: validation
  name: QA
  at: ...
  by: jenkins
  with:
    status: FAILED
    description: Error in home page
- of: promotion
  name: COPPER
  at: ...
  by: ...
```

Each event is associated with a type (`of`) and some required attributes like its `name`, the time (`at`, ISO UTC time) and the originator (`by`). The event can optionally be associated with some data (`with`).

Such a content is created:

* manually, using the `git` commands
* through your CI engine, like Jenkins

`gitrack` provides libraries to facilitate the interaction with the content of the `gitrack` namespace.

The same `gitrack` libraries provide also tools to read and query on the content of those notes.

> Those libraries work in a standalone way and do not need to access any particular server.

Finally, there is also a `gitrack` Spring Boot application, which can be deployed on a server or run locally, to display graphs and queries on an annotated Git repository.

For example, we can display a commit graph where commits are filtered according to some criteria and are decorated according to the content of the notes.

## Types

The different event types (`of`) can be associated with some custom data.

For example, a `validation` event is associated with a status and an optional description. Two notations are possible:

* status only without any description:

```YAML
- of: validation
  name: CI
  at: ...
  by: jenkins
  with: PASSED
```

* status and description:

```YAML
- of: validation
  name: QA
  at: ...
  by: jenkins
  with:
    status: FAILED
    description: Error in home page
```

The type must be able to parse such mixte notations.

## Architecture overview

![Modules](doc/modules.png)

## Integration with Ontrack

An Ontrack project with a `gitrack` configuration can have its content (branches, builds, validations, etc.) entirely synchronised from the Git notes.

See [Ontrack #292](https://github.com/nemerosa/ontrack/issues/292) for the Ontrack integration.

## Architecture guidelines

## Modules

### Config

### Model

### Client

### API

### Web

### Jenkins

## History

The [Ontrack](https://github.com/nemerosa/ontrack) is born in order to allow to add traceability in the continuous pipelines of many projects and branches:

* branches
* builds and commits
* tracking of validations and promotions
* change logs between builds

Born with Subversion, it was then adapted to Git, but didn't really take advantage of this SCM.

_Gitrack_ is a conceptual fork of _Ontrack_ where we start to take full advantage of the capabilities of Git, leading to more flexibility and simplicity.
