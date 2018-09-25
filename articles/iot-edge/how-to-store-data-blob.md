---
title: Azure Blob-opslag op Azure IoT Edge-apparaten | Microsoft Docs
description: Een Azure Blob Storage-module implementeren op uw IoT Edge-apparaat voor het opslaan van gegevens aan de rand.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 09/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b9e48eba4b46f024b056fe53b3b3df24feec802e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995666"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Gegevens aan de rand met Azure Blob Storage Store op IoT Edge (preview)

Azure BLOB-opslag op IoT Edge biedt een block blob storage-oplossing aan de rand. Een blob storage-module op uw IoT Edge-apparaat zich gedraagt als een Azure blob-service, maar de blok-blobs worden lokaal opgeslagen op uw IoT Edge-apparaat. U kunt toegang tot uw blobs met behulp van de methodes SDK van Azure storage of blob API-aanroepen die u al hebt gebruikt om te blokkeren. 

In dit artikel vindt u instructies voor het implementeren van een Azure Blob-opslag op IoT Edge-container die wordt uitgevoerd van een blob-service op uw IoT Edge-apparaat. 

>[!NOTE]
>Azure Blob-opslag op IoT Edge is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).
* De Azure Blob-opslag op IoT Edge-module biedt ondersteuning voor de volgende configuraties:

   | Besturingssysteem | Architectuur |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu-Server 18.04 | AMD64 |
   | Windows 10 IoT Core (update van oktober) | AMD64 |
   | Windows 10 IoT Enterprise (update van oktober) | AMD64 |
   | WindowsServer 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Cloudresources:

* Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Blob-opslag op uw apparaat implementeren

Azure Blob-opslag op IoT Edge biedt drie standard containerinstallatiekopieën, twee voor Linux-containers (AMD64 en ARM32 architecturen) en één voor Windows-containers (AMD64). Wanneer u een van deze module-installatiekopieën implementeren van blob-opslag in uw IoT Edge-apparaat gebruikt, geeft u drie soorten informatie het configureren van de module-exemplaar voor uw apparaat:

* Een **accountnaam** en **accountsleutel**. Om te blijven consistent met de Azure-opslag, gebruik blob storage-modules accountnamen en accountsleutels om toegang te beheren. Accountnamen moet drie naar 24 tekens lang zijn en kleine letters en cijfers. Accountsleutels moeten met base64 gecodeerde en 64 bytes lang zijn. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64).
* Een **lokale-opslagoptie**. De blob storage-module blobs worden lokaal opgeslagen op het IoT Edge-apparaat, zodat de blobs behouden als de module service is gestopt of opnieuw wordt opgestart. Declareer een bestaande [volume](https://docs.docker.com/storage/volumes
) of pad naar lokale map waarin de blobs moeten worden opgeslagen op uw apparaat. 

Er zijn verschillende manieren om te implementeren van modules voor een IoT Edge-apparaat, en ze allemaal werken voor Azure Blob Storage in IoT Edge-modules. De twee meest eenvoudige methoden zijn de Azure portal of Visual Studio Code-sjablonen te gebruiken. 

### <a name="azure-portal"></a>Azure Portal

Volg de stappen in voor het implementeren van blob storage via Azure portal, [implementeren Azure IoT Edge-modules van de Azure-portal](how-to-deploy-modules-portal.md). Ga naar de module implementeren, Kopieer de URI van de installatiekopie en voorbereiden van de container maken voordat u opties op basis van het besturingssysteem van de container. Gebruik deze waarden in de **configureren van een implementatie-manifest** gedeelte van het artikel implementatie. 

Geef de URI van de installatiekopie voor de blob storage-module: **mcr.microsoft.com/azure-blob-storage**. 
   
