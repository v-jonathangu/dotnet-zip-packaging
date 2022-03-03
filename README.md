# Dotnet Zip packaging example

Dotnet zip packaging example

## Guide

### project setup

- create or add the solution via visual studio or dotnet command line  
`dotnet new sln`
- create or add the project via visual studio or dotnet command line  
`mkdir webapp`  
`cd webapp`  
`dotnet new webapp`
- Create a Folder publish profile like the following (needed only for MSBuild)
```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
https://go.microsoft.com/fwlink/?LinkID=208121.
-->
<Project>
  <PropertyGroup>
    <DeleteExistingFiles>False</DeleteExistingFiles>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <PublishProvider>FileSystem</PublishProvider>
    <PublishUrl>../release-web</PublishUrl>
    <WebPublishMethod>FileSystem</WebPublishMethod>
  </PropertyGroup>
</Project>
```
  - save the publishing profile on a preferred location, by default you can save it on the `project/Properties/PublishProfiles/FolderProfile.pubxml`
  - test that the publishing profile works
    > via msbuild restore the packages first  
`MSBuild.exe .\aspdotnet-zip.sln /p:Configuration=Release /p:Platform="Any CPU" -t:restore -p:RestorePackagesConfig=true`  
then build and pusblish the profile like this
`MSBuild.exe .\aspdotnet-zip.sln /p:Configuration=Release /p:Platform="Any CPU" /property:DeployOnBuild=True /p:PublishProfile=FolderProfile`

> with dotnet cli tool you can do it like this:
`dotnet publish -c Release -o release-web`

> please keep in mind that the directory output and contents might change depending on the SDK and MSBuild tools installed, for example MSBuild might output on a different directory than the dotnet sdk, and the sdk might change the files generated depending of the SDK for example SDK 4 is different than SDK 6. 

> Also it is not recommended do any extra manual packaging steps via MSBuild, if you need for example zip the project or create an executable with redistributables you should use another tool like an powershell script or a pipeline job/task in your environment, for example in powershell you can zip a directory like this
```powershell
$compress = @{
  # the directory where you release files are
  Path = "release-web/*"
  CompressionLevel = "Fastest"
  # the file output directory 
  DestinationPath = "release.zip"
}
Compress-Archive @compress -Force
```
