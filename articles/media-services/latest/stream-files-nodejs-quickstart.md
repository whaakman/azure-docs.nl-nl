---
title: Stream-video's met Azure Media Services - Node.js | Microsoft Docs
description: Volg de stappen in deze snelstartgids om een nieuw Azure Media Services-account te maken, een bestand te coderen en dit vervolgens te streamen naar Azure Media Player.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/26/2019
ms.author: juliako
ms.openlocfilehash: 22b7f2380b509daa4cb9931d6fc57c1297628e3d
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58522856"
---
# <a name="quickstart-stream-video-files---nodejs"></a>Quickstart: Stream videobestanden - Node.js

Deze snelstartgids laat zien hoe u met Azure Media Services eenvoudig video's kunt coderen en streamen naar allerlei verschillende browsers en apparaten. De invoerinhoud kan worden opgegeven met HTTPS-URL's, SAS-URL's of paden naar bestanden in Azure Blob-opslag.
Met het voorbeeld in dit onderwerp wordt inhoud gecodeerd die u toegankelijk maakt via een HTTPS-URL. Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.

Als u deze snelstartgids hebt voltooid, weet u hoe u een video kunt streamen.  

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

![Voer](./media/stream-files-nodejs-quickstart/run.png)

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

Als u de resources in de resourcegroep niet meer nodig hebt, met inbegrip van het Media Services-account en de opslagaccounts die u hebt gemaakt voor deze snelstartgids, verwijdert u de resourcegroep.

Voer de volgende CLI-opdracht uit:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="see-also"></a>Zie ook

[Taak-foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Media Services-concepten](concepts-overview.md)
