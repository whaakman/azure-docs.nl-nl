---
title: Power BI-App - aanbieding voor Azure Marketplace publiceren | Microsoft Docs
description: Een Power BI-App-aanbieding op marketplace, Microsoft AppSource publiceren.
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
ms.openlocfilehash: 0e82041dab1932ed7c129b598740b2441aa59315
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56822749"
---
# <a name="publish-a-power-bi-app-offer"></a>Een Power BI-App-aanbieding publiceren

De laatste stap is nadat u hebt een aanbieding gedefinieerd in de Cloud Partner-Portal en de bijbehorende technische activa gemaakt om in te dienen de aanbieding voor publicatie. Dit proces wordt gestart in het linkerdeelvenster van de **nieuwe aanbieding** venster **publiceren**. Zie voor meer informatie, [aanbiedingen publiceren op Azure Marketplace en AppSource](../manage-offers/cpp-publish-offer.md).


## <a name="publishing-steps"></a>Publiceren stappen

Dit zijn de belangrijkste stappen in het publicatieproces:

![Aanbieding publiceren processtappen voor Power BI-App](./media/publishing-process-steps.png)

Deze tabel elke stap wordt beschreven en vindt u de geschatte tijd:

|   Publiceren stap            |   Time     |   Description                                                                  |
| --------------------         |------------| ----------------                                                               |
| Valideer de vereisten       | 15 minuten     | Biedt informatie en bieden instellingen worden gevalideerd.                            |
| Certificering                | 1-7 dagen   | Het Team van Power BI-certificering analyseert uw aanbieding. Het team wordt uw Power BI-app uitgevoerd via een handmatige verificatie testen door de app via de installatie van de opgegeven URL te installeren. Primaire validaties worden uitgevoerd als onderdeel van het app-certificeringsproces (Zie verderop in dit document).         |
| Verpakking                    | \< 1 uur  | Technische activa van de aanbieding zijn verpakt voor gebruik door de klant.                        |
| Registratie van de potentiële klant genereren | \< 1 uur  | Potentiële klanten systemen zijn geconfigureerd en geïmplementeerd.                                      |
| Aftekening van de uitgever            | \-         | U voltooien een uiteindelijke controle en de bevestiging voordat de aanbieding live meteen. U hebt nu ook een koppeling om een voorbeeld van uw aanbieding. Nadat u tevreden bent met het uiterlijk van de Preview-versie, selecteert u **Go Live** op de **Status** tabblad. Hierdoor wordt een aanvraag verzonden naar het onboarding-team om uw Apps weer op AppSource weer te geven.    |
| Live                         | \< 3 uur | Uw aanbieding wordt nu openbaar ('live') op AppSource en klanten kunnen uw app kunt weergeven en deze implementeren in hun Power BI-abonnementen. U ontvangt ook een e-mail ter bevestiging. In de rechterkolom op de **alle aanbiedingen** tabblad ziet u de status van alle uw aanbiedingen. Op de **Status** tabblad ziet u de gedetailleerde publicatiestatus flow voor uw aanbieding. |
|   |   |

Toestaan dat maximaal acht dagen voor dit proces is voltooid. Nadat u deze publiceren stappen doorloopt, uw aanbieding van Power BI-App wordt weergegeven in de [AppSource](https://appsource.microsoft.com/marketplace/apps?product=power-bi%20) sectie voor Power BI-apps.


### <a name="app-certification-process"></a>App-certificeringsproces

Het Microsoft-voorbereidingsteam maakt gebruik van dit proces voor het valideren van uw inzending voor de aanbieding Power BI-App:

1. Bekijk de juridische documenten en help-koppelingen.
2. Valideer de contactgegevens voor ondersteuning.
3. Het installatieprogramma-URL gebruiken om te controleren of de juiste installatie.
4. Scan de app voor malware en andere schadelijke inhoud.
5. Verifieer dat de weergegeven inhoud overeenkomt met de beschrijving van de app.
6. Controleer of de app-gerelateerde bewerkingen werken zoals verwacht in Power BI. Het team van rapporten en dashboards met voorbeeldgegevens wordt geopend, maakt verbinding met aangepaste gegevensbronnen, vernieuwen van gegevens, enzovoort.

Het Team certificering biedt feedback als ze problemen vinden.  Zie voor meer informatie over vereisten voor Power BI-Apps, de [documentatie voor Power BI-app](https://go.microsoft.com/fwlink/?linkid=2028636).


## <a name="next-steps"></a>Volgende stappen

Het is raadzaam dat u regelmatig bewaken van uw app in de [AppSource-marketplace](https://appsource.microsoft.com).  U moet ook gebruiken de [verkoper Insights](../../cloud-partner-portal-orig/si-getting-started.md) functie van de [Cloud Partner-Portal](https://cloudpartner.azure.com/#insights) om inzichten over uw marketplace-klanten en het gebruik van de app te krijgen. Ten slotte kunt u [bijwerken van uw aanbieding](./cpp-update-existing-offer.md).
