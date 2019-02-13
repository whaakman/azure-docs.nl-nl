---
title: Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren | Microsoft Docs
description: " Informatie over het gebruik van beveiligingsbeleid en aanbevelingen in Azure Security Center om te beperken van een aanval op de beveiliging. "
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/2/2019
ms.author: rkarlin
ms.openlocfilehash: 5ff59a9ed7dc44bb7a4176f7a174be1e7d0ec9b6
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104903"
---
# <a name="use-azure-security-center-recommendations-to-enhance-security"></a>Gebruik Azure Security Center aanbevelingen om beveiliging te verbeteren
U kunt de kans op een aanzienlijke beveiligingsgebeurtenis beperken door een beveiligingsbeleid configureren en vervolgens implementeren van de aanbevelingen die door Azure Security Center. Dit artikel ziet u hoe u beveiligingsbeleid en aanbevelingen in Security Center gebruikt om te beperken van een aanval op de beveiliging.

De beveiligingsstatus van uw Azure-resources wordt gecontroleerd met Security Center. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, worden er aanbevelingen die u bij het begeleiden configureren van de vereiste beveiligingscontroles.

## <a name="scenario"></a>Scenario
In dit scenario laat zien hoe u Security Center gebruiken om te helpen verminderen de kans op een beveiligingsincident door Security Center aanbevelingen voor bewaking en actie te ondernemen. Het scenario wordt gebruikt voor het fictieve bedrijf, Contoso en rollen die zijn gepresenteerd in het Security Center [planning- en bedieningsgids voor](security-center-planning-and-operations-guide.md#security-roles-and-access-controls). In dit scenario focussen we op de functies van de volgende personen:

![Scenario-rollen](./media/security-center-using-recommendations/scenario-roles.png)

Contoso heeft enkele onlangs gemigreerd van de on-premises resources naar Azure. Contoso wil voor het beveiligen van hun resources en het verminderen van kwetsbaarheid van hun bronnen in de cloud.

## <a name="use-azure-security-center"></a>Azure Security Center gebruiken
David van Contoso de IT-beveiliging, heeft al gekozen voor onboarding Security Center voor Contoso abonnementen aan Azure Security Center om te voorkomen en detecteren van beveiligingsrisico's. 

Security Center analyseert de beveiligingsstatus van Contoso Azure-resources automatisch en standaard-beveiligingsbeleid is van toepassing. Wanneer Security Center potentiële beveiligingsproblemen worden geïdentificeerd, maakt het **aanbevelingen** op basis van de besturingselementen die zijn ingesteld in het beveiligingsbeleid. 

Beveiliging van Azure standard-laag, David uitgevoerd in alle abonnementen om op te halen van de volledige suite met aanbevelingen en beveiligingsfuncties die beschikbaar zijn. Jeff uitvoeren van Onboarding van alle zijn bestaande on-premises servers die nog niet hebt zijn gemigreerd naar de cloud zodat hij van Security Center hybrid profiteren kan bieden ook ondersteuning voor zijn [Windows](quick-onboard-windows-computer.md) en [Linux](quick-onboard-linux-computer.md) -servers.

Jeff is de eigenaar van een cloud-workload. Jeff is verantwoordelijk voor het toepassen van beveiligingscontroles in overeenstemming met het beveiligingsbeleid van Contoso. 

Jeff voert de volgende taken uit:

- Beveiligingsaanbevelingen voor die door Security Center bewaken
- Aanbevelingen voor beveiliging evalueren en bepalen of hij moet toepassen of verwijderen
- Aanbevelingen voor beveiliging toepassen

### <a name="remediate-threats-using-recommendations"></a>Bedreigingen met behulp van aanbevelingen herstellen
Als onderdeel van de dagelijkse bewakingsactiviteiten, Jeff zich aanmeldt bij Azure en Security Center wordt geopend. 

1. Jeff selecteert van diens werkbelasting abonnementen.

2. Jeroen controleert hij **score secure** om op te halen in een algemene afbeelding van hoe veilig de abonnementen zijn en hij ziet dat de score 548.

3. Jeff heeft om te bepalen welke aanbevelingen voor het afhandelen van eerst. Jeff klikt op beveiligde score en wordt gestart voor het afhandelen van aanbevelingen op basis van hoeveel zijn verbetert [score impact secure](security-center-secure-score.md).

4. Omdat Jeff voorzien van tal van verbonden virtuele machines en servers is, Jeff wil zich richten op **reken- en apps**.

5. Wanneer Jeff klikt **reken- en apps**, hij ziet een lijst met aanbevelingen en ze worden verwerkt op basis van de beveiligde impact te beoordelen.

6. Jeff heeft een groot aantal virtuele machines voor internetgerichte en omdat de poorten worden blootgesteld, hij is vastgesteld dat een aanvaller de controle over de servers kan krijgen. Dus Jeff wil gebruiken (**just-in-time-VM-toegang**) [security-center-just-in-time.md].

Jeff blijft verplaatsen door middel van de hoge prioriteit en aanbevelingen voor gemiddelde prioriteit en beslissingen over de uitvoering. Voor elke aanbeveling kijkt Jeff naar de gedetailleerde informatie opgegeven door Security Center om te begrijpen welke bronnen worden beïnvloed, wat de veilige score van invloed is, welke elke aanbeveling betekent en welke stappen voor het oplossen van problemen.

## <a name="conclusion"></a>Conclusie
Bewaking van de aanbevelingen in Security Center helpt u beveiligingsproblemen te elimineren voordat een aanval plaatsvindt. Als u aanbevelingen voor herstel, wordt uw beveiligde score en uw workloads beveiligingspostuur verbeteren. Security Center detecteert automatisch nieuwe resources implementeren, beoordeelt ze op basis van uw beveiligingsbeleid en aanbevelingen geven voor het beveiligen van deze nieuwe.


## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat u hebt een bewakingsproces aanwezig is, in die u regelmatig controleren of de aanbevelingen in Security Center zodat u zeker dat u uw resources beveiligen na verloop van tijd kunt maken.

In dit scenario laat zien hoe u beveiligingsbeleid en aanbevelingen in Security Center gebruikt om te beperken van een aanval op de beveiliging. Zie de [respons op incidenten scenario](security-center-incident-response.md) voor meer informatie over hoe u met reacties op incidenten in plaats van plan bent voordat er een aanval plaatsvindt.

Meer informatie over het reageren op bedreigingen met [respons op incidenten](security-center-incident-response.md).
