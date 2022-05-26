## PowerShell

Open een PowerShell terminal in Admin mode (pwsh) in Windows en Linux (als PowerShell is geinstalleerd) en voer de onderstaande commando's in

### General O365 Admin

```
$O365Credential = Get-Credential
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $O365Credential -Authentication Basic -AllowRedirection
Import-PSSession -Session $Session
```

### Connecting to Microsoft Teams

Install the Teams module from [PowershellGallery](https://www.powershellgallery.com/packages/MicrosoftTeams) first with:

```
Install-Module -Name MicrosoftTeams
```

To connect, use:

```
Connect-MicrosoftTeams
```

### Connecting to SharePoint Online

SharePoint online management shell installeren met:

```
Install-Module -Name Microsoft.Online.SharePoint.PowerShell
```

Controleren op versie

```
Get-Module -Name Microsoft.Online.SharePoint.PowerShell -ListAvailable | Select Name,Version
```

Inloggen zonder MFA

```
Install-Module -Name Microsoft.Online.SharePoint.PowerShell
$adminUPN="admin@mnconnect.onmicrosoft.com"
$orgName="mnconnect"
$userCredential = Get-Credential -UserName $adminUPN -Message "Type the password."
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential
```

of

```
Get-Module -Name Microsoft.Online.SharePoint.PowerShell -ListAvailable | Select Name,Version
Install-Module -Name Microsoft.Online.SharePoint.PowerShell
$SPOModulePath = 'C:\Program Files\SharePoint Online Management Shell\'
$Env:PSModulePath = '{0};{1}' -f $Env:PSModulePath, $SPOModulePath

# Connecting to SPO

$adminUPN="<the full email address of a SharePoint administrator account, example: username@usercompany.onmicrosoft.com>"
$orgName="<name of your Office 365 organization, example: usercompany>"
$userCredential = Get-Credential -UserName $adminUPN -Message "Type the password."
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential

# Connecting with MFA

$orgName="<name of your Office 365 organization, example: usercompany>"
Connect-SPOService -Url https://$orgName-admin.sharepoint.com
```

For example:

```
Get-Module -Name Microsoft.Online.SharePoint.PowerShell -ListAvailable | Select Name,Version
Install-Module -Name Microsoft.Online.SharePoint.PowerShell
$SPOModulePath = 'C:\Program Files\SharePoint Online Management Shell\'
$Env:PSModulePath = '{0};{1}' -f $Env:PSModulePath, $SPOModulePath

$adminUPN="admin@mnconnect.onmicrosoft.com"
$orgName="mnconnect"
$userCredential = Get-Credential -UserName $adminUPN -Message "Type the password."
Connect-SPOService -Url https://$orgName-admin.sharepoint.com -Credential $userCredential
```

## Connection to SharePoint PNP

Eerst module installeren met:

```
Install-Module SharePointPnPPowerShellOnline
```

Controleren van de moduleversie

```
Get-Module SharePointPnPPowerShell* -ListAvailable | Select-Object Name,Version | Sort-Object Version -Descending
```

### Verbinden met de Tenant

Zonder MFA:

```
Connect-PnPOnline –Url https://mnconnect.sharepoint.com –Credentials (Get-Credential)
```

Met MFA:

```
Connect-PnPOnline –Url https://yoursite.sharepoint.com –UseWebLogin
```

To view all cmdlets, enter:

```
Get-Command -Module SharePointPnPPowerShell*
```

## Security & Compliance center

```
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://ps.compliance.protection.outlook.com/powershell-liveid/ -Credential $O365Credential -Authentication Basic -AllowRedirection
Import-PSSession -Session $Session
```

## Connecting to Exchange Online

This script connects to Exchange online without MFA:

```
Set-executionpolicy -ExecutionPolicy Unrestricted
$cred = Get-Credential
$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $cred -Authentication Basic -AllowRedirection
Import-PSSession $session
```

To use MFA, use the following script:

```
Connect-EXOPSSession -UserPrincipalName alland@Tenantname.onmicrosoft.com -ConfigurationName Microsoft.Exchange -ConnectionUri https://ps.outlook.com/powershell -Authentication Basic -AllowRedirection   
```

## Connecting to Skype for Business

First download and install the Skype for Business Online module for Windows PowerShell at http://go.microsoft.com/fwlink/?LinkId=294688

```
Set-executionpolicy -ExecutionPolicy Unrestricted
Import-Module SkypeOnlineConnector
$cred = Get-Credential
$session = New-CsOnlineSession –Credential $cred
Import-PSSession $session
```

## Disconnecting from a session

When finishing work in a certain PowerShell session, you should disconnect from it by using:

```
Remove-PSSession $Session
```
