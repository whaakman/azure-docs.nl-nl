---
title: Het sluiten van actieve risicogebeurtenissen in Azure Active Directory Identity Protection | Microsoft Docs
description: Lees welke opties dat u sluiten active risicogebeurtenissen hebt.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: markvi
ms.reviewer: raluthra
ms.collection: M365-identity-device-management
ms.openlocfilehash: 50448cc5d4fe6714aa0cd29216209506eccf7a7c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201615"
---
# <a name="how-to-close-active-risk-events"></a>Procedure: Actieve risicogebeurtenissen sluiten

Met [risicogebeurtenissen](../reports-monitoring/concept-risk-events.md), Azure Active Directory detecteert indicatoren voor eventueel verdachte gebruikersaccounts. Als beheerder, die u wilt ophalen van alle risicogebeurtenissen gesloten, zodat de betrokken gebruikers niet meer risico zijn.

In dit artikel biedt een overzicht van de aanvullende opties die u moet actieve risicogebeurtenissen sluiten.

## <a name="options-to-close-risk-events"></a>Opties voor het sluiten van de risicogebeurtenissen 

De status van een risicogebeurtenis is **active** of **gesloten**. Alle actieve risicogebeurtenissen bijdragen aan de berekening van een waarde met de naam van gebruiker risiconiveau. Het risiconiveau van de gebruiker is een indicator (laag, Gemiddeld, hoog) voor de waarschijnlijkheid dat een account is aangetast. 

Als u wilt sluiten active risicogebeurtenissen, hebt u de volgende opties:

- Wachtwoord opnieuw instellen met een beleid voor gebruikersrisico 's

- Handmatige voor wachtwoord opnieuw instellen
 
- Alle risicogebeurtenissen verwijderen 

- Afzonderlijke risicogebeurtenissen handmatig sluiten



## <a name="require-password-reset-with-a-user-risk-policy"></a>Wachtwoord opnieuw instellen met een beleid voor gebruikersrisico 's

Door het [beleid voor voorwaardelijke toegang op basis van gebruikersrisico](howto-user-risk-policy.md) te configureren, kunt u een wachtwoordwijziging eisen als er automatisch een opgegeven gebruikersrisiconiveau is gedetecteerd. 

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/13.png)

Een wachtwoord opnieuw instellen wordt gesloten gebeurtenissen voor alle actieve risico's van de gerelateerde gebruiker en voorziet in de identiteit weer in een veilige status heeft. Met behulp van een beleid voor gebruikersrisico's is de aanbevolen methode om te sluiten, actieve risicogebeurtenissen omdat deze methode is geautomatiseerd. Er is geen tussenkomst vereist tussen de betrokken gebruiker en de help helpdesk of beheerder.

Met behulp van een beleid voor gebruikersrisico's is echter niet altijd van toepassing. Dit is van toepassing, bijvoorbeeld op:

- Gebruikers die niet zijn geregistreerd voor multi-factor authentication (MFA).
- Gebruikers met actieve risicogebeurtenissen die zijn verwijderd.
- Een onderzoek die blijkt dat een gerapporteerde risicogebeurtenis is uitgevoerd door de bevoegde gebruiker.


## <a name="manual-password-reset"></a>Handmatige voor wachtwoord opnieuw instellen

Als u een wachtwoord opnieuw instellen met behulp van een beleid voor gebruikersrisico's kan niet worden gebruikt, krijgt u alle risicogebeurtenissen voor een gebruiker gesloten met een handmatige wachtwoord opnieuw instellen.

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/04.png)


De gerelateerde dialoogvenster biedt twee verschillende methoden om een wachtwoord opnieuw instellen:

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/05.png)


**Genereer een tijdelijk wachtwoord** -door het genereren van een tijdelijk wachtwoord, u kunt onmiddellijk terugbrengen een identiteit met een veilige status. Deze methode moet de interactie met de betrokken gebruikers, omdat ze nodig hebben om te weten wat het tijdelijke wachtwoord is. U kunt bijvoorbeeld het tijdelijke wachtwoord verzenden naar een alternatieve e-mailadres voor de gebruiker of naar de manager van de gebruiker. Omdat het wachtwoord tijdelijk is, wordt de gebruiker gevraagd het wachtwoord moet wijzigen tijdens de volgende aanmelding.


