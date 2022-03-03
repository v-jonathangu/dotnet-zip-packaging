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
- Create a Folder publish profile like the following
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
`dotnet publish -p:PublishProfile=FolderProfile`
