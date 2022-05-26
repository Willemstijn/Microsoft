# SharePoint Online OnePager

## Site collection management

### GET commands

Show list of all SPO Sites

```powershell
Get-SPOSite
```

Show list of all sites and webs (no MySites / ODfB)

```powershell
Get-SPOSite | Get-SPOWeb
```

Show all site methods and properties

```powershell
Get-SPOSite | gm
```

Show sites and misc. properties (see get-member for properties)

```powershell
Get-SPOSite | ft title, url, owner, template
# or
Get-SPOSite | ft title,url,owner,StorageQuota,lockstate,template
```

Retrieve basic information about each site collection except MySites

```powershell
Get-SPOSite -Detailed | Export-Csv -LiteralPath c:/temp/SPOSites.csv -NoTypeInformation
```

Show the types of templates that you can use for making specific sites

```powershell
Get-SPOWebTemplate
```

Retrieve detailed information about each site collection except MySites (*favorite*)

```powershell
Get-SPOSite -Detailed | Export-Csv -LiteralPath c:/temp/SPOSites.csv -NoTypeInformation
```

### New commands

Make new SPO Site with project template and storagequota (no O365 group!):

```powershell
New-SPOSite -Url https://adatumvs2615.sharepoint.com/sites/AcctsProj -Owner Holly@Adatumvs2615.virsoftlabs.com -StorageQuota 500 -NoWait -Template PROJECTSITE#0 –Title "Accounts Project"
```

Make new SPO site with default teamsite template and standard quota:

```powershell
New-SPOSite -Url https://adatumvs2615.sharepoint.com/sites/AcctsProj -Owner Holly@Adatumvs2615.virsoftlabs.com -StorageQuota 26214400 -NoWait -Template STS#0 –Title "VB Reporting"
```

Make a site with dutch language, timezone and teamsite template (*favorite*)

```powershell
# Standard Teamsite
New-SPOSite -Url https://tenant.sharepoint.com/sites/sitename -Owner Holly@Adatumvs2615.virsoftlabs.com -StorageQuota 26214400 -NoWait -Template "STS#0" –Title "sitename" -LocaleId 1043 -TimeZoneId 4
# PublishingPortal (currently not available in PowerShell)
New-SPOSite -Url https://tenant.sharepoint.com/sites/sitename -Owner Holly@Adatumvs2615.virsoftlabs.com -StorageQuota 26214400 -NoWait -Template "SITEPAGEPUBLISHING#0" –Title "sitename" -LocaleId 1043 -TimeZoneId 4
```

### Edit commands

Change Site owner

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -Owner "Holly@Adatumvs2615.virsoftlabs.com"
```

Configure storage and usage quota

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -StorageQuota 1000 -ResourceQuota 500
```

Change site title

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -Title "New Site title"
```

Lock a site

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -LockState NoAccess
```

Other lockstates are "Unlock" or "NoAccess".`

Redirect locked site visitors to a tenant wide redirect url

```powershell
Set-SPOTenant -NoAccessRedirectURL https://contoso.sharepoint.com/Pages/Locked.aspx

Set-SPOTenant -NoAccessRedirectUrl
Write-Host (Get-SPOTenant).NoaccessRedirectUrl -ForegroundColor Green
```

Configure External Sharing Capabilities for a site

No sharing

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -SharingCapability Disabled
```

External and (anonymous) guests

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -SharingCapability ExternalUserAndGuestSharing
```

External only

```powershell
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Set-SPOSite -Identity $url -SharingCapability ExternalUserSharingOnly
# or
Set-SPOSite -Identity https://tenant.sharepoint.com/sites/groupsite -SharingCapability ExternalUserSharingOnly
```

Known external users only

```powershell
Set-SPOSite -Identity https://tenant.sharepoint.com/sites/groupsite -SharingCapability ExistingExternalUserSharingOnly
```

### Remove commands

Delete a SharePoint Online site (sites will wind up in a recycle bin)

```powershell
Remove-SPOSite -Identity https://adatumvs2615.sharepoint.com/sites/adatummarketing -NoWait -Confirm
```

Show deleted sites in recycle bin (sites will be permanently deleted after 30 days)

```powershell
Get-SPODeletedSite
```

Restore a deleted site (within 30 days of deletion)

```powershell
Restore-SPODeletedSite -Identity https://adatumvs2615.sharepoint.com/sites/adatummarketing -NoWait
```

Delete sites in recyclebin permanently

```powershell
Remove-SPODeletedSite -Identity https://adatumvs2615.sharepoint.com/sites/adatummarketing -NoWait -Confirm
```

Remove site permanent workflow

```powershell
$sitename = "https://tenant.sharepoint.com/sites/sitename"
get-sposite $sitename
Remove-SPOSite -Identity $sitename -NoWait
Remove-SPODeletedSite -Identity $sitename
```

## SharePoint Online Tenant management

Get all Tenant wide settings

```powershell
Get-SPOTenant
```

Get specific value from the atributes list. Eg. expiry for anonymous links

```powershell
$Tenant = Get-SPOTenant
$Tenant.RequireAnonymousLinksExpireInDays
```

Set expiry for anonymous links to 30 days

```powershell
Set-SPOTenant -RequireAnonymousLinksExpireInDays "30"
```

Allow Tenant wide sharing to external users and guests

```powershell
Set-SPOTenant -SharingCapability ExternalUserAndGuestSharing
```

Redirect locked site visitors to a tenant wide redirect url

```powershell
Set-SPOTenant -NoAccessRedirectUrl
Write-Host (Get-SPOTenant).NoaccessRedirectUrl -ForegroundColor Green
```

## SharePoint User management

Note: Preferred method of user management is via the Office 365 Groups. 

Get a list of all users of a site

```powershell
Get-SPOUser -Site https://adatumvs2615.sharepoint.com/sites/adatummarketing
# or
$url = "https://adatumvs2615.sharepoint.com/sites/adatummarketing"
Get-SPOUser -Limit All -Site $url | Select DisplayName, LoginName, Groups
```

## SharePoint Online Management Shell Options

Count available cmdlets

```powershell
Import-Module Microsoft.Online.SharePoint.PowerShell -DisableNameChecking
$SPOL = Get-Module -Name Microsoft.Online.SharePoint.PowerShell
Write-Host "SharePoint Online has" $SPOL.ExportedCmdlets.Count "cmdlets" -ForegroundColor Green

