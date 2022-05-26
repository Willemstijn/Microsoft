; Login to exchange online:
$credential = Get-Credential
Connect-MsolService -Credential $credential
$ExchangeSession = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri "https://outlook.office365.com/powershell-liveid/" -Credential $credential -Authentication "Basic" -AllowRedirection
Import-PSSession $ExchangeSession -DisableNameChecking

; In Echange online certain objects are consolidated to save space, enable this (only required to run once on organizaton)
Enable-OrganizationCustomization

; Adding a new Exchange admin role group and add member:
New-RoleGroup –Name BranchOfficeAdmins –roles "Mail Recipients", "Distribution Groups", "Move Mailboxes", "Mail Recipient Creation"
Add-RoleGroupMember "BranchOfficeAdmins" -Member Martina
Get-RoleGroupMember "BranchOfficeAdmins"

; Create a new role assignment policy (exchange user permission):
New-RoleAssignmentPolicy "Limited Mailbox Configuration" -Roles MyBaseOptions, MyAddressInformation, MyDisplayName
Set-RoleAssignmentPolicy "Limited Mailbox Configuration" -IsDefault

# User management
; Generate list of email recipients
Get-Recipient | Select DisplayName, RecipientType, EmailAddresses

; Generate list of recipients and export them to a .csv file
Get-Recipient | Select DisplayName, RecipientType, EmailAddresses | Export-Csv EmailAddresses.csv

Importing initial contacts from CSV:
Import-Csv .\ExternalcontactsExchange.csv | ForEach {New-MailContact -Name $_.Name -DisplayName $_.Name -ExternalEmailAddress $_.ExternalEmailAddress -FirstName $_.FirstName -LastName $_.LastName}

Add additional info for contacts from csv:
Import-Csv .\ExternalContactsExchange.csv | ForEach {Set-Contact $_.Name -StreetAddress $_.StreetAddress -City $_.City -StateorProvince $_.StateorProvince -PostalCode $_.PostalCode -Phone $_.Phone -MobilePhone $_.MobilePhone -Pager $_.Pager -HomePhone $_.HomePhone -Company $_.Company -Title $_.Title -OtherTelephone $_.OtherTelephone -Department $_.Department -Fax $_.Fax -Initials $_.Initials -Notes $_.Notes -Office $_.Office -Manager $_.Manager}



# Recipient management

Get all Exchange Online recipients
```powershell
Get-Recipient | Select DisplayName, RecipientType, EmailAdress
```

Get recipients and export to csv
```powershell
Get-Recipient | Select DisplayName, RecipientType, EmailAdress | Export-CSV EmaiAdresses.csv
```

Get only the primary email adresses
```powershell
Get-Mailbox | Select-Object DisplayName, PrimarySmtpAddress
```

Adding Email Address to a user's mailbox (Note: primary email will become secondary email)
```powershell
Set-mailbox Catherine@Adatumvs2615.virsoftlabs.com -EmailAddress Catherine.Higgs@Adatumvs2615.virsoftlabs.com
```
Add Primary and secondary email addresses at once to a mailbox
```powershell
Set-mailbox Catherine.Higgs@Adatumvs2615.virsoftlabs.com -EmailAddress Catherine@Adatumvs2615.virsoftlabs.com,sales@Adatumvs2615.virsoftlabs.com,office@Adatumvs2615.virsoftlabs.com
```

Adding only aliasses to the mailbox
```powershell
Set-mailbox Catherine@Adatumvs2615.virsoftlabs.com -EmailAddress @{Add='software@Adatumvs2615.virsoftlabs.com'}
```

Removing an email address from a mailbox
```powershell
Set-mailbox Catherine@Adatumvs2615.virsoftlabs.com -EmailAddress @{Remove='software@Adatumvs2615.virsoftlabs.com'}
```

<a name="mailbox"/>

# Mailbox managment
List mailboxes
```powershell
Get-mailbox
```

Get mailbox from a specific user
```powershell
Get-mailbox Catherine@Adatumvs2615.virsoftlabs.com
```

