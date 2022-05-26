# Het instellen van retentie in Office 365
Onderstaande voorbeelden laten zien hoe via PowerShell retentiebeleid is in te stellen binnen een Office 365 tenant op verschillende onderdelen zoals groups, emails, data en andere zaken.
## Instellen van retentie op O365 groups
[Bron](https://docs.microsoft.com/nl-nl/azure/active-directory/users-groups-roles/groups-lifecycle)
Installeer eerst de AzureAD powershell module. Open een powershell terminal in Administrator modus en voer de volgende commando's uit:
```
Install-Module -Name AzureAD
Connect-AzureAD
```
[Screenshot](./images/2019-10-03-13_27_34.png)

Haal vervolgens het huidige beleid op van de groups lifecycle policy met:
```
Get-AzureADMSGroupLifecyclePolicy
```

Als je niets ziet, dan is er nog geen policy ingesteld.

Stel de group policy in met:
```
New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
```

Deze policy wordt automatisch ingesteld op alle Office 365 groepen.  
Controleer daarna of de policy is doorgevoerd.
```
Get-AzureADMSGroupLifecyclePolicy

ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
--                                    ------------------- ----------------- ---------------------------
26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
```
Hier zie je:
* De beleids-ID
* De levensduur voor alle Office 365-groepen in de Azure AD-organisatie is ingesteld op 365 dagen
* Meldingen voor het vernieuwen van Office 365-groepen zonder eigenaars worden verzonden naar emailaddress@contoso.com

Wijzigen van de bestaande policy kan op de volgende manier (van 365 naar 180 dagen):
```
Set-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
```

Er kunnen ook specifieke groepen toegekend worden aan de policy:
```
Add-AzureADMSLifecyclePolicyGroup -Id "26fcc232-d1c3-4375-b68d-15c296f1f077" -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
```

Met het onderstaande commando wordt het beleid voor alle Office 365 groepen uitgeschakeld:
```
Remove-AzureADMSGroupLifecyclePolicy -Id "26fcc232-d1c3-4375-b68d-15c296f1f077"
```