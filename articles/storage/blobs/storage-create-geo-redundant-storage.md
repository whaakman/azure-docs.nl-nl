---
title: Toepassingsgegevens maximaal beschikbaar maken in Azure | Microsoft Docs
description: Geografisch redundante opslag met leestoegang gebruiken om uw toepassingsgegevens maximaal beschikbaar te maken
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: ce72c1a68c1dbe5cede33dd42adc1b002a81326e
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Uw toepassingsgegevens maximaal beschikbaar maken met Azure-opslag

Deze zelfstudie is deel één van een serie. Hier ziet u hoe u uw toepassingsgegevens maximaal beschikbaar maakt in Azure. Wanneer u klaar bent, hebt u een consoletoepassing die een blob ophaalt en uploadt naar een [geografisch redundant](../common/storage-redundancy.md#read-access-geo-redundant-storage) opslagaccount met leestoegang (RA-GRS). RA-GRS werkt door transacties te repliceren van de primaire naar de secundaire regio. Dit replicatieproces zorgt ervoor dat de gegevens in de secundaire regio uiteindelijk consistent zijn. De toepassing gebruikt het [Circuitonderbrekers](/azure/architecture/patterns/circuit-breaker)patroon om te bepalen met welk eindpunt verbinding moet worden gemaakt. De toepassing schakelt over naar het secundaire eindpunt wanneer er een fout wordt gesimuleerd.

In deel 1 van de reeks leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Het voorbeeld downloaden
> * De verbindingsreeks instellen
> * De consoletoepassing uitvoeren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:
 
# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
  - **Azure-ontwikkeling**

  ![Azure-ontwikkeling (onder Web en Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

* (Optioneel) [Fiddler](https://www.telerik.com/download/fiddler) downloaden en installeren
 
# <a name="python-tabpython"></a>[Python] (#tab/python) 

* [Python](https://www.python.org/downloads/) installeren
* [Azure Storage-SDK voor Python](https://github.com/Azure/azure-storage-python) downloaden en installeren
* (Optioneel) [Fiddler](https://www.telerik.com/download/fiddler) downloaden en installeren

---

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Een opslagaccount biedt een unieke naamruimte voor het opslaan en openen van uw Azure Storage-gegevensobjecten.

Volg deze stappen om een account voor geografisch redundante opslag met leestoegang te maken:

1. Selecteer de knop **Een resource maken** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Opslag** op de pagina **Nieuw** en selecteer **Opslagaccount - Blob, File, Table, Queue** onder **Aanbevolen**.
3. Vul het formulier voor het opslagaccount in met de informatie uit de volgende afbeelding en selecteer **Maken**:

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Naam** | mystorageaccount | Een unieke naam voor uw opslagaccount |
   | **Implementatiemodel** | Resource Manager  | Resource Manager bevat de nieuwste functies.|
   | **Type account** | StorageV2 | Zie [Typen opslagaccounts](../common/storage-introduction.md#types-of-storage-accounts) voor meer informatie over de verschillende typen accounts |
   | **Prestaties** | Standard | Standard is voldoende voor het voorbeeldscenario. |
   | **Replicatie**| Geografisch redundante opslag met leestoegang (RA-GRS) | Dit is nodig om het voorbeeld te laten werken. |
   |**Veilige overdracht vereist** | Uitgeschakeld| Veilige overdracht is niet vereist voor dit scenario. |
   |**Abonnement** | uw abonnement |Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   |**ResourceGroup** | myResourceGroup |Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   |**Locatie** | VS - oost | Kies een locatie. |

![opslagaccount maken](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) en pak (unzip) het bestand storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip uit. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een consoletoepassing.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git 
```
# <a name="python-tabpython"></a>[Python] (#tab/python) 

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) en pak (unzip) het bestand storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip uit. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een basis Python-toepassing.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```
---


## <a name="set-the-connection-string"></a>De verbindingsreeks instellen

In de toepassing moet u de verbindingsreeks voor uw opslagaccount opgeven. U wordt aangeraden deze verbindingsreeks binnen een omgevingsvariabele op te slaan op de lokale computer waarop de toepassing wordt uitgevoerd. Volg een van de onderstaande voorbeelden afhankelijk van uw besturingssysteem voor het maken van de omgevingsvariabele.

Ga in Azure Portal naar uw opslagaccount. Selecteer bij **Instellingen** in uw opslagaccount de optie **Toegangssleutels**. Kopieer de **verbindingsreeks** uit de primaire of secundaire sleutel. Vervang \<yourconnectionstring\> door uw werkelijke verbindingsreeks door afhankelijk van uw besturingssysteem een van de volgende opdrachten uit te voeren. Met deze opdracht slaat u een omgevingsvariabele naar de lokale machine op. In Windows is de omgevingsvariabele pas beschikbaar wanneer u de **-opdrachtprompt** of shell die u gebruikt opnieuw laadt. Vervang **\<storageConnectionString\>** in het volgende voorbeeld:

# <a name="linux-tablinux"></a>[Linux] (#tab/linux) 
export storageconnectionstring=\<yourconnectionstring\> 

# <a name="windows-tabwindows"></a>[Windows] (#tab/windows) 
setx storageconnectionstring "\<yourconnectionstring\>"

---


## <a name="run-the-console-application"></a>De consoletoepassing uitvoeren

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)
Druk in Visual Studio op **F5** of selecteer **Start** om de foutopsporing van de toepassing te starten. Visual Studio herstelt automatisch ontbrekende NuGet-pakketten als dit zo is geconfigureerd. Ga naar [Installing and reinstalling packages with package restore](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) (Pakketten installeren en opnieuw installeren met pakketherstel) voor meer informatie.

Er wordt een consolevenster geopend en de toepassing wordt gestart. De toepassing uploadt de afbeelding **HelloWorld.png** vanuit de oplossing naar het opslagaccount. De toepassing controleert of de afbeelding is gerepliceerd naar het secundaire RA-GRS-eindpunt. Daarna wordt de afbeelding maximaal 999 keer gedownload. Elke leesbewerking wordt voorgesteld door een **P** of een **S**. **P** staat voor het primaire eindpunt en **S** voor het secundaire eindpunt.

![Console-app die wordt uitgevoerd](media/storage-create-geo-redundant-storage/figure3.png)

In de voorbeeldcode wordt de taak `RunCircuitBreakerAsync` in het bestand `Program.cs` gebruikt om een afbeelding met behulp van de [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet)-methode te downloaden vanuit het opslagaccount. Voorafgaand aan de download is een [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) gedefinieerd. De bewerkingscontext definieert de gebeurtenis-handlers. Deze worden geactiveerd wanneer een download is voltooid of wanneer een download is mislukt en opnieuw wordt uitgevoerd.

# <a name="python-tabpython"></a>[Python] (#tab/python) 
Als u de toepassing wilt uitvoeren op een terminal of opdrachtprompt, gaat u naar de map **circuitbreaker.py** en voert u `python circuitbreaker.py` in. De toepassing uploadt de afbeelding **HelloWorld.png** vanuit de oplossing naar het opslagaccount. De toepassing controleert of de afbeelding is gerepliceerd naar het secundaire RA-GRS-eindpunt. Daarna wordt de afbeelding maximaal 999 keer gedownload. Elke leesbewerking wordt voorgesteld door een **P** of een **S**. **P** staat voor het primaire eindpunt en **S** voor het secundaire eindpunt.

![Console-app die wordt uitgevoerd](media/storage-create-geo-redundant-storage/figure3.png)

In de voorbeeldcode wordt de methode `run_circuit_breaker` in het bestand `circuitbreaker.py` gebruikt om een afbeelding met behulp van de [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html)-methode te downloaden vanuit het opslagaccount. 

De functie Opnieuw van het opslagobject is ingesteld op een lineair beleid voor nieuwe pogingen. De functie Opnieuw bepaalt of een aanvraag opnieuw moet worden geprobeerd en geeft het aantal seconden op dat moet worden gewacht voordat opnieuw wordt geprobeerd de aanvraag uit te voeren. Stel de waarde van **retry\_to\_secondary** in op True als de aanvraag bij een volgende poging worden uitgevoerd naar het secundaire eindpunt als de eerste aanvraag naar het primaire eindpunt is mislukt. In de voorbeeldtoepassing is een aangepast beleid voor nieuwe pogingen gedefinieerd in de functie `retry_callback` van het opslagobject.
 
Vóór het downloaden zijn de functies [retry_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) en [response_callback](https://docs.microsoft.com/en-us/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) van het serviceobject gedefinieerd. Deze functies definiëren gebeurtenis-handlers die worden geactiveerd wanneer een download is voltooid of wanneer een download is mislukt en opnieuw wordt uitgevoerd.  

---

### <a name="retry-event-handler"></a>Gebeurtenis-handler opnieuw proberen

# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

De gebeurtenis-handler `OperationContextRetrying` wordt aangeroepen wanneer het downloaden van de afbeelding is mislukt en is ingesteld op Opnieuw proberen. Als het maximale aantal nieuwe pogingen dat in de toepassing is gedefinieerd, is bereikt, verandert de [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) van de aanvraag in `SecondaryOnly`. Deze instelling zorgt ervoor dat de toepassing de afbeelding probeert te downloaden van het secundaire eindpunt. Deze configuratie vermindert de tijd die nodig is om de afbeelding op te vragen omdat het primaire eindpunt niet oneindig opnieuw wordt geprobeerd.

In de voorbeeldcode wordt de taak `RunCircuitBreakerAsync` in het bestand `Program.cs` gebruikt om een afbeelding met behulp van de [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet)-methode te downloaden vanuit het opslagaccount. Voorafgaand aan de download is een [OperationContext](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) gedefinieerd. De bewerkingscontext definieert de gebeurtenis-handlers. Deze worden geactiveerd wanneer een download is voltooid of wanneer een download is mislukt en opnieuw wordt uitgevoerd.
 
```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

# <a name="python-tabpython"></a>[Python] (#tab/python) 
De gebeurtenis-handler `retry_callback` wordt aangeroepen wanneer het downloaden van de afbeelding is mislukt en is ingesteld op Opnieuw proberen. Als het maximale aantal nieuwe pogingen dat in de toepassing is gedefinieerd, is bereikt, verandert de [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) van de aanvraag in `SECONDARY`. Deze instelling zorgt ervoor dat de toepassing de afbeelding probeert te downloaden van het secundaire eindpunt. Deze configuratie vermindert de tijd die nodig is om de afbeelding op te vragen omdat het primaire eindpunt niet oneindig opnieuw wordt geprobeerd.  

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

---


### <a name="request-completed-event-handler"></a>Gebeurtenis-handler Aanvraag voltooid
 
# <a name="net-tabdotnet"></a>[.NET] (#tab/dotnet)

De gebeurtenis-handler `OperationContextRequestCompleted` wordt aangeroepen wanneer het downloaden van de afbeelding is geslaagd. Als de toepassing het secundaire eindpunt gebruikt, blijft de toepassing dit eindpunt maximaal 20 keer gebruiken. Na 20 keer stelt de toepassing de [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) weer in op `PrimaryThenSecondary` en wordt het primaire eindpunt weer geprobeerd. Als een aanvraag is geslaagd, blijft de toepassing van het primaire eindpunt lezen.
 
```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

# <a name="python-tabpython"></a>[Python] (#tab/python) 

De gebeurtenis-handler `response_callback` wordt aangeroepen wanneer het downloaden van de afbeelding is geslaagd. Als de toepassing het secundaire eindpunt gebruikt, blijft de toepassing dit eindpunt maximaal 20 keer gebruiken. Na 20 keer stelt de toepassing de [LocationMode](https://docs.microsoft.com/en-us/python/api/azure.storage.common.models.locationmode?view=azure-python) weer in op `PRIMARY` en wordt het primaire eindpunt weer geprobeerd. Als een aanvraag is geslaagd, blijft de toepassing van het primaire eindpunt lezen.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

---

## <a name="next-steps"></a>Volgende stappen

In deel één van de serie hebt u geleerd hoe u een toepassing maximaal beschikbaar maakt met RA-GRS-opslagaccounts. U hebt bijvoorbeeld het volgende geleerd:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Het voorbeeld downloaden
> * De verbindingsreeks instellen
> * De consoletoepassing uitvoeren

Ga naar deel twee van de serie voor informatie over hoe u een fout simuleert en uw toepassing dwingt om het secundaire RA-GRS-eindpunt te gebruiken.

> [!div class="nextstepaction"]
> [Een fout in de verbinding met het primaire eindpunt van de opslag simuleren](storage-simulate-failure-ragrs-account-app.md)
