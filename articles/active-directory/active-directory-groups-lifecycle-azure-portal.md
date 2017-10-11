---
title: Voorbeeld bekijken van verlopen van Office 365-groepen in Azure Active Directory | Microsoft Docs
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
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8a43df84fd050d7b4bd8d937b8c55e744cb805d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a>Verlooptijd van Office 365-groepen (preview) configureren

U kunt nu de levenscyclus van Office 365-groepen beheren door de vervaldatum voor Office 365-groepen die u selecteert. Zodra deze verlooptijd is ingesteld, worden eigenaars van die groepen gevraagd voor het vernieuwen van de groepen als ze nog steeds de groepen nodig. Een Office 365-groep die niet wordt verlengd worden, verwijderd. Een Office 365-groep is verwijderd, kan door de eigenaars Groepsbeleid of door de beheerder binnen 30 dagen worden hersteld.  


> [!NOTE]
> U kunt instellen dat verlopen voor Office 365-groepen.
>
> Vervaldatum voor O365 groepen is vereist dat een Azure AD Premium-licentie is toegewezen aan
>   - De beheerder de instellingen van de vervaldatum voor de tenant configureert
>   - Alle leden van de groepen die zijn geselecteerd voor deze instelling

## <a name="set-office-365-groups-expiration"></a>Instellen dat verlopen voor Office 365-groepen

1. Open de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder in uw Azure AD-tenant.

2. Open Azure AD, selecteer **gebruikers en groepen**.

3. Selecteer **groepsinstellingen** en selecteer vervolgens **verlopen** om de verloopdatum-instellingen te openen.
  
  ![Blade verlopen](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Op de **verlopen** blade kunt u:

  * De levensduur van de groep in dagen ingesteld. U kunt een van de vooraf gedefinieerde waarden, of een aangepaste waarde (moet 31 dagen of meer) selecteren. 
  * Geef een e-mailadres waar de vernieuwing en verlopen-meldingen moeten worden verzonden wanneer een groep geen eigenaar heeft. 
  * Selecteer welke groepen door Office 365 verloopt. U kunt de vervaldatum voor inschakelen **alle** Office 365-groepen, kunt u een van de Office 365-groepen selecteren of u selecteert **geen** verlopen voor alle groepen uitschakelen.
  * Uw instellingen opslaan als u bent met het selecteren van klaar **opslaan**.

Zie voor instructies over het downloaden en installeren van de Microsoft PowerShell-module voor het configureren van de vervaldatum voor Office 365-groepen via PowerShell [Azure Active Directory V2 PowerShell-Module - openbare Preview-versie 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

E-mailmeldingen zoals deze zijn verzonden naar de Office 365-eigenaars Groepsbeleid 30 dagen, 15 dagen en 1 dag vóór de vervaldatum van de groep.

![Verlopen van e-mailmeldingen](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

De Groepseigenaar kunt selecteren **vernieuwen groep** voor het vernieuwen van de Office 365 groep, of kunt selecteren **gaat u naar de groep** om te zien van de leden en andere details over de groep.

Wanneer een groep is verlopen, kan de groep één dag na de vervaldatum is verwijderd. Een e-mailmelding zoals deze wordt verzonden naar de Office 365-groepseigenaren waarin ze worden geïnformeerd over de vervaldatum en de volgende verwijdering van hun Office 365-groep.

![E-mailmeldingen in verwijderen](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

De groep kan worden hersteld door het selecteren van **groepsbeleidsobject herstellen** of met behulp van PowerShell-cmdlets, zoals beschreven in [terugzetten van een verwijderde Office 365-groep in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-Groups-Restore-Azure-Portal).
    
Als de groep die u terugzet documenten, SharePoint-sites of andere permanente objecten bevat, kan deze volledig herstellen van de groep en de inhoud ervan tot 24 uur duren.

> [!NOTE]
> * Wanneer u de instellingen voor verlooptijd implementeert, is er mogelijk bepaalde groepen die ouder dan het venster verloopt zijn. Deze groepen worden niet onmiddellijk verwijderd, maar zijn ingesteld op 30 dagen tot vervaldatum. De eerste verlenging meldingse-mail wordt verzonden binnen een dag. Bijvoorbeeld, groep A 400 dagen geleden is gemaakt en het Vervalinterval opgegeven is ingesteld op 180 dagen. Wanneer u instellingen voor verlooptijd toepast, heeft groep A 30 dagen voordat deze wordt verwijderd, tenzij de eigenaar deze vernieuwt.
> * Wanneer een dynamische groep is verwijderd en hersteld, wordt gezien als een nieuwe groep en opnieuw volgens de regel is ingevuld. Dit proces kan 24 uur duren.

## <a name="next-steps"></a>Volgende stappen
Deze artikelen bevatten aanvullende informatie over Azure AD-groepen.

* [Zie de bestaande groepen](active-directory-groups-view-azure-portal.md)
* [Instellingen van een groep beheren](active-directory-groups-settings-azure-portal.md)
* [Leden van een groep beheren](active-directory-groups-members-azure-portal.md)
* [Lidmaatschap van een groep beheren](active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](active-directory-groups-dynamic-membership-azure-portal.md)
