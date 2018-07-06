---
title: De SharePoint-Server-Connector in uw logische Apps gebruiken | Microsoft Docs
description: Aan de slag met de SharePoint Server-Connector in uw logische apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 3ac3105231017cdbf8bfcf143b26451a68da5283
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868698"
---
# <a name="get-started-with-the-sharepoint-connector"></a>Aan de slag met de SharePoint-connector
De SharePoint-Connector biedt een manier om te werken met lijsten op SharePoint-lijsten.

Aan de slag met het maken van een logische app; Zie [maken van een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-sharepoint"></a>Maak een verbinding met SharePoint
Voor het gebruik van de SharePoint-Connector, maakt u eerst een **verbinding** geeft u de details voor deze eigenschappen: 

| Eigenschap | Vereist | Beschrijving |
| --- | --- | --- |
| Token |Ja |SharePoint-referenties opgeven |

Verbinding maken met **SharePoint**, voer je identiteit (gebruikersnaam en wachtwoord, smartcardreferenties, enzovoort). Nadat u hebt geverifieerd, kunt u doorgaan met het gebruik van de SharePoint-connector in uw logische app. 

Terwijl in de ontwerpfunctie van de logische app, gebruikt u de volgende stappen uit om te melden en maken het **verbinding** voor gebruik in uw logische app:

1. SharePoint in het zoekvak invoeren en wacht tot de zoekopdracht om te retourneren van alle vermeldingen met SharePoint in de naam:   
   ![SharePoint configureren][1]  
2. Selecteer **SharePoint: wanneer een bestand wordt gemaakt**   
3. Selecteer **aanmelden bij SharePoint**:   
   ![SharePoint configureren][2]    
4. Geef uw SharePoint-referenties om aan te melden om te verifiëren met SharePoint   
   ![SharePoint configureren][3]     
5. Nadat de verificatie is voltooid, wordt u doorgestuurd naar uw logische app om deze te voltooien door het configureren van SharePoint **wanneer een bestand wordt gemaakt** dialoogvenster.          
   ![SharePoint configureren][4]  
6. U kunt vervolgens een andere triggers en acties die u nodig hebt voor uw logische app toevoegen.   
7. Sla uw werk hiervoor **opslaan** in het menu (boven).

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/sharepoint/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [lijst van API's](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
