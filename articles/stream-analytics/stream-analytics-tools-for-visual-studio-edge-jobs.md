---
title: Edge-taken in Azure Stream Analytics-hulpprogramma's voor Visual Studio
description: In dit artikel wordt beschreven hoe u schrijven, fouten opsporen en uw Stream Analytics Edge-taken met behulp van de Stream Analytics-hulpprogramma's voor Visual Studio maakt.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/13/2018
ms.openlocfilehash: 5dc90a1334b525c02be3eae2985900ab07cf2e05
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696245"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Stream Analytics Edge-taken met behulp van Visual Studio-hulpprogramma's ontwikkelen

In deze zelfstudie leert u hoe u Stream Analytics-hulpprogramma's voor Visual Studio gebruikt om te schrijven, fouten opsporen en uw Stream Analytics Edge-taken maken. Nadat u maken en testen van de taak, gaat u naar de Azure portal om te implementeren op uw apparaten. 

## <a name="prerequisites"></a>Vereisten

U moet de volgende vereisten om deze zelfstudie te voltooien:

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), of [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise- (Ultimate/Premium), Professional- en Community-edities worden ondersteund. De Express-editie wordt niet ondersteund.  

* Ga als volgt de [installatie-instructies](stream-analytics-tools-for-visual-studio-edge-jobs.md) Stream Analytics-hulpprogramma's voor Visual Studio installeren.
 
## <a name="create-a-stream-analytics-edge-project"></a>Een Stream Analytics Edge-project maken 

Selecteer in Visual Studio, **bestand** > **nieuw** > **Project**. Navigeer naar de **sjablonen** lijst aan de linkerkant > Vouw **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Azure Stream Analytics Edge-toepassing**. Geef een naam voor uw project en selecteer op de naam, locatie en oplossing **OK**.

![Nieuwe Edge-project](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Nadat het project wordt gemaakt, gaat u naar de **Solution Explorer** om de mappenhiërarchie weer te geven.

![Solution explorer-weergave](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Kies het juiste abonnement

1. Vanuit uw Visual Studio **weergave** in het menu **Server Explorer**.  

2. Klik met de rechtermuisknop op **Azure** > Selecteer **verbinding maken met Microsoft Azure-abonnement** > en zich vervolgens aanmelden met uw Azure-account.

## <a name="define-inputs"></a>Invoer definiëren

1. Uit de **Solution Explorer**, vouw de **invoer** knooppunt ziet u een invoer met de naam **EdgeInput.json**. Dubbelklik om de instellingen ervan weer te geven.  

2. Zorg ervoor dat het brontype is ingesteld op **gegevens Stream** > bron is ingesteld op **Edge Hub** > serialisatie-indeling voor gebeurtenissen die zijn ingesteld op **Json** > en codering is ingesteld op **UTF8**. (Optioneel) u kunt de naam van de **invoer Alias**, laten we dit zo voor dit voorbeeld. Als u de naam van de ingevoerde alias, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan.  
   ![Configuratie van invoer](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Uitvoer definiëren

1. Uit de **Solution Explorer**, vouw de **uitvoer** knooppunt ziet u uitvoer met de naam **EdgeOutput.json**. Dubbelklik om de instellingen ervan weer te geven.  

2. Zorg ervoor dat de Sink is ingesteld om te selecteren **Edge Hub** > serialisatie-indeling voor gebeurtenissen die zijn ingesteld op **Json** > en codering is ingesteld op **UTF8** > en indeling is ingesteld op  **Matrix**. (Optioneel) u kunt de naam van de **uitvoeralias**, laten we dit zo voor dit voorbeeld. Als u de naam van de uitvoeralias, gebruikt u de naam die u hebt opgegeven bij het definiëren van de query. Selecteer **Opslaan** om de instellingen op te slaan. 
   ![Uitvoer configureren](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>De transformatiequery definiëren

Stream Analytics-taken geïmplementeerd in de Edge-omgevingen ondersteunen de meeste [: referentie voor querytaal voor Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396), maar de volgende bewerkingen zijn nog niet ondersteund voor Edge-taken: 


|**Categorie**  | **Opdracht**  |
|---------|---------|
|Georuimtelijke operators |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Andere operatoren | <ul><li>PARTITION BY</li><li>TIJDSTEMPEL DOOR VIA</li><li>AFZONDERLIJKE</li><li>De expressieparameter in de COUNT-operator</li><li>Wachttijden van microseconden in datum- en tijdfuncties</li><li>JavaScript-UDA (deze functie is nog in preview voor taken die zijn geïmplementeerd in de cloud)</li></ul>   |

Wanneer u een Edge-taak in de portal maakt, waarschuwt de compiler automatisch u als u geen van een ondersteunde operator gebruikmaakt.

Vanuit uw Visual Studio, kunt u de volgende transformatiequery definiëren in de query-editor (**script.asaql bestand**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>De taak lokaal testen

Als u wilt testen van de query moet u lokaal de voorbeeldgegevens uploaden. U kunt voorbeeldgegevens krijgen door het downloaden van de registratie van de [GitHub-opslagplaats](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) en sla deze op uw lokale computer. 

1. Het uploaden van voorbeeldgegevens, > Klik met de rechtermuisknop op **EdgeInput.json** bestand > Kies **lokale invoer toevoegen**  

2. In het pop-upvenster > **Bladeren** de voorbeeldgegevens uit het lokale pad > Selecteer **opslaan**.
   ![Configuratie van lokale invoer](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Een bestand met de naam **local_EdgeInput.json** wordt automatisch toegevoegd aan de map invoer.  
4. u kunt deze lokaal uitvoeren of verzenden naar Azure. Voor het testen van de query > Selecteer **lokaal uitvoeren**.  
   ![Opties voor het uitvoeren](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. Het opdrachtpromptvenster toont de status van de taak. Wanneer de taak is uitgevoerd, maakt een map die als '2018-02-23-11-31-42 eruitziet' in het pad voor de project 'Visual Studio 2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42'. Navigeer naar het pad om de resultaten in de lokale map weer te geven:

   U kunt ook aanmelden bij de Azure-portal en controleer of de taak is gemaakt. 

   ![Resultaat-map](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>Verzenden van de taak naar Azure

1. Voordat u de taak naar Azure verzenden, moet u verbinding met uw Azure-abonnement. Open **Server Explorer** > Klik met de rechtermuisknop op **Azure** > **verbinding maken met Microsoft Azure-abonnement** > aanmelden bij uw Azure-abonnement.  

2. Als u wilt verzenden van de taak naar Azure, gaat u naar de query-editor > Selecteer **verzenden naar Azure**.  

3. Een pop-upvenster wordt geopend, waarin u kunt kiezen voor het bijwerken van een bestaande taak voor Edge of een nieuwe maken. Wanneer u een bestaande taak bijwerken, taakconfiguratie, in dit scenario wordt vervangen, publiceert u een nieuwe taak. Selecteer **een nieuwe Azure Stream Analytics-taak maken** > Voer een naam voor uw taak iets zoals **MyASAEdgeJob** > Kies de vereiste **abonnement**, **Resourcegroep**, en **locatie** > Selecteer **indienen**.

   ![Verzenden naar Azure](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Nu uw Stream Analytics Edge-taak is gemaakt, kunt u verwijzen naar de [taken uitvoeren in IoT Edge-zelfstudie](stream-analytics-edge.md) voor informatie over het implementeren op uw apparaten. 

## <a name="manage-the-job"></a>Beheren van de taak 

U kunt de status van de taak en het taakdiagram in Server Explorer bekijken. Uit de **Server Explorer** > **Stream Analytics** > Vouw het abonnement en de resourcegroep waar u het Edge-taak geïmplementeerd > u kunt de MyASAEdgejob met de status weergeven **Gemaakt**. Vouw het taakknooppunt van uw uit en dubbelklik erop om de taakweergave te openen.

![Opties voor server explorer](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
Het venster taak biedt u bewerkingen zoals het vernieuwen van de taak, de taak, de taak openen vanuit Azure portal enzovoort wordt verwijderd.

![Taakdiagram en andere opties](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over Azure Iot Edge](../iot-edge/about-iot-edge.md)
* [ASA on IoT Edge-zelfstudie](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Feedback verzenden naar het team met behulp van deze enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
