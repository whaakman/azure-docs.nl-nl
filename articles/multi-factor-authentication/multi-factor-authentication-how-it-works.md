---
title: Azure multi-factor Authentication - hoe het werkt
description: Multi-Factor Authentication van Azure helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers met een eenvoudige aanmeldprocedure. Deze extra beveiliging biedt doordat een tweede vorm van verificatie en sterke verificatie via een bereik van eenvoudige verificatie-opties te bezorgen.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d14db902-9afe-4fca-b3a5-4bd54b3d8ec5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 6fee02885cc76b3a4fdad11e8702f623d6fe6597
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-azure-multi-factor-authentication-works"></a>De werking van Azure multi-factor Authentication
De beveiliging van verificatie in twee stappen ligt de gelaagde benadering. Inbreuk op meerdere verificatiefactoren geeft een belangrijke uitdaging voor kwaadwillende personen. Zelfs als een aanvaller erin slaagt voor meer informatie over het wachtwoord van gebruikers, is deze onbruikbaar zonder dat ook bezit is van het vertrouwde apparaat. 

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)

Multi-Factor Authentication van Azure helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers met een eenvoudige aanmeldprocedure.  Deze extra beveiliging biedt doordat een tweede vorm van verificatie en sterke verificatie via een bereik van eenvoudige verificatie-opties te bezorgen.


## <a name="methods-available-for-two-step-verification"></a>Methoden voor verificatie in twee stappen
Wanneer een gebruiker zich aanmeldt, wordt een aanvullende verificatie verzonden naar de gebruiker.  Hieronder vindt u een lijst met methoden die kunnen worden gebruikt voor deze tweede verificatie.

| Verificatiemethode | Beschrijving |
| --- | --- |
| Telefoonoproep |Een aanroep van wordt het geregistreerde telefoonnummer van een gebruiker geplaatst. De gebruiker voert een PINCODE, indien nodig worden vervolgens de #-toets indrukt. |
| SMS-bericht |Een SMS-bericht verzonden naar een gebruiker mobiele telefoon met een code van zes cijfers. De gebruiker voert deze code op de aanmeldingspagina. |
| Meldingen voor mobiele Apps |Een verzoek voor identiteitverificatie verzonden naar een gebruiker Smartphone. De gebruiker een PINCODE invoert, indien nodig en vervolgens selecteert **controleren** op de mobiele app. |
| Verificatiecode van mobiele app |De mobiele app, die wordt uitgevoerd op de Smartphone van een gebruiker, wordt een bevestigingscode die elke 30 seconden wordt gewijzigd. De gebruiker de meest recente code zoekt en krijgt de op de aanmeldingspagina. |
| OATH-tokens van derde partij | Azure multi-factor Authentication-Server kan worden geconfigureerd voor het accepteren van derden verificatiemethoden te gebruiken. |

Azure multi-factor Authentication biedt selecteerbare verificatiemethoden voor zowel cloud als de server. U kunt kiezen welke methoden zijn beschikbaar voor uw gebruikers: telefoonoproep, tekst, app-melding of app-codes. Zie voor meer informatie [selecteerbare verificatiemethoden](multi-factor-authentication-whats-next.md#selectable-verification-methods).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de verschillende [versies en verbruikmethoden voor Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md)

- Kies of u voor het implementeren van Azure MFA [in de cloud of on-premises](multi-factor-authentication-get-started.md)

- Lees de antwoorden voor [Veelgestelde vragen](multi-factor-authentication-faq.md)