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

  # Setup NetBeans distribution directory on github workspace.
  - uses: cjmach/setup-netbeans-action@v2.1
    name: Setup Autopsy distribution
    with:
      version: '15'

  # Setup autopsy distribution directory on github workspace.
  - uses: cjmach/setup-autopsy-action@v1
    name: Setup Autopsy distribution
    with:
      version: '4.21.0'

  # Run the ant command, with the required Autopsy properties set.
  - name: Run the Ant build target
    run: >-
      ant -noinput -buildfile build.xml
      -Dnbplatform.Autopsy_4.21.0.netbeans.dest.dir=${{ github.workspace }}/autopsy
      -Dnbplatform.Autopsy_4.21.0.harness.dir=${{ github.workspace }}/autopsy/harness
      build
```

The term "Autopsy_4.21.0" in the Autopsy properties on the ant command 
line represents the name of the NetBeans platform and must match the value 
of the property `nbplatform.active` set in the file `nbproject/platform.properties`
of your module (suite).