Gebruik de volgende JSON-sjabloon voor de **Container maken opties** veld. Configureer de JSON met uw storage-accountnaam, de opslagaccountsleutel en de opslag directory verbindt.  
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":[
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       ]
   }
   ```   
   
Werk in de opties voor het maken JSON, `\<your storage account name\>` met een willekeurige naam. Update `\<your storage account key\>` met een 64-byte base64-sleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64) waarmee u de lengte in bytes. U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules.

Werk in de opties voor het maken JSON, `<storage directory bind>` afhankelijk van uw besturingssysteem voor de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens.  

   * Linux-containers:  **\<opslagpad >: / blobroot**. Bijvoorbeeld: / srv/containerdata: / blobroot. Of, in mijn volume: / blobroot. 
   * Windows-containers:  **\<opslagpad >: C: / BlobRoot**. Bijvoorbeeld: C: / ContainerData:C: / BlobRoot. Of, Mijn-volume: C: / blobroot. 


U hoeft niet registerreferenties voor toegang tot Azure Blob-opslag op IoT Edge op te geven en u hoeft niet te declareren van routes voor uw implementatie. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code-sjablonen

Azure IoT Edge zorgt voor sjablonen in Visual Studio Code om u te helpen bij het edge-oplossingen te ontwikkelen. Deze stappen vereisen dat u met [Visual Studio Code](https://code.visualstudio.com/) op uw ontwikkelcomputer geïnstalleerd en geconfigureerd met de [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Gebruik de volgende stappen voor het maken van een nieuwe IoT Edge-oplossing met een blob storage-module en configureer het manifest van de implementatie. 

1. Selecteer **weergave** > **Command Palette**. 

2. Voer in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**. 

3. Volg de aanwijzingen voor het maken van een nieuwe oplossing: 

   1. **Selecteer de map** -Blader naar de map waar u de nieuwe oplossing maken.  
   
   2. **Geef de oplossingsnaam van een** : Voer een naam in voor uw oplossing of accepteer de standaardwaarde.
   
   3. **Selecteer module sjabloon** -Kies **Module (Enter afbeeldings-URL) van de bestaande**.
   
   4. **Geef de modulenaam van een** -Geef een herkenbare naam voor uw module, zoals **Azure BLOB Storage**.
   
   5. **Docker-installatiekopie opgeven voor de module** -Geef de URI van de installatiekopie: **mcr.microsoft.com/azure-blob-storage**

VS Code haalt de gegevens die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster. 

Sjabloon van de oplossing maakt een manifest implementatiesjabloon die uw blob storage-module-installatiekopie bevat, maar u wilt configureren van de module opties maken. 

1. Open **deployment.template.json** in uw nieuwe werkruimte voor de oplossing en zoek de **modules** sectie. 

2. Verwijder de **tempSensor** -module, zoals deze het niet nodig voor deze implementatie. 

3. Kopieer en plak de volgende code in de **createOptions** veld van uw blob storage-module: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Opties voor update-module maken](./media/how-to-store-data-blob/create-options.png)

4. Werk in de opties voor het maken JSON, `<storage directory bind>` afhankelijk van uw besturingssysteem voor de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens.  

   * Linux-containers:  **\<opslagpad >: / blobroot**. Bijvoorbeeld: / srv/containerdata: / blobroot. Of, in mijn volume: / blobroot.
   * Windows-containers:  **\<opslagpad >: C: / BlobRoot**. Bijvoorbeeld: C: / ContainerData:C: / BlobRoot. Of, Mijn-volume: C: / blobroot.

5. Sla **deployment.template.json**.

6. Open **.env** in de werkruimte van uw oplossing. 

7. U hebt geen alle registerwaarden container invoeren voor de installatiekopie van het blob-opslag, omdat het is openbaar beschikbaar. Twee nieuwe omgevingsvariabelen in plaats daarvan toevoegen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Geef een naam op voor naam van het opslagaccount en een 64-byte base64 sleutel opgeven voor de opslagaccountsleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules. 

9. Sla **.env**. 

10. Met de rechtermuisknop op **deployment.template.json** en selecteer **genereren IoT Edge-implementatie manifest**. 

Visual Studio Code wordt de informatie die u in de deployment.template.json en .env en gebruikt voor het maken van een nieuwe implementatie-manifestbestand. Het manifest van de implementatie wordt gemaakt in een nieuwe **config** map in de werkruimte van uw oplossing. Zodra u dat bestand hebt, kunt u de stappen in [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) of [implementeren Azure IoT Edge-modules met de Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Verbinding maken met uw blob storage-module

U kunt de accountnaam en accountsleutel dat u hebt geconfigureerd voor de module voor toegang tot de blob-opslag op uw IoT Edge-apparaat gebruiken. 

Geef uw IoT Edge-apparaat als de blobeindpunt voor alle opslag aanvragen die u uitvoert. U kunt [maken van een verbindingsreeks voor een expliciete opslageindpunt](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) met behulp van de gegevens van de IoT Edge-apparaat en de accountnaam op die u hebt geconfigureerd. 

De blobeindpunt voor Azure Blob-opslag op IoT Edge is `http://<IoT Edge device hostname>:11002/<account name>`. 

