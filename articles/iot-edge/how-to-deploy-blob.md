---
title: 'De Azure Blob Storage-module implementeren op apparaten: Azure IoT Edge | Microsoft Docs'
description: Een Azure Blob Storage-module implementeren op uw IoT Edge-apparaat voor het opslaan van gegevens aan de rand.
author: arduppal
ms.author: arduppal
ms.date: 06/19/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: arduppal
manager: mchad
ms.openlocfilehash: 468e4fca5e67850949e7d5826e4bc88fa504b9d6
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295202"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>De Azure Blob-opslag op IoT Edge-module op uw apparaat implementeren

Er zijn verschillende manieren om te implementeren van modules voor een IoT Edge-apparaat en ze allemaal werken voor Azure Blob Storage in IoT Edge-modules. De twee meest eenvoudige methoden zijn de Azure portal of Visual Studio Code-sjablonen te gebruiken.

> [!NOTE]
> Azure Blob-opslag op IoT Edge is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

- Een [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement.
- Een [IoT Edge-apparaat](how-to-register-device-portal.md) met IoT Edge-runtime geïnstalleerd.
- [Visual Studio Code](https://code.visualstudio.com/) en de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) als implementeert vanuit Visual Studio Code.

## <a name="deploy-from-the-azure-portal"></a>Implementeren vanuit Azure portal

De Azure-portal begeleidt u bij het maken van een manifest van de implementatie en het pushen van de implementatie naar een IoT Edge-apparaat.

### <a name="select-your-device"></a>Selecteer uw apparaat

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.
1. Selecteer **IoT Edge** in het menu.
1. Klik op de ID van het doelapparaat uit de lijst met apparaten.
1. Selecteer **Modules instellen**.

### <a name="configure-a-deployment-manifest"></a>Een manifest van de implementatie configureren

Het manifest voor een implementatie is een JSON-document waarin wordt beschreven welke modules te implementeren, hoe gegevens stromen tussen de modules, en de gewenste eigenschappen van de moduledubbels. De Azure portal heeft een wizard waarmee wordt beschreven hoe u een implementatie van het manifest, in plaats van het JSON-document handmatig. Het heeft drie stappen: **Modules toevoegen**, **routes opgeven**, en **implementatie bekijken**.

#### <a name="add-modules"></a>Modules toevoegen

1. In de **implementatie modules** sectie van de pagina, selecteer **toevoegen**.

1. Selecteer in de typen van modules in de vervolgkeuzelijst, **IoT Edge-Module**.

1. Geef een naam op voor de module en geef vervolgens de container-installatiekopie:

   - **Naam** -azureblobstorageoniotedge
   - **URI installatiekopie** -mcr.microsoft.com/azure-blob-storage:latest

   > [!IMPORTANT]
   > Azure IoT Edge is hoofdlettergevoelig bij het aanroepen van modules die u maakt, en de Storage-SDK standaard ook naar kleine letters. Hoewel de naam van de module in de [Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) is **AzureBlobStorageonIoTEdge**, wijzigen van de naam naar kleine letters helpt ervoor te zorgen dat uw verbindingen met de Azure Blob-opslag op IoT Edge-module worden niet onderbroken.

1. De standaardwaarde **Container maken opties** waarden de poort-bindingen die de container moet definiëren, maar u moet ook toevoegen van gegevens van uw opslagaccount en een binding voor de opslagmap op uw apparaat. De standaardwaarde JSON in de portal vervangen door de onderstaande JSON:

   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage directory bind>"
       ],
     "PortBindings":{
       "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

1. Update voor de JSON die u hebt gekopieerd met de volgende informatie:

   - Vervang `<your storage account name>` met een naam die u gemakkelijk kunt onthouden. Accountnamen moet 3 tot 24 tekens lang zijn en kleine letters en cijfers. Zonder spaties.

   - Vervang `<your storage account key>` met een 64-byte base64-sleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules.

   - Vervang `<storage directory bind>` op basis van het besturingssysteem van de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens. Een locatie op uw apparaat dat u opgeeft de binding van de directory opslag toegewezen aan een set-locatie in de module.

     - Voor Linux-containers, de indeling is  *\<opslagpad >: / blobroot*. Bijvoorbeeld, **/srv/containerdata: / blobroot** of **mijn volume: / blobroot**.
     - Voor Windows-containers, de notatie is  *\<opslagpad >: C: / BlobRoot*. Bijvoorbeeld, **C: / ContainerData:C: / BlobRoot** of **mijn-volume: C: / blobroot**.

     > [!IMPORTANT]
     > De tweede die helft van de map storage binden waarde, die naar een specifieke locatie in de module verwijst niet wijzigen. De binding van de map storage altijd moet eindigen met **: / blobroot** voor Linux-containers en **: C: / BlobRoot** voor Windows-containers.

1. Stel [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) en [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) eigenschappen voor de module met de volgende JSON kopiëren en plakken in de **Set moduledubbel de gewenste eigenschappen** vak. Elke eigenschap configureren met een geschikte waarde, opslaan en doorgaan met de implementatie.

   ```json
   {
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading":<true,false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload":<true,false>
       }
     }
   }

      ```

   ![container set opties, deviceAutoDeleteProperties en deviceToCloudUploadProperties eigenschappen maken](./media/how-to-deploy-blob/iotedge-custom-module.png)

   Zie voor meer informatie over het configureren van deviceToCloudUploadProperties en deviceAutoDeleteProperties nadat uw module is geïmplementeerd, [bewerken van de Moduledubbel](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Zie voor meer informatie over de gewenste eigenschappen [definiëren of update gewenste eigenschappen](module-composition.md#define-or-update-desired-properties).

1. Selecteer **Opslaan**.

1. Selecteer **volgende** om door te gaan naar de sectie routes.

#### <a name="specify-routes"></a>Routes opgeven

Houd de standaardroutes, en selecteer **volgende** om door te gaan naar de sectie controleren.

#### <a name="review-deployment"></a>Implementatie bekijken

De revisie sectie ziet u de JSON-implementatie manifest dat is gemaakt op basis van uw selecties in de vorige twee secties. Er zijn ook twee modules aangegeven dat u hebt toegevoegd: **$edgeAgent** en **$edgeHub**. Deze twee modules waaruit de [IoT Edge-runtime](iot-edge-runtime.md) en zijn vereist standaardwaarden in elke implementatie.

Lees de informatie van uw implementatie, en selecteer vervolgens **indienen**.

### <a name="verify-your-deployment"></a>Controleer of uw implementatie

Nadat u de implementatie hebt ingediend, wordt u terugkeert naar de **IoT Edge** pagina van uw IoT-hub.

1. Selecteer de IoT Edge-apparaat dat u met de implementatie te openen van de details ervan wordt benaderd.
1. Controleer of dat de blob storage-module wordt vermeld als zowel in de apparaatgegevens **opgegeven in de implementatie** en **gerapporteerd door device**.

Het duurt een paar seconden voor de module worden gestart op het apparaat en klikt u vervolgens teruggemeld aan IoT-Hub. Vernieuw de pagina om een bijgewerkte status te bekijken.

## <a name="deploy-from-visual-studio-code"></a>Implementeren vanuit Visual Studio Code

Azure IoT Edge zorgt voor sjablonen in Visual Studio Code om u te helpen bij het edge-oplossingen te ontwikkelen. Gebruik de volgende stappen uit voor het maken van een nieuwe IoT Edge-oplossing met een blob storage-module en het configureren van het manifest van de implementatie.

1. Selecteer **weergave** > **Command Palette**.

1. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

   Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer voor Visual Studio Code om de oplossingsbestanden te maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Module (Enter afbeeldings-URL) van de bestaande**. |
   | Een modulenaam opgeven | Voer de naam van een all-kleine voor uw module, zoals **azureblobstorageoniotedge**.<br /><br />Het is belangrijk dat u een kleine naam op voor de Azure Blob-opslag op IoT Edge-module. IoT Edge is hoofdlettergevoelig met betrekking tot modules en de Storage-SDK standaard naar kleine letters. |
   | Docker-installatiekopie opgeven voor de module | Geef de URI van de installatiekopie: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code wordt de informatie die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster. Sjabloon van de oplossing maakt een manifest implementatiesjabloon die uw blob storage-module-installatiekopie bevat, maar u wilt configureren van de module opties maken.

1. Open *deployment.template.json* in uw nieuwe werkruimte voor de oplossing en zoek de **modules** sectie. De volgende configuratiewijzigingen aanbrengen:

   1. Verwijder de **tempSensor** -module, zoals deze het niet nodig voor deze implementatie.

   1. Kopieer en plak de volgende code in de `createOptions` veld:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Module createOptions - Visual Studio Code bijwerken](./media/how-to-deploy-blob/create-options.png)

1. Vervang `<your storage account name>` met een naam die u gemakkelijk kunt onthouden. Accountnamen moet 3 tot 24 tekens lang zijn en kleine letters en cijfers. Zonder spaties.

1. Vervang `<your storage account key>` met een 64-byte base64-sleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules.

1. Vervang `<storage directory bind>` op basis van het besturingssysteem van de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens. Een locatie op uw apparaat dat u opgeeft de binding van de directory opslag toegewezen aan een set-locatie in de module.  

      - Voor Linux-containers, de indeling is  *\<opslagpad >: / blobroot*. Bijvoorbeeld, **/srv/containerdata: / blobroot** of **mijn volume: / blobroot**.
      - Voor Windows-containers, de notatie is  *\<opslagpad >: C: / BlobRoot*. Bijvoorbeeld, **C: / ContainerData:C: / BlobRoot** of **mijn-volume: C: / blobroot**.

      > [!IMPORTANT]
      > De tweede die helft van de map storage binden waarde, die naar een specifieke locatie in de module verwijst niet wijzigen. De binding van de map storage altijd moet eindigen met **: / blobroot** voor Linux-containers en **: C: / BlobRoot** voor Windows-containers.

1. Configureer [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) en [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) voor uw module door toe te voegen van de volgende JSON naar de *deployment.template.json* bestand. Elke eigenschap configureren met een geschikte waarde en sla het bestand.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading": <true, false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload": <true, false>
       }
     }
   }
   ```

   ![gewenste eigenschappen instellen voor azureblobstorageoniotedge - Visual Studio Code](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Zie voor meer informatie over het configureren van deviceToCloudUploadProperties en deviceAutoDeleteProperties nadat uw module is geïmplementeerd, [bewerken van de Moduledubbel](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Voor meer informatie over container opties maken, opnieuw opstarten van beleid en de gewenste status, Zie [EdgeAgent gewenste eigenschappen](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. Sla het bestand *deployment.template.json* op.

1. Met de rechtermuisknop op **deployment.template.json** en selecteer **genereren IoT Edge-implementatie manifest**.

1. Visual Studio Code haalt de gegevens die u hebt opgegeven in *deployment.template.json* en gebruikt voor het maken van een nieuwe implementatie-manifestbestand. Het manifest van de implementatie wordt gemaakt in een nieuwe **config** map in de werkruimte van uw oplossing. Zodra u dat bestand hebt, kunt u de stappen in [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) of [implementeren Azure IoT Edge-modules met de Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Meerdere exemplaren van de module implementeren

Als u implementeren meerdere exemplaren van de Azure Blob-opslag op IoT Edge-module wilt, moet u een ander opslagaccount-pad opgeven en wijzig de `HostPort` waarde die de module wordt gebonden aan. De blob-opslag-modules altijd poort 11002 in de container beschikbaar, maar u kunt aangeven welke poort gebonden aan op de host.

Bewerken **Container maken opties** (in de Azure portal) of de **createOptions** veld (in de *deployment.template.json* bestand in Visual Studio Code) om te wijzigen van de `HostPort` waarde:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Wanneer u verbinding met aanvullende blob storage-modules maken, wijzigt u het eindpunt om te verwijzen naar de bijgewerkte host-poort.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe implementatie werk manifesten en hoe ze worden gemaakt, [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).
