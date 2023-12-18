# Export Gradle Properties

Export Gradle properties as step outputs.

This action creates a [map of outputs](https://docs.github.com/en/actions/using-jobs/defining-outputs-for-jobs) via
[Gradle's property report task](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.PropertyReportTask.html).
These outputs can then be referenced by other steps or jobs.


## Usage

```yml
steps:
  - uses: actions/checkout@v4

  - uses: guillermocalvo/gradle-properties@v2
    id: properties
    with:
      # An optional directory to change to.
      # Default: root directory.
      # Can be used when Gradle wrapper is in a different directory.
      change_dir: my_path

      # An optional start directory for Gradle.
      # Default: root project.
      # Can be used in multi-project builds.
      project_dir: lib

      # An optional property to output.
      # Default: all project properties are exported.
      # Can be used to export just one specific property.
      property: name

  - run: 'echo Project name: ${{ steps.properties.outputs.name }}'
```


## Scenarios

These are some example scenarios.


### Export all properties in the root project

```yml
- uses: guillermocalvo/gradle-properties@v2
  id: properties
```


### Export just one property in the root project

```yml
- uses: guillermocalvo/gradle-properties@v2
  id: properties
  with:
    property: version
```


### Export all properties in a subproject

```yml
- uses: guillermocalvo/gradle-properties@v2
  id: properties
  with:
    project_dir: lib
```


### Export all properties when Gradle wrapper is in a different directory

```yml
- uses: guillermocalvo/gradle-properties@v2
  id: properties
  with:
    change_dir: my_path
```


### Create a SNAPSHOT release

```yml
steps:

  - uses: guillermocalvo/gradle-properties@v2
    id: properties
    with:
      property: version

  - uses: softprops/action-gh-release@v2
    if: endsWith(${{ steps.properties.outputs.version }}, '-SNAPSHOT')
    with:
      prerelease: true
```
