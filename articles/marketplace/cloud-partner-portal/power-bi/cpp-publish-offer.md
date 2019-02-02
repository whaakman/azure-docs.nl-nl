---
title: Power BI-App - aanbieding voor Azure Marketplace publiceren | Microsoft Docs
description: Een Power BI-App-aanbieding op Microsoft AppSource-Marketplace publiceren.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 2b3783060cf5502076ce3bc98cf07f005366a9e2
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666714"
---
# <a name="publish-power-bi-app-offer"></a>Power BI-App-aanbieding publiceren

De laatste stap is nadat u hebt de aanbieding gedefinieerd in de portal en de bijbehorende technische activa gemaakt om in te dienen de aanbieding voor publicatie.  Als u wilt starten, klikt u op de **publiceren** knop op het verticale menu in de **nieuwe aanbieding** venster.  Zie voor meer informatie, [aanbiedingen publiceren op Azure Marketplace en AppSource](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Publiceren stappen

Het volgende diagram ziet u de belangrijkste stappen in het publicatieproces ' om live te gaan'.

![Publiceren processtappen voor Power BI-App](./media/publishing-process-steps.png)

De volgende tabel beschrijft de stappen en biedt een geschatte maximale tijd voor de voltooiing:

|   Publiceren stap            |   Time     |   Description                                                                  |
| --------------------         |------------| ----------------                                                               |
| Valideer de vereisten       | 15 min     | Biedt informatie en bieden instellingen worden gevalideerd.                            |
| Certificering                | 1-7 dagen   | Het Team van Power BI-certificering analyseert uw aanbieding. We uitvoeren uw Power BI-App via een handmatige verificatie testen door de app via de installatie van de opgegeven URL te installeren. Belangrijkste validaties worden uitgevoerd als onderdeel van het app-certificeringsproces; Zie hieronder.         |
| Verpakken                    | \< 1 uur  | Technische activa van de aanbieding zijn verpakt voor gebruik door de klant.                        |
| Registratie van de potentiële klant genereren | \< 1 uur  | Potentiële klanten systemen zijn geconfigureerd en geïmplementeerd.                                      |
| Aftekening van de uitgever            | \-         | Laatste uitgever controleren en de bevestiging voordat de aanbieding live meteen. U hebt nu ook een koppeling om een voorbeeld van uw aanbieding. Wanneer u tevreden bent met het uiterlijk van de Preview-versie, klikt u op de **Go Live** knop in de **Status** tabblad. Deze actie wordt een aanvraag verzonden naar het onboarding-team om uw Apps weer op AppSource weer te geven.    |
| Live                         | \< 3 uur | Uw aanbieding wordt nu openbaar vermeld ('live') op AppSource en klanten kunnen weergeven en uw app implementeren in hun Power BI-abonnementen. U ontvangt ook een e-mailbericht. Op elk gewenst moment kunt u klikken op de **alle aanbiedingen** tabblad, en zien dat de status voor alle uw aanbiedingen die worden vermeld in de rechterkolom. U kunt klikken op de **Status** tabblad om te bekijken van de publicatiestatus van de stroom in de details voor uw aanbieding. |
|   |   |

Toestaan dat maximaal acht dagen voor dit proces is voltooid. Nadat u deze publiceren stappen doorloopt, uw aanbieding van Power BI-App wordt weergegeven in de [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) sectie van de Power BI-Apps.


### <a name="app-certification-process"></a>App-certificeringsproces

Het Microsoft-voorbereidingsteam maakt gebruik van het volgende proces voor het valideren van het indienen van uw Power BI-aanbieding:

1. Juridische documenten en help-koppelingen worden beoordeeld.
2. Neem contact op met ondersteuningsinformatie wordt gevalideerd.
3. Installatieprogramma URL wordt gebruikt om te controleren of de juiste installatie. 
4. App is gescand op schadelijke software en andere schadelijke inhoud. 
5. Verificatie wordt uitgevoerd die inhoud weergegeven komt overeen met de app-beschrijving.
6. App-gerelateerde bewerkingen werken zoals verwacht in Power BI: open rapporten en dashboards met voorbeeldgegevens, verbinding maken met aangepaste gegevensbronnen, vernieuwen, enzovoort.

Het Team certificering biedt feedback als ze problemen vinden.  Zie voor meer informatie over Power BI-App-vereisten, de [documentatie voor Power BI-App](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Volgende stappen

Het is raadzaam dat u regelmatig bewaken van uw app in de [AppSource-Marketplace](https://appsource.microsoft.com).  Bovendien moet u de [verkoper Insights](../../cloud-partner-portal-orig/si-getting-started.md) functie van de [Cloud Partner-Portal](https://cloudpartner.azure.com/#insights) om aan te geven u inzicht in uw marketplace-klanten en het gebruik.  U kunt ook bepaalde uitvoeren [updates voor uw aanbieding](./cpp-update-existing-offer.md).
