---
Description: This guide shows how to set up a job matrix: dynamically-created parallel jobs with different environment variables.
---

# Job Matrix

This guide shows how to set up a
[job matrix](https://docs.semaphoreci.com/reference/pipeline-yaml-reference/#matrix):
dynamically-created parallel jobs with different environment variables.

To configure a job matrix, you need to set the `matrix` property in
a job definition. `matrix` takes a list of `env_var` and its possible
`values`.

The job matrix will expand to the total possible combinations of variables
and create a job for each one.

## Testing against multiple language versions

In the following example, a job matrix is used to run different Java
versions:

``` yaml
blocks:
  - name: "Java versions"
    task:
      jobs:
      - name: Java versions
        matrix:
          - env_var: JAVA_VERSION
            values: [ "8", "11" ]
        commands:
          - sem-version java $JAVA_VERSION
          - java -version
```

[`sem-version`][sem-version]
switches the active Java version. `$JAVA_VERSION` takes the values 8 and 11:

``` bash
sem-version java 8
java -version
```

``` bash
sem-version java 11
java -version
```

## Multiple environment variables

The following job matrix has 2 variables, each with 3 possible values:

``` yaml
blocks:
  - name: "Job matrix"
    task:
      jobs:
      - name: Matrix
        matrix:
          - env_var: FOO
            values: [ "A", "B", "C" ]
          - env_var: BAR
            values: [ "1", "2", "3" ]
        commands:
          - echo FOO=$FOO BAR=$BAR
```

The matrix has `3 * 3 = 9` possible combinations. As a result, 9
parallel jobs are executed:

- `FOO=A BAR=1`
- `FOO=A BAR=2`
- `FOO=A BAR=3`
- `FOO=B BAR=1`
- `FOO=B BAR=2`
- `FOO=B BAR=3`
- `FOO=C BAR=1`
- `FOO=C BAR=2`
- `FOO=C BAR=3`

[sem-version]: https://docs.semaphoreci.com/ci-cd-environment/sem-version-managing-language-versions-on-linux/
