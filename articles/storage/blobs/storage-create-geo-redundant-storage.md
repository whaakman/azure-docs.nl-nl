---
title: Toepassingsgegevens maximaal beschikbaar maken in Azure | Microsoft Docs
description: Geografisch redundante opslag met leestoegang gebruiken om maximaal beschikbaar maken voor uw toepassingsgegevens
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 10/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 547ca7843f53bd11fdb922af8e0ae77e38f813d9
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Uw toepassingsgegevens maximaal beschikbaar is met de Azure-opslag maken

Deze zelfstudie maakt deel uit een reeks. Deze zelfstudie laat zien hoe uw toepassingsgegevens maximaal beschikbaar maken in Azure. Wanneer u klaar bent, hebt u een consoletoepassing die uploadt en haalt een blob naar een [leestoegang geografisch redundante](../common/storage-redundancy.md#read-access-geo-redundant-storage) opslagaccount (RA-GRS). RA-GRS werkt met het repliceren van transacties van de primaire naar de secundaire regio. Dit replicatieproces zorgt ervoor dat de gegevens in de secundaire regio uiteindelijk consistent is. De toepassing gebruikt de [Circuitonderbreker](/azure/architecture/patterns/circuit-breaker.md) patroon om te bepalen welk eindpunt verbinding maken met. De toepassing wordt overgeschakeld naar de secundaire eindpunt wanneer er een fout wordt gesimuleerd.

Deel een van de reeks, leert u hoe:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Het voorbeeld downloaden
> * De verbindingsreeks instellen
> * Start de consoletoepassing

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
  - **Azure-ontwikkeling**

  ![Ontwikkelen van Azure (waaronder Web- en Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

* Download en installeer [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Een opslagaccount maken

Een opslagaccount biedt een unieke naamruimte voor het opslaan en toegang tot uw Azure storage-gegevensobjecten.

Volg deze stappen om een geografisch redundante opslag met leestoegang account te maken:

1. Selecteer de **nieuw** knop gevonden in de linkerbovenhoek van de Azure portal.

2. Selecteer **opslag** van de **nieuw** pagina en selecteer **opslagaccount - blob, bestand, tabel, wachtrij** onder **aanbevolen**.
3. Vul het formulier voor storage-account met de volgende informatie zoals weergegeven in de volgende afbeelding en selecteer **maken**:

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Naam** | mystorageaccount | Een unieke waarde voor uw storage-account |
   | **Implementatiemodel** | Resource Manager  | Resource Manager bevat de nieuwste functies.  |
   | **Account-type** | Algemeen doel | Zie voor meer informatie over de verschillende typen accounts [typen opslagaccounts](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Prestaties** | Standard | Standaard is voldoende voor het voorbeeldscenario. |
   | **Replicatie**| Geografisch redundante opslag met leestoegang (RA-GRS) | Dit is nodig voor het voorbeeld om te werken. |
   |**Veilige overdracht vereist** | Uitgeschakeld| Veilige overdracht is niet vereist voor dit scenario. |
   |**Abonnement** | Uw abonnement |Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   |**ResourceGroup** | myResourceGroup |Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   |**Locatie** | VS - oost | Kies een locatie. |

![Storage-account maken](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip).

Pak (uitpakken) het storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip bestand.
Het voorbeeldproject bevat een consoletoepassing.

## <a name="set-the-connection-string"></a>De verbindingsreeks instellen

Open de *storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs* consoletoepassing in Visual Studio.

Onder de **appSettings** knooppunt in de **App.config** bestand, vervang de waarde van de _StorageConnectionString_ met de verbindingsreeks van uw storage-account. Deze waarde wordt opgehaald door het selecteren van **toegangssleutels** onder **instellingen** in uw opslagaccount in de Azure portal. Kopieer de **verbindingsreeks** van de primaire of secundaire sleutel en plak deze in de **App.config** bestand. Selecteer **opslaan**, sla het bestand als u klaar.

![App-configuratiebestand](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Start de consoletoepassing

Druk in Visual Studio op **F5** of selecteer **Start** foutopsporing van de toepassing te starten. Visual studio automatisch Hiermee herstelt u ontbrekende Nuget-pakketten als geconfigureerd, gaat u naar naar [installeren en opnieuw installeren van pakketten met het terugzetten van het pakket](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) voor meer informatie. 

Een consolevenster wordt gestart en de toepassing wordt gestart met. De toepassing uploadt de **HelloWorld.png** afbeelding van de oplossing op het opslagaccount. Controleert de toepassing om te controleren of de installatiekopie van het eindpunt van de secundaire RA-GRS is gerepliceerd. Vervolgens wordt gestart tot 999 keer downloaden van de installatiekopie. Elke leesbewerking is representated door een **P** of een **S**. Waar **P** vertegenwoordigt het primaire eindpunt en **S** vertegenwoordigt het secundaire eindpunt.

![Console-app die wordt uitgevoerd](media/storage-create-geo-redundant-storage/figure3.png)

In de voorbeeldcode de `RunCircuitBreakerAsync` taak de `Program.cs` bestand wordt gebruikt om een installatiekopie te downloaden vanuit de storage-account met de [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet) methode. Vóór het downloaden van een [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) is gedefinieerd. De bewerking context definieert die worden geactiveerd wanneer het downloaden is voltooid of als een download mislukt en gebeurtenis-handlers opnieuw uit te voeren.

### <a name="retry-event-handler"></a>Probeer de gebeurtenis-handler

De `Operation_context_Retrying` gebeurtenis-handler wordt aangeroepen wanneer het downloaden van de afbeelding is mislukt en is ingesteld op rety. Als het maximale aantal nieuwe pogingen die zijn gedefinieerd in de toepassing zijn bereikt, de [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) van de aanvraag wordt gewijzigd naar `SecondaryOnly`. Deze instelling zorgt ervoor dat de toepassing probeert te downloaden van de installatiekopie van het secundaire eindpunt. Deze configuratie vermindert de tijd om aan te vragen van de afbeelding als het primaire eindpunt is niet voor onbepaalde tijd opnieuw.

```csharp
private static void Operation_context_Retrying(object sender, RequestEventArgs e)
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

### <a name="request-completed-event-handler"></a>Gebeurtenis-handler van aanvraag is voltooid

De `Operation_context_RequestCompleted` gebeurtenis-handler wordt aangeroepen wanneer het downloaden van de afbeelding geslaagd is. Als de toepassing van het secundaire eindpunt gebruikmaakt, blijft de toepassing dit eindpunt maximaal 20 keer gebruiken. Na 20 keer toepassing stelt het de [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) terug naar `PrimaryThenSecondary` en probeert u het primaire eindpunt opnieuw. Als een aanvraag voltooid is wordt de toepassing te lezen van het primaire eindpunt blijft.

```csharp
private static void Operation_context_RequestCompleted(object sender, RequestEventArgs e)
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

## <a name="next-steps"></a>Volgende stappen

In deel 1 van de reeks, hebt u geleerd over maximaal beschikbaar is met RA-GRS storage-accounts, zoals het maken van een toepassing:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Het voorbeeld downloaden
> * De verbindingsreeks instellen
> * Start de consoletoepassing

Ga naar deel twee van de reeks voor informatie over het simuleren van een storing en afdwingen van uw toepassing het secundaire RA-GRS-eindpunt te gebruiken.

> [!div class="nextstepaction"]
> [Een fout in verbinding met het eindpunt van uw primaire opslag simuleren](storage-simulate-failure-ragrs-account-app.md)
