---
title: Azure-eindpunt abonnement beheren | Microsoft Docs
description: In dit artikel maakt u een gecontroleerde endpoint-sleutel voor uw account LUIS om onbeperkte verkeer naar uw eindpunt na een betaald abonnement.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 3526871f126ac975f323fe84b14883b361b684ae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345869"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Azure-eindpunt abonnement sleutels beheren

Gebruik voor het testen en prototype alleen de laag gratis (F0). Gebruik voor productiesystemen, een [betaald](https://aka.ms/luis-price-tier) laag. 

> [!NOTE]
> Gebruik niet de [authoring sleutel](luis-concept-keys.md#authoring-key) voor eindpunt query's in de productieomgeving.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>LUIS eindpuntsleutel maken

1. Aanmelden bij  **[Microsoft Azure](https://ms.portal.azure.com/)** 
2. Klik op de groene **+** log in het bovenste links Configuratiescherm en zoek naar 'LUIS' in de marketplace en klik vervolgens op **Language Understanding** en volg de **ervaring maken**  een abonnement LUIS account maken. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Het abonnement configureren met instellingen die de accountnaam, zoals prijzen lagen, enzovoort. 

    ![Azure API keuze](./media/luis-azure-subscription/azure-api-choice.png) 

4. Als u de service LUIS maakt, kunt u de toegangssleutels gegenereerd in bekijken **Resourcemanagement-sleutels >**.  

    ![Azure-sleutels](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > * Meld u aan bij uw regio [LUIS](luis-reference-regions.md) website en [toevoegen van de nieuwe LUIS eindpuntsleutel](Manage-Keys.md#assign-endpoint-key). 
    > * U moet onthouden van de naam van de Azure-service die u hebt gemaakt om te selecteren op het gebied [LUIS](luis-reference-regions.md) pagina publiceren.  

## <a name="change-luis-pricing-tier"></a>Wijziging LUIS prijscategorie

1.  In [Azure](https://portal.azure.com), uw abonnement LUIS vinden. Klik op de LUIS-abonnement.
    ![Uw abonnement LUIS vinden](./media/luis-usage-tiers/find.png)
2.  Klik op **prijscategorie** om te zien van de beschikbare Prijscategorieën. 
    ![Prijscategorieën weergeven](./media/luis-usage-tiers/subscription.png)
3.  Klik op de prijscategorie en op **Selecteer** uw wijziging op te slaan. 
    ![Uw betaling LUIS laag wijzigen](./media/luis-usage-tiers/plans.png)
4.  Wanneer de prijscategorie wijziging voltooid is, controleert de nieuwe prijscategorie of in een pop-upvenster. 
    ![Controleer of uw betaling LUIS laag](./media/luis-usage-tiers/updated.png)
5. Vergeet niet te [toewijzen van deze eindpuntsleutel](manage-keys.md#assign-endpoint-key) op de **publiceren** pagina en deze gebruiken in alle endpoint-query's. 

## <a name="exceed-pricing-tier-usage"></a>Prijscategorie laag gebruik overschrijden
Elke laag kan uw account LUIS eindpunt aanvragen met een bepaalde snelheid. Als de frequentie van aanvragen hoger dan de toegestane van uw gecontroleerde account per minuut of per maand is, ontvangen aanvragen van een HTTP-fout ' 429: te veel aanvragen. "

Elke laag kan accumulatieve aanvragen per maand. Als het totaal aantal aanvragen dat hoger is dan de toegestane snelheid, ontvangen aanvragen van een HTTP-fout ' 403: verboden '.  

## <a name="viewing-summary-usage"></a>Samenvatting van gebruik weergeven
U kunt informatie over het gebruik van LUIS weergeven in Azure. De **overzicht** pagina samenvattingsinformatie recente inclusief aanroepen en fouten. Als u een LUIS eindpunt aanvraag maakt, vervolgens onmiddellijk gecontroleerd op de **overzichtspagina**, kunt u maximaal vijf minuten voor het gebruik worden weergegeven.

![Samenvatting van gebruik weergeven](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Gebruik grafieken aanpassen
Metrische gegevens biedt een gedetailleerdere weergave in de gegevens.

![Standaard metrische gegevens](./media/luis-usage-tiers/metrics-default.png)

U kunt uw grafieken metrische gegevens voor de periode en type metrische gegevens configureren. 

![Aangepaste metrische gegevens](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Totaal aantal transacties drempelwaarde voor waarschuwing
Als u wilt weten wanneer een bepaalde drempelwaarde in de transactie, bijvoorbeeld 10.000 transacties is bereikt, kunt u een waarschuwing maken. 

![Standaard-waarschuwingen](./media/luis-usage-tiers/alert-default.png)

Een metriek waarschuwing toevoegen voor de **Totaal oproepen** metric voor een bepaalde periode. Toevoegen van e-mailadressen van alle gebruikers van de waarschuwing moet ontvangen. Toevoegen van webhooks voor alle systemen dat de waarschuwing moeten ontvangen. U kunt ook een logische app uitvoeren wanneer de waarschuwing wordt geactiveerd. 

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik [versies](luis-how-to-manage-versions.md) voor het beheren van wijzigingen aan uw app LUIS.