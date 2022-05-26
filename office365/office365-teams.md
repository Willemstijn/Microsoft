Source of reference for this page is: https://docs.microsoft.com/en-us/MicrosoftTeams/teams-powershell-overview

The cmdlets for creating and managing teams are in the [Microsoft Teams PowerShell module](https://www.powershellgallery.com/packages/MicrosoftTeams/).

Alle comdlets for Teams are on the page [Microsoft Teams PowerShell](https://docs.microsoft.com/en-us/powershell/module/teams/?view=teams-ps)

### Get-team

Toont een overzicht van alle teams of properties van een geselecteerd team.

```
get-team

Get-Team -GroupId b1d615fb-d975-4fe5-84c4-1b0f4f978764 | Format-List

GroupId                           : b1d615fb-d975-4fe5-84c4-1b0f4f978764
DisplayName                       : provisioning
Description                       : provisioning enviromnent
Visibility                        : Public
MailNickName                      : provisioning
Classification                    :
Archived                          : False
AllowGiphy                        : True
GiphyContentRating                : moderate
AllowStickersAndMemes             : True
AllowCustomMemes                  : True
AllowGuestCreateUpdateChannels    : False
AllowGuestDeleteChannels          : False
AllowCreateUpdateChannels         : True
AllowDeleteChannels               : True
AllowAddRemoveApps                : True
AllowCreateUpdateRemoveTabs       : True
AllowCreateUpdateRemoveConnectors : True
AllowUserEditMessages             : True
AllowUserDeleteMessages           : True
AllowOwnerDeleteMessages          : True
AllowTeamMentions                 : True
AllowChannelMentions              : True
ShowInTeamsSearchAndSuggestions   : True
```

### Get-TeamUser

```
Get-TeamUser

cmdlet Get-TeamUser at command pipeline position 1
Supply values for the following parameters:
GroupId: b1d615fb-d975-4fe5-84c4-1b0f4f978764

UserId                               User                            Name            Role
------                               ----                            ----            ----
ca068d7b-ae20-4d52-9854-8d0ff185b7a7 admin@mnconnect.onmicrosoft.com Bas Willemstijn owner
5b927eff-ac41-49f2-8078-c881a98633f3 bwn@mnconnect.onmicrosoft.com   Bas W           owner
```