## <a name="deploy-multiple-instances"></a>Meerdere exemplaren implementeren

Als u implementeren meerdere exemplaren van Azure Blob-opslag op IoT Edge wilt, moet u alleen de HostPort die de module wordt gebonden aan wijzigen. De blob-opslag-modules altijd poort 11002 in de container beschikbaar, maar u kunt aangeven welke poort gebonden aan op de host. 

De module bewerken Opties voor het wijzigen van de waarde HostPort maken:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Wanneer u verbinding met aanvullende blob storage-modules maken, wijzigt u het eindpunt om te verwijzen naar de bijgewerkte host-poort. 

### <a name="try-it-out"></a>Probeer het

De Azure Blob Storage-documentatie bevat snelstartgidsen die de voorbeeldcode in verschillende talen. U kunt deze voorbeelden als u wilt testen van Azure Blob-opslag op IoT Edge door het veranderen van de blob-eindpunt om te verwijzen naar uw blob storage-module kunt uitvoeren.

De volgende quickstarts talen die ook worden ondersteund door de IoT Edge, zodat u ze als IoT Edge-modules samen met de blob storage-module implementeren kunt gebruiken:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Ondersteunde opslagbewerkingen

BLOB storage-modules op IoT Edge gebruikt u de dezelfde Azure Storage-SDK's en consistent zijn met de 2018-03-28-versie van de API van Azure Storage voor blok-blob-eindpunten. Latere releases zijn afhankelijk van de behoeften van klanten. 

Niet alle Azure Blob Storage-bewerkingen worden ondersteund door Azure Blob-opslag op IoT Edge. De volgende secties details over welke bewerkingen een worden niet worden ondersteund. 

### <a name="account"></a>Account

Ondersteund: 
* Lijst met containers

Niet-ondersteund: 
* Ophalen en instellen van blob service-eigenschappen
* Voorbereidende blob-aanvraag
* Blob-servicestatistieken ophalen
* Ophalen van accountgegevens

### <a name="containers"></a>Containers

Ondersteund: 
* Maken en verwijderen van container
* Containereigenschappen en metagegevens ophalen
* Blobs vermelden

Niet-ondersteund: 
* Ophalen en instellen van de container ACL
* Lease-container
* Metagegevens van de container set

### <a name="blobs"></a>Blobs

Ondersteund: 
* Put, ophalen en verwijderen van blob
* Ophalen en instellen van blob-eigenschappen
* Ophalen en instellen van blob-metagegevens

Niet-ondersteund: 
* Lease-blob
* Momentopname maken van blob
* Kopieer en kopiëren van de blob afbreken
* Verwijdering van blob ongedaan maken
* Bloblaag instellen

### <a name="block-blobs"></a>Blok-blobs

Ondersteund: 
* Blok plaatsen
* En lijst met geblokkeerde websites

Niet-ondersteund:
* Blokkeren van URL plaatsen

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md)

