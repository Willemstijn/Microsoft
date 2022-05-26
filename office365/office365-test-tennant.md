## Create a temporary outlook email account

Go to https://outlook.live.com/owa/ and click "Create account".

My preferred way is to keep the email adress similar to the name of the test tenant later. 
So when the test tenant is called mytest0718.onmicrosoft.com, my email address is mytest0718@oulook.com.  
Use an easy to remember password (0718mytest).

Fill in a name and click next. Select your country and birthdate and click next until you finish.
After you have completed this step, you can continue creating a Office 365 tenant.

## Create a temporary Office 365 tenant
Go to https://products.office.com/en-us/business/compare-more-office-365-for-business-plans and select plan E3 or E5 to try for free.  
In my case i've chosen the [https://signup.microsoft.com/Signup?OfferId=101bde18-5ffb-4d79-a47b-f5b2c62525b3&dl=ENTERPRISEPREMIUM&culture=en-US&country=US&ali=1](Enterprise E5 Trial).

Choose your region and fill in the required fields. Make sure you use the outlook email adress and fill in the correct tenant name. Click next.  
Create an email address for the tenant administrator and make sure you use the tenant name similar to the outlook email address (mytest0718 in this case).  
Use an easy to remember password (0718-m-y-t-e-s-t).  
Provide your telephone number, this is a unavoidable step to get the tenant...  
Enter the verification code. After doing this you'll receive some information about your tenant.

Sign-in page  
https://portal.office.com/

Your user ID  
mytest@mytest0718.onmicrosoft.com

# Setting up the tenant
After you've got the tenant, it is time to set up the tenant, administrator and user accounts. First check some Tenant stuff:
- Check amount of Licenses: Get-MsolAccountSku
- Get the configured admins (should be only one company administrator): Get-MsolAdmins
- Get company information at this stage: Get-MsolCompanyInformation

## Setting up the administrator
Should be nothing more to do here. Just check if the account is licensed:  
```
Get-MsolUser -UserPrincipalName mytest@mytest0718.onmicrosoft.com
```

## Logging in to the tenant with powershell
I'll assume you have taken the necessary steps to install all the PowerShell requirements to be able to log. For more information, use the next page: [office365-login.md](Log in to Office 365 Services).

## Adding users to the tenant
Now it's time to bulk import users from a CSV file:

```
Import-Csv -Path C:\O365Users.csv | ForEach-Object { New-MsolUser -UserPrincipalName $_."UPN" -AlternateEmailAddresses $_."AltEmail" -FirstName $_."FirstName" -LastName $_."LastName" -DisplayName $_."DisplayName" -BlockCredential $false -ForceChangePassword $false -LicenseAssignment $_."LicenseAssignment" -Password $_."Password" -PasswordNeverExpires $true -Title $_."Title" -Department $_."Department" -Office $_."Office" -PhoneNumber $_."PhoneNumber" -MobilePhone $_."MobilePhone" -Fax $_."Fax" -StreetAddress $_."StreetAddress" -City $_."City" -State $_."State" -PostalCode $_."PostalCode" -Country $_."Country" -UsageLocation $_."UsageLocation" } | Export-Csv -Path "C:\Results.csv"
```

Use the following content in the CSV file format. Be sure to change the email addresses and accountSKU to math with your tenant.:

```
UPN,FirstName,LastName,DisplayName,AltEmail,BlockCredential,ForceChangePassword,LicenseAssignment,Password,PasswordNeverExpires,Title,Department,Office,PhoneNumber,MobilePhone,Fax,StreetAddress,City,State,PostalCode,Country,UsageLocation
Darth@mytest0718.onmicrosoft.com,Darth,Vader,Darth Vader,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Luke@mytest0718.onmicrosoft.com,Luke,SkyWalker,Luke SkyWalker,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Jedi,Jedi,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Obi-Wan@mytest0718.onmicrosoft.com,Obi-Wan,Kenobi,Obi-Wan Kenobi,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Jedi,Jedi,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Leia@mytest0718.onmicrosoft.com,Leia,Organa,Leia Organa,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Princess,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Han@mytest0718.onmicrosoft.com,Han,Solo,Han Solo,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Smuggler,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Emperor@mytest0718.onmicrosoft.com,Emperor,Palpatine,Emperor Palpatine,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Kylo@mytest0718.onmicrosoft.com,Kylo,Ren,Kylo Ren,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,New Order,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Padme@mytest0718.onmicrosoft.com,Padme,Amidala,Padme Amidala,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Accountant,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Boba@mytest0718.onmicrosoft.com,Boba,Fett,Boba Fett,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Bounty Hunter,Bounty Hunter,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Darth@mytest0718.onmicrosoft.com,Darth,Maul,Darth Maul,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Wilhuff@mytest0718.onmicrosoft.com,Wilhuff,Tarkin,Wilhuff Tarkin,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Admiral,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Qui-Gon@mytest0718.onmicrosoft.com,Qui-Gon,Jinn,Qui-Gon Jinn,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Jedi,Jedi,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Ashoka@mytest0718.onmicrosoft.com,Ashoka,Tano,Ashoka Tano,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Jedi,Jedi,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Asajj@mytest0718.onmicrosoft.com,Asajj,Ventress,Asajj Ventress,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Wedge@mytest0718.onmicrosoft.com,Wedge,Antilles,Wedge Antilles,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Pilot,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Nute@mytest0718.onmicrosoft.com,Nute,Gunray,Nute Gunray,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,King,Trade Federation,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Galen@mytest0718.onmicrosoft.com,Galen,Marek,Galen Marek,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Sith Lord,Empire,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Bail@mytest0718.onmicrosoft.com,Bail,Organa,Bail Organa,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,King,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Mace@mytest0718.onmicrosoft.com,Mace,Windu,Mace Windu,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Jedi,Jedi,,,,,Galaxy Far away,1,,1111,Netherlands,NL
Lando@mytest0718.onmicrosoft.com,Lando,Calrissian,Lando Calrissian,alt@none.com,$False,$False,mytest0718:ENTERPRISEPREMIUM,Pa55w.rd,$True,Scoundrell,Rebels,,,,,Galaxy Far away,1,,1111,Netherlands,NL
```

