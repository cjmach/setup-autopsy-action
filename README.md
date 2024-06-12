# setup-autopsy-action
A custom Github action that downloads, extracts and caches a Sleuthkit Autopsy ZIP distribution for building Ant-based Autopsy modules.

## Inputs

### `version`

**Required** The version of the Autopsy distribution to download (minimum version is '4.20.0').

## Outputs

No outputs.

## Example usage

```yaml

steps:
  - uses: actions/checkout@v4
    name: Checkout project

  - uses: actions/setup-java@v4
    name: Set up JDK 11
    with:
      java-version: '11'
      distribution: 'temurin'

  # Setup autopsy distribution directory on github workspace.
  - uses: cjmach/setup-autopsy-action@v1
    name: Setup Autopsy distribution
    with:
      version: '4.20.0'

  # Run the ant command, with the required Autopsy properties set.
  - name: Run the Ant build target
    run: >-
      ant -noinput -buildfile build.xml
      -Dnbplatform.default.netbeans.dest.dir=${{ github.workspace }}/autopsy
      -Dnbplatform.default.harness.dir=${{ github.workspace }}/autopsy/harness
      build
```
