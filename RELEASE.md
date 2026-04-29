# Release Instructions

## Prerequisites

- .NET 9.0 SDK installed
- Visual Studio or `dotnet` CLI available

## Steps

### 1. Bump the version

Update `<Version>` in `DWHelperUI/DWHelperUI.csproj`:

```xml
<Version>X.Y.Z</Version>
```

### 2. Update NuGet packages

Check for outdated packages:

```powershell
dotnet list DualWriteHelper.sln package --outdated
```

Update versions in the `.csproj` files:
- `DualWriteHelper/DWHelperCMD.csproj`
- `DWLibary/DWLibary.csproj`
- `DWHelperUI/DWHelperUI.csproj`

> **Note:** Selenium.WebDriver updates often change the DevTools version namespace. If upgraded, update the `V###` references in `DWLibary/EdgeUniversal.cs` (lines 9-10) to match the new version bundled in the package. Inspect available versions with:
> ```powershell
> [System.Reflection.Assembly]::LoadFrom("$env:USERPROFILE\.nuget\packages\selenium.webdriver\<VERSION>\lib\net8.0\WebDriver.dll").GetTypes() |
>   Where-Object { $_.Namespace -like "OpenQA.Selenium.DevTools.V*" } |
>   Select-Object -ExpandProperty Namespace -Unique | Sort-Object
> ```

### 3. Restore and build

```powershell
dotnet restore DualWriteHelper.sln
dotnet build DualWriteHelper.sln -c Release
```

Verify **0 errors** before proceeding (warnings are acceptable).

### 4. Create the release zip

```powershell
$src = "DWHelperUI\bin\Release\net9.0-windows7.0"
$zip = "DWHelper.zip"
if (Test-Path $zip) { Remove-Item $zip }
Compress-Archive -Path "$src\*" -DestinationPath $zip
```

The zip is excluded from git via `.gitignore`.

### 5. Distribute

Upload `DWHelper.zip` to the release target (GitHub Releases, SharePoint, etc.).

## Release History

| Version | Date       | Notes |
|---------|------------|-------|
| 1.0.14  | 2026-04-15 | NuGet package updates, Selenium DevTools V143→V145, map config ordering fix |
| 1.0.13  | —          | Previous release |
