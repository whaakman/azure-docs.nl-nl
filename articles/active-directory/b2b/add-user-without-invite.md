---
title: B2B-samenwerking gebruikers toevoegen aan Azure Active Directory zonder een uitnodiging | Microsoft Docs
description: U kunt een andere gastgebruikers toevoegen aan uw Azure AD zonder een uitnodiging in Azure Active Directory B2B-samenwerking wisselt gastgebruiker.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/21/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 50c64386f1eab07c299112617283b1d8d7295295
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591048"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>B2B-samenwerking gastgebruikers zonder een uitnodiging toevoegen

Nu kunt u gastgebruikers uitnodigen door het verzenden van een directe koppeling naar een gedeelde-app. Met deze methode wilt gastgebruikers niet langer gebruiken de uitnodiging voor e-mail, behalve in sommige gevallen speciale. Een gastgebruiker klikt op de koppeling van de app, beoordeelt en accepteert de voorwaarden van de privacy en vervolgens probleemloos toegang heeft tot de app. Zie voor meer informatie [B2B-samenwerking uitnodiging inwisseling](redemption-experience.md).   

Voordat u deze nieuwe methode beschikbaar is, kan u gastgebruikers uitnodigen zonder de uitnodigingsmail met het toevoegen van een uitnodiging antwoorden (van uw organisatie of een andere organisatie) aan de **Gast uitnodiging antwoorden** directory-rol, en vervolgens met de afzender van de uitnodiging gastgebruikers toevoegen aan de directory, groepen of toepassingen via de gebruikersinterface of PowerShell. (Als u PowerShell gebruikt, u kunt onderdrukken de uitnodigingsmail helemaal). Bijvoorbeeld:

1. Een gebruiker in de organisatie van de host (bijvoorbeeld WoodGrove) nodigt één gebruiker van de andere organisatie (bijvoorbeeld Sam@litware.com) als gast.
2. De beheerder in de organisatie van de host [beleid heeft ingesteld](delegate-invitations.md) waardoor Sam om te bepalen en andere gebruikers van de partnerorganisatie (Litware) toevoegen. (Sam moet worden toegevoegd aan de **Gast uitnodiging antwoorden** rol.)
3. Nu kunt Sam toevoegen andere gebruikers van Litware aan de directory WoodGrove, groepen of toepassingen zonder uitnodigingen die worden gebruikt. Als de juiste opsomming Sam in Litware bevoegdheden, wordt deze automatisch uitgevoerd.
 
Deze methode oorspronkelijke werkt nog altijd. Er is echter een klein verschil in gedrag. Als u PowerShell gebruikt, zult u merken dat een uitgenodigde gastaccount nu heeft een **PendingAcceptance** status in plaats van onmiddellijk weergegeven **geaccepteerde**. Hoewel de status in behandeling is, kan nog steeds de gastgebruiker aanmelden en toegang tot de app zonder een e-mailbericht uitnodiging voor een koppeling te klikken. De status in behandeling betekent dat de gebruiker heeft geen nog doorlopen de [ervaring toestemming](redemption-experience.md#privacy-policy-agreement), waar ze de voorwaarden van de privacy van de organisatie uitnodigen accepteren. De gastgebruiker ziet dit scherm toestemming wanneer ze zich voor de eerste keer aanmelden. 

Als u een gebruiker naar de map uitnodigen, de gastgebruiker toegang moet hebben tot de Azure portal voor resource-tenantspecifieke URL rechtstreeks (zoals https://portal.azure.com/ *resourcetenant*. onmicrosoft.com) weergeven en gaat akkoord met de voorwaarden van de privacy.

### <a name="next-steps"></a>Volgende stappen

- [Wat is Azure AD B2B-samenwerking?](what-is-b2b.md)
- [B2B-samenwerking uitnodiging inwisseling](redemption-experience.md)
- [Gemachtigde uitnodigingen voor Azure Active Directory B2B-samenwerking](delegate-invitations.md)
- [Hoe kunnen IT-medewerkers B2B-samenwerking gebruikers toevoegen](add-users-information-worker.md)

