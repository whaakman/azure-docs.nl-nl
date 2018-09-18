---
title: Uitnodigingen voor B2B-gebruikers van bepaalde organisaties - Azure Active Directory toestaan of blokkeren | Microsoft Docs
description: Laat zien hoe een beheerder de Azure portal of PowerShell gebruiken kunt voor het instellen van een of lijst wilt toestaan of blokkeren van B2B-gebruikers uit bepaalde domeinen weigeren.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 04/19/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 563b2d6393533a86305213b6cdec6ca901e53257
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985376"
---
# <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Uitnodigingen voor B2B-gebruikers van bepaalde organisaties toestaan of blokkeren

U kunt een lijst met toegestane of een lijst weigeren uitnodigingen toestaan of blokkeren voor B2B-gebruikers van specifieke organisaties. Bijvoorbeeld, als u blokkeren persoonlijke e-mailadresdomeinen wilt, kunt u instellen een lijst weigeren met domeinen, zoals Gmail.com en Outlook.com. Of, als uw bedrijf samen met andere bedrijven, zoals Contoso.com, Fabrikam.com en Litware.com heeft en u wilt beperken uitnodigingen voor alleen deze organisaties, kunt u toevoegen Contoso.com en Fabrikam.com Litware.com aan uw lijst voor toestaan.
  
## <a name="important-considerations"></a>Belangrijke overwegingen

