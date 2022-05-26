You can load the following functions in personal PowerShell profile to connect quickly to Office 365 and use 'popular' powershell scripts.  
You can read here how to load these functions in your PowerShell profile.

```powersell
# Connect to Office 365
function Connect-Office365 {
Import-Module MSOnline
$cred = Get-Credential
Connect-MsolService -Credential $cred
}

# Connect to Sharepoint Online
function Connect-SharePointOnline {
Param(
   [Parameter()]
   [ValidateNotNullOrEmpty()]
   [string]$tenant=$(throw "Tenant is mandatory, please provide a value.")
   ) #end param
Set-executionpolicy -ExecutionPolicy Unrestricted
Import-Module Microsoft.Online.SharePoint.PowerShell
$cred = Get-Credential
Connect-SPOService -Url https://$tenant-admin.sharepoint.com -Credential $cred
}

# Connect to Exchange Online
function Connect-ExchangeOnline {
Set-executionpolicy -ExecutionPolicy Unrestricted
$cred = Get-Credential
$session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $cred -Authentication Basic -AllowRedirection
Import-PSSession $session
}

# Connect to Skype for Business Online
function Connect-SkypeOnline {
Set-executionpolicy -ExecutionPolicy Unrestricted
Import-Module SkypeOnlineConnector
$cred = Get-Credential
$session = New-CsOnlineSession -Credential $cred
Import-PSSession $session
}

# Get an overview of all configured Office 365 admins
function Get-MsolAdmins {
Get-MsolRole  | Sort-Object -Property name |
# Where-Object {$_.Name -ne 'Directory Writers' -and $_.Name -ne 'Directory Readers'} | 
    ForEach-Object {
        Write-Output $_.Name
        Get-MsolRoleMember -RoleObjectId $_.ObjectId |
        Get-MSOLUser | Select DisplayName,UserPrincipalName,BlockCredential,IsLicensed,LastPasswordChangeTimestamp | 
        Format-Table 
        }
    }
```
