---
title: Azure Blob-opslag op Azure IoT Edge-apparaten | Microsoft Docs
description: Een Azure Blob Storage-module implementeren op uw IoT Edge-apparaat voor het opslaan van gegevens aan de rand.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 10/03/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fa88ff46b4fb93d55aa0087cca0e6184f3e087a0
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567278"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Gegevens aan de rand met Azure Blob Storage Store op IoT Edge (preview)

Azure Blob-opslag op IoT Edge biedt een [blok-blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) opslagoplossing aan de rand. Een blob storage-module op uw IoT Edge-apparaat zich gedraagt als een Azure blok-blob-service, maar de blok-blobs worden lokaal opgeslagen op uw IoT Edge-apparaat. U kunt toegang tot uw blobs met behulp van de methodes SDK van Azure storage of blob API-aanroepen die u al hebt gebruikt om te blokkeren. 

Scenario's waarbij gegevens zoals video's, afbeeldingen, financiële gegevens, ziekenhuis gegevens of gegevens die moeten worden bewaard lokaal, later die kan worden lokaal verwerkt of verzonden naar de cloud zijn goede voorbeelden het gebruik van deze module.

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

Er zijn verschillende manieren om te implementeren van modules voor een IoT Edge-apparaat, en ze allemaal werken voor Azure Blob Storage in IoT Edge-modules. De twee meest eenvoudige methoden zijn de Azure portal of Visual Studio Code-sjablonen te gebruiken. 

### <a name="azure-portal"></a>Azure Portal

#### <a name="find-the-module"></a>De module niet vinden

Kies een van de twee manieren om te zoeken van de blob storage-module:

1. In Azure Portal zoeken naar 'Azure Blob-opslag op IoT Edge'. En **Selecteer** de resultaat-zoekitem
2. Ga naar Marketplace in Azure Portal en klik op 'Internet of Things'. Selecteer onder de sectie "IoT Edge-Modules" 'Azure Blob-opslag op IoT Edge'. En klikt u op **maken**

#### <a name="steps-to-deploy"></a>Stappen voor het implementeren

**Apparaten voor IoT Edge-Module**

1. Selecteer het 'abonnement', waar uw IoT-Hub is geïmplementeerd.
2. Selecteer uw 'IoT-Hub'.
3. Geef de 'IoT Edge-apparaatnaam"waar u wilt implementeren in deze module. U kunt 'Apparaat vinden' gebruiken om te vinden van uw apparaat.
4. Klik op **Create**.

**Modules instellen**

1. In de sectie 'Modules toevoegen' onder 'Implementatie Modules' vindt u dat die module al wordt weergegeven met de naam die begint met 'AzureBlobStorageonIoTEdge'. 
2. **Selecteer** de blob storage-module uit de lijst met 'Implementatie Modules'. Zijpaneel 'IoT Edge-aangepaste Modules' wordt geopend.
3. **Naam**: U kunt de naam van de module hier wijzigen
4. **URI installatiekopie**: de URI die moet worden vervangen **mcr.microsoft.com/azure-blob-storage:latest**
5. **Container maken opties**: de onderstaande JSON met uw waarden bewerken en vervang deze door de JSON in de Portal-pagina:
   
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
   
    * Update `<your storage account name>`. Accountnamen moet drie naar 24 tekens lang zijn en kleine letters en cijfers.
    * Update `<your storage account key>` met een 64-byte base64-sleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules.
    * Update `<storage directory bind>`. Afhankelijk van het besturingssysteem van de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens.  

       * Linux-containers:  **\<opslagpad >: / blobroot**. Bijvoorbeeld: / srv/containerdata: / blobroot. Of, in mijn volume: / blobroot. 
       * Windows-containers:  **\<opslagpad >: C: / BlobRoot**. Bijvoorbeeld: C: / ContainerData:C: / BlobRoot. Of, Mijn-volume: C: / blobroot.
   
   > [!CAUTION]
   > Wijzig niet de "/ blobroot ' voor Linux en" C:/BlobRoot' voor Windows, voor  **\<Storage directory binden >** waarden.

    ![Waarden van de Module bijwerken](./media/how-to-store-data-blob/edit-module.png)

6. **Sla** de waarden in "IoT Edge-aangepaste Modules"
7. Klik op **volgende** in de sectie "Modules instellen"
8. Klik op **volgende** in de sectie 'Routes opgeven'
9. Nadat u hebt bekeken, klikt u op **indienen** in de sectie "Controleren implementatie".
10. Controleer of in uw IoT-Hub of de blob storage-module door het apparaat wordt uitgevoerd 

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
   
   5. **Docker-installatiekopie opgeven voor de module** -Geef de URI van de installatiekopie: **mcr.microsoft.com/azure-blob-storage:latest**

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
   
   > [!CAUTION]
   > Wijzig niet de "/ blobroot ' voor Linux en" C:/BlobRoot' voor Windows, voor  **\<Storage directory binden >** waarden.

5. Sla **deployment.template.json**.

6. Open **.env** in de werkruimte van uw oplossing. 

7. U hebt geen alle registerwaarden container invoeren voor de installatiekopie van het blob-opslag, omdat het is openbaar beschikbaar. Twee nieuwe omgevingsvariabelen in plaats daarvan toevoegen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Geef de waarde voor `STORAGE_ACCOUNT_NAME`, namen van drie naar 24 tekens lang zijn en kleine letters en cijfers moeten zijn. En geef een 64-byte base64-sleutel voor de `STORAGE_ACCOUNT_KEY`. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules. 

9. Sla **.env**. 

10. Met de rechtermuisknop op **deployment.template.json** en selecteer **genereren IoT Edge-implementatie manifest**. 

Visual Studio Code wordt de informatie die u in de deployment.template.json en .env en gebruikt voor het maken van een nieuwe implementatie-manifestbestand. Het manifest van de implementatie wordt gemaakt in een nieuwe **config** map in de werkruimte van uw oplossing. Zodra u dat bestand hebt, kunt u de stappen in [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) of [implementeren Azure IoT Edge-modules met de Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Verbinding maken met uw blob storage-module

U kunt de accountnaam en accountsleutel dat u hebt geconfigureerd voor de module voor toegang tot de blob-opslag op uw IoT Edge-apparaat gebruiken. 

Geef uw IoT Edge-apparaat als de blobeindpunt voor alle opslag aanvragen die u uitvoert. U kunt [maken van een verbindingsreeks voor een expliciete opslageindpunt](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) met behulp van de gegevens van de IoT Edge-apparaat en de accountnaam op die u hebt geconfigureerd. 

1. Voor modules die zijn geïmplementeerd op de dezelfde edge-apparaat waarop de 'Azure Blob-opslag op IoT Edge' wordt uitgevoerd, het eindpunt van blob is: `http://<Module Name>:11002/<account name>`. 
2. Voor modules die zijn geïmplementeerd op verschillende edge-apparaat, dan het edge-apparaat waar 'Azure Blob-opslag op IoT Edge' wordt uitgevoerd en is afhankelijk van uw installatie van de blob-eindpunt: `http://<device IP >:11002/<account name>` of `http://<IoT Edge device hostname>:11002/<account name>` of `http://<FQDN>:11002/<account name>`

## <a name="logs"></a>Logboeken

De logboeken in de container, kunt u vinden onder: 
* Voor Linux: /blobroot/logs/platformblob.log

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
* Containers weergeven

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
* Plaats blokkeren:-het blok moet kleiner dan of gelijk zijn aan 4 MB in grootte
* En lijst met geblokkeerde websites

Niet-ondersteund:
* Blokkeren van URL plaatsen

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md)

