---
title: Toestaan of blokkeren uitnodigingen voor B2B gebruikers van specifieke organisaties - Azure Active Directory | Microsoft Docs
description: Toont hoe een beheerder de Azure-portal of PowerShell gebruiken kunt voor de toegang is ingesteld of deny-lijst B2B gebruikers uit bepaalde domeinen blokkeren of toestaan.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/19/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 3b4b57dd2299c6278fe823f59a4f2c7d8721f712
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren

U kunt een lijst met toegestane of een lijst weigeren toestaan of blokkeren uitnodigingen voor B2B gebruikers van specifieke organisaties. Bijvoorbeeld, als u wilt een persoonlijk e-mailadres domeinen blokkeren, kunt u instellen om een lijst weigeren met domeinen zoals Gmail.com en Outlook.com. Als uw bedrijf verbonden met andere bedrijven zoals Contoso.com, Fabrikam.com en Litware.com is en u wilt beperken tot alleen deze organisaties inschrijvingen, kunt u toevoegen Contoso.com, Fabrikam.com en Litware.com naar de lijst voor toestaan.
  
## <a name="important-considerations"></a>Belangrijke overwegingen

- U kunt een lijst met toegestane of een lijst weigeren maken. U kunt beide typen lijsten niet instellen. Standaard welke domeinen zijn niet in de lijst zijn op de lijst weigeren, en vice versa. 
- U kunt slechts één beleid per organisatie maken. U kunt het beleid voor het opnemen van meer domeinen bijwerken of u het beleid voor het maken van een nieuwe kunt verwijderen. 
- Deze lijst werkt onafhankelijk van OneDrive voor bedrijven en SharePoint Online toestaan/blok lijsten. Als u beperken tot afzonderlijke delen van bestanden in SharePoint Online wilt, moet u instellen van toestaan of weigeren van de lijst voor OneDrive voor bedrijven en SharePoint Online. Zie voor meer informatie [domeinen delen in SharePoint Online en OneDrive voor bedrijven beperkt](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Deze lijst geldt niet voor externe gebruikers die u hebt de uitnodiging al ingewisseld. De lijst worden afgedwongen nadat de lijst is ingesteld. Als een uitnodiging voor een gebruiker in behandeling is en u een beleid instellen dat hun domein blokkeert, mislukt van de gebruiker naar de uitnodiging inwisselen.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Stel de toestaan of weigeren lijst beleid in de portal

Standaard de **uitnodigingen worden verzonden naar een willekeurig domein (meest inclusief) toestaan** instelling is ingeschakeld. In dit geval kunt u B2B gebruikers vanaf elke organisatie uitnodigen.

### <a name="add-a-deny-list"></a>Een lijst weigeren toevoegen

Dit is het meest voorkomende scenario waar uw organisatie wil werken met vrijwel elke organisatie, maar wil voorkomen dat gebruikers uit bepaalde domeinen worden uitgenodigd als B2B-gebruikers.

Een lijst weigeren toevoegen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
3. Onder **externe gebruikers**, selecteer **beheren van instellingen voor externe samenwerking**.
4. Onder **samenwerking beperkingen**, selecteer **uitnodigingen voor de opgegeven domeinen weigeren**.
5. Onder **DOELDOMEINEN**, voer de naam van een van de domeinen die u wilt blokkeren. Geef elk domein op een nieuwe regel voor meerdere domeinen. Bijvoorbeeld:

   ![Toont de optie weigeren met toegevoegde domeinen](./media/allow-deny-list/DenyListSettings.png)
 
6. Wanneer u bent klaar, klikt u op **opslaan**.

Nadat u het beleid hebt ingesteld als u probeert uit te nodigen van een gebruiker uit een geblokkeerde domein, ontvangen een bericht weergegeven dat het domein van de gebruiker momenteel is geblokkeerd door uw uitnodiging-beleid.
 
### <a name="add-an-allow-list"></a>Een lijst toevoegen

Dit is een meer beperkend configuratie, kunt u specifieke domeinen ingesteld in de lijst met toegestane en beperken van uitnodigingen aan een andere organisaties of domeinen die niet zijn genoemd. 

Als u wilt een acceptatielijst gebruiken, zorg ervoor dat u tijd besteden aan het volledig evalueren wat uw bedrijf moet zijn. Als u dit beleid te beperkend, kunt uw gebruikers documenten verzenden via e-mail of andere niet-IT erkende manieren om samen te werken niet vinden.


Een lijst toevoegen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
3. Onder **externe gebruikers**, selecteer **beheren van instellingen voor externe samenwerking**.
4. Onder **samenwerking beperkingen**, selecteer **uitnodigingen alleen voor de opgegeven domeinen (meest beperkende) toestaan**.
5. Onder **DOELDOMEINEN**, voer de naam van een van de domeinen die u wilt toestaan. Geef elk domein op een nieuwe regel voor meerdere domeinen. Bijvoorbeeld:

   ![Ziet u de optie toestaan met toegevoegde domeinen](./media/allow-deny-list/AllowListSettings.png)
 
6. Wanneer u bent klaar, klikt u op **opslaan**.

Nadat u het beleid hebt ingesteld als u probeert uit te nodigen van een gebruiker uit een domein dat zich niet op de lijst accepteren, ontvangen een bericht weergegeven dat het domein van de gebruiker momenteel is geblokkeerd door uw uitnodiging-beleid.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Overschakelen van de lijst en vice versa weigeren toestaan 

Als u van de ene naar de andere overschakelt, verwijdert deze de configuratie van het bestaande beleid. Zorg ervoor dat back-up van gegevens van uw configuratie voordat u de schakeloptie uitvoert. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>Stel de toestaan of weigeren lijst beleid met behulp van PowerShell

### <a name="prerequisite"></a>Vereiste

Als u wilt instellen toestaan of weigeren van lijst met behulp van PowerShell, moet u de preview-versie van de Azure Active Directory-Module voor Windows PowerShell installeren. Installeren in het bijzonder de AzureADPreview moduleversie 2.0.0.98 of hoger.

Controleer de versie van de module (en als deze geïnstalleerd):
 
1. Open Windows PowerShell als een verhoogde gebruiker (als Administrator uitvoeren). 
2. Voer de volgende opdracht om te zien als u alle versies van de Azure Active Directory-Module voor Windows PowerShell is geïnstalleerd op uw computer hebt:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````

Als de module is niet geïnstalleerd of u niet een vereiste versie hebt, een van de volgende handelingen uit:

- Als er geen resultaten worden geretourneerd, voert u de volgende opdracht voor het installeren van de meest recente versie van de module AzureADPreview:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- Als alleen de module AzureAD wordt weergegeven in de resultaten, voer de volgende opdrachten voor het installeren van de module AzureADPreview: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- Als alleen de module AzureADPreview wordt weergegeven in de resultaten, maar de versie minder dan 2.0.0.98 is, voer de volgende opdrachten bij te werken: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- Als de AzureAD en de AzureADPreview modules in de resultaten worden weergegeven, maar de versie van de module AzureADPreview minder dan 2.0.0.98 is, voert u de volgende opdrachten bij te werken: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>De AzureADPolicy-cmdlets gebruiken voor het beleid configureren

Gebruik wilt maken, toestaan of weigeren lijst, de [nieuw AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Het volgende voorbeeld ziet hoe u een lijst weigeren die blokkeert het domein 'live.com' instelt.

````powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Hieronder vindt u in het hetzelfde voorbeeld, maar met de definitie beleid inline.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Voor het instellen van het toestaan of weigeren lijst beleid, gebruikt u de [Set AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

Als u het beleid, gebruikt de [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

U kunt het beleid met de [verwijderen AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B [wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- Zie voor meer informatie over voorwaardelijke toegang en B2B-samenwerking [voorwaardelijke toegang voor gebruikers voor B2B-samenwerking](conditional-access.md).



