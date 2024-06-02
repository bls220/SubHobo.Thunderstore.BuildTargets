[![NuGet package](https://img.shields.io/nuget/v/SubHobo.Thunderstore.BuildTargets.svg)](https://nuget.org/packages/SubHobo.Thunderstore.BuildTargets)
[![NuGet downloads](https://img.shields.io/nuget/dt/SubHobo.Thunderstore.BuildTargets.svg)](https://nuget.org/packages/SubHobo.Thunderstore.BuildTargets)
# SubHobo.Thunderstore.BuildTargets
## Overview
This package is meant to make it simple to setup and package your content for use on [Thunderstore.io](https://Thunderstore.io).

This package will suppress dependencies from being output with your build. This is means any dependencies that you want to be included in your Thunderstore packge must be explicitly marked or copied. Common additional files, such as `CHANGELOG.md`, will automatically be included.

Upon building:
- A `manifest.json` will be generated using [information specified in your csproj file](#configure).
- An [error](#errors) will occur if any of `manifest.json`, `icon.png`, and `README.md` files don't exist in the build output.

## Install
Install the SubHobo.Thunderstore.BuildTargets package using the Visual Studio NuGet Package Manager GUI, or the NuGet Package Manager Console:
```
Install-Package SubHobo.Thunderstore.BuildTargets
```
## Configure

### Package Properties
Configuring of your packages metadata is as simple as setting a property in your csproj file. For example, to set a description of your package:
```xml
<PropertyGroup>
    <Description>My awesome mod that cures world hunger!</Description>
</PropertyGroup>
```
See the table below for a full list of properties:

| Property                   | Default                                             | Description |
| :---                       | :---                                                | :---        |
| Product                    |                                                     | Name of your mod.<br><br>_Note: This is for convenience in setting defaults for other properties_ |
| ThunderPackageId           | ```$(Product.Replace(' ','').Replace('.','_'))```   | Name of your mod.<br><br>This corresponds to `name` in the `manifest.json`. |
| Description                |                                                     | Short description of your mod. Shows on modlist in Thunderstore.<br><br>This corresponds to `description` in the `manifest.json` |
| Version                    |                                                     | Versio of your mod, following [SemVer2](https://semver.org/spec/v2.0.0.html) format.<br><br>This corresponds to `version_number` in the `manifest.json` |
| RepositoryUrl              |                                                     | URL of the mod's website (e.g. GitHub repo)<br><br>This corresponds to `website_url` in the `manifest.json` |
| Title                      | ```$(Product.Replace('_',' '))```                   | Name of your mod as it will be displayed on Thunderstore.<br><br>_Note: This is for convenience._ |
| ThunderPackageTeamName     | ```$(Company.Replace(' ','').Replace('.','_'))```   | Name of your 'team' on Thunderstore. |
| ThunderPackageName         | ```$(ThunderPackageTeamName)-$(ThunderPackageId)``` | Determines the file name for the generated package.  |
| ThunderPackageIcon         | ```$(ProjectDir)icon.png```                         | Location of the icon for your mod. |
| ThunderPackagingRoot       | ```$(SolutionDir)dist/```                           | Path where generated Thunderstore package should be stored. This is where the zipped package will be stored. |
| ThunderPackagingPath       | ```$(ThunderPackagingRoot)$(MSBuildProjectName)/``` | Path where package **contents** will be placed. This folder will be zipped to make the final package. |
| DeployToGamePath           | ```false```                                         | Should the package be deployed to your local game path? |
| ThunderPackageDeployFolder | ```BepInEx/plugins/$(ThunderPackageName)```         | Path relative to the `GamePath` where the package contents should be deployed. |
| GamePath                   |                                                     | Path to game folder. This is required for deploying locally.<br><br>Example: ```<GamePath>$([MSBuild]::GetRegistryValue(`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\Steam App 1063420`, `InstallLocation`))\</GamePath>``` |

### ThunderStore Dependencies
To declare Thunderstore dependencies, add an `ItemGroup` to your csproj and create a `ThunderDependency`. The `Include` must be the full package name and will be inserted as-is in the generated `manifest.json`.
```xml
<ItemGroup>
    <ThunderDependency Include="BepInEx-BepInExPack-5.4.2100"/>
</ItemGroup>
```

### Minimal Example
```xml
    <!--Thunderstore Info-->
    <ItemGroup>
        <ThunderDependency Include="BepInEx-BepInExPack-5.4.2100"/>
    </ItemGroup>
    <PropertyGroup>
        <Company>HobosInSpace</Company>
        <Product>Voider_Crew</Product>
        <Version>0.0.1</Version>
        <Description>This mod increases the player max to 8 per lobby.</Description>
        <RepositoryUrl>https://github.com/bls220/Void_Crew_Mods/</RepositoryUrl>
    </PropertyGroup>
```

## Errors
| Code   | Severity | Description |
| :---   | :---     | :--- |
| TSP001 | Error    | `ThunderPackageId` or `Product` are required to be set. |
| TSP002 | Error    | `ThunderPackageIcon` is set incorrectly |
| TSP003 | Warning  | `ThunderPackageTeamName` and/or `Company` are not set. |
| TSP005 | Error    | `README.md` is required, but does not exist |
| TSP006 | Error    | `icon.png` is required, but does not exist |
| TSP007 | Error    | `manifest.json` is required, but does not exist |
| TSP008 | Error    | `GamePath` is required to be set, if deploying to game folder. |


## License
This project is licened under [CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/?ref=chooser-v1)<img alt="CC" width="1.5rem" style="width:1.5rem!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img alt="BY" height="22px" style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><img alt="NC" height="22px" style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/nc.svg?ref=chooser-v1"><img alt="SA" height="22px" style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1">

## Attributions
For full list of attributions see [NOTICE.md](./NOTICE.md)