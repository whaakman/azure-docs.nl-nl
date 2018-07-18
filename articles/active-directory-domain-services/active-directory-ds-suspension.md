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
ms.topic: article
ms.date: 07/16/2018
ms.author: ergreenl
ms.openlocfilehash: 5f350bacdc21ec4e5077c93ecc9d97f2fe6c39a5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090906"
---
# <a name="suspended-domains"></a>Onderbroken domeinen
Als Azure AD Domain Services kan niet worden uitgevoerd een beheerd domein voor een lange periode is, wordt het beheerde domein in een onderbroken status geplaatst. In dit artikel wordt uitgelegd waarom de beheerde domeinen worden onderbroken, de lengte van onderbreking en het herstellen van een domein onderbroken.


## <a name="states-your-managed-domain-can-be-in"></a>Statussen uw beheerde domein, kunnen zich in

![Onderbroken domein tijdlijn](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

De voorgaande afbeelding geeft een overzicht van hoe een domein is onderbroken, hoe lang het wordt opgeschort en uiteindelijk de verwijdering van een beheerd domein. De volgende secties bevatten informatie over de redenen waarom een domein kan worden onderbroken en hoe u de toegang van een beheerd domein herstellen.

### <a name="running-state"></a>Status 'Actief'
Een beheerd domein dat correct geconfigureerde en operationele regelmatig wordt de **met** staat.

**Wat u kunt verwachten:**
* Microsoft kan regelmatig controleren of de status van uw beheerde domein.
* Domeincontrollers voor uw beheerde domein zijn gevuld en regelmatig bijgewerkt.
* Wijzigingen van Azure Active Directory worden regelmatig gesynchroniseerd naar uw beheerde domein.
* Regelmatige back-ups worden genomen voor uw beheerde domein.


### <a name="needs-attention-state"></a>Status 'Aandacht is vereist'
Een beheerd domein is in de **aandacht** staat, als een of meer problemen vereist. een beheerder actie onderneemt. De health-pagina van uw beheerde domein een of meer waarschuwingen in deze status wordt een lijst. Bijvoorbeeld, als u een beperkende NSG voor het virtuele netwerk hebt geconfigureerd, Microsoft mogelijk niet om te werken en bewaken van uw beheerde domein. Deze configuratie ongeldig resulteert in een waarschuwing wordt gegenereerd en uw beheerde domein wordt geplaatst in de status van uw aandacht nodig heeft.

Elke waarschuwing heeft een reeks stappen voor het oplossen. Sommige waarschuwingen tijdelijk zijn en zal automatisch worden opgelost door de service. U kunt bepaalde andere waarschuwingen oplossen door de instructies in de bijbehorende Oplossingsstappen voor deze waarschuwing. Voor bepaalde kritieke waarschuwingen oplossen, moet u contact op met Microsoft ondersteuning.

Zie voor meer informatie, [het oplossen van waarschuwingen in een beheerd domein](active-directory-ds-troubleshoot-alerts.md).

**Wat u kunt verwachten:**

In sommige gevallen (bijvoorbeeld als u hebt een ongeldige configuratie), de domeincontrollers voor uw beheerde domein niet bereikbaar. Daarom, garandeert Microsoft niet uw beheerde domein wordt bewaakt, gevuld, bijgewerkt of back-up op een geregelde in deze status.

* Uw beheerde domein is in orde en voortdurende statuscontrole wordt mogelijk beïnvloed, totdat de waarschuwing opgelost is.
* Domeincontrollers voor uw beheerde domein kunnen niet worden hersteld of bijgewerkt.
* Wijzigingen van Azure Active Directory kunnen niet worden gesynchroniseerd met uw beheerde domein.
* Back-ups voor uw beheerde domein kunnen worden genomen, indien mogelijk.
* Als u de waarschuwingen die invloed hebben op uw beheerde domein hebt opgelost, kan het mogelijk zijn om te herstellen van uw beheerde domein naar de status 'Running'.
* Kritieke waarschuwingen worden geactiveerd voor problemen met configuratie waarbij Microsoft is niet bereikbaar van uw domeincontrollers. Als deze waarschuwingen niet binnen vijftien dagen worden opgelost, wordt uw beheerde domein in de status 'Onderbroken' worden geplaatst.


### <a name="suspended-state"></a>'Onderbroken' status
Een beheerd domein wordt geplaatst de **onderbroken** staat voor de volgende redenen:
* Een of meer kritieke waarschuwingen zijn in 15 dagen niet opgelost. Dit kan worden veroorzaakt door een onjuiste configuratie die blokkeert de toegang tot resources die nodig zijn voor Azure AD Domain Services.
    * Bijvoorbeeld, als het beheerde domein heeft waarschuwing [AADDS104: netwerkfout](active-directory-ds-troubleshoot-nsg.md) niet meer dan 15 dagen worden opgelost.
* Er is een factureringsprobleem met uw Azure-abonnement of als het Azure-abonnement is verlopen.

Beheerde domeinen worden onderbroken als niet te beheren, controleren, patch of back-up van het domein regelmatig door Microsoft worden.

**Wat u kunt verwachten:**
* Domeincontrollers voor uw beheerde domein niet zijn ingericht en zijn niet bereikbaar is binnen het virtuele netwerk.
* Toegang van Secure LDAP aan het beheerde domein via internet (indien ingeschakeld) werkt niet.
* U ziet fouten in de verificatie bij het beheerde domein aanmelden bij domein verbonden virtuele machines verbinding maken via LDAP/LDAPS enzovoort.
* Back-ups voor uw beheerde domein worden niet meer genomen.
* U moet oplossen van de waarschuwing veroorzaakt door uw beheerde domein worden in de status 'Onderbroken' en vervolgens contact op met ondersteuning.
* Ondersteuning kan mogelijk om terug te zetten van uw beheerde domein alleen als er een bestaande back-up die is minder dan 30 dagen.


### <a name="deleted-state"></a>'Verwijderd' status
Een beheerd domein blijft in de status 'Onderbroken' 15 dagen is **verwijderde**.

**Wat u kunt verwachten:**
* Alle resources en back-ups voor het beheerde domein worden verwijderd.
* U het beheerde domein niet kan herstellen en moet een nieuwe beheerde domein voor het gebruik van Azure AD Domain Services maken.
* U, wordt niet in rekening gebracht voor het beheerde domein.


## <a name="what-happens-when-a-managed-domain-is-suspended"></a>Wat gebeurt er wanneer een beheerd domein wordt onderbroken?
Als een domein is onderbroken, wordt Azure AD Domain Services wordt gestopt en ongedaan maken de domeincontrollers voor uw beheerde domein wordt ingericht. Als gevolg hiervan, back-ups niet meer worden genomen, gebruikers kunnen geen om aan te melden bij uw domein en synchronisatie met Azure AD wordt gestopt.

Het beheerde domein blijft alleen de status onderbroken voor een maximum van 15 dagen. Om ervoor te zorgen een tijdig herstel, wordt aangeraden dat u de onderbreking zo snel mogelijk kunt oplossen.


## <a name="how-do-i-know-if-my-managed-domain-is-suspended"></a>Hoe weet ik als mijn beheerde domein wordt onderbroken?
U ziet een [waarschuwing](active-directory-ds-troubleshoot-alerts.md) op de pagina status van Azure AD Domain Services in Azure portal met het domein is onderbroken. Bovendien ziet u de status van het domein 'Onderbroken'.


## <a name="how-do-i-restore-a-suspended-domain"></a>Hoe herstel ik een onderbroken domein?
Voor het herstellen van een domein in de status 'Onderbroken', voert u de volgende stappen uit:

1. Navigeer naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal
2. Klik op het domein dat u wilt toegang herstellen
3. Klik op de navigatiebalk links **Health**
4. Klik op de waarschuwing. De waarschuwings-ID wordt AADDS503 of AADDS504, zijn afhankelijk van de oorzaak van onderbreking.
5. Klik op de koppeling oplossing in de waarschuwing en volg de stappen om de waarschuwing te verhelpen.

Uw domein kan alleen worden teruggezet naar de datum van laatste back-up. De datum van de laatste back-up wordt weergegeven op de pagina status van uw beheerde domein. Alle wijzigingen die is opgetreden nadat de laatste back-up wordt niet hersteld. Back-ups voor een beheerd domein worden maximaal 30 dagen opgeslagen. Back-ups ouder dan 30 dagen zijn verwijderd.


## <a name="deleting-domains"></a>Domeinen verwijderen
Als het domein is onderbroken voor meer dan 15 dagen, wordt het beheerde domein vanwege inactiviteit en de modeldatabase groter voor het domein in Azure AD Domain Services verwijderd. U wordt niet meer gefactureerd voor Azure AD Domain Services. U kunt geen op dit moment herstellen van uw beheerde domein en moet deze opnieuw maken.


## <a name="next-steps"></a>Volgende stappen
- [Los waarschuwingen op uw beheerde domein](active-directory-ds-troubleshoot-alerts.md)
- [Lees meer over Azure AD Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).