Get all mailbox attributes from a specific user
```powershell
Get-mailbox Catherine@Adatumvs2615.virsoftlabs.com | FL
```

List mailboxes with forwarding enabled
```powershell
Get-mailbox | where {($_.ForwardingSMTPAddress -ne $null) -or ($_.ForwardingAddress -ne $null)} | Select Name, ForwardingSMTPAddress, ForwardingAddress, DeliverToMailboxAndForward
```

List Archive Mailboxes
```powershell
Get-mailbox -Archive
```

Enable archiving on a mailbox
```powershell
Enable-Mailbox Catherine@Adatumvs2615.virsoftlabs.com -Archive
```

Inventory of mailbox sizes
```powershell
Get-Mailbox | Get-MailboxStatistics | FT DisplayName, TotalItemSize
```

<a name="resource"/>

# Resource management

Resources are Rooms and IT items like beamers and such...

Create a Room mailbox
```powershell
New-mailBox -Name "Meeting Room 1" -Alias "MeetingRoom1" -Office "Ground Floor" -ResourceCapacity 12 -Phone 12345678 -Room
```

Let the room automatically accept bookings
```powershell
Set-CalendarProcessing "meeting Room 1" -AutomateProcessing AutoAccept
```

List all rooms
```powershell
Get-Mailbox -Filter `(RecipientTypeDetails -eq "RoomMailBox")` | Select Name, Alias, Office
```

Create other equipment
```powershell
New-mailBox -Name "Beamer 1" -Alias "Beamer1" -Equipment
```

List all equipment
```powershell
Get-Mailbox -Filter `(RecipientTypeDetails -eq "EquipmentMailBox")` | Select Name, Alias, Office
```

<a name="mailboxpermissions"/>

# Mailbox Permissions
We can grant users to access and use other mailboxes with permissions.

Get mailbox permissions
```powershell
Get-MailboxPermissions Catherine@Adatumvs2615.virsoftlabs.com | Select User, AccessRights, Deny
```

Grant a user (Amanda) full access to another users (Catherine) mailbox 
```powershell
Add-MailboxPermission -Identity Catherine@Adatumvs2615.virsoftlabs.com -User Amanda@Adatumvs2615.virsoftlabs.com -AccessRights FullAccess -InheritanceType All
```

Let a user (Amanda) send mail like as it was from Catherine (Send as permissions)
```powershell
Add-Recipientpermission "Catherine@Adatumvs2615.virsoftlabs.com" -AccessRights SendAs -Trustee Amanda@Adatumvs2615.virsoftlabs.com
```

<a name="mailboxshared"/>

# Shared mailboxes

List shared mailboxes
```powershell
Get-Mailbox -RecipientTypeDetails SharedMailbox
```

Add a (free) shared mailbox
```powershell
New-mailbox -Name "Shared" -DisplayName "Shared" -Alias "Info" -PrimarySmtpAddress "info@Adatumvs2615.virsoftlabs.com" -Shared
```

Remove a Shared Mailbox
```powershell
Remove-Msoluser -userPrincipalName info@Adatumvs2615.virsoftlabs.com
```

Convert a mailbox to a shared mailbox  
(in case you want to keep a users mailbox when the user leaves - default policy is removal of all user content after 30 days when removing a users license)
```powershell
Set-Mailbox Catherine@Adatumvs2615.virsoftlabs.com -Type Shared
```

Returing a shared mailbox to a normal mailbox
```powershell
Set-Mailbox Catherine@Adatumvs2615.virsoftlabs.com -Type Regular
```
Get shared mailbox permissions
```powershell
Get-MailboxPermission Accounts | Select user, Accessrights, Deny
```

<a name="group"/>

# Group management
Groups are used for distribution lists. New Office 365 groups are used as the base for sharing all Office 365 application content and access rights over all Office 365 users in that group.

List groups
```powershell
Get-DistributionGroup
```

Add a distribution group
```powershell
New-DistributionGroup -name "Services" -Displayname "Services" - Alias "Aliases" -PrimarySmtpAddress services@Adatumvs2615.virsoftlabs.com
```

