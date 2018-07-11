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
ms.date: 07/09/2018
ms.author: ergreenl
ms.openlocfilehash: 37524fbdf3cd521414a507f6fb67421708343ccd
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934246"
---
# <a name="suspended-domains"></a>Onderbroken domeinen
Als Azure AD Domain Services kan niet worden uitgevoerd een beheerd domein voor een lange periode is, wordt het beheerde domein in een onderbroken status geplaatst. In dit artikel wordt uitgelegd waarom de beheerde domeinen worden onderbroken, de lengte van onderbreking en het herstellen van een domein onderbroken.


## <a name="overview-of-suspended-domains"></a>Overzicht van onderbroken domeinen

![Onderbroken domein tijdlijn](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

De voorgaande afbeelding geeft een overzicht van hoe een domein is onderbroken, hoe lang het wordt opgeschort en uiteindelijk de verwijdering van een beheerd domein. De volgende secties bevatten informatie over de redenen waarom een domein kan worden onderbroken en hoe u de toegang van een beheerd domein herstellen.


## <a name="why-are-managed-domains-suspended"></a>Waarom kan ik beheerde domeinen onderbroken?

Beheerde domeinen worden onderbroken wanneer ze zich in een status waarin Azure AD Domain Services kan niet voor het beheren van het domein is. Dit kan worden veroorzaakt door een onjuiste configuratie die blokkeert de toegang tot resources die nodig zijn voor Azure AD Domain Services of een niet-actief Azure-abonnement. Na 15 dagen worden kan niet worden uitgevoerd een beheerd domein, wordt Azure AD Domain Services onderbreken het domein.

Hieronder vindt u de precieze redenen waarom uw domein kan worden onderbroken:
* Een of meer van de volgende waarschuwingen die aanwezig zijn hebben in uw domein gedurende 15 opeenvolgende dagen:
   * [AADDS104: Netwerkfout](active-directory-ds-troubleshoot-nsg.md).
* Uw Azure-abonnement is verlopen of niet actief


## <a name="what-happens-when-a-domain-is-suspended"></a>Wat gebeurt er wanneer een domein wordt onderbroken?

Wanneer een domein is onderbroken, wordt in Azure AD Domain Services de virtuele machines die uw beheerde domein-service gestopt. Dit betekent dat back-ups niet meer worden genomen, gebruikers kunnen geen om aan te melden bij uw domein en niet langer de synchronisatie met Azure AD wordt uitgevoerd.

Het domein blijven alleen de status van een onderbreking voor een maximum van 15 dagen. Om ervoor te zorgen een tijdig herstel, wordt aangeraden dat u de onderbreking zo snel mogelijk kunt oplossen.

## <a name="how-do-i-know-if-my-domain-is-suspended"></a>Hoe weet ik als mijn domein wordt onderbroken?
Het beheerde domein ontvangt een [waarschuwing](active-directory-ds-troubleshoot-alerts.md) op de pagina status van Azure AD Domain Services in Azure portal met het domein is onderbroken. Bovendien wordt dat de status van het domein 'Onderbroken' worden gelabeld.


## <a name="unsuspending-and-restoring-domains"></a>Domeinen unsuspending en terugzetten

Als u wilt toegang met het herstellen van een domein, worden de volgende stappen nodig:

1. Navigeer naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal
2. Klik op het domein dat u wilt toegang herstellen
3. Klik op de navigatiebalk links **Health**
4. Klik op de onderbreking waarschuwing (de waarschuwings-ID worden AADDS503 of AADDS504, afhankelijk van de oorzaak van onderbreking).
5. Klik op de koppeling oplossing in de waarschuwing en volg de stappen voor het oplossen van uw onderbreking.

Uw domein kan alleen worden hersteld vanaf de datum van laatste back-up. De datum van de laatste back-up wordt weergegeven op de pagina status van uw beheerde domein. Wijzigingen sinds de laatste back-up wordt niet hersteld na unsuspension. Azure AD Domain Services kunnen bovendien alleen back-ups opslaan voor 30 dagen. Als de meest recente back-up meer dan 30 dagen is, de back-up moet worden verwijderd en Azure AD Domain Services kan niet herstellen vanuit een back-up.

## <a name="deleting-domains"></a>Domeinen verwijderen

Als het domein is onderbroken voor meer dan 15 dagen, wordt het beheerde domein vanwege inactiviteit en de modeldatabase groter voor het domein in Azure AD Domain Services verwijderd. U wordt niet meer gefactureerd voor Azure AD Domain Services. Voor het herstellen van uw beheerde domein, moet u het opnieuw maken.


## <a name="next-steps"></a>Volgende stappen
- [Los waarschuwingen op uw beheerde domein](active-directory-ds-troubleshoot-alerts.md)
- [Lees meer over Azure AD Domain Services](active-directory-ds-overview.md)
- [Neem contact op met het productteam](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Contact opnemen

Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).
