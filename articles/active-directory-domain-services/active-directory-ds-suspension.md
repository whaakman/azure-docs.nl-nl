---
title: 'Azure Active Directory Domain Services: Onderbroken domeinen | Microsoft Docs'
description: Beheerde domein onderbreking en verwijdering
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: ergreenl
ms.openlocfilehash: 72e22fbe61b4e30191bbac553709241e1b13f1f5
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502246"
---
# <a name="suspended-domains"></a>Onderbroken domeinen
Wanneer Azure Active Directory Domain Services (Azure AD DS) kan niet worden uitgevoerd een beheerd domein voor een lange periode, worden het beheerde domein in de status onderbroken geplaatst. In dit artikel wordt uitgelegd waarom de beheerde domeinen worden onderbroken en het herstellen van een domein onderbroken.


## <a name="states-your-managed-domain-can-be-in"></a>Statussen uw beheerde domein, kunnen zich in

![Onderbroken domein tijdlijn](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

De voorgaande afbeelding geeft een overzicht van de mogelijke statussen een beheerd domein van Azure AD DS, kan zich in.

### <a name="running-state"></a>Status 'Running'
Een beheerd domein dat correct geconfigureerde en operationele regelmatig wordt de **met** staat.

**Wat u kunt verwachten**
* Microsoft kan de status van uw beheerde domein regelmatig controleren.
* Domeincontrollers voor uw beheerde domein zijn gevuld en regelmatig bijgewerkt.
* Wijzigingen van Azure Active Directory worden regelmatig gesynchroniseerd naar uw beheerde domein.
* Regelmatige back-ups worden genomen voor uw beheerde domein.


### <a name="needs-attention-state"></a>Status 'Aandacht is vereist'
Een beheerd domein is in de **aandacht** status als een of meer problemen vereist. een beheerder actie onderneemt. De health-pagina van uw beheerde domein bevat een of meer waarschuwingen in deze status. 

Bijvoorbeeld, als u een beperkende NSG voor het virtuele netwerk hebt geconfigureerd, Microsoft niet mogelijk om te werken en bewaken van uw beheerde domein. Deze ongeldige configuratie activeert een waarschuwing waarmee uw beheerde domein in de status "Aandacht" wordt geplaatst.

Elke waarschuwing heeft een reeks stappen voor het oplossen. Bepaalde waarschuwingen zijn tijdelijk en automatisch worden opgelost door de service. U kunt bepaalde andere waarschuwingen oplossen door de instructies in de bijbehorende Oplossingsstappen voor deze waarschuwing. Voor bepaalde kritieke waarschuwingen moet u contact op met Microsoft ondersteuning als u een oplossing.

Zie voor meer informatie, [het oplossen van waarschuwingen in een beheerd domein](active-directory-ds-troubleshoot-alerts.md).

**Wat u kunt verwachten**

In sommige gevallen (bijvoorbeeld als u hebt een ongeldige configuratie), kunnen de domeincontrollers voor uw beheerde domein niet bereikbaar zijn. Wanneer uw beheerde domein heeft de status 'Aandacht', wordt Microsoft niet garanderen dat deze wordt bewaakt, gevuld, bijgewerkt, of back-up op gezette tijden.

* Uw beheerde domein is in orde en voortdurende statuscontrole mogelijk niet meer totdat de waarschuwing opgelost is.
* Domeincontrollers voor uw beheerde domein kunnen niet worden hersteld of bijgewerkt.
* Wijzigingen van Azure Active Directory kunnen niet worden gesynchroniseerd met uw beheerde domein.
* Back-ups voor uw beheerde domein kunnen worden uitgevoerd, indien mogelijk.
* Als u waarschuwingen die zijn die invloed hebben op uw beheerde domein hebt opgelost, is het mogelijk dat u deze kunt herstellen naar de status 'Running'.
* Kritieke waarschuwingen worden geactiveerd voor problemen met configuratie waarbij Microsoft is niet bereikbaar van uw domeincontrollers. Als deze waarschuwingen binnen vijftien dagen niet zijn opgelost, wordt uw beheerde domein plaats in de status 'Onderbroken'.


### <a name="the-suspended-state"></a>De status 'Onderbroken'
Een beheerd domein wordt geplaatst de **onderbroken** staat voor de volgende redenen:

* Een of meer kritieke waarschuwingen nog niet zijn opgelost in 15 dagen. Kritieke waarschuwingen kunnen worden veroorzaakt door een onjuiste configuratie die blokkeert de toegang tot resources die nodig zijn voor Azure AD DS.
    * Bijvoorbeeld, de waarschuwing [AADDS104: netwerkfout](active-directory-ds-troubleshoot-nsg.md) heeft is niet opgelost gedurende meer dan 15 dagen in het beheerde domein.
* Er is een factureringsprobleem met uw Azure-abonnement of uw Azure-abonnement is verlopen.

Beheerde domeinen worden onderbroken als niet te beheren, controleren, patch of back-up van het domein regelmatig door Microsoft worden.

**Wat u kunt verwachten**
* Domeincontrollers voor uw beheerde domein niet zijn ingericht en zijn niet bereikbaar is binnen het virtuele netwerk.
* Toegang van Secure LDAP aan het beheerde domein via internet (als deze ingeschakeld) werkt niet.
* U ziet fouten in de verificatie bij het beheerde domein, zich aanmelden op virtuele machines van domein of met elkaar verbinden via LDAP/LDAPS.
* Back-ups voor uw beheerde domein worden niet meer genomen.
* Hiermee stopt u de synchronisatie met Azure AD.

Als u de waarschuwing opgelost, wordt uw beheerde domein in de status 'Onderbroken'. Vervolgens moet u contact op met ondersteuning.
Ondersteuning voor uw beheerde domein kan worden hersteld, maar alleen als een back-up die is minder dan 30 dagen bestaat.

Het beheerde domein wordt alleen de status onderbroken blijft 15 dagen. Als u wilt herstellen van uw beheerde domein, wordt aangeraden dat u de kritieke waarschuwingen onmiddellijk omzetten.


### <a name="deleted-state"></a>'Verwijderd' status
Een beheerd domein blijft in de status 'Onderbroken' 15 dagen is **verwijderde**.

**Wat u kunt verwachten**
* Alle resources en back-ups voor het beheerde domein worden verwijderd.
* U kan niet herstellen van het beheerde domein en moet maken van een nieuwe beheerde domein voor het gebruik van Azure AD DS.
* Nadat deze verwijderd, worden niet in rekening gebracht voor het beheerde domein.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Hoe weet u als uw beheerde domein wordt onderbroken?
U ziet een [waarschuwing](active-directory-ds-troubleshoot-alerts.md) op de pagina Azure status van de AD DS in Azure portal waarmee u verklaart dat het domein is onderbroken. Ook ziet u de status van het domein 'Onderbroken'.


## <a name="restore-a-suspended-domain"></a>Herstellen van een onderbroken domein
Voor het herstellen van een domein met de status 'Onderbroken', moet u de volgende stappen uitvoeren:

1. Ga naar de [Azure Active Directory Domain Services-pagina](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal.
2. Selecteer het beheerde domein.
3. Selecteer in het linkerdeelvenster **Health**.
4. Selecteer de waarschuwing. De waarschuwings-ID wordt AADDS503 of AADDS504, zijn afhankelijk van de oorzaak van onderbreking.
5. Selecteer de koppeling oplossing die opgegeven in de waarschuwing. Volg de stappen om de waarschuwing te verhelpen.

Uw beheerde domein kan alleen worden teruggezet naar de datum van de laatste back-up. De datum van de laatste back-up wordt weergegeven op de pagina status van uw beheerde domein. Eventuele wijzigingen die zijn opgetreden nadat de laatste back-up wordt niet worden hersteld. Back-ups voor een beheerd domein worden maximaal 30 dagen opgeslagen. Back-ups die ouder dan 30 dagen zijn zijn verwijderd.


## <a name="next-steps"></a>Volgende stappen
- [Los waarschuwingen voor uw beheerde domein](active-directory-ds-troubleshoot-alerts.md)
- [Lees meer over Azure Active Directory Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).
