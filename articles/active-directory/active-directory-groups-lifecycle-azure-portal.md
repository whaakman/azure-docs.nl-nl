---
title: Verlooptijd voor Office 365-groepen in Azure Active Directory | Microsoft Docs
description: Het instellen van de vervaldatum voor Office 365-groepen in Azure Active Directory
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
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 1e500be80374edd4d2f53fd2f0604e4a63f640fc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="configure-the-expiration-policy-for-office-365-groups"></a>Het vervalbeleid voor Office 365-groepen configureren

Een verloopbeleid voor deze instelling kunt u nu de levenscyclus van Office 365-groepen beheren. U kunt verloopbeleid instellen voor alleen Office 365-groepen in Azure Active Directory (Azure AD). 

Wanneer u een groep verloopt instellen:
-   Eigenaars van de groep worden gewaarschuwd voor het vernieuwen van de groep omdat de vervaldatum bijna
-   Een groep die niet wordt verlengd wordt verwijderd
-   Een Office 365-groep die is verwijderd kan door de eigenaars Groepsbeleid of door de beheerder binnen 30 dagen worden hersteld

> [!NOTE]
> Configureren en gebruiken van het verloopbeleid voor Office 365-groepen, moet u voorhanden Azure AD Premium-licenties hebben voor alle leden van de groepen waarop het verloopbeleid wordt toegepast.

