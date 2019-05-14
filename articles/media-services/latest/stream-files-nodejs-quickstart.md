---
title: Stream-video's met Azure Media Services - Node.js | Microsoft Docs
description: Volg de stappen in deze zelfstudie om te maken van een nieuwe Azure Media Services-account, een bestand coderen en streamen met Azure Media Player.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/26/2019
ms.author: juliako
ms.openlocfilehash: 3e4172cd149726e28e0c7dff435ec1f7a59ee169
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550171"
---
# <a name="tutorial-stream-video-files---nodejs"></a>Zelfstudie: Videobestanden streamen - Node.js

Deze zelfstudie leert u hoe eenvoudig het is om te coderen en streamen van video's op een groot aantal browsers en apparaten met Azure Media Services. De invoerinhoud kan worden opgegeven met HTTPS-URL's, SAS-URL's of paden naar bestanden in Azure Blob-opslag.

Het voorbeeld in dit artikel worden de inhoud die u toegankelijk zijn via een HTTPS-URL gecodeerd. Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.

Aan het einde van de zelfstudie zich kunt u een video te streamen.  

![De video afspelen](./media/stream-files-nodejs-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

- [Node.js](https://nodejs.org/en/download/) installeren
- [Een Azure Media Services-account maken](create-account-cli-how-to.md).<br/>Vergeet niet de waarden die u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account.
- Volg de stappen in [Access Azure Media Services API with the Azure CLI](access-api-cli-how-to.md) (Toegang tot de Azure Media Services-API met de Azure CLI) en sla de referenties op. U hebt deze nodig voor toegang tot de API.

## <a name="download-and-configure-the-sample"></a>Het voorbeeld downloaden en configureren

Kloon een GitHub-opslagplaats met het streaming Node.js-voorbeeld op de computer met de volgende opdracht:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
 ```

Het voorbeeld bevindt zich de [StreamFilesSample](https://github.com/Azure-Samples/media-services-v3-node-tutorials/tree/master/AMSv3Samples/StreamFilesSample) map.

Open [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L25) in u project hebt gedownload. Vervang de `endpoint config` waarden met referenties die u hebt verkregen via [toegang tot API's](access-api-cli-how-to.md).

In het voorbeeld worden de volgende acties uitgevoerd:

1. Er wordt een **transformatie** gemaakt (eerst wordt gecontroleerd of de opgegeven transformatie bestaat). 
2. Er wordt een uitvoer**asset** gemaakt die wordt gebruikt als uitvoer van de coderings**taak**.
3. De invoer van de **taak** wordt gemaakt en is gebaseerd op een HTTPS-URL.
4. De coderings**taak** wordt met behulp van de eerder gemaakte invoer en uitvoer verzonden.
5. De status van de taak wordt gecontroleerd.
6. Er wordt een **streaming-locator** gemaakt.
7. Er worden streaming-URL's samengesteld.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

1. De app wordt gedownload van gecodeerde bestanden. Maak een map waar u voor de uitvoerbestanden om te gaan en werk de waarde van de **outputFolder** variabele in de [index.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials/blob/master/AMSv3Samples/StreamFilesSample/index.js#L39) bestand.
1. Open **opdrachtprompt**, blader naar de directory van het voorbeeld en voer de volgende opdrachten uit.

    ```
    npm install 
    node index.js
    ```

Nadat het is uitgevoerd, ziet u soortgelijke uitvoer:

![Uitvoeren](./media/stream-files-nodejs-quickstart/run.png)

## <a name="test-with-azure-media-player"></a>Testen met Azure Media Player

In dit artikel gebruiken we Azure Media Player om de stream te testen. 

> [!NOTE]
> Als een speler wordt gehost op een https-site, moet u de URL bijwerken naar 'https'.

1. Open een browser en ga naar [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Plak in het vak **URL:** een van de streaming URL's die u hebt verkregen door het uitvoeren van de toepassing. 
 
     U kunt de URL plakken in de HLS-, Dash-, of Smooth-indeling. Azure Media Player schakelt over op naar een geschikt streaming-protocol zodat de stream automatisch op uw apparaat wordt afgespeeld.
3. Klik op **Update Player**.

Azure Media Player kan worden gebruikt voor testdoeleinden, maar mag niet worden gebruikt in een productieomgeving. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet meer nodig van de resources in de resourcegroep hebt, met inbegrip van de Media Services en storage-accounts die u hebt gemaakt voor deze zelfstudie, moet u de resourcegroep verwijderen.

Voer de volgende CLI-opdracht uit:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Zie ook

[Taak-foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Media Services-concepten](concepts-overview.md)
