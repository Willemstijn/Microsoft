## Site templates downloaden, aanpassen en uploaden

### Bronnen

- https://pnp.github.io/
- https://github.com/SharePoint/PnP-Provisioning-Templates
- https://github.com/pnp/PnP-PowerShell
- [PNP PowerShell overview](https://docs.microsoft.com/en-us/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets?view=sharepoint-ps)
- [SharePoint PnP PowerShell](https://docs.microsoft.com/en-us/powershell/module/sharepoint-pnp/?view=sharepoint-ps)
- [SharePoint PNP Schema](https://docs.microsoft.com/en-us/sharepoint/dev/solution-guidance/pnp-provisioning-schema)

## Install PNP module in Windows 10

SharePoint Online: ``Install-Module SharePointPnPPowerShellOnline``

Or in Powershell v3 and higher: ``Invoke-Expression (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/sharepoint/PnP-PowerShell/master/Samples/Modules.Install/Install-SharePointPnPPowerShell.ps1')``

Each month there are updates: ``Update-Module SharePointPnPPowerShell*``

Check the installed PnP-PowerShell versions with: ``Get-Module SharePointPnPPowerShell* -ListAvailable | Select-Object Name,Version | Sort-Object Version -Descending``

## Connect to site in tenant

No MFA: ``Connect-PnPOnline –Url https://yoursite.sharepoint.com –Credentials (Get-Credential)``

With MFA: ``Connect-PnPOnline -Url https://yoursite.sharepoint.com -UseWebLogin``

Show all commands: ``Get-Command -Module *PnP*``

## Copy a site template and paste it onto a new site

Step 1: Get the Source Site Schema XML

```
#Set variables
$SiteURL = "https://crescent.sharepoint.com/sites/projects/london"
$SchmaXMLPath = "C:\Temp\SiteSchema.xml"
 
#Connect to PnP Online
Connect-PnPOnline -Url $siteUrl -UseWebLogin
 
#Get Site Schema
Get-PnPProvisioningTemplate -Out ($SchmaXMLPath) -PersistBrandingFiles -PersistPublishingFiles
```

Step 2: Create a New Subsite

```	
#Set variables
$SiteURL = "https://crescent.sharepoint.com/sites/projects"
 
#Connect to PnP Online
Connect-PnPOnline -Url $SiteURL -UseWebLogin
 
#Create a new subsite
$NewWeb = New-PnpWeb -Title "Project Morocco" -Url "morocco" -Description "Team Site for Morocco Project" -Locale 1033 -Template "STS#3"
```

Step 3: Apply the Site Schema into Target Site with Apply-PnP ProvisioningTemplate

```
#Set variables
$SiteURL = "https://crescent.sharepoint.com/sites/projects/morocco"
$SchmaXMLPath = "C:\Temp\SiteSchema.xml"
 
#Connect to PnP Online
Connect-PnPOnline -Url $SiteURL -UseWebLogin
 
#Apply Pnp Provisioning Template
Apply-PnPProvisioningTemplate -Path $SchmaXMLPath -ClearNavigation
```

## Een of ander gevonden scriptje:

```
$askCred = Get-Credential
$templateFile = "E:\Path\template.xml";
$siteTitle = "New Publishing Site";
$siteDescription = "Publishing site created with PnP Provisioning";
$siteUrl = "PnPSite";
 
# Office 365 Login 
$webUrl = "https://{0}.sharepoint.com{1}/" -f $tenant, $targetSite;
Write-Output $("Connecting to {0}" -f $webUrl);
Connect-PnPOnline -Url $webUrl -Credentials $askCred;
      
# Get site context
Write-Output $("Getting site context {0}" -f $siteTitle);
$newSite = (Get-PnPWeb).ServerRelativeUrl + "/" + $siteUrl
 
# If the site does not exist creates it
if((Get-PnPSubWebs).ServerRelativeUrl -contains $newSite){
    #Get site context
    $web = Get-PnPWeb -Identity $newSite
} else {
    #Creates the publishing site and gets context
    $web = New-PnpWeb -Title "$siteTitle" -Url "$siteUrl" -Description "$siteDescription" -Locale 1033 -Template "BLANKINTERNET#0"
}
 
# Apply template
Write-Output $("Applying PnP template [{0}] to site [{1} ({2})]" -f $templateFile, $web.Title, $web.Url);
Set-PnPTraceLog -On -LogFile "E:\Path\log.txt" -Level Debug
Apply-PnPProvisioningTemplate -Web $web -Path $templateFile
```