## Configuring roles to users

Now that people have licenses, it's time to add some administrative roles to some users. Use the lines below to add the following roles (Power BI Service Administrator, Helpdesk Administrator, Billing Administrator, Exchange Service Administrator, Lync Service Administrator, User Account Administrator, Company Administrator, SharePoint Service Administrator) to some users.

```
Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberEmailAddress "Emperor@mytest0718.onmicrosoft.com"
Add-MsolRoleMember -RoleName "Helpdesk Administrator" -RoleMemberEmailAddress "Obi-Wan@mytest0718.onmicrosoft.com"
Add-MsolRoleMember -RoleName "Exchange Service Administrator" -RoleMemberEmailAddress "Ashoka@mytest0718.onmicrosoft.com"
Add-MsolRoleMember -RoleName "Exchange Service Administrator" -RoleMemberEmailAddress "Galen@mytest0718.onmicrosoft.com"
Add-MsolRoleMember -RoleName "SharePoint Service Administrator" -RoleMemberEmailAddress "Darth@mytest0718.onmicrosoft.com"
Add-MsolRoleMember -RoleName "User Account Administrator" -RoleMemberEmailAddress "Leia@mytest0718.onmicrosoft.com"
```

Check afterwards if the roles are correctly assigned:

```
Get-MsolAdmins
```

## Configuring services to users

In this step we'll be configuring the individual services to users. All services are on by default, in this testing environment we want some services disabled on some users to test inventory scripts.

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

And some other services for another user as well.

```
$userUPN="Leia@mytest0718.onmicrosoft.com"
$accountSkuId="mytest0718:ENTERPRISEPREMIUM"
$planList=@( "SWAY","YAMMER_ENTERPRISE","FLOW_O365_P3" )
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

# End of initial setup

The following lines contain additional setup of the tenant for testing purposes.

## Create SharePoint Sites and add the users

Create a CSV file and add the following content

```
Owner,StorageQuota,Url,ResourceQuota,Template,TimeZoneID,Name
Han@mytest0718.onmicrosoft.com,100,https://mytest0718.sharepoint.com/sites/TeamSite01,25,EHS#1,4,TeamSite
Luke@mytest0718.onmicrosoft.com,100,https://mytest0718.sharepoint.com/sites/Blog01,25,BLOG#0,4,Blog
Darth@mytest0718.onmicrosoft.com,150,https://mytest0718.sharepoint.com/sites/Project01,25,PROJECTSITE#0,4,Project
Padme@mytest0718.onmicrosoft.com,150,https://mytest0718.sharepoint.com/sites/Community01,25,COMMUNITY#0,4,Community
```

Save it as the file 'sitecollections.csv'

Connect to SharePoint Online tenant and execute the following command:

```
Import-Csv C:\temp\SiteCollections.csv | ForEach-Object {New-SPOSite -Owner $_.Owner -StorageQuota $_.StorageQuota -Url $_.Url -NoWait -ResourceQuota $_.ResourceQuota -Template $_.Template -TimeZoneID $_.TimeZoneID -Title $_.Name}
```

Now check if all site collections are made correctly

```
Get-SPOSite -Detailed | Format-Table -AutoSize
```

## Adding users and groups to the sharepoint sites

Make two csv files, one for the groups and permissions and one for the users:

```
Site,Group,PermissionLevels
https://mytest0718.sharepoint.com/sites/Community01,mytest Project Leads,Full Control
https://mytest0718.sharepoint.com/sites/Community01,mytest Auditors,View Only
https://mytest0718.sharepoint.com/sites/Community01,mytest Designers,Design
https://mytest0718.sharepoint.com/sites/TeamSite01,XT1000 Team Leads,Full Control
https://mytest0718.sharepoint.com/sites/TeamSite01,XT1000 Advisors,Edit
https://mytest0718.sharepoint.com/sites/Blog01,mytest Blog Designers,Design
https://mytest0718.sharepoint.com/sites/Blog01,mytest Blog Editors,Edit
https://mytest0718.sharepoint.com/sites/Project01,Project Alpha Approvers,Full Control
```

Save this as the file SPOSiteGroupsAndPermissions.csv


```
Group,LoginName,Site
mytest Project Leads,Leia@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Community01
mytest Auditors,emperor@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Community01
mytest Designers,galen@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Community01
XT1000 Team Leads,nute@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/TeamSite01
XT1000 Advisors,wedge@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/TeamSite01
mytest Blog Designers,Boba@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Blog01
mytest Blog Editors,ashoka@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Blog01
Project Alpha Approvers,padme@mytest0718.onmicrosoft.com,https://mytest0718.sharepoint.com/sites/Project01
```

Save this as the file ImportSPOUsers.csv

Finally run the following command to make the groups, permissions and their users.

```
Import-Csv C:\temp\SPOSiteGroupsAndPermissions.csv | ForEach-Object {New-SPOSiteGroup -Group $_.Group -PermissionLevels $_.PermissionLevels -Site $_.Site} 
Import-Csv C:\temp\ImportSPOUsers.csv | where {Add-SPOUser -Group $_.Group –LoginName $_.LoginName -Site $_.Site}
```

You can also put this command in a .ps1 script file for execution from the commandline.