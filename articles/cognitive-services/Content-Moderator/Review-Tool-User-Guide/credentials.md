---
title: Beheren van referenties in Azure Content Moderator - Content Moderator
titlesuffix: Azure Cognitive Services
description: Content Moderator referenties beheren, moet u de API's gebruiken.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 0da6b6b0fef0f998e20789253b2a65c54121532c
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260006"
---
# <a name="manage-content-moderator-service-credentials"></a>Content Moderator Servicereferenties beheren

De referenties van uw Content Moderator worden gemaakt in de volgende locaties:

- [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [De Content Moderator-controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/)

In dit artikel wordt uitgelegd waar u kunt ze vinden en hun relatie met elkaar.

## <a name="the-azure-portal"></a>Azure Portal

Selecteer op de Azure portal-dashboard, uw Content Moderator-account. Onder **resourcebeheer**, selecteer **sleutels**. Selecteer het pictogram aan de rechterkant van de sleutel voor het kopiëren van de sleutel.

![Content Moderator-sleutels in Azure portal](images/credentials-azure-portal-keys.PNG)

### <a name="use-the-azure-account-with-the-review-tool-and-review-api"></a>Het Azure-account gebruiken met het hulpprogramma voor beoordeling en API bekijken
Voor het gebruik van uw Azure-sleutel met de beoordelings-API's, kopieert u de Resource-ID weergegeven op de **eigenschappen** scherm in de volgende schermopname, en voer deze in het beoordelingsprogramma referenties scherm van de **Whitelisted voor de Resource (s)** zoals wordt weergegeven in de volgende velden **Resource-ID** sectie. 

> [!NOTE]
> De regio van uw Content Moderator-abonnement moet overeenkomen met de beoordelingsteam regio voor het herkennen van uw team en toegang tot de gegevens van het team. Bijvoorbeeld, in de afbeeldingen op deze pagina de **VS-West** regio **(4)** bevat het Content Moderator-Azure-abonnement en uw team controleren.
>
> Nadat u de twee plaatsen in het beoordelingsprogramma door de sleutel en de Resource-ID van uw Azure-abonnement vervangen, uw **proefversie Ocp-Apim-Subscription-Key** weergegeven op de referenties scherm wordt niet meer gebruikt, maar is altijd beschikbaar.
> De proefversie sleutel u beperkt tot maximaal 5000 transacties per maand bij 1 aanvraag per seconde (RPS).

![Content Moderator-Resource-ID in Azure portal](images/credentials-azure-portal-resourceid.PNG)

### <a name="use-the-azure-account-with-the-workflows-in-the-review-tool"></a>Gebruik de Azure-account met de werkstromen in het controlehulpprogramma

Voor het gebruik van uw Azure-sleutel voor de werkstromen die beschikbaar zijn in de Content Moderator, geef deze op in de **Ocp-Apim-Subscription-Key** veld in de **Werkstroominstellingen** zoals wordt weergegeven in de volgende sectie  **Werkstromen** sectie. Klik op de **'+'** om op te slaan van de resource-ID.

![Content Moderator werkstroom referenties in het controlehulpprogramma](images/credentials-workflow.PNG)

## <a name="the-review-tool"></a>Het beoordelingsprogramma

Hulpprogramma voor de beoordeling-Dashboard op de **instellingen** tabblad **referenties**.

![Content Moderator-referenties in het controlehulpprogramma](images/credentials-trial-resource-workflow.PNG)

De volgende sectie onderzoekt de voorgaande afbeelding in meer detail:

### <a name="api"></a>API

Het eerste deel een lijst met uw **bekijken van de API-eindpunt**, **team-ID**, en de **Ocp-Apim-Subscription-Key (Content Moderator proefversie sleutel)** gegenereerd als onderdeel van uw team controleren het maken. Ze gebruiken om alle Content Moderator API's, met inbegrip van de beoordeling-API aanroepen.

Let ook op uw regio-id voor uw API-eindpunt. Bijvoorbeeld, **westus** is de regio in ' https:\//westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0 "

![Content Moderator-sleutel in het controlehulpprogramma](images/credentials-trialkey.PNG)

### <a name="resource-id"></a>Resource-id

Deze set met velden wordt beschreven in de vorige sectie [het Azure-account gebruiken met het hulpprogramma voor beoordeling en API](credentials.md#use-the-azure-account-with-the-review-tool-and-review-api). Dit veld is meestal leeg, tenzij u uw Azure-Resource-Id aan dit veld toevoegen, zoals wordt beschreven in de vorige sectie.

### <a name="workflows"></a>Workflows

Deze set met velden wordt beschreven in de vorige sectie [het Azure-account gebruiken met de werkstromen in het controlehulpprogramma](credentials.md#use-the-azure-account-with-the-workflows-in-the-review-tool). Standaard, het beoordelingsprogramma maakt gebruik van de automatisch gegenereerde proefversie sleutel voor het uitvoeren van de werkstromen en dat is wat wordt eerst weergegeven. De andere twee velden kunnen met behulp van lijsten termijn en afbeelding in de tekst op het scherm en evalueren afbeelding operations respectievelijk.

## <a name="next-steps"></a>Volgende stappen

* Informatie over het gebruik van de Content Moderator-referenties in uw [werkstromen](workflows.md).
