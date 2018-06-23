---
title: De referenties in Azure inhoud beheerder | Microsoft Docs
description: Inhoud beheerder referenties voor gebruik met de API's beheren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: 4531fa4c8bbb7bb9c1daeaaac6f9e7078dae250a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344453"
---
# <a name="manage-credentials"></a>Referenties beheren

De referenties van uw inhoud beheerder zijn gemaakt in de volgende locaties:

- [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator)
- [Het hulpprogramma voor het controleren van inhoud beheerder](http://contentmoderator.cognitive.microsoft.com/)

In dit artikel wordt uitgelegd waar deze en hun relatie met elkaar.

## <a name="the-azure-portal"></a>De Azure-portal

Selecteer uw account inhoud beheerder op de Azure-portaldashboard. Onder **bronbeheer**, selecteer **sleutels**. Selecteer het pictogram aan de rechterkant van de sleutel voor het kopiëren van de sleutel.

![Inhoud beheerder sleutels in de Azure-portal](images/credentials-azure-portal-keys.PNG)

### <a name="how-to-use-your-azure-account-with-the-review-tool"></a>Het gebruik van uw Azure-account met het hulpprogramma controleren
Voor het gebruik van uw Azure-sleutel met de revisie API's, kopieert u de Resource-ID weergegeven op de **eigenschappen** scherm in de volgende schermafbeelding en voer deze in de evaluatie-hulpprogramma referenties scherm van de **goedgekeurde lijst Resource id('s)** zoals weergegeven in de volgende velden **Resource-ID** sectie. 

Voor het gebruik van uw Azure-sleutel voor de werkstromen die beschikbaar zijn in inhoud beheerder, invoeren in de **Ocp-Apim-Subscription-Key** veld in de **Werkstroominstellingen** zoals weergegeven in de volgende sectie  **Werkstromen** sectie.

> [!NOTE]
> Zodra u de twee locaties in het hulpprogramma voor beoordeling door de sleutel en de Resource-ID van uw Azure-abonnement vervangen, uw **proefversie Ocp-Apim-Subscription-Key** weergegeven van de referenties scherm wordt niet meer gebruikt, maar is altijd beschikbaar.
> De evaluatieversie sleutel beperkt tot maximaal 5.000 transacties per maand 1 aanvragen per seconde (RPS).

![Inhoud beheerder bron-ID in de Azure-portal](images/credentials-azure-portal-resourceid.PNG)


## <a name="the-review-tool"></a>Het hulpprogramma controleren

Hulpprogramma voor de controle-Dashboard op de **instellingen** tabblad **referenties**.

![Referenties van de beheerder in het hulpprogramma voor beoordeling van inhoud](images/credentials-trial-resource-workflow.PNG)

De volgende sectie onderzoekt de voorgaande afbeelding in meer detail:


### <a name="api"></a>API

Het eerste onderdeel een lijst met uw **API-eindpunt bekijken**, **ID in een team**, en de **Ocp-Apim-abonnement-sleutel voor inhoud beheerder proefversie** gegenereerd als onderdeel van uw team controleren het maken. Ze gebruiken voor alle inhoud beheerder API's, waaronder de revisie-API aanroepen.

Let ook op uw regio-id voor uw API-eindpunt. Bijvoorbeeld: **westus** de regio in 'https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0'

![Inhoudssleutel beheerder in het hulpprogramma controleren](images/credentials-trialkey.PNG)


### <a name="resource-id"></a>Resource-id

De tweede onderdeel gestart uit als leeg is en er is geen resource-ID. **Voor het gebruik van de sleutel van uw Azure-abonnement met de revisie-API, navigeer naar het resource-ID-scherm, zoals eerder en kopieer het naar het veld weergegeven**. Klik op de **'+'** om op te slaan van de bron-ID.

> [!NOTE]
> De regio van uw abonnement inhoud beheerder moet overeenkomen met de regio van het controle-team voor het herkennen van uw team en toegang tot de gegevens van het team. Bijvoorbeeld, in de installatiekopieën op deze pagina de **VS-West** regio **(4)** bevat het inhoud beheerder Azure-abonnement en uw team controleren.

![Inhoud beheerder bron-ID in het hulpprogramma controleren](images/credentials-resourceids.PNG)


### <a name="workflows"></a>Werkstromen

Het derde onderdeel wordt de informatie die wordt gebruikt voor het uitvoeren van werkstromen. Dit gaat uit de automatisch gegenereerde proefversie sleutel wordt standaard weergegeven. 

**Werken met uw Azure-sleutel bij het ophalen van een Azure-abonnement**. De andere twee velden staat het gebruik van de termijn en de installatiekopie van een lijst met respectievelijk in de tekst op het scherm en evalueren van de installatiekopie van bewerkingen.

![Beheerder werkstroom referenties in het hulpprogramma voor beoordeling van inhoud](images/credentials-workflow.PNG)


## <a name="next-steps"></a>Volgende stappen

* Informatie over het gebruik van de referenties van de inhoud beheerder in uw [werkstromen](workflows.md).
