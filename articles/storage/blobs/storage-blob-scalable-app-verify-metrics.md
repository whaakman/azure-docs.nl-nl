---
title: Controleer of de doorvoer en latentie metrische gegevens voor een opslagaccount in de Azure portal | Microsoft Docs
description: Informatie over het controleren van de doorvoer en latentie metrische gegevens voor een opslagaccount in de portal.
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
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: b3102bd4e40e10fe88c12295794da37e359c56f1
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="verify-throughput-and-latency-metrics-for-a-storage-account"></a>Controleer of de doorvoer en latentie metrische gegevens voor een opslagaccount

Deze zelfstudie wordt vier en het laatste gedeelte van een serie. In de vorige zelfstudies hebt u geleerd hoe uploaden en downloaden van larges hoeveelheden willekeurige gegevens naar Azure storage-account. Deze zelfstudie leert u hoe u kunt metrische gegevens om weer te geven van de doorvoer en latentie in de Azure portal.

In de vierde deel van de reeks leert u hoe:

> [!div class="checklist"]
> * Grafieken in de Azure-portal configureren
> * Metrische gegevens over doorvoer en latentie controleren

[Metrische gegevens Azure storage](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Azure monitor gebruikt om een uniform overzicht van de prestaties en beschikbaarheid van uw opslagaccount.

## <a name="configure-metrics"></a>Metrische gegevens configureren

Navigeer naar **metrische gegevens (preview)** onder **instellingen** in uw opslagaccount.

Kies Blob uit de **SUB SERVICE** vervolgkeuzelijst.

Onder **METRIEK**, selecteer een van de metrische gegevens gevonden in de volgende tabel:

De volgende metrische gegevens geven u een idee van de latentie en de doorvoer van de toepassing. De metrische gegevens die u in de portal configureert zijn in 1 minuut gemiddelden. Als een transactie voltooid in het midden van een minuut is dat de minuut gegevens halfed voor de gemiddelde. De bewerkingen uploaden en downloaden is een time-out opgetreden in de toepassing en mits u de uitvoer van de werkelijke hoeveelheid tijd die nodig was om te uploaden en downloaden de bestanden. Deze informatie kan worden gebruikt in combinatie met de portal metrische gegevens voor een volledig begrip doorvoer.

|Gegevens|Definitie|
|---|---|
|**Geslaagde E2E-latentie**|De gemiddelde end-to-end-latentie van geslaagde aanvragen voor een opslagservice of de opgegeven API-bewerking. Deze waarde bevat de vereiste verwerkingstijd in Azure Storage te lezen van de aanvraag, antwoord verzenden en ontvangen van de bevestiging van het antwoord.|
|**Geslaagde Server latentie**|De gemiddelde tijd die wordt gebruikt voor het verwerken van de aanvraag is gelukt door Azure Storage. Deze waarde is niet de netwerklatentie die is opgegeven in SuccessE2ELatency opgenomen. |
|**Transacties**|Het aantal aanvragen voor een opslagservice of de opgegeven API-bewerking. Dit is inclusief geslaagde en mislukte aanvragen als aanvragen die fouten heeft geproduceerd. In het voorbeeld wordt is de blokgrootte ingesteld op 100 MB. In dit geval een transactie wordt beschouwd als elk blok van 100 MB.|
|**Inkomend**|De hoeveelheid inkomende gegevens. Dit is inclusief de inkomende gegevens via een externe client in Azure Storage, evenals een inkomend binnen Azure. |
|**Uitgaande**|De hoeveelheid uitgaande gegevens. Dit is inclusief uitgaande van een externe client in Azure Storage, evenals een uitgaande binnen Azure. Dit nummer is daardoor niet factureerbare uitgaande doorgevoerd. |

Selecteer **afgelopen 24 uur (automatisch)** naast **tijd**. Kies **afgelopen uur** en **minuut** voor **tijdgranulatie**, klikt u vervolgens op **toepassen**.

![Metrische gegevens Storage-account](./media/storage-blob-scalable-app-verify-metrics/figure1.png)

Grafieken meer dan een waarde die aan hen toegewezen kunnen hebben, maar het toewijzen van meer dan één metriek verhindert groep per dimensie.

## <a name="dimensions"></a>Dimensies

[Dimensies](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#metrics-dimensions) worden gebruikt om te zoeken dieper in de grafieken en krijgt u meer gedetailleerde informatie. Andere metrische gegevens hebben verschillende dimensies. Een dimensie die beschikbaar is, is de **API-naam** dimensie. Deze dimensie wordt verbroken buiten de grafiek in elke afzonderlijke API-aanroep. De eerste afbeelding hieronder toont een voorbeeld van grafiek van totaal aantal transacties voor een opslagaccount. De tweede afbeelding toont dezelfde grafiek maar met de API naam dimensie is geselecteerd. Zoals u ziet, wordt elke transactie vermeld geeft meer details in het aantal aanvragen zijn gemaakt met de naam van de API.

![Metrische gegevens Storage-account - transacties zonder een dimensie](./media/storage-blob-scalable-app-verify-metrics/transactionsnodimensions.png)

![Metrische gegevens Storage-account - transacties](./media/storage-blob-scalable-app-verify-metrics/transactions.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de virtuele machine niet meer nodig hebt, verwijdert u de resourcegroep, de machine zelf én alle gerelateerde resources. De resourcegroep voor de virtuele machine selecteren en op verwijderen om dit te doen.

## <a name="next-steps"></a>Volgende stappen

In de vierde deel van de reeks, hebt u geleerd over het weergeven van metrische gegevens voor de Voorbeeldoplossing, zoals het:

> [!div class="checklist"]
> * Grafieken in de Azure-portal configureren
> * Metrische gegevens over doorvoer en latentie controleren

Volg deze link om te zien van vooraf samengestelde opslag voorbeelden.

> [!div class="nextstepaction"]
> [Voorbeelden van Azure-opslag-script](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-blob-scalable-app-download-files.md