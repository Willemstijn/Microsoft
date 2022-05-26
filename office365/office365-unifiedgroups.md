---
Order: 
Area: Office 365
TOCTitle: Office 365 Unified groups management
ContentId: 
PageTitle: Office 365 Unified groups Management with PowerShell
DateApproved: 
MetaDescription: Page containing information about unified groups management in office 365 with powershell.
---
[Creating Unified Groups](#create)  
[Edit Unified Groups](#edit)  
[Delete Unified Groups](#delete)  
[Undelete Unified Groups](#undelete)  
[More information](#more)  

<a name="create"/>

# Create Office 365 Unified groups
Allowing the creation of Unified Groups in Office 365
```powershell
Set-OwaMailboxPolicy -Identity contoso.comOwaMailboxPolicy-Default -GroupCreationEnabled $true
```

Disabling the creation of Unified Groups in Office 365
```powershell
Set-OwaMailboxPolicy -Identity test.comOwaMailboxPolicy-Default -GroupCreationEnabled $false
```

# Upgrading a standard distribution list to a Unified Group
First of all you'll have to check which distribution lists are eligible for an upgrade.
```powershell
Get-EligibleDistributionGroup
```

Or check a specific list
```powershell
Get-DistributionGroup <DL SMTP address> | Get-EligibleDistributionGroup
```

Upgrading a single distribution group:
```powershell
Upgrade-DistributionGroup –DlIdentities <Dl SMTP address>

Upgrade-DistributionGroup –DlIdentities dl1@contoso.com
```

You can also upgrade multiple distribution groups with the following command:
```powershell
Upgrade-DistributionGroup –DlIdentities <DL SMTP address1>, < DL SMTP address2>, etcetera...

Upgrade-DistributionGroup –DlIdentities dl1@contoso.com, dl2@contoso.com, dl3@contoso.com, dl4@contoso.com, dl5@contoso.com
```

[more information about upgrading distribution groups here](https://support.office.com/en-us/article/upgrade-distribution-lists-to-office-365-groups-in-outlook-787d7a75-e201-46f3-a242-f698162ff09f?ui=en-US&rs=en-US&ad=US)

# Limiting users that can make unified groups
In normal circumstances, all users can make unified groups. Unified groups can be created with the creation of the following services:
* Outlook
* SharePoint
* Yammer
* Microsoft Teams: Both admins and users won't be able to create teams.
* StaffHub: Both admins and managers won't be able to create teams.
* Planner: Users won't be able to create a new plan in Planner web and mobile apps.
* PowerBI

The best way to do this is to create a security group and add those people to the security group that are allowed to create unified groups.
Also, the administrator who configures the settings, and the members of the affected groups, must have Azure AD Premium licenses assigned to them.

[More information about this subject](https://support.office.com/en-us/article/manage-who-can-create-office-365-groups-4c46c8cb-17d0-44b5-9776-005fced8e618)

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


## Change the default setting of Office 365 Groups for Outlook to Public or Private
Office 365 Groups in Outlook are created as Private by default. If your organization wants Office 365 Groups for Outlook to be created as Public by default (or back to Private)

```powershell
Set-OrganizationConfig -DefaultGroupAccessType Public

# To set to Private:

Set-OrganizationConfig -DefaultGroupAccessType Private

# To verify the setting:

Get-OrganizationConfig | ft DefaultGroupAccessType
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


<a name="edit"/>

# Edit Office 365 Unified groups

## Hide Office 365 Groups from GAL
You can specify whether a Office 365 group appears in the global  address list (GAL) and other lists in your organization.
```powershell
Set-UnifiedGroup -Identity "Legal Department" -HiddenFromAddressListsEnabled $true
```
## Allow only internal users to send message to Office 365 group
If you don't want users from other organization to send email to a Office 365 group, you can change the settings for that group.
```powershell
Set-UnifiedGroup -Identity "Internal senders only" - RequireSenderAuthenticationEnabled $true
```

## Add MailTips to the Office 365 Groups
Whenever a sender tries to send an email to an Office 365 group, a MailTip can be shown to him.

```powershell
Set-UnifiedGroup -Identity "MailTip Group" -MailTip “This group has a MailTip”
```

Along with MailTip, you can also set MailTipTranslations, which specifies additional languages fro the MailTip.

```powershell
Set-UnifiedGroup -Identity "MailTip Group" -MailTip "This group has a MailTip" -MailTipTranslations "@{Add="ES:Esta caja no se supervisa."
```

## Change Display name of the Office 365 group
Display name specifies the name of the Office 365 group. You can see this name in your exchange admin center or o365 admin portal.

```powershell
Set-UnifiedGroup -Identity "mygroup@contoso.com" -DisplayName “My new group”
```

```powershell
```

```powershell
```

```powershell
```


<a name="delete"/>

# Delete Office 365 Unified groups

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

<a name="undelete"/>

# Undelete Office 365 Unified groups
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



Add a new user to Office 365
```powershell
New-MsolUser -UserPrincipalName csg@ur-tenant.onmicrosoft.com -DisplayName "CSG" -FirstName "Firstname" -LastName "Lastname"
New-MsolUser "UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com "DisplayName "Catherine Richard" "FirstName "Catherine" "LastName "Richard" "Password 'Pa55w.rd' "ForceChangePassword $false "UsageLocation "CH"
```
Add the user to a role
```powershell
Add-MsolRoleMember -RoleName "Company Administrator" "RoleMemberEmailAddress  csg@ur-tenant.onmicrosoft.com
```
Set password to user
```powershell
Set-MsolUserPassword -userPrincipalName csg@ur-tenant.onmicrosoft.com -NewPassword "Abcd1234" -ForceChangePassword $false
```
Give user an alternative email address
```powershell
Get-MsolUser -UserPrincipalName csg@ur-tenant.onmicrosoft.com | Set-MsolUser -AlternativeEmailAddress csg@hotmail.com
```
Show all users
```powershell
Get-Msoluser
```
To get the list of properties and methods you can view of users, use:
```powershell
Get-MsolUser | Get-Member
```
Show all attributes of a specific user:
```powershell
get-msoluser -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com | FL
```
Show a table with all users and specific attributes (see get-member for attributes):
```powershell
get-msoluser | FT FirstName, LastName, WhenCreated, validationstatus, IsLicensed, Licenses, PasswordNeverExpires

get-msoluser | FT FirstName, LastName, WhenCreated, validationstatus, IsLicensed, Licenses, PasswordNeverExpires | Export-Csv -Path "C:\Users.csv"

get-msoluser -MaxResults 10 | FT FirstName, LastName, WhenCreated, validationstatus, IsLicensed, Licenses, PasswordNeverExpires | Export-Csv -Path "C:\temp\Users.csv"
```

Get users with their appointed Office 365 license
```powershell
Get-MsolUser -MaxResults 10 |Where-Object {$_.Islicensed -eq "True"} | Select-Object userPrincipalName, Firstname, lastname, Licenses
```

Show a specific property (see get-member for options):
```powershell
(get-msoluser -UserPrincipalName "holly@Adatumvs2615.virsoftlabs.com").lastname
(get-msoluser -UserPrincipalName "holly@Adatumvs2615.virsoftlabs.com").validationstatus
(get-msoluser -UserPrincipalName "holly@Adatumvs2615.virsoftlabs.com").LastDirSyncTime
```
etcetera...

Prevent user from signing in (Block user):
```powershell
Set-MsolUser -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com -BlockCredential $true
```
Allow a user to sign in:
```powershell
Set-MsolUser -UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com -BlockCredential $false
```
Remove user:
```powershell
Remove-MsolUser "UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com "Force
```
Remove the user from the Recycle bin:
```powershell
Remove-MsolUser "UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com "RemoveFromRecycleBin
```
Bulk remove users:
```powershell
Get-MsolUser | Remove-MsolUser -Force
```
Bulk remove users from the recycle bin:
```powershell
Get-MsolUser "ReturnDeletedUsers | Remove-MsolUser -RemoveFromRecycleBin -Force
```
Search for a user:
```powershell
Get-MsolUser -SearchString Vera
```
View deleted users:
```powershell
Get-MsolUser "ReturnDeletedUsers
```
Restore deleted user:
```powershell
Restore-MsolUser "UserPrincipalName Catherine@Adatumvs2615.virsoftlabs.com
```
Bulk import from CSV file:
```powershell
Import-Csv -Path C:\O365Users.csv | ForEach-Object { New-MsolUser -UserPrincipalName $_."UPN" -AlternateEmailAddresses $_."AltEmail" -FirstName $_."FirstName" -LastName $_."LastName" -DisplayName $_."DisplayName" -BlockCredential $false -ForceChangePassword $false -LicenseAssignment $_."LicenseAssignment" -Password $_."Password" -PasswordNeverExpires $true -Title $_."Title" -Department $_."Department" -Office $_."Office" -PhoneNumber $_."PhoneNumber" -MobilePhone $_."MobilePhone" -Fax $_."Fax" -StreetAddress $_."StreetAddress" -City $_."City" -State $_."State" -PostalCode $_."PostalCode" -Country $_."Country" -UsageLocation $_."UsageLocation" } | Export-Csv -Path "C:\Results.csv"
```
CSV file format:
```
UPN,FirstName,LastName,DisplayName,AltEmail,BlockCredential,ForceChangePassword,LicenseAssignment,Password,PasswordNeverExpires,Title,Department,Office,PhoneNumber,MobilePhone,Fax,StreetAddress,City,State,PostalCode,Country,UsageLocation
Nona@Adatumvs2615.virsoftlabs.com,Nona,Snider,Nona Snider,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Accountant,Accounts,,,,,1 First St.,London,,1211,United Kingdom,GB
Jessica@Adatumvs2615.virsoftlabs.com,Jessica ,Jennings,Jessica Jennings,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Network Manager,Information Technology,,,,,1 First St.,London,,1211,United Kingdom,GB
Misty@Adatumvs2615.virsoftlabs.com,Misty,Phillips,Misty Phillips,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Accounting Manager,Accounts,,,,,1 First St.,London,,1211,United Kingdom,GB
Elvis@Adatumvs2615.virsoftlabs.com,Elvis,Cress,Elvis Cress,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Network Support,Information Technology,,,,,1 First St.,London,,1211,United Kingdom,GB
Leila@Adatumvs2615.virsoftlabs.com,Leila,Macdonald,Leila Macdonald,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Help Desk Support,Information Technology,,,,,1 First St.,London,,1211,United Kingdom,GB
Sherri@Adatumvs2615.virsoftlabs.com,Sherri,Harrell,Sherri Harrell,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Help Desk Support,Information Technology,,,,,1 First St.,London,,1211,United Kingdom,GB
Mickey@Adatumvs2615.virsoftlabs.com,Mickey,Earls,Mickey Earls,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Accounting Manager,Accounts,,,,,1 First St.,London,,1211,United Kingdom,GB
Leanna@Adatumvs2615.virsoftlabs.com,Leanna,Goodwin,Leanna Goodwin,user@alt.none,$False,$False,Adatumvs2615:ENTERPRISEPACK,Pa55w.rd,$True,Accountant,Accounts,,,,,1 First St.,London,,1211,United Kingdom,GB
```
<a name="password"/>

# Password management
Get password policy
```powershell
Get-MsolPasswordPolicy -DomainName Adatumvs2615.virsoftlabs.com
```
Set password validity period:
```powershell
Set-MsolPasswordPolicy -DomainName Adatumvsxxxx.onmicrosoft.com -ValidityPeriod 90 -NotificationDays 14
```
Change userpassword:
```powershell
Set-MsolUserPassword -UserPrincipalName Tameka@Adatumvs2615.virsoftlabs.com -NewPassword 'Pa55w.rd123'
```
Change userpassword ad force change password on logon:
```powershell
Set-MsolUserPassword -UserPrincipalName Tameka@Adatumvs2615.virsoftlabs.com -NewPassword 'Pa55w.rd123' -ForceChangePassword $true
```
Show users who s password never expires:
```powershell
Get-MsolUser | Set-MsolUser -PasswordNeverExpires $false
```
Check a specific user on Password never expires setting:
```powershell
Get-MsolUser -UserPrincipalName Holly@Adatumvs2615.virsoftlabs.com | select  PasswordNeverExpires
``` 
Check all users on PasswordNeverExpires setting:
```powershell
Get-MsolUser | FT DisplayName, PasswordNeverExpires
```
Set password to never expire on a user ($false for set to expire):
```powershell
Get-MsolUser -UserPrincipalName Holly@Adatumvs2615.virsoftlabs.com | Set-MsolUser -PasswordNeverExpires $true
```
Set password to never expire on all users ($false for set to expire):
```powershell
Get-MsolUser | Set-MsolUser -PasswordNeverExpires $true
```
<a name="roles"/>

# Roles management
Get list of roles available
```powershell
Get-MsolRole
Get-MsolRole | ft name, description
```
Get information about a specific role
```powershell
Get-MsolRole -RoleName "Helpdesk Administrator" | FL
```
View users in administrator role:
```powershell
$role = Get-MsolRole -RoleName" Service Support Administrator"
Get-MsolRoleMember "RoleObjectId $role.ObjectId
```
View users in Company administrator role:
```powershell
$role = Get-MsolRole -RoleName "Company Administrator"
Get-MsolRoleMember -RoleObjectId $role.ObjectId
```
View users in Billing role (etc):
```powershell
$role = Get-MsolRole -RoleName "Billing Administrator"
Get-MsolRoleMember -RoleObjectId $role.ObjectId
```
Set user to a specific role:
```powershell
Add-MsolRoleMember -RoleName "Billing Administrator" -RoleMemberEmailAddress Amy@Adatumvs2615.virsoftlabs.com
```
Remove role from user:
```powershell
Remove-MsolRoleMember -RoleName "Billing Administrator" -RolememberType User -RoleMemberEmailAddress Amy@Adatumvs2615.virsoftlabs.com
```
Add user to role
```powershell
Add-MsolRoleMember -RoleName "Service Support Administrator" -RoleMemberEmailAddress "Sallie@yourdomain.hostdomain.com"
```
Get overview of all users in a certain role
```powershell
$role = Get-MsolRole -RoleName "Service Support Administrator"
Get-MsolRoleMember -RoleObjectId $role.ObjectId
```
Other roles are:
* Billing administrator
* Dynamics 365 service administrator
* Exchange administrator
* Password administrator
* Skype for Business administrator
* Power BI service administrator
* Reports Reader administrator
* Service administrator
* SharePoint administrator
* User management administrator
* Password management
<a name="groups"/>

# Group management
Get all groups
```powershell
Get-MSOLGroup
```
Get members of a group
```powershell
```
Create a new group
```powershell
New-MsolGroup -DisplayName "Marketing" -Description "Marketing department users"
```
Add users to a group using variables
```powershell
$MktGrp = Get-MsolGroup | Where-Object {$_.DisplayName -eq "Marketing"}
$Catherine = Get-MsolUser | Where-Object {$_.DisplayName -eq "Catherine Richard"}
$Tameka = Get-MsolUser | Where-Object {$_.DisplayName -eq "Tameka Reed"}
Add-MsolGroupMember -GroupObjectId $MktGrp.ObjectId -GroupMemberType "User" -GroupMemberObjectId $Catherine.ObjectId
Add-MsolGroupMember -GroupObjectId $MktGrp.ObjectId -GroupMemberType "User" -GroupMemberObjectId $Tameka.ObjectId
```
Get members of the marketing group
```powershell
$MktGrp = Get-MsolGroup | Where-Object {$_.DisplayName -eq "Marketing"}
Get-MsolGroupMember -GroupObjectId $MktGrp.ObjectId
```

<a name="services"/>

# Services management
THIS SECTION IS IN PROGRESS

Disable RMS, Sway, Intune, yammer, FLOW_O365_P3 and FORMS_PLAN_E5 for only one person
```
$userUPN="Ashoka@mytest0718.onmicrosoft.com"
$accountSkuId="mytest0718:ENTERPRISEPREMIUM"
$planList=@( "RMS_S_ENTERPRISE","SWAY","INTUNE_O365","YAMMER_ENTERPRISE","FLOW_O365_P3","FORMS_PLAN_E5" )
$licenseOptions=New-MsolLicenseOptions -AccountSkuId $accountSkuId -DisabledPlans $planList
$user=Get-MsolUser -UserPrincipalName $userUPN
$usageLocation=$user.Usagelocation
Set-MsolUserLicense -UserPrincipalName $userUpn -AddLicenses $accountSkuId -ErrorAction SilentlyContinue
Sleep -Seconds 5
Set-MsolUserLicense -UserPrincipalName $userUpn -LicenseOptions $licenseOptions -ErrorAction SilentlyContinue
Set-MsolUser -UserPrincipalName $userUpn -UsageLocation $UsageLocation
```

Disable SharePoint and Office Online for everybody (SHAREPOINTENTERPRISE, SHAREPOINTWAC):
```
$disabledservices = New-MsolLicenseOptions -AccountSkuId “mytest0718:ENTERPRISEPREMIUM” -DisabledPlans “SHAREPOINTENTERPRISE”, “SHAREPOINTWAC”
Get-MsolUser -all | Where-Object {$_.isLicensed -eq $True} | Set-MsolUserLicense -LicenseOptions $disabledservices
```

https://docs.microsoft.com/en-us/office365/enterprise/powershell/disable-access-to-services-with-office-365-powershell


https://docs.microsoft.com/en-us/office365/enterprise/powershell/disable-access-to-services-while-assigning-user-licenses

https://docs.microsoft.com/en-us/office365/enterprise/powershell/disable-access-to-services-with-office-365-powershell

http://www.74k.org/office-365-licensing-enable-disable-service-plans



Find Your License SKU’s
To find out what license SKU’s you have type in the following command in Office365 PowerShell
Get-MsolAccountSku | Format-List –property accountskuid,activeunits,consumedunits

Results:
AccountSkuId : Your_AccountSkuId:AAD_PREMIUM
ActiveUnits : 200
ConsumedUnits : 4
AccountSkuId : Your_AccountSkuId:OFFICESUBSCRIPTION_FACULTY
ActiveUnits : 100
ConsumedUnits : 0
AccountSkuId : Your_AccountSkuId:STANDARDWOFFPACK_FACULTY
ActiveUnits : 300
ConsumedUnits : 0
AccountSkuId : Your_AccountSkuId:ENTERPRISEPACK_FACULTY
ActiveUnits : 300
ConsumedUnits : 274
AccountSkuId : Your_AccountSkuId:POWERAPPS_INDIVIDUAL_USER
ActiveUnits : 100
ConsumedUnits : 1
AccountSkuId : Your_AccountSkuId:STANDARDWOFFPACK_IW_FACULTY
ActiveUnits : 5000
ConsumedUnits : 31
AccountSkuId : Your_AccountSkuId:STANDARDWOFFPACK_IW_STUDENT
ActiveUnits : 10000
ConsumedUnits : 12
AccountSkuId : Your_AccountSkuId:EXCHANGESTANDARD_ALUMNI
ActiveUnits : 100
ConsumedUnits : 23

AccountSkuId:
The “Your_AccountSkuId” before the license name is your “AccountSkuId”, contained within each AccountSkuId are the various Service Plans .

These Service Plan names can be found within the SKU “ENTERPRISEPACK_FACULTY” by using the command below:

Find Service Plan Licenses Withing the SKU
Get-MsolAccountSku | Where-Object {$_.SkuPartNumber -eq “ENTERPRISEPACK_FACULTY”} | ForEach-Object {$_.ServiceStatus}

Results:

ServicePlan ProvisioningStatus
———– ——————
OFFICE_FORMS_PLAN_2 Success
PROJECTWORKMANAGEMENT Success
SWAY Success
INTUNE_O365 Success
YAMMER_EDU Success
RMS_S_ENTERPRISE Success
OFFICESUBSCRIPTION Success
MCOSTANDARD Success
SHAREPOINTWAC_EDU Success
SHAREPOINTENTERPRISE_EDU Success
EXCHANGE_S_ENTERPRISE Success

If you look at your Office365 console the licenses will have the name to the left of the “=” below.

Office 365 ProPlus = OFFICESUBSCRIPTION
Skype for Business Online (Plan 2) = MCOSTANDARD
Office Online for Education = SHAREPOINTWAC
SharePoint Plan 2 for EDU = SHAREPOINTENTERPRISE_EDU
Exchange Online (Plan 2) = EXCHANGE_S_ENTERPRISE
Microsoft Forms (Plan 2) = OFFICE_FORMS_PLAN_2
Microsoft Planner = PROJECTWORKMANAGEMENT
Sway = SWAY
Mobile Device Management for Office 365 ?
Yammer for Academic = YAMMER_EDU
Azure Rights Management = RMS_S_ENTERPRISE

To exclude any of these Service Plans using PowerShell we need to use the Service Plan names above on the right hand side of the = above.

Disable Service Plans
To disable Service Plans run the command below substituting which particular service you want to disable.

First set the variable $x:

$x = New-MsolLicenseOptions -AccountSkuId “Your_AccountSkuId:ENTERPRISEPACK_FACULTY” -DisabledPlans “YAMMER_EDU”, “OFFICE_FORMS_PLAN_2”, “SWAY”, “PROJECTWORKMANAGEMENT”, “INTUNE_O365”, “RMS_S_ENTERPRISE”,”MCOSTANDARD”

Then apply the value of the variable $x to all users:

Get-MsolUser -all | Where-Object {$_.isLicensed -eq $True} | Set-MsolUserLicense -LicenseOptions $x

To add Service Plans back in you can reset the variable $x and apply it to one or all users:

$x = New-MsolLicenseOptions -AccountSkuId “Your_AccountSkuId:ENTERPRISEPACK_FACULTY” -DisabledPlans “YAMMER_EDU”

If the above code was run followed by the code previous to that, all other licenses would be enabled except Yammer.

Remove a Single User’s Service Plan
To remove a single user’s Service Plan(s) run the code below:

Set-MsolUserLicense -UserPrincipalName user@mydomain.com -LicenseOptions $x

Find the Status of a User’s Service Plan
To see the status of a single user’s Service Plan(s) run the code below:

(Get-MsolUser -UserPrincipalName user@mydomain.com).Licenses.ServiceStatus

Restore all Service Plans
To make all licenses available again run the code below:

$x = New-MsolLicenseOptions -AccountSkuId “Your_AccountSkuId:ENTERPRISEPACK_FACULTY” -DisabledPlans $null

Then apply $x to an individual or all users

The commands below assume the value of $x to be “-DisabledPlans $null”

Restore all Service Plans for an Individual
Set-MsolUserLicense -UserPrincipalName user@mydomain.com -LicenseOptions $x

Restore all Service Plans for Everyone
Get-MsolUser -all | Where-Object {$_.isLicensed -eq $True} | Set-MsolUserLicense -LicenseOptions $x

Reference Link Below
https://technet.microsoft.com/library/dn771769.aspx

https://docs.microsoft.com/en-us/office365/enterprise/powershell/disable-access-to-services-while-assigning-user-licenses

<a name="more"/>

# More information
* https://support.office.com/en-us/article/manage-office-365-groups-with-powershell-aeb669aa-1770-4537-9de2-a82ac11b0540