SharePoint Online has 75 cmdlets
```

Get a list of all cmdlets

```powershell
$SPOL.ExportedCommands
```

* [SharePoint Online Module Browser](https://docs.microsoft.com/nl-nl/powershell/module/sharepoint-online/?view=sharepoint-ps)
* [PowerShell module browser](https://docs.microsoft.com/en-us/powershell/module/)

## SharePoint Online JSON Code

Om de opmaak van een specifieke kolom te veranderen (@currentField), Open [opmaak kolom](images/2019-11-18-11_53_06.png) (Kolom pulldown > Kolom instellingen > Deze kolom opmaken). Zet code daarma in Json veld.

Change column width

```
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/column-formatting.schema.json",
  "elmType": "div",
  "txtContent": "@currentField",
  "style": {
    "color": "green",
    "width": "300px"
  }
}
```

Kleurcode geven aan een cel met bepaalde waarde

```
{
  "elmType": "div",
  "style": {
    "padding": "0 4px"
  },
  "attributes": {
    "class": {
      "operator": ":",
      "operands": [
        {
          "operator": "==",
          "operands": [
            {
              "operator": "toLowerCase",
              "operands": [
                "@currentField"
              ]
            },
            {
              "operator": "toLowerCase",
              "operands": [
                  <!-- Waarde voor kleur -->
                "Online"
              ]
            }
          ]
        },
        <!-- Microsoft kleurcode voor "succes" criterium = groen-->
        "sp-css-backgroundColor-successBackground",
        {
          "operator": ":",
          "operands": [
            {
              "operator": "==",
              "operands": [
                {
                  "operator": "toLowerCase",
                  "operands": [
                    "@currentField"
                  ]
                },
                {
                  "operator": "toLowerCase",
                  "operands": [
                      <!-- Waarde voor kleur -->
                    "On-Premises"
                  ]
                }
              ]
            },
            <!-- Microsoft kleurcode voor "Warning" criterium = geel-->
            "sp-css-backgroundColor-warningBackground",
            {
              "operator": ":",
              "operands": [
                {
                  "operator": "==",
                  "operands": [
                      <!-- Waarde voor kleur -->
                    {
                      "operator": "toLowerCase",
                      "operands": [
                        "@currentField"
                      ]
                    },
                    {
                      "operator": "toLowerCase",
                      "operands": [
                        ""
                      ]
                    }
                  ]
                },
                <!-- Microsoft kleurcode voor "Blocking" criterium = rood-->
                "sp-css-backgroundColor-blockingBackground",
                ""
              ]
            }
          ]
        }
      ]
    }
  },
  "txtContent": "@currentField"
}
```

Geef cel een gele kleur als de waarde van $LaatsteControle meer of gelijk is dan vandaag - 1 jaar (in seconden).  
Zie [voorbeeld](images/2019-09-20-13_56_42-Instellingen.png).

```
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "div",
  "debugMode": true,
  "txtContent": "@currentField",
  "style": {
    "background-color": "=if([$LaatsteControle] <= (@now -15552000000), '#FFFF66', '')"
  }
}
```

List items in een 'Cards' look formatteren. Zie [voorbeeld](images/2019-09-24-10_54_56.png)

```
{
  "schema": "https://developer.microsoft.com/json-schemas/sp/view-formatting.schema.json",
  "hideSelection": true,
  "hideColumnHeader": true,
  "rowFormatter": {
    "elmType": "div",
    "attributes": {
      "class": "sp-row-card"
    },
    "children": [
      {
        "elmType": "div",
        "style": {
          "text-align": "left"
        },
        "children": [
          {
            "elmType": "div",
            "attributes": {
              "class": "sp-row-title"
            },
            "txtContent": "[$Title]"
          },
          {
            "elmType": "div",
            "attributes": {
              "class": "sp-row-listPadding"
            },
            "txtContent": "[$Omschrijving]"
          }
        ]
      }
    ]
  }
}
```
