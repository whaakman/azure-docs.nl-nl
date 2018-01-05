---
title: Simuleer een fout bij het openen van redundante opslag met leestoegang in Azure | Microsoft Docs
description: Een fout opgetreden bij het openen van geografisch redundante opslag met leestoegang simuleren
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/05/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 151e875bd72598b0b788d68eee7fb186fca86f46
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Een fout bij het openen van redundante opslag met leestoegang simuleren

Deze zelfstudie maakt deel uit twee van een serie. In deze zelfstudie injecteren u een mislukte reactie met Fiddler voor aanvragen voor een [leestoegang geografisch redundante](../common/storage-redundancy.md#read-access-geo-redundant-storage) opslagaccount (RA-GRS) om te simuleren een fout en de toepassing lezen van het secundaire eindpunt hebben.

![Scenario-app](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Deel 2 van de reeks, leert u hoe:

> [!div class="checklist"]
> * Uitvoeren en de toepassing te onderbreken
> * Simuleer een fout
> * Het herstel van de primaire eindpunt simuleren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

* Download en installeer [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Voor het voltooien van deze zelfstudie, u moet de zelfstudie hebt voltooid vorige opslag: [uw toepassingsgegevens maximaal beschikbaar met Azure storage maken][previous-tutorial].

## <a name="launch-fiddler"></a>Fiddler starten

Open Fiddler, selecteer **regels** en **regels aanpassen**.

![Fiddler regels aanpassen](media/storage-simulate-failure-ragrs-account-app/figure1.png)

De ScriptEditor Fiddler gestart waarin de **SampleRules.js** bestand. Dit bestand wordt gebruikt om aan te passen Fiddler. Plak het volgende codevoorbeeld in de `OnBeforeResponse` functie. De nieuwe code uitgecommentarieerd om ervoor te zorgen dat de logica die het maakt niet onmiddellijk geïmplementeerd. Wanneer u klaar bent Selecteer **bestand** en **opslaan** uw wijzigingen op te slaan.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure2.png)

## <a name="start-and-pause-the-application"></a>Starten en de toepassing te onderbreken

Druk in Visual Studio op **F5** of selecteer **Start** foutopsporing van de toepassing te starten. Zodra de toepassing wordt gestart bij het lezen van het primaire eindpunt, drukt u op **een willekeurige toets** in het consolevenster onderbreken van de toepassing.

## <a name="simulate-failure"></a>Simuleer fout

Met de toepassing onderbroken u kunt nu opmerkingen bij de aangepaste regel is opgeslagen in Fiddler een vorige stap. Deze code voorbeeld zoekt aanvragen met de RA-GRS-opslagaccount en als het pad de naam van de afbeelding bevat `HelloWorld`, wordt een Antwoordcode van `503 - Service Unavailable`.

Navigeer naar Fiddler en selecteer **regels** -> **regels aanpassen...** .  Opmerking verwijderen uit de volgende regels, vervangen door `STORAGEACCOUNTNAME` met de naam van uw opslagaccount. Selecteer **bestand** -> **opslaan** uw wijzigingen op te slaan.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Als u wilt doorgaan met de toepassing, drukt u op **een willekeurige toets** .

Zodra de toepassing wordt gestart opnieuw uit te voeren, begint de aanvragen naar de primaire eindpunt mislukken. De toepassing probeert opnieuw verbinding maken met het primaire eindpunt 5 keer. Na de foutdrempelwaarde voor van vijf pogingen vraagt deze de installatiekopie van het secundaire alleen-lezen-eindpunt. Nadat de toepassing is haalt de installatiekopie van het 20 keer van het secundaire eindpunt, wordt de toepassing probeert te maken met het primaire eindpunt. Als het primaire eindpunt nog steeds niet bereikbaar is, hervat de toepassing bij het lezen van het secundaire eindpunt. Dit patroon is de [Circuitonderbreker](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) patroon beschreven in de vorige zelfstudie.

![Aangepaste regel plakken](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>Het herstel van de primaire eindpunt simuleren

Met de Fiddler aangepaste regelset in de vorige stap, mislukken aanvragen naar de primaire eindpunt. Om te simuleren het primaire eindpunt weer werkt, verwijdert u de logica invoeren de `503` fout.

De toepassing onderbreken, drukt u op **een willekeurige toets**.

### <a name="remove-the-custom-rule"></a>De aangepaste regel verwijderen

Navigeer naar Fiddler en selecteer **regels** en **regels aanpassen...** .  Opmerkingen of verwijderen van de aangepaste logica in de `OnBeforeResponse` functie, de standaardfunctie verlaten. Selecteer **bestand** en **opslaan** de wijzigingen wilt opslaan.

![Aangepaste regel verwijderen](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Als u klaar is, drukt u op **een willekeurige toets** hervatten van de toepassing. De toepassing blijft bij het lezen van het primaire eindpunt totdat dit bij 999 leesbewerkingen aankomt.

![Hervatten van de toepassing](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>Volgende stappen

In deel twee van de reeks, hebt u geleerd over simuleren geografisch redundante opslag met leestoegang zoals het testen van een mislukt:

> [!div class="checklist"]
> * Uitvoeren en de toepassing te onderbreken
> * Simuleer een fout
> * Het herstel van de primaire eindpunt simuleren

Volg deze link om te zien van vooraf samengestelde opslag voorbeelden.

> [!div class="nextstepaction"]
> [Voorbeelden van Azure-opslag-script](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md