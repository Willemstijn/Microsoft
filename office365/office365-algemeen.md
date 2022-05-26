---
Order: 
Area: Office 365
TOCTitle: Office 365 generic management
ContentId: 
PageTitle: Office 365 Generic management with PowerShell
DateApproved: 
MetaDescription: This page contains information about Office 365 generic powershell management.
---
[Set company information](#company)  
[Managing subscriptions](#subscriptions)  
[Managing licenses](#licenses)  

<a name="company"/>

# Set company information
Get company information
```powershell
Get-MsolCompanyInformation
```
Set technical notifications email
```powershell
Set-MsolCompanyContactInformation -TechnicalNotificationEmails "youremail@here.com"
```
<a name="subscriptions"/>

# Managing subscriptions
Return list of available subscriptions
```powershell
Get-MsolAccountSku
```
Return list of available subscriptions with all information
```powershell
Get-MsolAccountSku | FL
```
To view details about the Office 365 services that are available in a specific licensing plan, use the following syntax. see: https://technet.microsoft.com/en-us/library/dn771773.aspx
```powershell
(Get-MsolAccountSku | where {$_.AccountSkuId -eq 'Adatumvs2615:ENTERPRISEPACK'}).ServiceStatus
```
<a name="licenses"/>

# Show Office 365 plans and services
Show all services in the aqcuired Office 365 service plan
```powershell
Get-MsolAccountSku | Select -ExpandProperty ServiceStatus
```
Shows the Office 365 services that are available in the litwareinc:ENTERPRISEPACK (Office 365 Enterprise E3) licensing plan.
```powershell
(Get-MsolAccountSku | where {$_.AccountSkuId -eq "litwareinc:ENTERPRISEPACK"}).ServiceStatus

# Output:
ServicePlan           ProvisioningStatus
-----------           ------------------
BPOS_S_TODO_2         Success
FORMS_PLAN_E3         Success
STREAM_O365_E3        Success
Deskless              Success
FLOW_O365_P2          Success
POWERAPPS_O365_P2     Success
TEAMS1                Success
PROJECTWORKMANAGEMENT Success
SWAY                  Success
INTUNE_O365           Success
YAMMER_ENTERPRISE     Success
RMS_S_ENTERPRISE      Success
OFFICESUBSCRIPTION    Success
MCOSTANDARD           Success
SHAREPOINTWAC         Success
SHAREPOINTENTERPRISE  Success
EXCHANGE_S_ENTERPRISE Success

(Get-MsolAccountSku | where {$_.AccountSkuId -eq "litwareinc:ENTERPRISEPREMIUM_NOPSTNCONF"}).ServiceStatus

# Output:
ServicePlan           ProvisioningStatus
-----------           ------------------
BPOS_S_TODO_3         Success
FORMS_PLAN_E5         Success
STREAM_O365_E5        Success
THREAT_INTELLIGENCE   Success
Deskless              Success
FLOW_O365_P3          Success
POWERAPPS_O365_P3     Success
TEAMS1                Success
ADALLOM_S_O365        Success
EQUIVIO_ANALYTICS     Success
LOCKBOX_ENTERPRISE    Success
EXCHANGE_ANALYTICS    Success
SWAY                  Success
ATP_ENTERPRISE        Success
MCOEV                 Success
BI_AZURE_P2           Success
INTUNE_O365           Success
PROJECTWORKMANAGEMENT Success
RMS_S_ENTERPRISE      Success
YAMMER_ENTERPRISE     Success
OFFICESUBSCRIPTION    Success
MCOSTANDARD           Success
EXCHANGE_S_ENTERPRISE Success
SHAREPOINTENTERPRISE  Success
SHAREPOINTWAC         Success


```
# Managing licenses
Show unlicensed users
```powershell
Get-MsolUser -UnlicensedUsersOnly
List Licensed users
```powershell
Get-MsolUser -All | where{$_.islicensed -eq $true}
```
List all users and licenses
```powershell
get-msoluser | ft displayname,licenses
```
Or
```powershell
Get-MsolUser -MaxResults 10 |Where-Object {$_.Islicensed -eq "True"} | Select-Object userPrincipalName, Firstname, lastname, Licenses
```
Get license types of a specific user:
```powershell
(get-msoluser -UserPrincipalName "Catherine@Adatumvs2615.virsoftlabs.com").licenses
```
Get servicestatus of specific user license
```powershell
(get-msoluser -UserPrincipalName "Catherine@Adatumvs2615.virsoftlabs.com").licenses[0].servicestatus
```
Add license to user
```powershell
Set-MsolUserLicense -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com –AddLicenses "Adatumvs2615:ENTERPRISEPACK"
```
Remove license from user:
```powershell
Set-MsolUserLicense -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com –RemoveLicenses "Adatumvs2615:ENTERPRISEPACK"
```
Switch license of a user
```powershell
Set-MsolUserLicense -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com –AddLicenses "Adatumvs2615:ENTERPRISEPREMIUM" –RemoveLicenses "Adatumvs2615:ENTERPRISEPACK"
```
# Billing

Get list of users that receive billing notifications
```powershell
$role = Get-MSOLRole -RoleName "Company Administrator"
Get-MsolRolemember -RoleObjectId $role.ObjectId
```

Add alternative adress to company administrator (in case the are locked out)
```powershell
Get-MsolUser -UserPrincipalname Catherine@Adatumvs2615.virsoftlabs.com | Set-MsolUser -AlternativeEmailadress Catherine@Outlook.com
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```
```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```

```powershell
```