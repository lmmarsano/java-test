# Test Project #

This gradle project [enables null analysis][settings] on the eclipse language server, which renders diagnostics on conformant [language server protocol][lsp] clients.

## Successful Render ##

To observe the diagnostics, clone the project

``` shellsession
$ git clone https://github.com/lmmarsano/java-test.git
$ cd java-test
```

and open in [Visual Studio Code][vscode] with [Language Support for Java][redhat] extension.

``` shellsession
$ code .
```

Notice the `App.java` *Problems*
- `Null type mismatch: required '@NonNull String' but the provided value is null`
- `Null type safety (type annotations): The expression of type 'String' needs unchecked conversion to conform to '@NonNull String'`

## Unsuccessful Render ##

[Emacs LSP Mode][emacs-lsp] doesn’t render the diagnostics.
However, setting `lsp-print-io` to non-`nil` reveals an appropriate notification in the protocol log buffer `*lsp-log: jdtls:`n`*`.
In the file [`src/main/java/test/package-info.java`][package-info], comment then uncomment the line

``` shellsession
@org.eclipse.jdt.annotation.NonNullByDefault
```

to get a similar notification in the log buffer `*lsp-log: jdtls:`n`*`.

```
[Trace - 03:35:12 PM]
Received notification 'textDocument/publishDiagnostics'.

Params: 
{
  "diagnostics": [
    {
      "message": "Null type safety (type annotations): The expression of type 'String' needs unchecked conversion to conform to '@NonNull String'",
      "source": "Java",
      "code": "536871867",
      "severity": 2,
      "range": {
        "end": {
          "character": 31,
          "line": 11
        },
        "start": {
          "character": 15,
          "line": 11
        }
      }
    },
    {
      "message": "Null type mismatch: required '@NonNull String' but the provided value is null",
      "source": "Java",
      "code": "16778126",
      "severity": 1,
      "range": {
        "end": {
          "character": 39,
          "line": 15
        },
        "start": {
          "character": 35,
          "line": 15
        }
      }
    }
  ],
  "uri": "file:///mnt/c/Users/luism/source/repos/java-demo/src/main/java/demo/App.java"
}
```

[settings]: .settings/org.eclipse.jdt.core.prefs
[lsp]: //microsoft.github.io/language-server-protocol/
[vscode]: //code.visualstudio.com/
[redhat]: //github.com/redhat-developer/vscode-java
[emacs-lsp]: //github.com/emacs-lsp/lsp-java
[package-info]: src/main/java/test/package-info.java
