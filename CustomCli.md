# Custom CLI #

## Execute a tool from a NuGet package

```csharp
[PathExecutable] readonly Nuke.Common.Tooling.Tool Echo;

Target Foo => _ => _
  .Executes(() =>
  {
    Echo("arguments");
  });
```
