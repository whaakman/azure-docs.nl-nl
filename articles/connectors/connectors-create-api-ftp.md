---
title: Verbinding maken met de FTP-server - Azure Logic Apps | Microsoft Docs
description: Maken, bewaken en beheren van bestanden op een FTP-server met Azure Logic Apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 07/22/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 3bb8c750846ee14331c749aeece4315d5c16d411
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34610562"
---
# <a name="get-started-with-the-ftp-connector"></a>Aan de slag met de FTP-connector
De FTP-connector gebruiken om te controleren, beheren en bestanden op een FTP-server maken. 

Gebruik [elke connector](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [maken van een logische app nu](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-ftp"></a>Verbinding maken met FTP
Om uw logische app toegang alle services tot, moet u eerst maken een *verbinding* naar de service. Een [verbinding](connectors-overview.md) biedt connectiviteit tussen een logische app en een andere service.  

### <a name="create-a-connection-to-ftp"></a>Maak een verbinding met FTP
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Een FTP-trigger te gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!IMPORTANT]
> De FTP-connector vereist een FTP-server die toegankelijk is vanaf het Internet en is geconfigureerd voor het werken met de passieve modus. De FTP-connector is ook **niet compatibel met impliciete FTPS (FTP via SSL)**. De FTP-connector ondersteunt alleen expliciete FTPS (FTP via SSL).  
> 
> 

In dit voorbeeld leest u het gebruik van de **FTP - wanneer een bestand wordt toegevoegd of gewijzigd** activeren voor het initiëren van een logische app werkstroom wanneer een bestand wordt toegevoegd aan of gewijzigd op een FTP-server. In een enterprise-voorbeeld: u kunt deze trigger voor het bewaken van een FTP-map voor nieuwe bestanden die die de orders van klanten vertegenwoordigt.  U kunt vervolgens een FTP-connector actie, zoals **bestandsinhoud ophalen** ophalen van de inhoud van de volgorde voor verdere verwerking en opslag in uw orderdatabase.

1. Voer *ftp* in het zoekvak op de ontwerpfunctie van logic apps selecteert u vervolgens de **FTP - wanneer een bestand wordt toegevoegd of gewijzigd** trigger   
   ![FTP-triggerafbeelding 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   De **wanneer een bestand wordt toegevoegd of gewijzigd** besturingselement wordt geopend  
   ![FTP-triggerafbeelding 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Selecteer de **...**  zich aan de rechterkant van het besturingselement. Hiermee opent u het kiezerbesturingselement map  
   ![FTP-triggerafbeelding 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Selecteer de **>** (pijl naar rechts) en Ga naar de map die u wilt controleren op nieuwe en gewijzigde bestanden. Selecteer de map en u ziet dat de map wordt nu weergegeven in de **map** besturingselement.  
   ![FTP-triggerafbeelding 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Op dit moment is uw logische app geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart wanneer een bestand wordt gewijzigd of in een specifieke FTP-map gemaakt. 

> [!NOTE]
> Voor een logische app wilt gebruiken, moet er ten minste één trigger en één actie bevatten. Volg de stappen in de volgende sectie voor een actie toevoegen.  
> 
> 

## <a name="use-a-ftp-action"></a>Gebruik een FTP-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Nu dat u een trigger hebt toegevoegd, volg deze stappen uit om een actie die u krijgt de inhoud van de nieuwe of gewijzigde bestand gevonden door de trigger.    

1. Selecteer **+ een nieuwe stap** toevoegen de de actie voor het ophalen van de inhoud van het bestand op de FTP-server  
2. Selecteer de **een actie toevoegen** koppeling.  
   ![FTP-afbeelding 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Voer *FTP* om te zoeken naar alle acties met betrekking tot FTP.
4. Selecteer **FTP - bestandsinhoud ophalen** als de actie moet worden uitgevoerd wanneer een nieuwe of gewijzigde bestand wordt gevonden in de FTP-map.      
   ![FTP-afbeelding 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   De **bestandsinhoud ophalen** beheren wordt geopend. **Opmerking**: u wordt gevraagd uw logische app toegang tot uw FTP-server-serviceaccount als u nog niet gedaan eerder autoriseren.  
   ![FTP-afbeelding 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Selecteer de **bestand** control (de witruimte zich onder ** bestand ***). Hier kunt u een van de verschillende eigenschappen van de nieuwe of gewijzigde bestand gevonden op de FTP-server.  
6. Selecteer de **inhoud bestand** optie.  
   ![FTP-afbeelding 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Het besturingselement wordt bijgewerkt, ten teken dat de **FTP - bestandsinhoud ophalen** actie krijgt de *inhoud bestand* van het bestand nieuw of gewijzigd op de FTP-server.      
   ![FTP-afbeelding 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Sla uw werk op en voeg vervolgens een bestand naar de FTP-map voor het testen van uw werkstroom.    

De logische app op dit moment is geconfigureerd met een trigger voor het bewaken van een map op een FTP-server en de werkstroom starten als er een nieuw bestand of een gewijzigd bestand op de FTP-server. 

De logische app is ook geconfigureerd met een actie om op te halen van de inhoud van het bestand nieuw of gewijzigd.

U kunt nu een andere actie, zoals toevoegen de [SQL Server - invoegrij](connectors-create-api-sqlazure.md) actie de inhoud van het bestand nieuw of gewijzigd in een tabel met SQL-database invoegen.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

