---
title: Verbinding maken met de FTP-server - Azure Logic Apps | Microsoft Docs
description: Maken, bewaken en beheren van bestanden op een FTP-server met Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/22/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 4355a767d2ecd500662cdf4522e8a7e12de86b80
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866148"
---
# <a name="get-started-with-the-ftp-connector"></a>Aan de slag met de FTP-connector
De FTP-connector gebruiken om te controleren, beheren en bestanden op een FTP-server maken. 

Gebruik [connectors](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [het maken van een logische app nu](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-ftp"></a>Verbinding maken met FTP
Voordat uw logische app krijgt een service tot toegang, moet u eerst maken een *verbinding* naar de service. Een [verbinding](connectors-overview.md) biedt connectiviteit tussen een logische app en een andere service.  

### <a name="create-a-connection-to-ftp"></a>Maak een verbinding met FTP
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>Een trigger FTP gebruiken
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!IMPORTANT]
> De FTP-connector vereist een FTP-server die toegankelijk is vanaf het Internet en is geconfigureerd voor gebruik met de passieve modus. De FTP-connector is ook **niet compatibel met impliciete FTPS (FTP via SSL)**. De FTP-connector biedt alleen ondersteuning voor expliciete FTPS (FTP via SSL).  
> 
> 

In dit voorbeeld ziet u het gebruik van de **FTP - wanneer een bestand wordt toegevoegd of gewijzigd** activeren voor het starten van een werkstroom voor logische Apps wanneer een bestand wordt toegevoegd aan of gewijzigd op een FTP-server. U kunt deze trigger in een enterprise-bijvoorbeeld gebruiken voor het bewaken van een FTP-map voor nieuwe bestanden die staan voor orders van klanten.  U kunt vervolgens een actie van FTP-connector, zoals **bestandsinhoud ophalen** om op te halen van de inhoud van de volgorde voor verdere verwerking en opslag in de bestellingsdatabase.

1. Voer *ftp* in het zoekvak in de ontwerper van logische apps schakelt u vervolgens de **FTP - wanneer een bestand wordt toegevoegd of gewijzigd** trigger   
   ![FTP-triggerafbeelding 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   De **wanneer een bestand wordt toegevoegd of gewijzigd** besturingselement wordt geopend  
   ![FTP-triggerafbeelding 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Selecteer de **...**  zich aan de rechterkant van het besturingselement. Hiermee opent u de map-besturingselement voor datumkiezer  
   ![FTP-triggerafbeelding 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Selecteer de **>** (pijl-rechts) en Ga naar de map die u wilt controleren op nieuwe of gewijzigde bestanden. Selecteer de map en u ziet dat de map wordt nu weergegeven in de **map** besturingselement.  
   ![FTP-triggerafbeelding 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Op dit moment is uw logische app geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart wanneer een bestand wordt gewijzigd of in de specifieke FTP-map gemaakt. 

> [!NOTE]
> Voor een logische app wilt gebruiken, moet het ten minste één trigger en één actie bevatten. Volg de stappen in de volgende sectie een actie toevoegen.  
> 
> 

## <a name="use-a-ftp-action"></a>Een actie FTP gebruiken
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Nu dat u een trigger hebt toegevoegd, volg deze stappen om een actie toevoegen die de inhoud van het nieuwe of gewijzigde bestand door de trigger wordt ophalen.    

1. Selecteer **+ nieuwe stap** om toe te voegen van de actie voor het ophalen van de inhoud van het bestand op de FTP-server  
2. Selecteer de **een actie toevoegen** koppeling.  
   ![FTP-afbeelding 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Voer *FTP* om te zoeken naar alle acties die verband houden met FTP.
4. Selecteer **FTP: ophalen van bestandsinhoud** als de actie te ondernemen wanneer een nieuwe of gewijzigde bestand wordt gevonden in de FTP-map.      
   ![FTP-afbeelding 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   De **bestandsinhoud ophalen** beheren wordt geopend. **Houd er rekening mee**: u wordt gevraagd om uw logische app toegang krijgt tot uw FTP-server-account als u niet hebt gedaan, eerder te autoriseren.  
   ![FTP-afbeelding 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Selecteer de **bestand** control (de witruimte zich bevindt onder ** bestand ***). Hier kunt u een van de verschillende eigenschappen van het nieuwe of gewijzigde bestand gevonden op de FTP-server.  
6. Selecteer de **bestandsinhoud** optie.  
   ![FTP-afbeelding 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Het besturingselement wordt bijgewerkt, ten teken dat de **FTP: ophalen van bestandsinhoud** actie krijgt de *bestandsinhoud* van het bestand nieuw of gewijzigd op de FTP-server.      
   ![FTP-afbeelding 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Sla uw werk en vervolgens een bestand toevoegt aan de FTP-map voor het testen van uw werkstroom.    

Op dit moment is de logische app geconfigureerd met een trigger voor het controleren van een map op een FTP-server en de werkstroom starten wanneer er een nieuw bestand of een gewijzigd bestand op de FTP-server. 

De logische app is ook geconfigureerd met een actie om op te halen van de inhoud van het bestand nieuw of gewijzigd.

U kunt nu nog een actie zoals toevoegen de [SQL Server - invoegrij](connectors-create-api-sqlazure.md) actie de inhoud van de nieuwe of gewijzigde bestand invoegen in een SQL-databasetabel.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md)

