---
title: Blok-blobs Store op apparaten - Azure IoT Edge | Microsoft Docs
description: Een Azure Blob Storage-module implementeren op uw IoT Edge-apparaat voor het opslaan van gegevens aan de rand.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 01/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: b5dcc4295467aee82547bc2a0894320bd110998f
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54065814"
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
   | Windows Server 2019 | AMD64 |
   | Raspbian Stretch | ARM32 |

Cloudresources:

* Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 


## <a name="deploy-blob-storage-to-your-device"></a>Blob-opslag op uw apparaat implementeren

Er zijn verschillende manieren om te implementeren van modules voor een IoT Edge-apparaat, en ze allemaal werken voor Azure Blob Storage in IoT Edge-modules. De twee meest eenvoudige methoden zijn de Azure portal of Visual Studio Code-sjablonen te gebruiken. 

### <a name="azure-portal"></a>Azure Portal

De Azure Marketplace biedt IoT Edge-modules die rechtstreeks naar uw IoT Edge-apparaten, met inbegrip van Azure Blob-opslag op IoT Edge kunnen worden geïmplementeerd. Volg deze stappen voor het implementeren van de module op basis van de Azure-portal.

1. In de [Azure-portal](https://portal.azure.com), zoek naar 'Azure Blob-opslag op IoT Edge'. En **Selecteer** het zoekresultaat uit de Marketplace.

   ![Module maken vanuit de Marketplace-zoekactie](./media/how-to-store-data-blob/marketplace-module.png)

2. Kies een IoT Edge-apparaat voor het ontvangen van deze module. Op de **apparaten voor IoT Edge-Module** pagina, geef de volgende informatie:

   1. Selecteer de **abonnement** waarin de IoT-hub die u gebruikt.

   2. Selecteer uw **IoT-Hub**.

   3. Als u weet dat uw **IoT Edge-apparaatnaam**, voer die in het tekstvak in. Of selecteer **apparaat vinden** kiezen uit een lijst met IoT Edge-apparaten in uw IoT-hub. 
   
   4. Selecteer **Maken**.

   Nu dat u hebt ervoor gekozen een IoT Edge-module van Azure Marketplace en een IoT Edge-apparaat voor het ontvangen van de module gekozen, er wordt een wizard drie stappen waarmee u precies hoe de module wordt geïmplementeerd.

3. In de **Modules toevoegen** stap van de wizard set modules, zoals u ziet dat de **AzureBlobStorageonIoTEdge** module al wordt vermeld onder **implementatie Modules**. 

2. Selecteer de blob storage-module in de lijst van de implementatie-modules openen van de details van de module. 

   ![Selecteer modulenaam modulegegevens openen](./media/how-to-store-data-blob/open-module-details.png)

3. Op de **aangepaste IoT Edge-modules** pagina, de Azure Blob-opslag op IoT Edge-module bij te werken met de volgende stappen uit:

   1. Wijzigen van de module **naam** moeten kleine letters. U kunt de module desgewenst wijzigen, of gebruik `azureblobstorageoniotedge`. 

      >[!IMPORTANT]
      >Azure IoT Edge is hoofdlettergevoelig bij het aanroepen van modules die u maakt, en de Storage-SDK standaard naar kleine letters. Om ervoor te zorgen dat uw verbindingen met de Azure Blob-opslag op IoT Edge-module worden niet onderbroken, geef deze de naam van een kleine letters. 

   2. De standaardwaarde **Container maken opties** de poort-bindingen die uw container nodig heeft, maar u moet ook gegevens van uw opslagaccount en een binding voor de opslagmap toevoegen op het apparaat. De JSON in de portal voor overschrijven met de onderstaande JSON:
    
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
   3. Update voor de JSON die u hebt gekopieerd met de volgende informatie: 

      * Vervang `<your storage account name>` met een naam die u gemakkelijk kunt onthouden. Accountnamen moet drie naar 24 tekens lang zijn en kleine letters en cijfers.
      * Vervang `<your storage account key>` met een 64-byte base64-sleutel. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules.
      * Vervang `<storage directory bind>` afhankelijk van uw besturingssysteem voor de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens. Een locatie op uw apparaat dat u opgeeft de binding van de directory opslag toegewezen aan een set-locatie in de module. 

         * Linux-containers:  **\<opslagpad >: / blobroot**. Bijvoorbeeld: / srv/containerdata: / blobroot. Of, in mijn volume: / blobroot. 
         * Windows-containers:  **\<opslagpad >: C: / BlobRoot**. Bijvoorbeeld: C: / ContainerData:C: / BlobRoot. Of, Mijn-volume: C: / blobroot.
   
      > [!IMPORTANT]
      > De tweede die helft van de map storage binden waarde, die naar een specifieke locatie in de module verwijst niet wijzigen. De binding van de map storage altijd moet eindigen met **: / blobroot** voor Linux-containers en **: C: / BlobRoot** voor Windows-containers.

      ![Update-module container opties - portal maken](./media/how-to-store-data-blob/edit-module.png)

   4. Selecteer **Opslaan**.

4. Selecteer **volgende** om door te gaan met de volgende stap van de wizard.
5. In de **Routes opgeven** stap van de wizard de optie **volgende**.
6. Selecteer in de stap **Implementatie beoordelen** van de wizard de optie **Verzenden**.
7. Nadat u de implementatie hebt ingediend, wordt u terugkeert naar de **IoT Edge** pagina van uw IoT-hub. Selecteer de IoT Edge-apparaat dat u met de implementatie te openen van de details ervan wordt benaderd. 
8. Controleer of dat de blob storage-module wordt vermeld als zowel in de apparaatgegevens **opgegeven in de implementatie** en **gerapporteerd door device**. Het duurt een paar seconden voor de module worden gestart op het apparaat en klikt u vervolgens teruggemeld aan IoT-Hub. Vernieuw de pagina om een bijgewerkte status te bekijken. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code-sjablonen

Azure IoT Edge zorgt voor sjablonen in Visual Studio Code om u te helpen bij het edge-oplossingen te ontwikkelen. Deze stappen vereisen dat u met [Visual Studio Code](https://code.visualstudio.com/) op uw ontwikkelcomputer geïnstalleerd en geconfigureerd met de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

Gebruik de volgende stappen voor het maken van een nieuwe IoT Edge-oplossing met een blob storage-module en configureer het manifest van de implementatie. 

1. Selecteer **weergave** > **Command Palette**. 

2. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Waarde |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Module (Enter afbeeldings-URL) van de bestaande**. |
   | Een modulenaam opgeven | Voer de naam van een all-kleine voor uw module, zoals **Azure BLOB Storage**.<br><br>Het is belangrijk dat u een kleine naam op voor de Azure Blob-opslag op IoT Edge-module. IoT Edge is hoofdlettergevoelig met betrekking tot modules en de Storage-SDK standaard naar kleine letters. |
   | Docker-installatiekopie opgeven voor de module | Geef de URI van de installatiekopie: **mcr.microsoft.com/azure-blob-storage:latest** |

   VS Code haalt de gegevens die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster. Sjabloon van de oplossing maakt een manifest implementatiesjabloon die uw blob storage-module-installatiekopie bevat, maar u wilt configureren van de module opties maken. 

3. Open **deployment.template.json** in uw nieuwe werkruimte voor de oplossing en zoek de **modules** sectie. De volgende configuratiewijzigingen aanbrengen:

   1. Verwijder de **tempSensor** -module, zoals deze het niet nodig voor deze implementatie. 

   2. Kopieer en plak de volgende code in de **createOptions** veld van uw blob storage-module: 

      ```json
      "Env": [
        "LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME","LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Module createOptions - VS Code bijwerken](./media/how-to-store-data-blob/create-options.png)

4. Werk in de opties voor het maken JSON, `<storage directory bind>` afhankelijk van uw besturingssysteem voor de container. Geef de naam van een [volume](https://docs.docker.com/storage/volumes/) of het absolute pad naar een map op uw IoT Edge-apparaat dat is waar u de blob-module voor het opslaan van de gegevens. Een locatie op uw apparaat dat u opgeeft de binding van de directory opslag toegewezen aan een set-locatie in de module.  

   * Linux-containers:  **\<opslagpad >: / blobroot**. Bijvoorbeeld: / srv/containerdata: / blobroot. Of, in mijn volume: / blobroot.
   * Windows-containers:  **\<opslagpad >: C: / BlobRoot**. Bijvoorbeeld: C: / ContainerData:C: / BlobRoot. Of, Mijn-volume: C: / blobroot.
   
   > [!IMPORTANT]
   > De tweede die helft van de map storage binden waarde, die naar een specifieke locatie in de module verwijst niet wijzigen. De binding van de map storage altijd moet eindigen met **: / blobroot** voor Linux-containers en **: C: / BlobRoot** voor Windows-containers.

5. Sla het bestand **deployment.template.json** op.

6. Open de **.env** bestand in de werkruimte van uw oplossing. 

7. Het .env-bestand voor het ontvangen van container registerreferenties is ingesteld, maar u niet nodig hebt die voor de installatiekopie van het blob-opslag omdat het is openbaar beschikbaar. In plaats daarvan moet u het bestand vervangen door twee nieuwe omgevingsvariabelen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. Geef een waarde voor `STORAGE_ACCOUNT_NAME`, namen van drie naar 24 tekens lang zijn en kleine letters en cijfers moeten zijn. Geef een 64-byte base64-sleutel voor de `STORAGE_ACCOUNT_KEY`. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules. 

   Licentie omvatten geen spaties of aanhalingstekens rond de waarden die u opgeeft. 

9. Sla het **.env**-bestand op. 

10. Met de rechtermuisknop op **deployment.template.json** en selecteer **genereren IoT Edge-implementatie manifest**. 

11. Visual Studio Code wordt de informatie die u in de deployment.template.json en .env en gebruikt voor het maken van een nieuwe implementatie-manifestbestand. Het manifest van de implementatie wordt gemaakt in een nieuwe **config** map in de werkruimte van uw oplossing. Zodra u dat bestand hebt, kunt u de stappen in [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) of [implementeren Azure IoT Edge-modules met de Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Verbinding maken met uw blob storage-module

U kunt de accountnaam en accountsleutel dat u hebt geconfigureerd voor de module voor toegang tot de blob-opslag op uw IoT Edge-apparaat gebruiken. 

Geef uw IoT Edge-apparaat als de blobeindpunt voor alle opslag aanvragen die u uitvoert. U kunt [maken van een verbindingsreeks voor een expliciete opslageindpunt](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) met behulp van de gegevens van de IoT Edge-apparaat en de accountnaam op die u hebt geconfigureerd. 

1. Voor modules die zijn geïmplementeerd op de dezelfde edge-apparaat waarop de 'Azure Blob-opslag op IoT Edge' wordt uitgevoerd, het eindpunt van blob is: `http://<module name>:11002/<account name>`. 
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

