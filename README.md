# PowerShell Memo

- [PowerShell Memo](#powershell-memo)
  - [Ressources](#ressources)
  - [Get PowerShell version](#get-powershell-version)
  - [Filter command output](#filter-command-output)
  - [Environment variables](#environment-variables)
  - [Alias](#alias)
    - [List alias](#list-alias)
    - [Get a single alias](#get-a-single-alias)
    - [Create aliases with parameters](#create-aliases-with-parameters)
  - [Get available File Systems](#get-available-file-systems)
  - [Modules](#modules)
  - [Help](#help)
  - [Pipe commands](#pipe-commands)

## Ressources

Microsoft 

https://docs.microsoft.com/en-us/powershell/scripting/powershell-scripting?view=powershell-5.1

PowerShell master class videos

https://www.youtube.com/playlist?list=PLlVtbbG169nFq_hR7FcMYg32xsSAObuq8

## Get PowerShell version

```PowerShell
C:\>$PSVersionTable
C:\>get-host
```

## Filter command output

For any get command you can provide a search filter, the search filter by default apply on the name attribute of the objects.

You can search on full name:

```PowerShell
C:\>get-service SysMain
```

Part of name

```PowerShell
C:\>get-service *tc*
```

If you want to search on another attribute

```PowerShell
C:\>get-service -DisplayName *TwinCat*
```

## Environment variables
List environment variables, by name or partial
```PowerShell
C:\>Get-Childitem Env:
C:\>Get-Childitem Env:TM* 
```
## Alias
### List alias
```PowerShell
C:\>Get-Alias
```
### Get a single alias
```PowerShell
C:\>Get-Alias cd
```
### Create aliases with parameters
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-5.1

```PowerShell
C:\>function listAllFiles {Get-ChildItem -Force}
C:\>Set-Alias lsa listAllFiles
```

## Get available File Systems
In PowerShell not only drive can be file system, but also the registry, AD server, environment variables..

Return all navigable “file system”, drive, registry…

```PowerShell
C:\>Get-PSdrive
```
Then you can navigate in a given drive, for example, to list all environment variables :

```PowerShell
C:\>cd Env:
C:\>Set-Location Env:
C:\>ls
```

or shorter

```PowerShell
C:\>ls Env:
```

To navigate the registry:

```PowerShell
C:\>cd HKCU:
C:\>Get-ChildItem
C:\>cd Software
C:\>Get-ChildItem
```

## Modules
A module contains a set of PowerShell functionalities such as cmdlets.

List modules

```PowerShell
C:\>Get-Module
C:\>Get-Module -listavailable -SkipEditionCheck
```

Import a module

```PowerShell
C:\>Import-Module <module_name>
```

List command in a module

```PowerShell
C:\>Get-Command -Module <module>
```

Install/Update
To instal or update a module run the following commands, `as administrator !`.

```PowerShell
C:\>Install-Module
C:\>Update-Module
```

## Help
To get help on a command use :

```PowerShell
C:\>get-Help <cmdlet> [-full, -detailed, -examples, -online, -showindow]
```

To get all help locally (examples) run command:

```PowerShell
C:\>update-help
```

To get all commands with a given noun or verb

```PowerShell
C:\>get-command -noun process
C:\>get-command -noun *net*
C:\>get-command -noun *adapter*
C:\>get-command -verb wait
```

## Pipe commands
Use `|` between commands, what’s passed between commands are list of object. Attributes of this objects can be used to refine the list returned.

```PowerShell
C:\>get-module | select-object -Unique Name | sort_object Name
```

Two commands can also be run together using semicolon ;

```PowerShell
C:\>get-process a*; get-service a*
```

Get object information (methods, properties, events, aliasProperties ...)

```PowerShell
C:\>get-process a* | get-member
C:\>get-process a* | gm
```

Call a method of a object, here how to kill a process:

```PowerShell
C:\>notepad
C:\>get-process | where-object {$_.name -eq “notepad”}
C:\>get-process | where-object {$_.name -eq “notepad”} | stop-process
C:\>notepad
C:\>(get-process | where-object {$_.name -eq “notepad”}).kill()
```

## Run PowerShell scripts

First make sur you can run scripts, by default you are not allowed, the simplest way is
```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
```