- U kunt een lijst met toegestane of een lijst weigeren maken. U kunt beide soorten lijsten niet instellen. Standaard, ongeacht domeinen zijn niet in de acceptatielijst zijn op de lijst weigeren, en vice versa. 
- U kunt slechts één beleid per organisatie maken. U kunt het beleid om op te nemen meer domeinen bijwerken of u het beleid voor het maken van een nieuw wachtwoord kunt verwijderen. 
- Deze lijst werkt onafhankelijk van OneDrive voor bedrijven en SharePoint Online toestaan/blokkeren lijsten. Als u wilt een afzonderlijk bestand delen in SharePoint Online beperken, moet u instellen toestaan of weigeren van de lijst voor OneDrive voor bedrijven en SharePoint Online. Zie voor meer informatie, [beperkte domeinen delen in SharePoint Online en OneDrive voor bedrijven](https://support.office.com/article/restricted-domains-sharing-in-sharepoint-online-and-onedrive-for-business-5d7589cd-0997-4a00-a2ba-2320ec49c4e9).
- Deze lijst is niet van toepassing op externe gebruikers die u hebt de uitnodiging al ingewisseld. De lijst worden afgedwongen nadat de lijst is ingesteld. Als een uitnodiging voor een gebruiker in behandeling is en u een beleid instellen dat hun domein blokkeert, mislukken de poging van de gebruiker de uitnodiging inwisselen.

## <a name="set-the-allow-or-deny-list-policy-in-the-portal"></a>Instellen van het toestaan of weigeren van beleid in de portal

Standaard de **toestaan dat uitnodigingen worden verzonden naar elk domein (meest inclusief)** instelling is ingeschakeld. In dit geval kunt u B2B-gebruikers van elke organisatie uitnodigen.

### <a name="add-a-deny-list"></a>Een lijst weigeren toevoegen

Dit is de meest voorkomende scenario, waar uw organisatie wil werken met nagenoeg elke organisatie, maar wil voorkomen dat gebruikers in specifieke domeinen worden uitgenodigd als B2B-gebruikers.

Een lijst weigeren toevoegen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
3. Onder **externe gebruikers**, selecteer **instellingen voor externe samenwerking beheren**.
4. Onder **samenwerkingsbeperkingen**, selecteer **uitnodigingen voor de opgegeven domeinen weigeren**.
5. Onder **DOELDOMEINEN**, voer de naam van een van de domeinen die u wilt blokkeren. Voer elk domein op een nieuwe regel voor meerdere domeinen. Bijvoorbeeld:

   ![Toont de optie weigeren met toegevoegde domeinen](./media/allow-deny-list/DenyListSettings.png)
 
6. Wanneer u klaar bent, klikt u op **opslaan**.

Nadat u het beleid hebt ingesteld als u probeert een gebruiker van een geblokkeerde domein uitnodigen, ontvangt u een bericht weergegeven dat het domein van de gebruiker die momenteel is geblokkeerd door uw uitnodigingsbeleid.
 
### <a name="add-an-allow-list"></a>Een lijst voor toestaan toevoegen

Dit is een meer beperkende configuratie, kunt u specifieke domeinen in de acceptatielijst instellen en beperken uitnodigingen voor een andere organisaties of domeinen die niet zijn genoemd. 

Als u wilt een acceptatielijst gebruiken, zorg ervoor dat u tijd besteden aan het volledig evalueren wat uw bedrijf zijn. Als u dit beleid te beperken, kunnen uw gebruikers documenten verzenden via e-mail of andere niet-IT goedgekeurde manieren van samenwerking kiezen.


Een lijst met toegestane toevoegen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** > **gebruikers** > **gebruikersinstellingen**.
3. Onder **externe gebruikers**, selecteer **instellingen voor externe samenwerking beheren**.
4. Onder **samenwerkingsbeperkingen**, selecteer **toestaan dat uitnodigingen alleen voor de opgegeven domeinen (meest restrictief)**.
5. Onder **DOELDOMEINEN**, voer de naam van een van de domeinen die u wilt toestaan. Voer elk domein op een nieuwe regel voor meerdere domeinen. Bijvoorbeeld:

   ![Ziet u de optie toestaan met toegevoegde domeinen](./media/allow-deny-list/AllowListSettings.png)
 
6. Wanneer u klaar bent, klikt u op **opslaan**.

Nadat u het beleid hebt ingesteld als u probeert een gebruiker van een domein dat zich niet op de acceptatielijst uitnodigen, ontvangt u een bericht weergegeven dat het domein van de gebruiker die momenteel is geblokkeerd door uw uitnodigingsbeleid.

### <a name="switch-from-allow-to-deny-list-and-vice-versa"></a>Overschakelen van de lijst en vice versa mogelijk weigeren toestaan 

Als u van de ene naar de andere overschakelt, wordt dit de configuratie van het bestaande beleid verwijderd. Zorg ervoor dat u de back-up van gegevens van uw configuratie voordat u de switch uitvoert. 

## <a name="set-the-allow-or-deny-list-policy-using-powershell"></a>Instellen van het toestaan of weigeren van beleid met behulp van PowerShell

### <a name="prerequisite"></a>Vereiste

Als u wilt instellen van de toestaan of weigeren van lijst met behulp van PowerShell, moet u de preview-versie van de Azure Active Directory-Module voor Windows PowerShell installeren. Specifiek, installeert de AzureADPreview-moduleversie 2.0.0.98 of hoger.

Controleer de versie van de module (en of deze geïnstalleerd):
 
1. Open Windows PowerShell als een verhoogde gebruiker (als Administrator uitvoeren). 
2. Voer de volgende opdracht om te zien als u alle versies van de Azure Active Directory-Module voor Windows PowerShell op uw computer is geïnstalleerd:

   ````powershell  
   Get-Module -ListAvailable AzureAD*
   ````

Als de module niet is geïnstalleerd, of u geen een vereiste versie hebt, een van de volgende handelingen uit:

- Als er geen resultaten worden geretourneerd, voert u de volgende opdracht om de nieuwste versie van de AzureADPreview-module installeren:
  
   ````powershell  
   Install-Module AzureADPreview
   ````
- Als alleen de module AzureAD wordt weergegeven in de resultaten, voer de volgende opdrachten de AzureADPreview-module installeren: 

   ````powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ````
- Als er slechts de AzureADPreview-module wordt weergegeven in de resultaten, maar de versie minder dan 2.0.0.98 is, voer de volgende opdrachten bij te werken: 

   ````powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
   ````

- Als de AzureAD AzureADPreview modules en in de resultaten worden weergegeven, maar de versie van de AzureADPreview-module minder dan 2.0.0.98 is, voer de volgende opdrachten bij te werken: 

   ````powershell 
   Uninstall-Module AzureAD 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
    ````

### <a name="use-the-azureadpolicy-cmdlets-to-configure-the-policy"></a>De AzureADPolicy-cmdlets gebruiken voor het beleid configureren

Gebruik wilt maken van een toestaan of weigeren van de lijst, de [New-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Het volgende voorbeeld ziet hoe u een lijst weigeren die blokkeert het domein 'live.com' instelt.

````powershell 
$policyValue = @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}")

New-AzureADPolicy -Definition $policyValue -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Hieronder ziet u het hetzelfde voorbeeld, maar met de definitie van beleid inline.

````powershell  
New-AzureADPolicy -Definition @("{`"B2BManagementPolicy`":{`"InvitationsAllowedAndBlockedDomainsPolicy`":{`"AllowedDomains`": [],`"BlockedDomains`": [`"live.com`"]}}}") -DisplayName B2BManagementPolicy -Type B2BManagementPolicy -IsOrganizationDefault $true 
````

Als u wilt instellen van het toestaan of weigeren van beleid, gebruikt u de [Set AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/set-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell   
Set-AzureADPolicy -Definition $policyValue -Id $currentpolicy.Id 
````

Als u het beleid, gebruikt de [Get-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell
$currentpolicy = Get-AzureADPolicy | ?{$_.Type -eq 'B2BManagementPolicy'} | select -First 1 
````

Als u wilt verwijderen van het beleid, gebruikt u de [Remove-AzureADPolicy](https://docs.microsoft.com/powershell/module/azuread/remove-azureadpolicy?view=azureadps-2.0-preview) cmdlet. Bijvoorbeeld:

````powershell
Remove-AzureADPolicy -Id $currentpolicy.Id 
````

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure AD B2B [wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- Zie voor meer informatie over voorwaardelijke toegang en B2B-samenwerking [voorwaardelijke toegang voor gebruikers van B2B-samenwerking](conditional-access.md).



