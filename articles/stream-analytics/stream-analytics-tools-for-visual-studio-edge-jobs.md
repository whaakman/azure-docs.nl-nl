---
title: Edge-taken in Azure Stream Analytics-tools voor Visual Studio
description: In dit artikel wordt beschreven hoe auteur, fouten opsporen en het maken van uw Stream Analytics rand taken met de Stream Analytics-tools voor Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/13/2018
ms.openlocfilehash: ec916ea2104df9d694aad2462df7ca5a487017f2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907755"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Ontwikkelen met Visual Studio tools rand van Stream Analytics-taken

In deze zelfstudie leert u hoe u met Stream Analytics-tools voor Visual Studio ontwerpen en maken van de rand van Stream Analytics-taken fouten opsporen in. Nadat u maken en testen van de taak, kunt u gaan naar de Azure portal om te implementeren op uw apparaten. 

## <a name="prerequisites"></a>Vereisten

U moet de volgende vereisten om deze zelfstudie te voltooien:

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), of [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional en Community-edities worden ondersteund. Express edition wordt niet ondersteund.  

* Ga als volgt de [installatie-instructies](stream-analytics-tools-for-visual-studio-edge-jobs.md) Stream Analytics-hulpprogramma's voor Visual Studio installeren.
 
## <a name="create-a-stream-analytics-edge-project"></a>Een Stream Analytics Edge-project maken 

Selecteer in Visual Studio **bestand** > **nieuw** > **Project**. Navigeer naar de **sjablonen** lijst aan de linkerkant > Vouw **Azure Stream Analytics** > **Stream Analytics rand**  >   **Azure Stream Analytics rand toepassing**. Geef een naam voor uw project en selecteer op de naam, locatie en oplossing **OK**.

![Nieuwe Edge-project](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Nadat het project wordt gemaakt, gaat u naar de **Solution Explorer** de mappenhiërarchie weergeven.

![Weergave in Solution explorer](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Kies het juiste abonnement

1. Vanuit uw Visual Studio **weergave** selecteert u **Server Explorer**.  

2. Klik met de rechtermuisknop op **Azure** > Selecteer **verbinding maken met Microsoft Azure-abonnement** > en meldt u zich aan met uw Azure-account.

## <a name="define-inputs"></a>Invoer definiëren

1. Van de **Solution Explorer**, vouw de **invoer** knooppunt ziet u een invoer met de naam **EdgeInput.json**. Dubbelklik om de instellingen weer te geven.  

2. Zorg ervoor dat het Type gegevensbron is ingesteld op **gegevensstroom** > bron is ingesteld op **rand Hub** > gebeurtenis serialisatie-indeling is ingesteld op **Json** > en codering is ingesteld op **UTF8**. U kunt desgewenst wijzigen de **invoer Alias**, gaan we ongewijzigd laten in dit voorbeeld is. Als u de naam van de invoeralias wijzigen, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan.  
   ![Invoer-configuratie](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Uitvoer definiëren

1. Van de **Solution Explorer**, vouw de **uitvoer** knooppunt ziet u uitvoer met de naam **EdgeOutput.json**. Dubbelklik om de instellingen weer te geven.  

2. Zorg ervoor dat de Sink is ingesteld om te selecteren **rand Hub** > gebeurtenis serialisatie-indeling is ingesteld op **Json** > en codering is ingesteld op **UTF8** > en de indeling is ingesteld op  **Matrix**. U kunt desgewenst wijzigen de **Uitvoeraliassen**, gaan we ongewijzigd laten is voor dit voorbeeld. Als u de naam van de uitvoeralias wijzigen, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan. 
   ![Configuratie van de uitvoer](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Definieer de query voor transformatie

Stream Analytics-taken is geïmplementeerd in de Edge-omgevingen ondersteunen de meeste [Stream Analytics Query Language reference](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396), maar de volgende bewerkingen worden nog niet ondersteund voor rand taken: 


|**Categorie**  | **Opdracht**  |
|---------|---------|
|Georuimtelijke operators |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Andere operatoren | <ul><li>PARTITIONEREN</li><li>TIJDSTEMPEL DOOR VIA</li><li>DISTINCT</li><li>De expressieparameter in de COUNT-operator</li><li>Microseconden in datum- en tijdfuncties</li><li>JavaScript UDA (deze functie is nog steeds in preview voor taken die zijn geïmplementeerd in de cloud)</li></ul>   |

Wanneer u een Edge-taak in de portal maakt, wordt de compiler automatisch u gewaarschuwd als u geen van een ondersteunde operator gebruikmaakt.

Definieer vanuit uw Visual Studio de volgende transformatie-query in de query-editor (**script.asaql bestand**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>De taak lokaal testen

Als u wilt testen van de query moet u lokaal door de voorbeeldgegevens uploaden. U kunt de voorbeeldgegevens ophalen door het downloaden van gegevens van registratie van de [GitHub-opslagplaats](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) en sla deze op uw lokale computer. 

1. Voor het uploaden van voorbeeldgegevens > Klik met de rechtermuisknop op **EdgeInput.json** bestand > Kies **lokale invoer toevoegen**  

2. In het pop-upvenster > **Bladeren** de voorbeeldgegevens uit uw lokale pad > Selecteer **opslaan**.
   ![Lokale invoer configuratie](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Een bestand met de naam **local_EdgeInput.json** wordt automatisch toegevoegd aan de map van uw invoer.  
4. u kunt deze lokaal uitvoeren of verzenden naar Azure. Voor het testen van de query > Selecteer **lokaal uitvoeren**.  
   ![Opties uitvoeren](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. Het opdrachtpromptvenster toont de status van de taak. Wanneer de taak met succes wordt uitgevoerd, wordt een map ziet als '2018-02-23-11-31-42 eruit' gemaakt in uw project pad naar 'Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42'. Ga naar het pad naar de map de resultaten weergeven in de lokale map:

   U kunt ook aanmelden bij de Azure-portal en controleren of de taak is gemaakt. 

   ![Resultaat-map](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>Verzenden van de taak naar Azure

1. Voordat u de taak Azure verzendt, moet u verbinding met uw Azure-abonnement. Open **Server Explorer** > Klik met de rechtermuisknop op **Azure** > **verbinding maken met Microsoft Azure-abonnement** > Meld u bij uw Azure-abonnement.  

2. Als u de taak naar Azure, gaat u naar de query-editor > Selecteer **verzenden naar Azure**.  

3. Een pop-upvenster wordt geopend, waarin u een bestaande taak van de rand bijwerken of maak een nieuwe kunt kiezen. Wanneer u een bestaande taak bijwerken, taakconfiguratie, in dit scenario wordt vervangen, publiceert u een nieuwe taak. Selecteer **maken van een nieuwe Azure Stream Analytics-taak** > Voer een naam voor uw project als **MyASAEdgeJob** > Kies de vereiste **abonnement**, **Resourcegroep**, en **locatie** > Selecteer **indienen**.

   ![Verzenden naar Azure](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Nu uw rand van Stream Analytics-taak is gemaakt, kunt u verwijzen naar de [taken uitvoeren op de rand van de IoT-zelfstudie](stream-analytics-edge.md) voor informatie over het implementeren op uw apparaten. 

## <a name="manage-the-job"></a>Beheren van de taak 

U kunt de status van de taak en het diagram van de taak in Server Explorer weergeven. Van de **Server Explorer** > **Stream Analytics** > Vouw het abonnement en de resourcegroep waar u de taak rand geïmplementeerd > u kunt de MyASAEdgejob met de status weergeven **Gemaakt**. Vouw het knooppunt van uw project en dubbelklik erop om de taakweergave te openen.

![Server explorer-opties](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
Het venster van de taak weergeven biedt u bewerkingen zoals het vernieuwen van de taak, het verwijderen van de taak, het openen van de taak van de Azure portal enzovoort.

![Diagram van de taak en andere opties](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure Iot-zijde](../iot-edge/how-iot-edge-works.md)
* [ASA op de rand van de IoT-zelfstudie](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Feedback verzenden naar het team met behulp van deze enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
