# dotCover #

Using dotCover with Visual Studio Tests, you should build the test dll first
```csharp
VSTestSettings testSettings = new VSTestSettings()
    .SetTestAssemblies(GlobFiles(SourceDirectory, "**/bin/debug/Tests.dll"))
    .SetLogger($"Trx;LogFileName={RootDirectory / "Tests.trx"}");

DotCoverTasks.DotCoverCover(s => s
    .SetTargetSettings(testSettings)
    .SetOutputFile(ArtifactsDirectory / "Coverage.dcvr")
);
```

https://www.jetbrains.com/help/dotcover/Running_Coverage_Analysis_from_the_Command_LIne.html
