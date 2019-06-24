---
title: Een Batch-taak met behulp van Azure Functions activeren
description: 'Zelfstudie: OCR van toepassing op gescande documenten als ze worden toegevoegd aan een opslag-blob'
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 05/30/2019
ms.author: peshultz
ms.custom: mvc
ms.openlocfilehash: d5a5197227ff62ca0c610e2c4e269480690d3faf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342029"
---
# <a name="tutorial-trigger-a-batch-job-using-azure-functions"></a>Zelfstudie: Een Batch-taak met behulp van Azure Functions activeren

In deze zelfstudie leert u hoe u kunt een Batch-taak met behulp van Azure Functions activeren. We nemen door een voorbeeld waarin de documenten die worden toegevoegd aan een Azure Storage blob-container optische tekenherkenning (hebt) toegepast via Azure Batch. Voor het stroomlijnen van de verwerking van OCR, zullen we een Azure-functie die wordt uitgevoerd van een Batch-OCR-taak telkens wanneer die een bestand wordt toegevoegd aan de blob-container configureren.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een Azure Batch-account en een gekoppeld Azure Storage-account. Zie [maken van een Batch-account](quick-create-portal.md#create-a-batch-account) voor meer informatie over het maken en koppelen van accounts.
* [Batch Explorer](https://azure.github.io/BatchExplorer/)
* [Azure-opslagverkenner](https://azure.microsoft.com/features/storage-explorer/)

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-batch-pool-and-batch-job-using-batch-explorer"></a>Een Batch-pool en een Batch-taak met behulp van Batch Explorer maken

In deze sectie maakt u Batch Explorer gebruikt om de Batch-pool en Batch-taak die wordt uitgevoerd OCR-taken te maken. 

### <a name="create-a-pool"></a>Een groep maken

1. Aanmelden bij de Batch Explorer met behulp van uw Azure-referenties.
1. Een pool maken door te selecteren **Pools** in de linkerzijbalk, dan zal de **toevoegen** knop boven het zoekformulier. 
    1. Kies een ID en weergavenaam. We gebruiken `ocr-pool` voor dit voorbeeld.
    1. Het schaaltype ingesteld op **vaste grootte**, en het aantal toegewezen knooppunten ingesteld op 3.
    1. Selecteer **Ubuntu 18.04-LTS** als het besturingssysteem.
    1. Kies `Standard_f2s_v2` als de grootte van de virtuele machine.
    1. De begintaak inschakelen en de opdracht toevoegen `/bin/bash -c "sudo update-locale LC_ALL=C.UTF-8 LANG=C.UTF-8; sudo apt-get update; sudo apt-get -y install ocrmypdf"`. Zorg ervoor dat u het instellen van de identiteit van de gebruiker als **taak standaardgebruiker (Admin)** , waarmee Begintaken om op te nemen van opdrachten met `sudo`.
    1. Selecteer **OK**.
### <a name="create-a-job"></a>Een taak maken

1. Een taak maken in de pool door te selecteren **taken** in de linkerzijbalk, dan zal de **toevoegen** knop boven het zoekformulier. 
    1. Kies een ID en weergavenaam. We gebruiken `ocr-job` voor dit voorbeeld.
    1. De pool ingesteld op `ocr-pool`, of de servernaam die u hebt gekozen voor uw pool.
    1. Selecteer **OK**.


## <a name="create-blob-containers"></a>Blob-containers maken

Hier kunt u blob-containers die opslaan van uw invoer en uitvoer bestanden voor de OCR-Batch-taak maken.

1. Aanmelden bij Storage Explorer met behulp van uw Azure-referenties.
1. Twee blob-containers (één voor de invoerbestanden, één voor de uitvoerbestanden) met behulp van het opslagaccount dat is gekoppeld aan uw Batch-account maken met de volgende stappen op [maken van een blob-container](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#create-a-blob-container).

In dit voorbeeld wordt de invoercontainer heet `input` en waar alle documenten zonder OCR in eerste instantie worden geüpload voor verwerking. De uitvoercontainer heet `output` en waarbij de Batch-taak verwerkte documenten met OCR schrijft.  
    * In dit voorbeeld noemen we onze invoercontainer `input`, en onze uitvoercontainer `output`.  
    * De invoercontainer is waarin alle documenten zonder OCR in eerste instantie worden geüpload.  
    * De uitvoercontainer is waar de Batch-taak documenten met OCR schrijft.  

Maak een shared access signature voor de uitvoercontainer in Storage Explorer. Dit doen door met de rechtermuisknop op de uitvoercontainer en selecteren **Shared Access Signature ophalen...** . Onder **machtigingen**, Controleer **schrijven**. Er zijn geen andere machtigingen nodig.  

## <a name="create-an-azure-function"></a>Een Azure-functie maken

In deze sectie maakt u de Azure-functie waarmee de OCR-Batch-taak wordt geactiveerd wanneer een bestand wordt geüpload naar de invoercontainer.

1. Volg de stappen in [een door Azure Blob storage geactiveerde functie maken](https://docs.microsoft.com/azure/azure-functions/functions-create-storage-blob-triggered-function) om een functie te maken.
    1. Wanneer u hierom wordt gevraagd voor een opslagaccount, gebruikt u hetzelfde opslagaccount die u aan uw Batch-account gekoppeld.
    1. Voor **runtimestack**, kiest u .NET. We onze functie schrijft C# gebruikmaken van de Batch .NET-SDK.
1. Zodra de blob-geactiveerde functie is gemaakt, gebruikt u de [ `run.csx` ](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/run.csx) en [ `function.proj` ](https://github.com/Azure-Samples/batch-functions-tutorial/blob/master/function.proj) vanuit GitHub in de functie.
    * `run.csx` wordt uitgevoerd wanneer een nieuwe blob wordt toegevoegd aan uw input-blob-container.
    * `function.proj` Geeft een lijst van de externe bibliotheken in uw functiecode aan te geven, bijvoorbeeld de Batch .NET-SDK.
1. Wijzig de tijdelijke aanduiding voor waarden van de variabelen in de `Run()` functie van de `run.csx` bestand in overeenstemming met de referenties van uw Batch- en storage. U vindt de referenties van uw Batch- en storage-account in Azure portal in de **sleutels** sectie van uw Batch-account.
    * Ophalen van de referenties voor uw Batch- en storage-account in de Azure portal in de **sleutels** sectie van uw Batch-account. 

## <a name="trigger-the-function-and-retrieve-results"></a>Activeer de functie en resultaten op te halen

Uploaden van een of meer van de gescande bestanden uit de [ `input_files` ](https://github.com/Azure-Samples/batch-functions-tutorial/tree/master/input_files) Active directory op GitHub naar de invoercontainer. Controleren van de Batch Explorer om te bevestigen dat een taak wordt toegevoegd aan `ocr-pool` voor elk bestand. Het bestand met de OCR toegepast wordt na enkele seconden toegevoegd aan de uitvoercontainer. Het bestand is vervolgens zichtbaar en is opgehaald in Storage Explorer.

Bovendien kunt u het bestand Logboeken onder aan het Azure Functions webvenster editor voor Hier ziet u berichten die lijken voor elk bestand dat u uploadt naar de invoercontainer bekijken:

```
2019-05-29T19:45:25.846 [Information] Creating job...
2019-05-29T19:45:25.847 [Information] Accessing input container <inputContainer>...
2019-05-29T19:45:25.847 [Information] Adding <fileName> as a resource file...
2019-06-21T20:02:35.129 [Information] Name of output text file: <outputTxtFile>
2019-06-21T20:02:35.130 [Information] Name of output PDF file: <outputPdfFile>
2019-05-29T19:45:26.200 [Information] Adding OCR task <taskID> for <fileName> <size of fileName>...
```

Als u wilt de uitvoerbestanden van Storage Explorer downloaden naar uw lokale computer, selecteert u eerst de bestanden die u wilt en selecteer vervolgens de **downloaden** op het lint boven. 

> [!TIP]
> De gedownloade bestanden zijn doorzoekbaar als geopend in een PDF-reader.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd: 

> [!div class="checklist"]
> * Batch Explorer gebruiken voor het maken van pools en taken
> * Storage Explorer gebruiken om blob-containers en een shared access signature (SAS) te maken
> * Een Azure blob-geactiveerde functie maken
> * Invoerbestanden uploaden naar Storage
> * Taakuitvoering controleren
> * Uitvoerbestanden ophalen

* Zie voor meer voorbeelden van het gebruik van de .NET API om te plannen en te verwerken van Batch-workloads [de voorbeelden op GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp). 

* Zie voor meer Azure Functions-triggers die u gebruiken kunt om uit te voeren van de Batch-workloads [de documentatie voor Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).
