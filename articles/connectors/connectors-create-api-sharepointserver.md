---
title: De SharePoint-Server-Connector in uw logische Apps gebruiken | Microsoft Docs
description: Aan de slag met de de SharePoint-Server-Connector in Logic apps
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a>Aan de slag met de SharePoint-connector
De SharePoint-Connector biedt een manier om te werken met lijsten in SharePoint.

Aan de slag door het maken van een logische app; Zie [een logische app maken](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-sharepoint"></a>Maak een verbinding met SharePoint
Voor het gebruik van de SharePoint-Connector maakt u eerst een **verbinding** Geef vervolgens de details voor deze eigenschappen: 

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Token |Ja |SharePoint-referenties opgeven |

Verbinding maken met **SharePoint**, voert u uw identiteit (gebruikersnaam en wachtwoord, smart card-referenties, enz.) naar SharePoint. Zodra u hebt geverifieerd, kunt u doorgaan met het gebruik van de SharePoint-connector in uw logische app. 

Volg deze stappen voor aanmelding bij SharePoint om de verbinding te maken terwijl op de ontwerper van uw logische app, **verbinding** voor gebruik in uw logische app:

1. Voer SharePoint in het zoekvak en wacht totdat de zoekopdracht te retourneren van alle vermeldingen met SharePoint in de naam:   
   ![SharePoint configureren][1]  
2. Selecteer **SharePoint - als een bestand is gemaakt**   
3. Selecteer **aanmelden bij SharePoint**:   
   ![SharePoint configureren][2]    
4. Geef uw SharePoint-referenties om aan te melden om te verifiëren met SharePoint   
   ![SharePoint configureren][3]     
5. Nadat de verificatie is voltooid wordt u omgeleid naar uw logische app om deze te voltooien door het configureren van SharePoint **wanneer een bestand wordt gemaakt** dialoogvenster.          
   ![SharePoint configureren][4]  
6. Vervolgens kunt u andere triggers en acties die u moet voltooien van uw logische app toevoegen.   
7. Sla uw werk door te selecteren **opslaan** boven in het menu.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/sharepoint/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
