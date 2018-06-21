---
title: Capaciteit van een exemplaar van Azure API Management | Microsoft Docs
description: Dit artikel wordt uitgelegd wat de metriek capaciteit is en het maken van gefundeerde beslissingen te nemen of voor het schalen van een exemplaar van Azure API Management.
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
ms.openlocfilehash: 4983854a14a6efe9214692dc677dedeada73933b
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296104"
---
# <a name="capacity-of-an-azure-api-management-instance"></a>Capaciteit van een exemplaar van Azure API Management

**Capaciteit** is de belangrijkste één [Azure Monitor metriek](api-management-howto-use-azure-monitor.md#view-metrics-of-your-apis) voor het maken van gefundeerde beslissingen te nemen of u wilt schalen, zodat er ruimte meer load exemplaar van API Management. De constructie is complex en legt bepaalde gedrag.

Dit artikel wordt uitgelegd wat de **capaciteit** is en hoe deze zich gedraagt. Er wordt weergegeven hoe u toegang tot **capaciteit** metrische gegevens in de Azure-portal en suggesties voor het overwegen schalen of upgraden van uw exemplaar van API Management.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een exemplaar APIM. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

## <a name="what-is-capacity"></a>Wat is er capaciteit

![Capaciteit metrische gegevens](./media/api-management-capacity/capacity-ingredients.png)

**Capaciteit** is een indicator van de belasting van een APIM-exemplaar. Hiermee wordt aangegeven verbruik van de resources (CPU, geheugen) en netwerk wachtrij lengten. CPU- en geheugengebruik blijkt verbruik van bronnen door:

+ APIM services, zoals beheer-acties of aanvraag verwerking, met inbegrip van aanvragen door te sturen of het uitvoeren van een beleid
+ geselecteerde besturingssysteemprocessen, met inbegrip van processen met betrekking tot de kosten van het SSL-handshakes op nieuwe verbindingen.

Totaal aantal **capaciteit** is een gemiddelde van de eigen waarden van elke eenheid van het exemplaar van API Management.

## <a name="capacity-metric-behavior"></a>Capaciteit metrische gedrag

Vanwege de constructie, in de praktijk **capaciteit** kunnen worden beïnvloed door veel variabelen, bijvoorbeeld:

+ verbinding patronen (nieuwe verbinding op een aanvraag tegenover hergebruiken van de bestaande verbinding)
+ grootte van een aanvraag en -antwoord
+ beleid dat is geconfigureerd op elke API of het aantal clients verzenden van aanvragen.

De complexere bewerkingen op de aanvragen zijn, des te hoger de **capaciteit** verbruik zijn. Bijvoorbeeld verbruikt complexe claimtransformatiebeleidsinstellingen meer CPU dan een eenvoudige aanvraag doorsturen. Antwoorden van trage back-end-service wordt het te verhogen.

> [!IMPORTANT]
> **Capaciteit** is geen directe meting van het aantal aanvragen dat wordt verwerkt.

![Capaciteit metrische pieken](./media/api-management-capacity/capacity-spikes.png)

**Capaciteit** ook afwisselend pieken of niet groter zijn dan nul, zelfs als er zijn geen aanvragen worden verwerkt. Het gebeurt door systeem - of platform-specifieke acties en mag niet in aanmerking worden genomen bij het bepalen of u een exemplaar te schalen.
  
## <a name="use-the-azure-portal-to-examine-capacity"></a>De Azure Portal gebruiken om te onderzoeken capaciteit
  
![Capaciteit metrische gegevens](./media/api-management-capacity/capacity-metric.png)  

1. Navigeer naar uw APIM exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **metrische gegevens (preview)**.
3. Selecteer in de sectie paarse **capaciteit** metrische gegevens van de beschikbare metrische gegevens en laat de standaardwaarde **Gem** aggregatie.

    > [!TIP]
    > U ziet altijd op een **capaciteit** metrische uitsplitsing per locatie om te voorkomen dat de verkeerde interpretatie.

4. Selecteer in de sectie groen **locatie** voor het splitsen van de metrische gegevens voor dimensie.
5. Kies een gewenste periode van de bovenste balk van de sectie.

    U kunt de waarschuwing te laten u weten wanneer er iets onverwachts gebeurt er in een metrische instellen. Bijvoorbeeld, mailmeldingen meer ontvangen als uw exemplaar APIM exceededing is de capaciteit van de verwachte piek gedurende meer dan 20 minuten.

    >[!TIP]
    > U kunt waarschuwingen om u weten wanneer uw service heeft onvoldoende capaciteit of Azure Monitor-functionaliteit voor automatisch schalen gebruiken voor het automatisch toevoegen van een Azure API Management-eenheid te laten configureren. Bewerking schalen kan ongeveer 30 minuten duren, dus moet u uw regels dienovereenkomstig plannen.  
    > Alleen de master locatie schalen is toegestaan.

## <a name="use-capacity-for-scaling-decisions"></a>Met een capaciteit voor het nemen van beslissingen schalen

**Capaciteit** is de metrische gegevens voor het nemen van besluiten of u wilt schalen, zodat er ruimte meer load exemplaar van API Management. Houd rekening met:

+ Een lange termijn trend en gemiddelde kijken.
+ Wordt genegeerd plotselinge pieken die waarschijnlijk niet gerelateerd aan een toename in load (Zie de sectie 'Capaciteit metrische gedrag' voor uitleg).
+ Een upgrade of schalen van uw exemplaar wanneer **capaciteit**van waarde hoger is dan 60% of 70% voor een langere periode (bijvoorbeeld 30 minuten). Verschillende waarden kunnen werken beter voor uw service of het scenario.

>[!TIP]  
> Als u kunt uw verkeer tevoren schatten, test u uw exemplaar APIM op werkbelastingen dat u verwacht. U kunt de werklast geleidelijk verhogen op uw tenant en controleren wat is de waarde van de capaciteit metrische gegevens komt overeen met uw piekbelasting. Volg de stappen in de voorgaande sectie Azure portal gebruiken om te begrijpen hoeveel capaciteit wordt gebruikt op elk moment.

## <a name="next-steps"></a>Volgende stappen

[Schalen of een exemplaar van Azure API Management-service bijwerken](upgrade-and-scale.md)