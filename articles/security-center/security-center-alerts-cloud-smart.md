---
title: Cloud Smart alert correlatie in Azure Security Center (incidenten) | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe fusie gebruikmaakt van Smart alert-correlatie voor het genereren van beveiligings incidenten in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 7ba2cf14d9fac100f44a1ef23997b27ba062bee0
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295867"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Azure Security Center voor de correlatie van de cloud in een andere (incidenten)

Security Center analyseert voortdurend de werk belasting van de hybride Cloud met geavanceerde analyses en bedreigings informatie om u te waarschuwen over schadelijke activiteiten.

Naarmate de omvang van de bedreigings dekking toeneemt en de nood zaak is om zelfs de zeer lichte indicatie van verhoogde toename te detecteren, kan het lastig zijn voor beveiligings analisten om de verschillende waarschuwingen te sorteren en een echte aanval te identificeren. Security Center helpt analisten bij het uitvallen van waarschuwings-vermoeidheid en het onderzoeken van aanvallen wanneer deze zich voordoen, door verschillende waarschuwingen en signalen met lage betrouw baarheid in beveiligings incidenten te correleren.

Fusion is de technologie en analytische back-end waarmee Security Center incidenten worden gesignaleerd, waardoor het mogelijk is om verschillende waarschuwingen en contextuele signalen samen te correleren. Fusion werkt door te kijken naar de verschillende signalen die zijn gerapporteerd voor een abonnement over de resources, en het vinden van gang bare patronen die de voortgang van een aanval tonen of signalen met gedeelde contextuele informatie die aangeeft dat een uniforme reactie procedure moet worden gemaakt.

Fusion Analytics combi neer de kennis van het beveiligings domein met AI voor het analyseren van waarschuwingen, het detecteren van nieuwe aanvals patronen wanneer deze zich voordoen. 

Security Center maakt gebruik van MITRE-aanvals matrix om waarschuwingen te koppelen aan hun waargenomen intentie, waardoor de kennis van de beveiliging van het domein wordt formaliseren. Daarnaast kunt u met behulp van de informatie die voor elke stap van een aanval wordt verzameld, Security Center activiteiten uitvoeren die de stappen van een aanval vermoeden, maar dat is niet het geval.  

Omdat aanvallen vaak optreden voor verschillende tenants, kan Security Center AI-algoritmen combi neren voor het analyseren van aanvals reeksen die op elk abonnement worden gerapporteerd om ze te identificeren als bekende waarschuwings patronen in plaats van alleen incidenteel te koppelen daarenteg.

Tijdens een onderzoek naar een incident hebben analisten vaak extra context nodig om een verdict te bereiken over de aard van de dreiging en hoe de oplossing kan worden verholpen. Bijvoorbeeld, zelfs wanneer er een afwijkend netwerk wordt gedetecteerd, zonder te begrijpen wat er nog meer gebeurt op het netwerk of met betrekking tot de doel resource, is het moeilijk te weten welke acties u vervolgens moet uitvoeren. Om u te helpen, kan een beveiligings incident artefacten, gerelateerde gebeurtenissen en informatie bevatten. De aanvullende informatie die beschikbaar is voor beveiligings incidenten, is afhankelijk van het type bedreiging dat is gedetecteerd en de configuratie van uw omgeving. 

![Details van beveiligings incident](./media/security-center-alerts-cloud-smart/security-incident.png)

Zie beveiligings incidenten afhandelen [in azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-incident)voor meer informatie over beveiligings incidenten.

