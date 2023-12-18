# Export Gradle Properties

Export Gradle properties as step outputs.

This action creates a [map of outputs](https://docs.github.com/en/actions/using-jobs/defining-outputs-for-jobs) via
[Gradle's property report task](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.PropertyReportTask.html).
These outputs can then be referenced by other steps or jobs.


## Usage

```yml
steps:
  - uses: actions/checkout@v4

  - uses: guillermocalvo/gradle-properties@v1
    id: properties

  - run: 'echo Project name: ${{ steps.properties.outputs.name }}'
```