Zie voor meer informatie over het downloaden en installeren van de Azure AD PowerShell-cmdlets [Azure Active Directory PowerShell voor een grafiek 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="roles-and-permissions"></a>Rollen en machtigingen
Hier volgen de functies die u kunnen configureren en gebruiken van de vervaldatum voor Office 365-groepen in Azure AD.

Rol | Machtigingen
-------- | --------
Globale beheerder of gebruiker accountbeheerder | Kunt maken, lezen, bijwerken of verwijderen van de beleidsinstellingen voor verlopen van Office 365-groepen<br>Een Office 365-groep kunnen vernieuwen
Gebruiker | Een Office 365-groep waarvan ze eigenaar kunnen vernieuwen<br>Een Office 365-groep waarvan ze eigenaar kunt herstellen<br>De vervaldatum kunnen beleidsinstellingen lezen

Zie voor meer informatie over machtigingen voor het herstellen van een verwijderde groep [een verwijderde Office 365-groep in Azure Active Directory herstellen](active-directory-groups-restore-azure-portal.md).

## <a name="set-group-expiration"></a>Verlopen van de set-groep

1. Open de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder in uw Azure AD-tenant.

2. Selecteer **groepen**, selecteer daarna **verlopen** om de verloopdatum-instellingen te openen.
  
  ![Blade verlopen](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Op de **verlopen** blade kunt u:

  * De levensduur van de groep in dagen ingesteld. U kunt een van de vooraf gedefinieerde waarden, of een aangepaste waarde (moet 31 dagen of meer) selecteren. 
  * Geef een e-mailadres waar de vernieuwing en verlopen-meldingen moeten worden verzonden wanneer een groep geen eigenaar heeft. 
  * Selecteer welke groepen door Office 365 verloopt. Kunt u de vervaldatum voor **alle** Office 365-groepen die u kunt kiezen om in te schakelen alleen **geselecteerde** Office 365-groepen, of u selecteert **geen** verlopen voor alle groepen uitschakelen .
  * Uw instellingen opslaan als u bent met het selecteren van klaar **opslaan**.


E-mailmeldingen zoals deze zijn verzonden naar de Office 365-eigenaars Groepsbeleid 30 dagen, 15 dagen en 1 dag vóór de vervaldatum van de groep.

![Verlopen van e-mailmeldingen](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Van de **vernieuwen groep** e-mailmelding, groep eigenaren kunnen rechtstreeks toegang tot de detailpagina groep in het deelvenster toegang. Meer informatie over de groep, zoals de beschrijving, wanneer deze is laatst vernieuwd wanneer het verloopt en ook de mogelijkheid voor het vernieuwen van de groep krijgt daar van de gebruikers. De detailpagina groep nu bevat ook koppelingen naar de resources van de Office 365-groep, zodat de Groepseigenaar gemakkelijk de inhoud en activiteit in de groep weergeven kunt.

Wanneer een groep is verlopen, kan de groep één dag na de vervaldatum is verwijderd. Een e-mailmelding zoals deze wordt verzonden naar de Office 365-groepseigenaren waarin ze worden geïnformeerd over de vervaldatum en de volgende verwijdering van hun Office 365-groep.

![E-mailmeldingen in verwijderen](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

De groep kan worden hersteld binnen 30 dagen na het verwijderen door het selecteren van **groepsbeleidsobject herstellen** of met behulp van PowerShell-cmdlets, zoals beschreven in [een verwijderde Office 365-groep in Azure Active Directory herstellen](active-directory-groups-restore-azure-portal.md).
    
Als de groep die u terugzet documenten, SharePoint-sites of andere permanente objecten bevat, kan deze volledig herstellen van de groep en de inhoud ervan tot 24 uur duren.

> [!NOTE]
> * Wanneer u eerst een vervaldatum ingesteld, worden de groepen die ouder dan het Vervalinterval opgegeven zijn ingesteld op 30 dagen tot vervaldatum. De eerste verlenging meldingse-mail wordt verzonden binnen een dag. 
>   Bijvoorbeeld, groep A 400 dagen geleden is gemaakt en het Vervalinterval opgegeven is ingesteld op 180 dagen. Wanneer u instellingen voor verlooptijd toepast, heeft groep A 30 dagen voordat deze wordt verwijderd, tenzij de eigenaar deze vernieuwt.
> * Op dit moment slechts één verloopbeleid kan worden geconfigureerd voor Office 365-groepen op een tenant.
> * Wanneer een dynamische groep is verwijderd en hersteld, wordt gezien als een nieuwe groep en opnieuw volgens de regel is ingevuld. Dit proces kan 24 uur duren.

## <a name="how-office-365-group-expiration-works-with-a-mailbox-on-legal-hold"></a>Hoe de vervaldatum voor Office 365-groep is werkt met een postvak in wettelijke bewaring
Wanneer een groep is verlopen en wordt verwijderd, klikt u vervolgens 30 dagen na het verwijderen van de groep gegevens van apps, zoals Planner, Sites, of Teams wordt definitief verwijderd, maar het groepspostvak die in juridische bewaring wordt behouden en wordt niet permanent verwijderd. De beheerder kan Exchange-cmdlets gebruiken voor het postvak voor het ophalen van de gegevens herstellen. 

## <a name="how-office-365-group-expiration-works-with-retention-policy"></a>De werking van verlopen van Office 365-groep met bewaarbeleid
Het bewaarbeleid is geconfigureerd in de beveiliging en naleving Center. Als u hebt een bewaarbeleid voor Office 365-groepen ingesteld wanneer een groep is verlopen en wordt verwijderd, de conversaties groep in het groepspostvak en bestanden in de groep site blijven behouden in de container voor de bewaarperiode voor het specifieke aantal dagen dat is gedefinieerd in de bewaarperiode het beleid. Gebruikers zien de groep of de inhoud na de verloopdatum, maar de site en Postvak gegevens via het e-detectie kunnen herstellen.

## <a name="powershell-examples"></a>PowerShell-voorbeelden
Hier volgen enkele voorbeelden van hoe u PowerShell-cmdlets gebruiken kunt voor het configureren van de instellingen voor verlooptijd voor Office 365-groepen in uw tenant:

1. De module PowerShell v2.0 Preview (2.0.0.137) installeren en meld u aan bij de PowerShell-prompt:
  ````
  Install-Module -Name AzureADPreview
  connect-azuread 
  ````
2. Configureer de instellingen voor verlooptijd nieuw AzureADMSGroupLifecyclePolicy: deze cmdlet wordt de levensduur ingesteld voor alle Office 365-groepen in de tenant en 365 dagen. Vernieuwing meldingen voor Office 365-groepen zonder eigenaar wordt verzonden naar 'emailaddress@contoso.com'
  
  ````
  New-AzureADMSGroupLifecyclePolicy -GroupLifetimeInDays 365 -ManagedGroupTypes All -AlternateNotificationEmails emailaddress@contoso.com
  ````
3. Ophalen van het bestaande beleid Get-AzureADMSGroupLifecyclePolicy: deze cmdlet haalt de huidige Office 365 verloopt groepsinstellingen die zijn geconfigureerd. In dit voorbeeld kunt u het volgende zien:
  * De beleids-ID 
  * De levensduur voor alle Office 365-groepen in de tenant is ingesteld op 365 dagen
  * Vernieuwing meldingen voor Office 365-groepen zonder eigenaar wordt verzonden naar 'emailaddress@contoso.com.'
  
  ````
  Get-AzureADMSGroupLifecyclePolicy
  
  ID                                    GroupLifetimeInDays ManagedGroupTypes AlternateNotificationEmails
  --                                    ------------------- ----------------- ---------------------------
  26fcc232-d1c3-4375-b68d-15c296f1f077  365                 All               emailaddress@contoso.com
  ```` 
   
4. Bijwerken van het bestaande beleid Set AzureADMSGroupLifecyclePolicy: deze cmdlet wordt gebruikt om een bestaand beleid. In het volgende voorbeeld wordt de levensduur van de groep in het bestaande beleid gewijzigd van 365 dagen op 180 dagen. 
  
  ````
  Set-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”   -GroupLifetimeInDays 180 -AlternateNotificationEmails "emailaddress@contoso.com"
  ````
  
5. Specifieke groepen toevoegen aan het beleid toevoegen AzureADMSLifecyclePolicyGroup: deze cmdlet wordt een groep aan het lifecycle-beleid. Als u een voorbeeld: 
  
  ````
  Add-AzureADMSLifecyclePolicyGroup -Id “26fcc232-d1c3-4375-b68d-15c296f1f077” -groupId "cffd97bd-6b91-4c4e-b553-6918a320211c"
  ````
  
6. Verwijder het bestaande beleid Remove-AzureADMSGroupLifecyclePolicy: deze cmdlet verwijdert u de instellingen van de Office 365 verloopt, maar de beleid-id vereist. Hiermee wordt de vervaldatum voor Office 365-groepen uitgeschakeld. 
  
  ````
  Remove-AzureADMSGroupLifecyclePolicy -Id “26fcc232-d1c3-4375-b68d-15c296f1f077”
  ````
  
De volgende cmdlets kunnen worden gebruikt voor het beleid configureren in meer detail. Zie voor meer informatie [PowerShell documentatie](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview&branch=master#groups).

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

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
