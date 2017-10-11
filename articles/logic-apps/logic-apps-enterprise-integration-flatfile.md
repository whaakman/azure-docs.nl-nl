---
title: Codeer of decoderen platte bestanden in Azure logic apps | Microsoft Docs
description: Het gebruik van het bestand bestand encoder of decoder in de Enterprise Integration Pack in uw logische apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: bc3430624844cdeb92958433fba295f67a8ae0ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Overzicht van enterprise-integratie met platte bestanden

U kunt voor het coderen van XML-inhoud voordat u deze naar een zakelijke partner in een scenario met business-to-business (B2B verzenden). U kunt in een logische app, de platte bestandscodering connector gebruiken om dit te doen. De logische app die u maakt kan de XML inhoud ophalen van tal van bronnen, met inbegrip van de trigger van een HTTP-aanvraag van een andere toepassing of zelfs op een van de vele [connectors](../connectors/apis-list.md). Zie voor meer informatie over logische apps, de [logic apps documentatie](logic-apps-what-are-logic-apps.md "meer informatie over logische apps").  

## <a name="create-the-flat-file-encoding-connector"></a>De platte bestandscodering connector maken
Volg deze stappen voor het toevoegen van een plat bestand-connector voor uw logische app codering.

1. Een logische app maken en [koppelen aan uw account integratie](logic-apps-enterprise-integration-accounts.md "informatie over het koppelen van een integratie-account aan een logische app"). Dit account bevat het schema dat u gebruiken wilt voor het coderen van de XML-gegevens.  
2. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.  
   ![Schermafbeelding van de trigger selecteren](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
3. De platte bestandscodering actie, als volgt toevoegen:
   
    a. Selecteer de **plus** aanmelding.
   
    b. Selecteer de **een actie toevoegen** koppeling (wordt weergegeven nadat u hebt geselecteerd dat het plusteken).
   
    c. Voer in het zoekvak *platte* voor het filteren van de acties met de fout die u wilt gebruiken.
   
    d. Selecteer de **platte bestandscodering** optie uit de lijst.   
   ![Schermopname van platte bestandscodering optie](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
4. Op de **platte bestandscodering** selecteert u de **inhoud** in het tekstvak.  
   ![Schermopname van inhoud in het tekstvak](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
5. Selecteer de label body als de inhoud die u wilt coderen. De hoofdtekst-code wordt het veld inhoud gevuld.     
   ![Schermopname van hoofdtekst tag](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. Selecteer de **schemanaam** keuzelijst en kies het schema dat u gebruiken wilt voor het coderen van de inhoud van de invoer.    
   ![Schermafbeelding van de naam van de Schema-keuzelijst](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
7. Sla uw werk op.   
   ![Pictogram schermafbeelding opslaan](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Op dit moment bent u klaar instellen van uw codering plat bestand-connector. In een toepassing praktijk wilt u mogelijk de gecodeerde gegevens opslaan in een line-of-business-toepassing, zoals Salesforce. Of u kunt verzenden dat gecodeerde gegevens naar een trading partner. U kunt gemakkelijk een actie voor het verzenden van de uitvoer van de codering actie Salesforce, of naar uw trading partner met behulp van een van de andere connectors opgegeven toevoegen.

U kunt nu uw connector testen door het maken van een aanvraag voor het HTTP-eindpunt en de XML-inhoud, inclusief in de hoofdtekst van de aanvraag.  

## <a name="create-the-flat-file-decoding-connector"></a>De platte bestanden decoderen connector maken

> [!NOTE]
> Deze stappen uit te voeren, moet u een schemabestand al geüpload naar u integratie-account hebben.

1. Voeg een **aanvraag: wanneer een HTTP-aanvraag wordt ontvangen** trigger aan uw logische app.  
   ![Schermafbeelding van de trigger selecteren](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
2. De platte bestanden decoderen actie, als volgt toevoegen:
   
    a. Selecteer de **plus** aanmelding.
   
    b. Selecteer de **een actie toevoegen** koppeling (wordt weergegeven nadat u hebt geselecteerd dat het plusteken).
   
    c. Voer in het zoekvak *platte* voor het filteren van de acties met de fout die u wilt gebruiken.
   
    d. Selecteer de **plat bestand decoderen** optie uit de lijst.   
   ![Schermopname van plat bestand decoderen optie](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
3. Selecteer de **inhoud** besturingselement. Dit resulteert in een lijst van de inhoud van de eerdere stappen die u als de inhoud gebruiken kunt te decoderen. U ziet dat de *hoofdtekst* van de binnenkomende HTTP aanvraag is beschikbaar moet worden gebruikt als de inhoud te decoderen. U kunt ook de inhoud rechtstreeks in decoderen invoeren de **inhoud** besturingselement.     
4. Selecteer de *hoofdtekst* label. U ziet de hoofdtekst-tag bevindt zich nu in de **inhoud** besturingselement.
5. Selecteer de naam van het schema dat u gebruiken wilt voor het decoderen van de inhoud. De volgende schermafbeelding ziet u dat *OrderFile* is de naam van het geselecteerde schema. De naam van dit schema had eerder geüpload integratie rekening te houden.
   
   ![Schermopname van plat bestand decoderen van het dialoogvenster](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Sla uw werk op.  
   ![Pictogram schermafbeelding opslaan](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Op dit moment bent u klaar instellen van uw plat bestand decoderen van connector. In een toepassing praktijk wilt u mogelijk de gecodeerde gegevens opslaan in een line-of-business-toepassing zoals Salesforce. U kunt gemakkelijk een actie voor het verzenden van de uitvoer van de actie decoderen naar Salesforce toevoegen.

U kunt nu uw connector testen door te maken van een aanvraag voor het HTTP-eindpunt en met inbegrip van de XML-inhoud die u wilt decoderen in de hoofdtekst van de aanvraag.  

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over het Enterprise-integratiepakket](logic-apps-enterprise-integration-overview.md "meer informatie over Enterprise Integration Pack").  

