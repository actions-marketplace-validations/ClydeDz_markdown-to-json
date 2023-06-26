# Markdown to JSON

Convert markdown files from the provided directory into a single JSON file.

Uses the [markdown-json npm package](https://www.npmjs.com/package/markdown-json) to convert the markdown files from a directory into a single JSON file. This is especially useful in the case of a static API where you're updating content in markdown and then generating a JSON that serves as an API.

## Inputs

This action requires only one mandatory input as outlined below.

| **Parameter**      | **Description**                                                                   | **Is required?** |
| ------------------ | --------------------------------------------------------------------------------- | ---------------- |
| settings-json-file | Path to the settings json file that will be supplied to the Markdown to JSON tool | Yes              |


### What does the settings JSON file look like? 

This is a sample settings JSON file that you could use. Explanation provided below.
```json
{
  "name": "My API",
  "cwd": "./",
  "src": "example/content/",
  "filePattern": "**/*.md",
  "ignore": "*(icon|input)*",  
  "metadata": true,  
  "deterministicOrder": false,
  "dist": "example/output.json",
  "display": false,  
  "server": true,
  "port": 3215,
}
```

| **Key**            | **Description**                                                         |
| ------------------ | ----------------------------------------------------------------------- |
| name               | Optional friendly name for the settings JSON file                       |
| cwd                | Working directory (Default is ./)                                       |
| src                | File(s) directory (Default is ./)                                       |
| filePattern        | Inclusion file pattern (Default is **/*.md)                             |
| ignore             | Exclusion file pattern (Default is '')                                  |
| metadata           | Generates metadata in the JSON content for each item (Default is false) |
| deterministicOrder | Enables deterministic output ordering                                   |
| dist               | Output file directory (Default is ./dist/output.json)                   |
| display            | Display verbose logs (Default is true)                                  |
| server             | Serves the converted JSON files on a server (Default is false)          |
| port               | If server enabled, this server port is used (Default is 3001)           |

## Outputs

The JSON file will be sent to the directory provided in the `dist` key of the settings JSON file. Use the `actions/upload-artifact` to make the JSON file available as an artifact, or commit these files into the repository if that better suits your requirements.  

## Example usage

Let's say this is your project structure. You have some markdown files in a folder called markdown that you want to convert and combine into a single JSON file. You also have the settings JSON file in the setting folder which the GitHub action will use. 
```
your project
|
|── .github/workflows/
|  |── convert.yml
|── markdown
|  |── abc.md
|  |── def.md
|── settings
|  |── convert.json
```

This is how you'll consume this GitHub action. 
```yaml
- name: Convert markdown to json
  uses: ClydeDz/markdown-to-json@1.0.0
  with:
    settings-json-file: settings/convert.json
```

This is what your settings JSON file will look like assuming the project structure above.
```json
{
  "cwd": "./",
  "src": "markdown/",
  "filePattern": "**/*.md",
  "dist": "example/api.json",
}
```