Accept external senders (distribution groups accept only internal users email by default!)
```powershell
Set-DistributionGroup services@Adatumvs2615.virsoftlabs.com -RequireSenderAuthenticationEnabled $False
```

Add another email to a group (SMTP is primary email, smtp is secondary email address)
```powershell
Set-DistributionGroup "Services" -EmailAddresses SMTP:services@Adatumvs2615.virsoftlabs.com,contact@Adatumvs2615.virsoftlabs.com
```

Removing a group
```powershell
Remove-DistributionGroup "Services"
```

List Group members
```powershell
Get-DistributionGroupMember "Services"
```

Add a user to a group
```powershell
Add-DistributionGroupMember "Services" -Member Catherine@Adatumvs2615.virsoftlabs.com -BypassSecurityGroupmanagerCheck
```

Remove user from group
```powershell
Remove-DistributionGroupMember "Services" -Member Catherine@Adatumvs2615.virsoftlabs.com 
```

<a name="contact"/>

# External Contact management
List external contacts
```powershell
Get-MailContact
```

Show all attributes of a specific contact
```powershell
Get-MailContact -Identity "Bruce Banner (ExternalAdmin)" | FL
```

Show only the external email address
```powershell
Get-MailContact -Identity "Bruce Banner (ExternalAdmin)" | Select ExternalEmailAddress
```

Add a contact
```powershell
New-MailContact -Name "Steve Rogers (Captain)" -ExternalEmailAddress Captain@America.com -Alias "CaptainAmerica"
```

Remove contact
```powershell
Remove-MailContact -Name "Steve Rogers (Captain)" 
```

Update contact
```powershell
Set-MailContact -Identity "Steve Rogers (Captain)" -ExternalMailAddress Steve@America.com
```

<a name="folder"/>

# Public folders
List all public folders
```powershell
Get-Mailbox -PublicFolder
```

Create Public folder mailbox
```powershell
New-Mailbox -PublicFolder -Name PublicMailbox
```

Remove a public folder mailbox
```powershell
Remove-Mailbox -PublicFolder -Identity "PublicMailbox"
```

List public folder mailboxes
```powershell
Get-PublicFolder -Recurse
```

Add Public folder to root location
```powershell
New-PublicFolder -Name "Marketing"
```

Create new public folder in marketing root location
```powershell
New-PublicFolder -Name "Sales" -Path "\Marketing"
```

Create new public folder in sales location
```powershell
New-PublicFolder -Name "Reports" -Path "\Marketing\Sales"
```

Remove a public folder with its subfolders
```powershell
New-PublicFolder -Name "Marketing" -Recurse
```

Public folder permissions  
The following roles can be assigned to a user in a public folder:
* None
* Reviewer
* Contributor
* Non editing author
* Author
* Publishing Editor
* Publishing Author
* Owner

Add permissions to a public folder
```powershell
Add-PublicFolderClientPermission -Identity "\Marketing" -User "Catherine Higgs" -AccessRights PublishingEditor
```

Recursively adding rights to a main root folder and all its subfolders
```powershell
Get-PublicFolder "\Marketing" | Add-PublicFolderClientPermission -User "Catherine Higgs" -AccessRights PublishingEditor
```

Show public folder permissions for a specific folder
```powershell
Get-PublicFolderClientpermission "\Marketing"
```

Show permissions from all folders
```powershell
Get-PublicFolder -Recurse | Get-PublicFolderClientpermission
```

 # Mailbox management
 ; Get a list of all the mailboxes
Get-Mailbox

; Get a list of mailboxes with some properties:
Get-Mailbox |Select-Object DisplayName,PrimarySmtpAddress

; Set a new primary e-mail adress on a mailbox - the old mail adress will be an alias:
Set-Mailbox ian.waters@office365lab.co.uk -EmailAddress ian@office365lab.co.uk 

; Update the primary email address and add extra aliasses:
Set-Mailbox ian.waters@office365lab.co.uk -EmailAddress ian@office365lab.co.uk,info@office365lab.co.uk,sales@office365lab.co.uk

