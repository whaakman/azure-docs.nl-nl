---
title: Coderen of decoderen van platte bestanden in Azure logic apps | Microsoft Docs
description: Over het gebruik van het coderingsprogramma van bestand of de decoder in de Enterprise Integration Pack in uw logische apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: jeconnoc
editor: ''
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; divswa
ms.openlocfilehash: b13e8da04c984456027f152f5af63cfa6604ddc4
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054239"
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Overzicht van de enterprise integration met platte bestanden

U wilt coderen van XML-inhoud voordat u deze naar een zakelijke partner in een scenario voor business-to-business (B2B verzendt). In een logische app, kunt u het platte bestand connector codering om dit te doen. De logische app die u maakt krijgt de XML-inhoud uit diverse bronnen, met inbegrip van een HTTP-aanvraag als trigger, van een andere toepassing of zelfs op een van de vele [connectors](../connectors/apis-list.md). Zie voor meer informatie over logic apps, de [documentatie over logic apps](logic-apps-overview.md "meer informatie over Logic apps").  

## <a name="create-the-flat-file-encoding-connector"></a>Maken van het platte bestand connector-codering
Volg deze stappen voor het toevoegen van een plat bestand codering connector aan uw logische app.

1. Een logische app maken en [koppelen aan uw integratieaccount](logic-apps-enterprise-integration-accounts.md "Leer hoe u een integratieaccount koppelt aan een logische app"). Dit account bevat het schema dat u gebruiken wilt voor het coderen van de XML-gegevens.  
1. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.  
   ![Schermafbeelding van de trigger selecteren](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
1. Toevoegen van het platte bestand codering actie, als volgt:
   
    a. Selecteer de **plus** ondertekenen.
   
    b. Selecteer de **een actie toevoegen** koppeling (wordt weergegeven nadat u het plusteken hebt geselecteerd).
   
    c. Voer in het zoekvak *platte* voor het filteren van alle acties in de map die u wilt gebruiken.
   
    d. Selecteer de **platte bestandscodering** optie in de lijst.   
   ![Schermafbeelding van plat bestandscodering optie](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
1. Op de **platte bestandscodering** in het dialoogvenster, selecteer de **inhoud** in het tekstvak.  
   ![Schermafbeelding van de inhoud in het tekstvak](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
1. Selecteer de body >-tag als de inhoud die u wilt coderen. De hoofdtekst van de tag wordt het veld inhoud gevuld.     
   ![Schermafbeelding van body >-tag](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
1. Selecteer de **schemanaam** in een keuzelijst en kiest u het schema dat u wilt gebruiken voor het coderen van de invoer-inhoud.    
   ![Schermafbeelding van de naam van de Schema-keuzelijst](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
1. Sla uw werk op.   
   ![Pictogram Screenshot van opslaan](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Op dit moment bent u klaar instellen van uw codering met platte bestanden-connector. In een werkelijke toepassing, kunt u de gecodeerde gegevens opslaan in een line-of-business-toepassing, zoals Salesforce. Of u kunt verzenden dat gecodeerde gegevens naar een trading partner. U kunt eenvoudig een actie voor het verzenden van de uitvoer van de codering actie met Salesforce, of naar uw trading partner, met behulp van een van de andere connectors opgegeven toevoegen.

U kunt nu uw connector testen door die een aanvraag verzendt naar de HTTP-eindpunt en met inbegrip van de XML-inhoud in de hoofdtekst van de aanvraag.  

## <a name="create-the-flat-file-decoding-connector"></a>Maken van het platte bestand decoderen van connector

> [!NOTE]
> Als u wilt deze stappen hebt voltooid, moet u een schemabestand dat is al geüpload naar u integratie-account hebben.

1. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.  
   ![Schermafbeelding van de trigger selecteren](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
1. Toevoegen van het platte bestand decoderen actie, als volgt:
   
    a. Selecteer de **plus** ondertekenen.
   
    b. Selecteer de **een actie toevoegen** koppeling (wordt weergegeven nadat u het plusteken hebt geselecteerd).
   
    c. Voer in het zoekvak *platte* voor het filteren van alle acties in de map die u wilt gebruiken.
   
    d. Selecteer de **plat bestand decoderen** optie in de lijst.   
   ![Schermafbeelding van plat bestand decoderen optie](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
1. Selecteer de **inhoud** besturingselement. Dit resulteert in een lijst van de inhoud uit de eerdere stappen die u als de inhoud gebruiken kunt moet worden gedecodeerd. U ziet dat de *hoofdtekst* uit de binnenkomende HTTP aanvraag moet worden gebruikt als de inhoud moet worden gedecodeerd beschikbaar is. U kunt ook de inhoud moet worden gedecodeerd rechtstreeks naar de **inhoud** besturingselement.     
1. Selecteer de *hoofdtekst* tag. U ziet dat de body >-tag is nu in de **inhoud** besturingselement.
1. Selecteer de naam van het schema dat u wilt gebruiken voor het decoderen van de inhoud. De volgende schermafbeelding ziet u dat *OrderFile* is de naam van de geselecteerde schema. De naam van dit schema had eerder is geüpload naar het integratieaccount.
   
   ![Schermafbeelding van plat bestand decoderen in het dialoogvenster](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
1. Sla uw werk op.  
   ![Pictogram Screenshot van opslaan](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Op dit moment bent u klaar instellen van uw plat bestand decoderen van de connector. In een werkelijke toepassing, kunt u de gedecodeerde gegevens opslaan in een line-of-business-toepassing zoals Salesforce. U kunt eenvoudig een actie voor het verzenden van de uitvoer van de decodering actie aan Salesforce toevoegen.

U kunt nu uw connector testen door die een aanvraag verzendt naar de HTTP-eindpunt en met inbegrip van de XML-inhoud die u wilt dat moet worden gedecodeerd in de hoofdtekst van de aanvraag.  

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack").  

