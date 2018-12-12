---
title: Azure-resource-mgmt
titleSuffix: Language Understanding - Azure Cognitive Services
description: In dit artikel maakt u een eindpuntsleutel met een datalimiet voor uw LUIS-account voor onbeperkte verkeer naar het eindpunt een betalingsplannen te volgen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 5d3e837cf644e0cb9b35a0cfc715a9dcace592e3
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082982"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Azure-eindpunt abonnementssleutels beheren

Gebruik voor testen en prototype alleen de gratis laag van (F0). Gebruik voor productiesystemen, een [betaalde](https://aka.ms/luis-price-tier) laag. 

> [!NOTE]
> Gebruik niet de [ontwerpen sleutel](luis-concept-keys.md#authoring-key) voor eindpunt query's in de productieomgeving.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>LUIS-eindpuntsleutel maken

1. Aanmelden bij  **[Microsoft Azure](https://ms.portal.azure.com/)**. 
2. Klik op de groene **+** zich in de bovenste linkerdeelvenster en zoek naar 'LUIS' in de marketplace, en klik vervolgens op **Language Understanding** en volg de **ervaring maken**  een LUIS-abonnement-account maken. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Het abonnement configureren met instellingen die de accountnaam, zoals prijzen van lagen, enzovoort. 

    ![Azure API kiezen](./media/luis-azure-subscription/azure-api-choice.png) 

4. Als u de LUIS-service maakt, kunt u de toegangssleutel die is gegenereerd in bekijken **Resourcemanagement -> sleutels**.  

    ![Azure-sleutels](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > Meld u aan bij van uw regio [LUIS](luis-reference-regions.md) website en [toewijzen van de nieuwe LUIS eindpuntsleutel](luis-how-to-manage-keys.md#assign-endpoint-key). U moet de naam van het abonnement LUIS uit stap 3.

## <a name="change-luis-pricing-tier"></a>Wijziging LUIS-prijscategorie

1.  In [Azure](https://portal.azure.com), zoek uw LUIS-abonnement. Klik op de LUIS-abonnement.
    ![Zoek uw LUIS-abonnement](./media/luis-usage-tiers/find.png)
2.  Klik op **prijscategorie** om te zien van de beschikbare Prijscategorieën. 
    ![Prijscategorieën weergeven](./media/luis-usage-tiers/subscription.png)
3.  Klik op de prijscategorie en klikt u op **Selecteer** uw wijziging op te slaan. 
    ![De betaling LUIS laag wijzigen](./media/luis-usage-tiers/plans.png)
4.  Wanneer met de prijswijziging voltooid is, controleert de nieuwe prijscategorie of in een pop-upvenster. 
    ![Controleer of uw LUIS betaling-laag](./media/luis-usage-tiers/updated.png)
5. Houd er rekening mee te [toewijzen van deze eindpuntsleutel](luis-how-to-manage-keys.md#assign-endpoint-key) op de **publiceren** pagina en deze gebruiken in alle endpoint-query's. 

## <a name="exceed-pricing-tier-usage"></a>Prijzen laag gebruik overschrijden
Elke laag kunt eindpunt aanvragen naar uw LUIS-account met een specifieke snelheid. Als het aantal aanvragen hoger dan het toegestane aantal uw naar gebruik-account per minuut of per maand is, aanvragen ontvangen van een HTTP-fout "429: te veel aanvragen."

Elke laag kan wél cumulatief aanvragen per maand. Als het totaal aantal aanvragen dat hoger is dan de toegestane frequentie, aanvragen ontvangen van een HTTP-fout ' 403: verboden '.  

## <a name="viewing-summary-usage"></a>Overzicht gebruik weergeven
U kunt informatie over het gebruik van LUIS weergeven in Azure. De **overzicht** pagina toont recente samenvattende informatie, inclusief aanroepen en fouten. Als u een LUIS-aanvraag voor het claimeindpunt aanbrengt, klikt u vervolgens onmiddellijk volgen het **overzichtspagina**, leiden tot vijf minuten voor het gebruik worden weergegeven.

![Overzicht gebruik weergeven](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Gebruik grafieken aanpassen
Metrische gegevens biedt meer inzicht in de gegevens.

![Standaard metrische gegevens](./media/luis-usage-tiers/metrics-default.png)

U kunt uw grafieken met metrische gegevens voor een bepaalde periode en type van metrische gegevens configureren. 

![Aangepaste metrische gegevens](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Totaal aantal transacties drempelwaarde voor waarschuwing
Als u wilt weten wanneer u een bepaalde drempelwaarde in de transactie, bijvoorbeeld 10.000 transacties hebt bereikt, kunt u een waarschuwing maken. 

![Standaard-waarschuwingen](./media/luis-usage-tiers/alert-default.png)

Toevoegen van een waarschuwing voor metrische gegevens voor de **totaal aantal aanroepen** metrische gegevens voor een bepaalde periode. Scheid e-mailadressen van alle personen die de waarschuwing moet ontvangen. Voeg webhooks voor alle systemen die de waarschuwing moeten ontvangen. U kunt ook een logische app uitvoeren wanneer de waarschuwing wordt geactiveerd. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van [versies](luis-how-to-manage-versions.md) voor het beheren van wijzigingen in uw LUIS-app.