; Add an extra email aliass in the adress aray:
Set-Mailbox -Identity ian.waters@office365lab.co.uk -EmailAddresses @{Add=‘accounts@office365lab.co.uk’}

; Remove an email alias adress from the adress array:
Set-Mailbox ian.waters@office365lab.co.uk -EmailAddresses @{Remove=“info@office365lab.co.uk”}

; Return the individual entries of a users email array :
(Get-Mailbox ian.waters@office365lab.co.uk).EmailAddresses[0]

; Get the list of mailboxes of a user:
Get-Mailbox ian.waters@office365lab.co.uk

; Get all of the mailbox attributes of a user:
Get-mailbox ian.waters@office365lab.co.uk | FL

; List all mailboxes who has set forwarding to other email adresses:
Get-Mailbox | Where {($_.ForwardingSMTPAddress -ne $null) -or ($_.ForwardingAddress -ne $null)} | Select Name, ForwardingSMTPAddress, ForwardingAddress, DeliverToMailboxAndForward

; Get a list of all mailboxes with archiving enabled
Get-Mailbox -Archive

; Enable mailbox archiving for a user
Enable-Mailbox ian.waters@office365lab.co.uk –Archive

; List the size of all mailboxes:
Get-Mailbox | Get-MailboxStatistics | FT DisplayName, TotalItemSize

; ===== [Mailbox Permissions] ======
; Permissions can be: FullAccess, DeleteItem, ReadPermission, ChangePermission, ChangeOwner, SendAs.

; Get overview of mailbox permissions:
Get-MailboxPermission fay.williams@office365lab.co.uk | Select User, AccessRights, Deny

; Grant a user full access to certain mailbox of another user:
Add-MailboxPermission -Identity “ian.waters@office365lab.co.uk” -User “michael.freeman@office365lab.co.uk” AccessRights FullAccess -InheritanceType All

; Grant user "send as" permissions to another users mailbox:
Add-RecipientPermission “ian.waters@office365lab.co.uk” -AccessRights SendAs -Trustee Michael.freeman@office365lab.co.uk

; ===== [Shared Mailboxes] ======
; List shared mailboxes
Get-Mailbox -RecipientTypeDetails SharedMailbox 

; Get shared mailbox permissions
Get-MailboxPermission Accounts | Select User, AccessRights, Deny

; Add shared mailbox:
New-Mailbox -Name “Info” –DisplayName “Info” -Alias “info” -PrimarySmtpAddress “info@office365lab.co.uk” – Shared

; Remove a shared mailbox
Remove-MsolUser –UserPrincipalName info@office365lab.co.uk

; Convert a mailbox to a shared mailbox
Set-Mailbox projects@office365lab.co.uk –Type Shared

; Convert a shared mailbox to a regular mailbox
Set-Mailbox projects@office365lab.co.uk –Type Regular

# Exchange resource management
; Creating resource mailboxes:
New-MailBox -Name “Meeting Room 1” -Alias “GFMeetingRoom” –Office “Ground Floor” -ResourceCapacity 10 Phone 01323287828 –Room

New-Mailbox -Name "Conference Room" –Room

; Set rooms to automatically accept bookings:
Set-CalendarProcessing "Conference Room" -AutomateProcessing AutoAccept

; Set room with capacity:
Set-Mailbox "Conference room" –ResourceCapacity 25

; Make new resource for equipment:
New-Mailbox -Name "Demonstration Laptop" -Alias "DemoLaptop" –Equipment
Set-CalendarProcessing "Demonstration Laptop" -AutomateProcessing AutoAccept

; make new projector resource:
New-Mailbox -Name “Projector 1” -Alias “Projector1” –Equipment

; Get overview of all rooms (room mailboxes)
Get-Mailbox -Filter ‘(RecipientTypeDetails -eq “RoomMailBox”)’ | Select Name,Alias,Office

; Get list of all equipment:
Get-Mailbox -Filter ‘(RecipientTypeDetails -eq “EquipmentMailBox”)’ | Select Name,Alias


