# How to Validate a Preset File

DecentSampler includes a built-in **Validate Preset** tool in the Developer Tools menu. It reads the currently loaded `.dspreset` file directly from disk and reports any structural or file-reference problems—without modifying the file.

## Accessing the Tool

1. Open a `.dspreset` file in DecentSampler.
2. Click **FILE…** to open the file menu.
3. Navigate to **Developer Tools → Validate Preset**.

```{note}
**Validate Preset** is only available for non-copy-protected sample libraries. It will not appear in the menu (or will be disabled) when a commercially encrypted preset is loaded.
```

## What Gets Checked

### 1. Preset File Availability

The tool first confirms that the loaded `.dspreset` URL points to a real, readable file on disk. If no preset is loaded, or if the file has been moved or deleted since it was last opened, this check will fail with an error.

### 2. XML Validity

The entire file is parsed as XML. Any syntax error—such as an unclosed tag, illegal character, or malformed declaration—is reported with the parser's own error message. All subsequent checks are skipped if the XML is not well-formed.

### 3. File Path References

The following element/attribute pairs are scanned for file path references:

| Element       | Attribute  | Typical use                     |
|---------------|------------|---------------------------------|
| `<sample>`    | `path`     | Audio sample files              |
| `<effect>`    | `irFile`   | Convolution reverb IR files     |
| `<image>`     | `path`     | Custom image files              |
| `<ui>`        | `bgImage`  | Background image                |
| *(any)*       | `file`     | Any other file attribute        |

For each reference the validator:

- Resolves the path relative to the directory that contains the `.dspreset` file.
- Performs a **case-insensitive** directory and file name search, traversing each path component individually.
- Reports an **error** if the file cannot be found at all.
- Reports an **error** if the file exists on disk but with **different capitalization** than the path written in the preset. This is important because paths that work on case-insensitive file systems (macOS, Windows) will silently break on case-sensitive ones (most Linux distributions and some package-based deployments).

## Reading the Report

The report dialog shows:

- The full path to the preset file on disk.
- Whether the XML parsed successfully.
- A count of errors and warnings.
- A numbered list of every issue, prefixed with `[ERROR]` or `[WARNING]`, and the exact element, attribute name, and offending path string.

A clean preset produces the message:

```
✅ Validation passed. No issues were found.
```

## Common Errors and How to Fix Them

### Missing file

```
ERROR: Missing file reference: <sample> @path="Samples/Cello_A3.wav"
```

The file `Samples/Cello_A3.wav` could not be found relative to the preset directory. Check that the file exists and that the path in the `<sample>` tag is correct.

### Incorrect capitalization

```
ERROR: Incorrect path capitalization: <sample> @path="samples/cello_A3.wav" (disk path: "Samples/Cello_A3.wav")
```

The file exists, but the folder name is written as `samples` in the preset while the real folder on disk is `Samples`. Update the `path` attribute to match the exact capitalization on disk, otherwise the preset will fail to load on case-sensitive platforms.
