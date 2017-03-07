# FAKE.Dotnet

[![NuGet Status](http://img.shields.io/nuget/v/FAKE.dotnet.svg?style=flat)](https://www.nuget.org/packages/FAKE.Dotnet/)

"FAKE.Dotnet" is FAKE build automation system extension for [.NET Core CLI](http://github.com/dotnet/cli). It contains helpers to download and install specific versions .NET Core SDK with CLI. Currently supports only windows.

See the [API documentation](http://dolly22.github.io/FAKE.Dotnet/apidocs) for all available helpers and [Release Notes](https://github.com/dolly22/FAKE.Dotnet/blob/master/release_notes.md) for version details

## .NET Core CLI (SDK) helper usage

.NET Core SDK is downloaded and installed using [dotnet-install.ps1](https://github.com/dotnet/cli/blob/rel/1.0.0/scripts/obtain/dotnet-install.ps1) powershell script. By default SDK is installed to `%LOCALAPPDATA%\Microsoft\dotnet` folder (and version subfolders).

* DotnetDownloadInstaller - just download .NET Core SDK installer script
* DotnetCliInstall - install .NET Core SDK (when needed)
* Dotnet - generic dotnet CLI command helper
* DotnetRestore - dotnet restore packages helper
* DotnetPack - dotnet pack helper
* DotnetPublish - dotnet publish helper
* DotnetBuild - dotnet build helper

### example

There is sample project and build script for [1.0.1 SDK](https://github.com/dolly22/FAKE.Dotnet/blob/master/samples/NetCoreSdk101/scripts/build.fsx) tooling

```fsharp
#r "tools/FAKE.Dotnet/tools/Fake.Dotnet.dll" // include Fake.Dotnet lib
open Fake.Dotnet

Target "Initialize" (fun _ ->
	DotnetCliInstall id
)

Target "BuildProjects" (fun _ ->
	  !! "src/*/project.json"
	  |> Seq.iter(fun proj ->
		  DotnetRestore id proj
		  DotnetPack (fun c ->
			  { c with
				  Configuration = Debug;
				  OutputPath = Some (currentDirectory @@ "artifacts")
			  }) proj
	  )
)

"Initialize"            // define the dependencies
	  ==> "BuildProjects"

Run "BuildProjects"
```

# Build the project

* Windows: Run *build.cmd*
