---
title: Capaciteit van een exemplaar van Azure API Management | Microsoft Docs
description: In dit artikel wordt uitgelegd wat de metriek capaciteit is en hoe u weloverwogen beslissingen te nemen of voor het schalen van een Azure API Management-exemplaar.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: 31959cc1bef6b6434f6d3f586052a845837aa438
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442588"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Capaciteit van een Azure API Management-exemplaar

**Capaciteit** is de belangrijkste één [metrische gegevens van Azure Monitor](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) voor het maken van gefundeerde beslissingen te nemen of voor het schalen van een exemplaar van API Management om meer aankan. De constructie is complex en legt een bepaald gedrag.

In dit artikel wordt uitgelegd wat de **capaciteit** is en hoe deze zich gedraagt. U leert hoe u toegang tot **capaciteit** metrische gegevens in Azure portal en suggesties om te overwegen schalen of het upgraden van uw exemplaar van API Management.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een APIM-instantie. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="what-is-capacity"></a>Wat is capaciteit

![Metrische capaciteitswaarde](./media/api-management-capacity/capacity-ingredients.png)

**Capaciteit** is een indicator van de belasting van een APIM-instantie. Het weergegeven verbruik van resources (CPU, geheugen) en network lengtes. CPU- en geheugengebruik blijkt verbruik van bronnen door:

+ APIM-services, zoals management acties of aanvraag verwerkt, waaronder aanvragen doorsturen of het uitvoeren van een beleid kunt
+ processen van het besturingssysteem, met inbegrip van processen die betrekking hebben op de kosten van het aantal SSL-handshakes op nieuwe verbindingen hebt geselecteerd.

Totaal aantal **capaciteit** is een gemiddelde van een eigen waarden van elke eenheid van een exemplaar van API Management.

## <a name="capacity-metric-behavior"></a>Capaciteit metrische gedrag

Vanwege de bouw, in de praktijk **capaciteit** kunnen worden beïnvloed door vele variabelen, bijvoorbeeld:

+ verbinding patronen (nieuwe verbinding op een aanvraag voor Visual Studio opnieuw gebruiken van de bestaande verbinding)
+ grootte van een aanvraag en antwoord
+ beleidsregels die zijn geconfigureerd op elke API of het aantal clients verzenden van aanvragen.

De complexe bewerkingen op de aanvragen bent, hoe hoger de **capaciteit** verbruik is. Bijvoorbeeld, verbruikt complexe transformatiebeleid veel meer CPU nodig is dan een eenvoudige aanvraag doorsturen. Antwoorden op trage back-end-service wordt het te verhogen.

> [!IMPORTANT]
> **Capaciteit** is niet een directe meting van het aantal aanvragen dat wordt verwerkt.

![Capaciteit metrische pieken](./media/api-management-capacity/capacity-spikes.png)

**Capaciteit** ook samen af en toe pieken of niet groter zijn dan nul, zelfs als er zijn geen aanvragen worden verwerkt. Het gebeurt door systeem - of platform-specifieke acties en moet niet in aanmerking worden genomen bij het bepalen of u een exemplaar schalen.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>De Azure-Portal gebruiken voor het onderzoeken van capaciteit
  
![Metrische capaciteitswaarde](./media/api-management-capacity/capacity-metric.png)  

1. Navigeer naar de APIM-instantie in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **metrische gegevens (preview)**.
3. Selecteer in de sectie paarse **capaciteit** metrische gegevens van de beschikbare metrische gegevens en laat de standaardwaarde **Avg** aggregatie.

    > [!TIP]
    > U moet altijd eerst gekeken een **capaciteit** metrische uitsplitsing per locatie om te voorkomen dat de verkeerde een perfecte ervaring bij.

4. Selecteer in de sectie groen **locatie** voor het splitsen van de metrische gegevens per dimensie.
5. Kies een gewenste periode in de bovenste balk van de sectie.

    U kunt een waarschuwing voor metrische gegevens zodat u weet wanneer er iets onverwachts gebeurt instellen. Bijvoorbeeld, meldingen ontvangen wanneer de APIM-instantie aanroepfrequentie is de verwachte piek-capaciteit van meer dan 20 minuten.

    >[!TIP]
    > U kunt waarschuwingen om u weten wanneer uw service is onvoldoende capaciteit of functionaliteit voor automatisch schalen van Azure Monitor gebruiken om toe te voegen automatisch een Azure API Management-eenheid te laten configureren. Schalen van de bewerking kan ongeveer 30 minuten duren, dus moet u uw regels dienovereenkomstig plannen.  
    > Alleen schalen van de master locatie is toegestaan.

## <a name="use-capacity-for-scaling-decisions"></a>Met een capaciteit voor het schalen van beslissingen

**Capaciteit** is de metrische gegevens voor het nemen van besluiten al dan niet schalen van een exemplaar van API Management om meer aankan. Houd rekening met:

+ Kijken naar een op de lange termijn trend en een gemiddelde.
+ Wordt genegeerd plotselinge pieken die waarschijnlijk niet met betrekking tot een toename in load (Zie de sectie 'Capaciteit metrische gedrag' voor uitleg over).
+ Een upgrade of schalen van uw exemplaar wanneer **capaciteit**van waarde hoger is dan 60% of 70% voor een langere periode (bijvoorbeeld 30 minuten). Verschillende waarden kunnen werken beter voor uw service of scenario.

>[!TIP]  
> Als u kunt vooraf schatting van uw verkeer, test u de APIM-instantie op werkbelastingen die u verwacht. U kunt de aanvraagbelasting op uw tenant geleidelijk verhoogd en controleren van welke waarde van de capaciteit metrische gegevens komt overeen met de piekbelasting. Volg de stappen uit de vorige sectie is het gebruik van Azure portal om te begrijpen hoeveel capaciteit wordt gebruikt op een bepaald moment.

## <a name="next-steps"></a>Volgende stappen

[Het bijwerken van een Azure API Management-service-exemplaar of schalen](upgrade-and-scale.md)