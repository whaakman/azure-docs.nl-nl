---
title: Gebruik van de Connector Slack in Azure logic apps | Microsoft Docs
description: Verbinding maken met Slack in logic apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 04ea4508495b227d6ace4a3105f283c474c51d14
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-slack-connector"></a>Aan de slag met de toegestane connector
Slack is een communicatieprogramma voor teams waarbij alle communicatie van uw team op één plaats wordt samengebracht waar deze direct doorzoekbaar en beschikbaar is vanaf elke locatie. 

Aan de slag door het maken van een logische app nu; Zie [een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-slack"></a>Maak een verbinding met Slack
Voor het gebruik van de toegestane connector maakt u eerst een **verbinding** Geef vervolgens de details voor deze eigenschappen: 

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Token |Ja |Slack-referenties opgeven |

Volg deze stappen voor het aanmelden bij de vertraging en voltooi de configuratie van de toegestane vertraging **verbinding** in uw logische app:

1. Selecteer **terugkeerpatroon**
2. Selecteer een **frequentie** en voer een **Interval**
3. Selecteer **een actie toevoegen**  
   ![Vertraging configureren][1]  
4. Vertraging in het zoekvak invoert en wacht tot de zoekopdracht te retourneren van alle vermeldingen met vertraging in de naam
5. Selecteer **Slack - bericht posten**
6. Selecteer **aanmelden met Slack**:  
   ![Vertraging configureren][2]
7. De toegestane referenties opgeven voor het aanmelden bij de toepassing    
   ![Vertraging configureren][3]  
8. U moet worden omgeleid naar de aanmeldingspagina van uw organisatie. **Autoriseren** Slack om te communiceren met uw logische app:      
   ![Vertraging configureren][5] 
9. Nadat de autorisatie is voltooid wordt u omgeleid naar uw logische app om deze te voltooien door het configureren van de **vertraging - alle berichten ophalen** sectie. Toevoegen van andere triggers en acties die u nodig hebt.  
   ![Vertraging configureren][6]
10. Sla uw werk door te selecteren **opslaan** boven in het menu.

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/slack/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
