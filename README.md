# Test Collector Buildkite Plugin (WIP)

A Buildkite plugin for uploading [JSON](https://buildkite.com/docs/test-analytics/importing-json) or [JUnit](https://buildkite.com/docs/test-analytics/importing-junit-xml) files to [Buildkite Test Analytics](https://buildkite.com/test-analytics) ✨

## 👉 Usage

### Upload a JSON file

To upload a JSON file to Test Analytics from a build step:

```yaml
steps:
  - label: "🔨 Test"
    command: "make test"
    plugins:
      - buildkite/test-collector#v0.0.1:
          files: "test-data*.json"
```

### Upload a JUnit file

```yaml
steps:
  - label: "🔨 Test"
    command: "make test"
    plugins:
      - buildkite/test-collector#v0.0.1:
          files: "test/junit-*.xml"
```

### Upload a build artifact

You can use the `artifact` property to upload artifacts that have been uploaded in previous steps:

```yaml
steps:
  - label: "🔨 Test"
    command: "make test"
    artifact_paths: "test-data*.json"

  - wait

  - label: "🔍 Upload tests"
    plugins:
      - buildkite/test-collector#main:
          artifacts: "test-data*.json"
```

### Custom file name formats

The file format is inferred from the file name: `.xml` is assumed JUnit, `.json` is assumed JSON. If you use a different file name pattern, you can specify the `format` property:

```yaml
steps:
  - label: "🔨 Test"
    command: "make test"
    plugins:
      - buildkite/test-collector#v0.0.1:
          files: "test-data"
          format: "json"
```

## Properties

* `files` — Optional — A file path pattern of files to upload to Test Analytics
  * Example: "test-data*.json"
* `artifacts` — Optional — An artifact file path pattern of files to download as artifacts and upload to Test Analytics
  * Example: "test-data*.json"
* `format`
    * Default: inferred from the filename (`.json` is assumed JSON, `.xml` is assumed JUnit)
    * Values: `"junit"`, `"json"`
* `api-token-env-name`— Optional — The name of the environment variable that contains the Buildkite Test Analytics API Token.
  * Default: `"BUILDKITE_ANALYTICS_API_TOKEN"`

## ⚒ Developing

You can use the [bk cli](https://github.com/buildkite/cli) to run the test pipeline locally, or just the tests using Docker Compose directly:

```bash
docker-compose run --rm tests
```

## 👩‍💻 Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/buildkite-plugins/test-collector-buildkite-plugin

## 📜 License

The package is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
