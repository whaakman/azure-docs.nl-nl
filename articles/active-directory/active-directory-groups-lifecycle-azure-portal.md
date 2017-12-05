---
title: Verlooptijd voor Office 365-groepen in Azure Active Directory | Microsoft Docs
description: Het instellen van de vervaldatum voor Office 365-groepen in Azure Active Directory (preview)
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: c2dd56bd34e5b7845298fab1f36e231113a2e28e
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Verlooptijd voor Office 365-groepen (preview) configureren

Verlopen van functies voor deze instelling kunt u nu de levenscyclus van Office 365-groepen beheren. U kunt de vervaldatum voor Office 365-groepen instellen in Azure Active Directory (Azure AD). Wanneer u een groep verloopt instellen:
-   Eigenaars van de groep worden gewaarschuwd voor het vernieuwen van de groep omdat de vervaldatum bijna
-   Een groep die niet wordt verlengd wordt verwijderd
-   Een Office 365-groep die is verwijderd kan door de eigenaars Groepsbeleid of door de beheerder binnen 30 dagen worden hersteld

> [!NOTE]
> Vervaldatum voor Office 365-groepen moet een Azure AD Premium-licentie of een Azure AD Basic EDU voor alle leden van de groepen waartoe de vervaldatum instellingen worden toegepast.
> 
> Voor Azure AD Basic EDU in licentie klanten gegeven: voor dit beleid voor het eerst configureert, de Azure Active Directory PowerShell-cmdlets gebruiken. Daarna kunt u de instellingen voor verlooptijd met PowerShell of Azure AD-portal met een account dat een gebruiker accountbeheerder of de globale beheerder in uw Azure AD-tenant bijwerken.

Zie voor meer informatie over het downloaden en installeren van de Azure AD PowerShell-cmdlets [Azure Active Directory PowerShell voor Graph - openbare Preview-versie 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="set-group-expiration"></a>Verlopen van de set-groep

1. Open de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder in uw Azure AD-tenant.

2. Open Azure AD, selecteer **gebruikers en groepen**.

3. Selecteer **groepsinstellingen** en selecteer vervolgens **verlopen** om de verloopdatum-instellingen te openen.
  
  ![Blade verlopen](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Op de **verlopen** blade kunt u:

  * De levensduur van de groep in dagen ingesteld. U kunt een van de vooraf gedefinieerde waarden, of een aangepaste waarde (moet 31 dagen of meer) selecteren. 
  * Geef een e-mailadres waar de vernieuwing en verlopen-meldingen moeten worden verzonden wanneer een groep geen eigenaar heeft. 
  * Selecteer welke groepen door Office 365 verloopt. U kunt de vervaldatum voor inschakelen **alle** Office 365-groepen, kunt u een van de Office 365-groepen selecteren of u selecteert **geen** verlopen voor alle groepen uitschakelen.
  * Uw instellingen opslaan als u bent met het selecteren van klaar **opslaan**.


E-mailmeldingen zoals deze zijn verzonden naar de Office 365-eigenaars Groepsbeleid 30 dagen, 15 dagen en 1 dag vóór de vervaldatum van de groep.

![Verlopen van e-mailmeldingen](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

De Groepseigenaar kunt selecteren **vernieuwen groep** voor het vernieuwen van de Office 365 groep, of kunt selecteren **gaat u naar de groep** om te zien van de leden en andere details over de groep.

Wanneer een groep is verlopen, kan de groep één dag na de vervaldatum is verwijderd. Een e-mailmelding zoals deze wordt verzonden naar de Office 365-groepseigenaren waarin ze worden geïnformeerd over de vervaldatum en de volgende verwijdering van hun Office 365-groep.

![E-mailmeldingen in verwijderen](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

De groep kan worden hersteld door het selecteren van **groepsbeleidsobject herstellen** of met behulp van PowerShell-cmdlets, zoals beschreven in [terugzetten van een verwijderde Office 365-groep in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-Groups-Restore-Azure-Portal).
    
Als de groep die u terugzet documenten, SharePoint-sites of andere permanente objecten bevat, kan deze volledig herstellen van de groep en de inhoud ervan tot 24 uur duren.

> [!NOTE]
> * Wanneer u eerst een vervaldatum ingesteld, worden de groepen die ouder dan het Vervalinterval opgegeven zijn ingesteld op 30 dagen tot vervaldatum. De eerste verlenging meldingse-mail wordt verzonden binnen een dag. 
>   Bijvoorbeeld, groep A 400 dagen geleden is gemaakt en het Vervalinterval opgegeven is ingesteld op 180 dagen. Wanneer u instellingen voor verlooptijd toepast, heeft groep A 30 dagen voordat deze wordt verwijderd, tenzij de eigenaar deze vernieuwt.
> * Wanneer een dynamische groep is verwijderd en hersteld, wordt gezien als een nieuwe groep en opnieuw volgens de regel is ingevuld. Dit proces kan 24 uur duren.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure AD-groepen.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
