---
Order: 
Area: Office 365
TOCTitle: Office 365 User and Group Management
ContentId: 
PageTitle: Office 365 User and Group Management with PowerShell
DateApproved: 
MetaDescription: Page containing information about user and group management in office 365 with powershell.
---
[User management](#users)  
[Password management](#password)  
[Roles management](#roles)  
[Groups management](#groups)  
[Services management](#services)  

<a name="users"/>

# User management
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



