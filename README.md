# Export Gradle Properties

Export Gradle properties as step outputs.

This action creates a [map of outputs](https://docs.github.com/en/actions/using-jobs/defining-outputs-for-jobs) via
[Gradle's property report task](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.PropertyReportTask.html).
These outputs can then be referenced by other steps or jobs.


## Usage

```yml
steps:
  - uses: actions/checkout@v4

  - uses: guillermocalvo/gradle-properties@v3
    id: properties
    with:
      # The output file to export properties to.
      # Can be used either to generate GitHub outputs, or to write them to a file.
      output_file: ${{ github.output }}

      # An optional directory to change to.
      # Default: root directory.
      # Can be used when Gradle wrapper is in a different directory.
      change_dir: my/path

      # An optional start directory for Gradle.
      # Default: root project.
      # Can be used in multi-project builds.
      project_dir: my_project

      # An optional list of comma-separated property names to export.
      # Default: all project properties are exported.
      # Can be used to export only specific properties.
      export: name

  - run: 'echo Project name: ${{ steps.properties.outputs.name }}'
```


## Scenarios

These are some example scenarios.


### Export all properties in the root project as step outputs

```yml
- uses: guillermocalvo/gradle-properties@v3
  id: properties
  with:
    output_file: ${{ github.output }}
```


### Export just one property in the root project as a file

```yml
- uses: guillermocalvo/gradle-properties@v3
  id: properties
  with:
    output_file: description.txt
    export: description
```


### Export all properties in a subproject

```yml
- uses: guillermocalvo/gradle-properties@v3
  id: properties
  with:
    output_file: ${{ github.output }}
    project_dir: my_project
```


### Export all properties when Gradle wrapper is in a different directory

```yml
- uses: guillermocalvo/gradle-properties@v3
  id: properties
  with:
    output_file: ${{ github.output }}
    change_dir: my/path
```


### Create a SNAPSHOT release

```yml
steps:

  - uses: guillermocalvo/gradle-properties@v3
    id: properties
    with:
      output_file: ${{ github.output }}
      property: version

  - uses: softprops/action-gh-release@v2
    if: ${{ endsWith(steps.properties.outputs.version, '-SNAPSHOT') }}
    with:
      prerelease: true
```