**Vereisen dat de gebruiker opnieuw instellen van wachtwoord** -self-herstel de gebruikers te verlangen dat wachtwoorden opnieuw instellen kunt zonder contact opnemen met de helpdesk of beheerder. Net als in het geval van een beleid voor gebruikersrisico's, deze methode is alleen van toepassing op gebruikers die zijn geregistreerd voor MFA. Deze optie niet beschikbaar voor gebruikers die niet zijn geregistreerd voor MFA nog.


## <a name="dismiss-all-risk-events"></a>Alle risicogebeurtenissen verwijderen

Als een wachtwoord opnieuw instellen is geen een optie voor u, kunt u ook alle risicogebeurtenissen verwijderen. 

![Wachtwoord opnieuw instellen](./media/howto-close-active-risk-events/03.png)

Wanneer u klikt op **alle gebeurtenissen sluiten**, alle gebeurtenissen worden gesloten en de betrokken gebruiker is niet meer risico. Echter, omdat deze methode niet van invloed op het bestaande wachtwoord zijn, deze niet terughalen de identiteit van de gerelateerde met een veilige status. De gewenste use-case voor deze methode is een verwijderde gebruiker met actieve risico's. 


## <a name="close-individual-risk-events-manually"></a>Afzonderlijke risicogebeurtenissen handmatig sluiten

U kunt afzonderlijke risicogebeurtenissen handmatig sluiten. Risicogebeurtenissen handmatig sluit, kunt u het risiconiveau van de gebruiker te verlagen. Normaal gesproken zijn risicogebeurtenissen handmatig afgesloten in reactie op een gerelateerde onderzoek. Bijvoorbeeld, wanneer de communicatie met geeft een gebruiker aan dat een actieve risicogebeurtenis niet vereist meer is. 
 
Wanneer risicogebeurtenissen handmatig sluiten, kunt u enkele van de volgende acties om de status van een risicogebeurtenis wijzigen:

![Acties](./media/howto-close-active-risk-events/06.png)

- **Oplossen** - als u na onderzoek van een risicogebeurtenis een juiste herstelactie buiten Identity Protection hebt ondernomen en van mening bent dat de risicogebeurtenis kan worden gesloten, markeert u de gebeurtenis als Opgelost. Een opgeloste risicogebeurtenis krijgt de status Gesloten. Bovendien draagt de risicogebeurtenis niet meer bij aan het gebruikersrisico.

- **Markeren als fout-positieve** -In sommige gevallen kan u een risicogebeurtenis onderzoeken en ontdek ten onrechte is gemarkeerd als een riskant. U kunt het nummer van deze instanties verminderen door de risicogebeurtenis als fout-positieve markeren. Hiermee kunt u de machine learning-algoritmen voor de classificatie van soortgelijke gebeurtenissen in de toekomst te verbeteren. De status van de fout-positieve gebeurtenissen wordt gesloten en ze niet meer dragen bij aan gebruikersrisico.

- **Negeren** :  als u geen herstelactie heb uitgevoerd, maar wilt dat de risicogebeurtenis wordt verwijderd uit de actieve lijst, kunt u een risicogebeurtenis markeren als Negeren. De status van de gebeurtenis wordt dan Gesloten. Genegeerde gebeurtenissen dragen niet bij aan het gebruikersrisico. Deze optie dient alleen in ongebruikelijke omstandigheden te worden gebruikt.

- **Opnieuw activeren** -risicogebeurtenissen die handmatig zijn gesloten (door oplossen, fout-positief, of op negeren te kiezen) kunnen opnieuw worden geactiveerd, de gebeurtenisstatus van de instellen terug naar actief. Opnieuw geactiveerde risicogebeurtenissen dragen bij aan de berekening van het gebruikersrisiconiveau. Risico's die gesloten zijn via herstelacties (zoals het opnieuw instellen van een beveiligd wachtwoord), kunnen niet opnieuw worden geactiveerd.

  

## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).
