---
title: Vervaldatum voor Office 365-groepen in Azure Active Directory | Microsoft Docs
description: Over het instellen van de vervaldatum voor Office 365-groepen in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 03/09/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 656577211a992432366033326362c9892f7ba6ef
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861701"
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Het vervalbeleid voor Office 365-groepen configureren

U kunt nu de levenscyclus van Office 365-groepen beheren door in te stellen van een verloopbeleid voor hen. U kunt de vervaldatum van het beleid instellen voor alleen Office 365-groepen in Azure Active Directory (Azure AD). 

Wanneer u een groep verlopen instellen:
-   Eigenaars van de groep krijgt een melding om te vernieuwen van de groep als de vervaldatum nadert
-   Een groep die niet wordt verlengd wordt verwijderd
-   Een Office 365-groep die is verwijderd kan door de eigenaren van groepen of door de beheerder binnen 30 dagen worden hersteld

> [!NOTE]
> Configureren en gebruiken van het vervalbeleid voor Office 365-groepen moet u bij de hand Azure AD Premium-licenties hebt voor alle leden van de groepen waarop het beleid wordt toegepast.

Zie voor meer informatie over het downloaden en installeren van de Azure AD PowerShell-cmdlets [Azure Active Directory PowerShell voor Graph 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Rollen en machtigingen
Hier volgen de rollen die u kunnen configureren en gebruiken van de vervaldatum voor Office 365-groepen in Azure AD.

Rol | Machtigingen
-------- | --------
Globale beheerder of beheerder van gebruikersaccounts | Kunt maken, lezen, bijwerken of verwijderen van de beleidsinstellingen voor verlopen van Office 365-groepen<br>Kan Office 365-groep vernieuwen
Gebruiker | Een Office 365-groep waarvan ze eigenaar kunt vernieuwen<br>Een Office 365-groep waarvan ze eigenaar kunt herstellen<br>De vervaldatum kunt beleidsinstellingen lezen

Zie voor meer informatie over machtigingen om terug te zetten van een verwijderde groep [een verwijderde Office 365-groep in Azure Active Directory terugzetten](../fundamentals/active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Vervaldatum van de groep instellen

1. Open de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder in uw Azure AD-tenant.

2. Selecteer **groepen**en selecteer vervolgens **verlopen** om de instellingen van de vervaldatum te openen.
  
  ![Verlopen-blade](./media/groups-lifecycle/expiration-settings.png)

4. Op de **verlopen** blade kunt u:

  * De Groepslevensduur van de in dagen instellen. U kunt een van de vooraf gedefinieerde waarden, of een aangepaste waarde (moet 31 dagen of meer) selecteren. 
  * Geef een e-mailadres waar de vernieuwing en verlopen-meldingen moeten worden verzonden wanneer een groep geen eigenaar heeft. 
  * Selecteer welke Office 365-groepen is verlopen. U kunt vervaldatum inschakelen voor **alle** Office 365-groepen die u kunt kiezen om in te schakelen alleen **geselecteerde** Selecteer Office 365-groepen, of u **geen** om uit te schakelen verlopen voor alle groepen .
  * Uw instellingen opslaan wanneer u klaar bent met het selecteren van **opslaan**.


E-mailmeldingen zoals deze worden verzonden naar de Office 365-groepseigenaren 30 dagen, 15 dagen en 1 dag vóór de vervaldatum van de groep.

![Melding over verlopen e-mailadres](./media/groups-lifecycle/expiration-notification.png)

Uit de **vernieuwen groep** e-mailmelding, groep-eigenaren kunnen rechtstreeks toegang tot de detailpagina van de groep in het toegangsvenster. Meer informatie over de groep, zoals de beschrijving, wanneer deze is laatst vernieuwd wanneer het verloopt en ook de mogelijkheid om te vernieuwen van de groep krijgt er, van de gebruikers. De pagina met details van de groep nu bevat ook koppelingen naar de Office 365-groep resources, zodat de eigenaar van de groep gemakkelijk de inhoud en de activiteit in de groep bekijken kunt.

Wanneer een groep is verlopen, kan de groep één dag na de vervaldatum is verwijderd. Een e-mailmelding zoals deze wordt verzonden naar de beheerder op de vervaldatum en de volgende verwijderen van hun Office 365-groep Eigenaren van de Office 365-groepen.

![E-mailmelding in verwijderen](./media/groups-lifecycle/deletion-notification.png)

De groep binnen 30 dagen na de verwijdering kan worden hersteld door het selecteren van **groep herstellen** of met behulp van PowerShell-cmdlets, zoals beschreven in [een verwijderde Office 365-groep in Azure Active Directory terugzetten](../fundamentals/active-directory-groups-restore-azure-portal.md).
    
Als de groep die u herstellen van wilt documenten, SharePoint-sites of andere permanente objecten bevat, is het duurt maximaal 24 uur aan de groep en de inhoud ervan volledig te herstellen.

> [!NOTE]
> * Wanneer u vervaldatum voor het eerst instelt, worden alle groepen die ouder dan het Vervalinterval opgegeven zijn ingesteld op 30 dagen voordat het vervalt. De eerste vernieuwing meldingse-mail wordt verzonden in een dag. 
>   Bijvoorbeeld, groep A 400 dagen geleden is gemaakt, en het Vervalinterval opgegeven is ingesteld op 180 dagen. Wanneer u instellingen voor verlooptijd toepast, heeft groep A 30 dagen voordat het wordt verwijderd, tenzij de eigenaar van de Hiermee vernieuwt u deze.
> * Op dit moment slechts één verloopbeleid kan worden geconfigureerd voor Office 365-groepen in een tenant.
> * Wanneer een dynamische groep wordt verwijderd en hersteld, is het gezien als een nieuwe groep en opnieuw worden ingevuld op basis van de regel. Dit proces kan tot 24 uur duren.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Hoe de vervaldatum van de Office 365-groep is werkt met een postvak in juridische bewaring
Wanneer een groep is verlopen en wordt verwijderd, klikt u vervolgens 30 dagen na verwijdering van de groep gegevens van apps, Planner, Sites, zoals of Teams worden definitief verwijderd, maar de groepspostvak dat zich op de juridische bewaring worden bewaard en is niet permanent verwijderd. De beheerder kan Exchange-cmdlets gebruiken voor het herstellen van het postvak om op te halen van de gegevens. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>De werking van de vervaldatum van de Office 365-groep met beleid voor Gegevensretentie
Het bewaarbeleid is geconfigureerd in de beveiligings- en Compliancecentrum. Of u hebt een bewaarbeleid voor Office 365-groepen, ingesteld wanneer een groep is verlopen en wordt verwijderd, worden de groepsgesprekken in het groepspostvak en bestanden in de groep site in de container bewaarperiode bewaard voor het specifieke aantal dagen dat is gedefinieerd in de bewaarperiode het beleid. Gebruikers de groep of de inhoud ervan niet weergegeven na de verloopdatum, maar de site en Postvak gegevens via het e-discovery kunnen herstellen.

## <a name="powershell-examples"></a>PowerShell-voorbeelden
Hier volgen enkele voorbeelden van hoe u PowerShell-cmdlets gebruiken kunt voor het configureren van de instellingen voor verlooptijd voor Office 365-groepen in uw tenant:

1. De module voor PowerShell versie 2.0 Preview (2.0.0.137) installeren en meld u aan bij de PowerShell-prompt:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Configureer de instellingen voor verlooptijd New-AzureADMSGroupLifecyclePolicy: met deze cmdlet wordt de levensduur ingesteld voor alle Office 365-groepen in de tenant en 365 dagen. Meldingen voor het vernieuwen voor Office 365 groepen zonder eigenaren wordt verzonden naar 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Ophalen van het bestaande beleid Get-AzureADMSGroupLifecyclePolicy: met deze cmdlet haalt de huidige Office 365-groep vervaldatum instellingen die zijn geconfigureerd. In dit voorbeeld kunt u het volgende zien:
  * De beleids-ID 
  * De levensduur van alle Office 365-groepen in de tenant is ingesteld op 365 dagen
  * Meldingen voor het vernieuwen voor Office 365 groepen zonder eigenaren wordt verzonden naar 'emailaddress@contoso.com.'
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Bijwerken van het bestaande beleid Set-AzureADMSGroupLifecyclePolicy: met deze cmdlet wordt gebruikt om een bestaand beleid. In het onderstaande voorbeeld wordt de Groepslevensduur van de in het bestaande beleid gewijzigd van 365 dagen op 180 dagen. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Specifieke groepen toevoegen aan het beleid Add-AzureADMSLifecyclePolicyGroup: met deze cmdlet een gebruikersgroep wordt toegevoegd aan het lifecycle-beleid. Als u een voorbeeld: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Verwijder het bestaande beleid Remove-AzureADMSGroupLifecyclePolicy: met deze cmdlet verwijdert u de instellingen voor de Office 365 verloopt echter wel vereist dat de beleids-ID. Hiermee wordt de vervaldatum voor Office 365-groepen uitgeschakeld. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
De volgende cmdlets kan worden gebruikt om het beleid configureren in meer detail. Zie voor meer informatie, [PowerShell-documentatie](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

* Get-AzureADMSGroupLifecyclePolicy
* New-AzureADMSGroupLifecyclePolicy
* Get-AzureADMSGroupLifecyclePolicy
* Set-AzureADMSGroupLifecyclePolicy
* Remove-AzureADMSGroupLifecyclePolicy
* Add-AzureADMSLifecyclePolicyGroup
* Remove-AzureADMSLifecyclePolicyGroup
* Reset-AzureADMSLifeCycleGroup   
* Get-AzureADMSLifecyclePolicyGroup

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure AD-groepen.

* [Zie de bestaande groepen](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](../fundamentals/active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)
