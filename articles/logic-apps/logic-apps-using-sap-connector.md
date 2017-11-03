---
title: Verbinding maken met een on-premises SAP-systeem in Azure Logic Apps | Microsoft Docs
description: Verbinding maken met een on-premises SAP-systeem van uw werkstroom logic app via de gateway van de lokale gegevens
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 3fea93f558d5a4ef62550fd1f6486903cb812930
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-an-on-premises-sap-system-from-logic-apps-with-the-sap-connector"></a>Verbinding maken met een on-premises SAP-systeem van logische apps met de SAP-connector 

De lokale data gateway kunt u gegevens beheren en veilige toegang tot resources die zich on-premises. Dit onderwerp wordt beschreven hoe u logic apps kunt verbinden met een on-premises SAP-systeem. In dit voorbeeld uw logische app een IDOC via HTTP-aanvragen en verzendt het antwoord terug.    

> [!NOTE]
> Huidige beperkingen: 
> - Uw logische app een time-out optreedt als alle stappen die nodig zijn voor het antwoord is niet voltooid binnen de [aanvraag time-outlimiet](./logic-apps-limits-and-config.md). In dit scenario mogelijk aanvragen worden geblokkeerd. 
> - De bestandskiezer worden niet weergegeven voor alle beschikbare velden. In dit scenario kunt u paden handmatig toevoegen.

## <a name="prerequisites"></a>Vereisten

- Installeren en configureren van de meest recente [lokale gegevensgateway](https://www.microsoft.com/download/details.aspx?id=53127) versie 1.15.6150.1 of hoger. [Verbinding maken met de lokale data gateway in een logische app](http://aka.ms/logicapps-gateway) vermeldt de stappen. De gateway moet worden geïnstalleerd op een on-premises machine voordat u verder kunt gaan.

- Download en installeer de meest recente SAP-clientbibliotheek op dezelfde computer waarop u de data gateway hebt geïnstalleerd. Gebruik een van de volgende SAP-versies: 
    - SAP-Server
        - Een SAP-Server die ondersteuning bieden voor de .NET-Connector (NCo) 3.0
 
    - SAP-Client
        - SAP .NET-Connector (NCo) 3.0

## <a name="add-triggers-and-actions-for-connecting-to-your-sap-system"></a>Triggers en acties voor het verbinden met uw SAP-systeem toevoegen

De SAP-connector heeft acties, maar geen triggers. We hebben dus gebruik een andere trigger aan het begin van de werkstroom. 

1. De aanvraag/antwoord-trigger toevoegen en selecteer vervolgens **nieuwe stap**.

2. Selecteer **een actie toevoegen**, en selecteer vervolgens de SAP-connector door te typen `SAP` in het zoekveld:    

     ![SAP-toepassingsserver of SAP berichtenserver selecteren](media/logic-apps-using-sap-connector/sap-action.png)

3. Selecteer [ **SAP-toepassingsserver** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) of [ **SAP Message Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm), op basis van uw SAP-instellingen. Als u een bestaande verbinding geen hebt, wordt u gevraagd een maken.

   1. Selecteer **verbinden via lokale gegevensgateway**, en voer de details voor uw SAP-systeem:   

       ![Verbindingsreeks SAP toevoegen](media/logic-apps-using-sap-connector/picture2.png)  

   2. Onder **Gateway**, selecteert u een bestaande gateway of selecteren voor het installeren van een nieuwe gateway **Gateway installeren**.

        ![Een nieuwe gateway installeren](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Nadat u alle gegevens invoeren, selecteert u **maken**. 
   Logic Apps configureert en test u de verbinding, om ervoor te zorgen dat de verbinding goed werkt.

4. Voer een naam voor uw SAP-verbinding.

5. De verschillende opties voor SAP zijn nu beschikbaar. Selecteer de categorie IDOC vindt in de lijst. Of geef het pad handmatig en selecteer het HTTP-antwoord in de **hoofdtekst** veld:

     ![SAP-actie](media/logic-apps-using-sap-connector/picture3.png)

6. Voeg de actie voor het maken van een **HTTP-antwoord**. Het antwoordbericht moet uit de SAP-uitvoer.

7. Sla uw logische app. Testen door te sturen een IDOC via de URL van de HTTP-trigger. Nadat de IDOC is verzonden, wacht u totdat de reactie van de logische app:   

     > [!TIP]
     > Bekijk hoe [uw logische Apps bewaken](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Nu dat de SAP-connector wordt toegevoegd aan uw logische app, start u de andere functionaliteiten verkennen:

- BAPI
- RFC

## <a name="get-help"></a>Help opvragen

Ga naar het [Forum voor Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) om vragen te stellen, vragen te beantwoorden en te zien wat andere gebruikers van Azure Logic Apps aan het doen zijn.

Ter verbetering van Azure Logic Apps en connectors kunt u stemmen op ideeën of ideeën indien op de [site voor gebruikersfeedback van Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

- Informatie over het valideren, transformeren en andere functies BizTalk-achtige in de [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Verbinding maken met on-premises gegevens](../logic-apps/logic-apps-gateway-connection.md) vanuit logic apps
