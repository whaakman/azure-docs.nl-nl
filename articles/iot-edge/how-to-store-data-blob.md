---
title: Blok-blobs Store op apparaten - Azure IoT Edge | Microsoft Docs
description: Een Azure Blob Storage-module implementeren op uw IoT Edge-apparaat voor het opslaan van gegevens aan de rand.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 03/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6f82f50ebaa7ad4440078d1fd4658109cf0e19b6
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57571283"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Gegevens aan de rand met Azure Blob Storage Store op IoT Edge (preview)

Azure Blob-opslag op IoT Edge biedt een [blok-blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) opslagoplossing aan de rand. Een blob storage-module op uw IoT Edge-apparaat zich gedraagt als een Azure blok-blob-service, maar de blok-blobs worden lokaal opgeslagen op uw IoT Edge-apparaat. U kunt toegang tot uw blobs met behulp van de methodes SDK van Azure storage of blob API-aanroepen die u al hebt gebruikt om te blokkeren. 

Deze module wordt geleverd met **automatisch opslaglagen** en **automatisch verlopen** functies.

> [!NOTE]
> Automatische lagen en automatisch verlopen functionaliteit zijn momenteel alleen beschikbaar in Linux AMD64- en Linux ARM32.

**Automatische lagen** is een configureerbare functionaliteit, zodat u kunt het automatisch uploaden van de gegevens van uw lokale blob-opslag naar Azure met onregelmatige connectiviteit-ondersteuning voor internet. Hiermee kunt u:
- Schakel aan, / uit, van de toegangslagen
- Kies de volgorde waarin de gegevens worden gekopieerd naar Azure, zoals NewestFirst of OldestFirst
- Geef de Azure Storage-account die u wilt dat uw gegevens die zijn geüpload.
- Geef de containers die u wilt uploaden naar Azure. Deze module kunt u zowel bron- en container-namen opgeven.
- Volledig opslaglagen voor blob (met behulp van `Put Blob` bewerking) of te blokkeren op blobniveau (met behulp van `Put Block` en `Put Block List` operations).

Deze module maakt gebruik van blok level tiering wanneer uw blob uit blokken bestaat. Hier volgen enkele algemene scenario's:
- Uw toepassing blokken van een eerder geüploade blob updates, deze module wordt alleen de bijgewerkte blokken en niet de hele blob uploaden.
- De module is blob uploaden en verbinding met internet wordt opgeslagen en wanneer de connectiviteit is weer opnieuw dat het wordt alleen de resterende blokken en niet de hele blob uploaden.

Als het beëindigen van een onverwachte processen (zoals stroomstoring) tijdens het uploaden van een blob gebeurt, wordt alle blokken die vervallen voor het uploaden zijn geüpload, wanneer de module weer online komt.

**Automatisch verlopen** is een configureerbare functionaliteit waar deze module wordt automatisch verwijderd uw blobs uit de lokale opslag wanneer Time to Live (TTL) is verlopen. Dit wordt gemeten in minuten. Hiermee kunt u:
- Schakel aan, / uit, van de functie auto-vervaldatum
- Geef de TTL-waarde in minuten

Scenario's waarbij gegevens zoals video's, afbeeldingen, financiële gegevens, ziekenhuis gegevens of gegevens die moeten worden bewaard lokaal, later die kan worden lokaal verwerkt of verzonden naar de cloud zijn goede voorbeelden het gebruik van deze module.

In dit artikel vindt u instructies voor het implementeren van een Azure Blob-opslag op IoT Edge-container die wordt uitgevoerd van een blob-service op uw IoT Edge-apparaat. 

>[!NOTE]
>Azure Blob-opslag op IoT Edge is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Bekijk de video voor een korte inleiding in
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

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

2. Kies het IoT Edge-apparaat dat deze module moet ontvangen. Geef op de pagina **Doelapparaten voor IoT Edge-module** de volgende informatie op:

   1. Selecteer de **abonnement** waarin de IoT-hub die u gebruikt.

   2. Selecteer uw **IoT-Hub**.

   3. Als u weet dat uw **IoT Edge-apparaatnaam**, voer die in het tekstvak in. Of selecteer **Apparaat kiezen** om een apparaat te kiezen uit een lijst met IoT Edge-apparaten in uw IoT Hub. 
   
   4. Selecteer **Maken**.

   Nu u een IoT Edge-module in de Azure Marketplace hebt gekozen en een IoT Edge-apparaat hebt geselecteerd dat de module moet ontvangen, gaat u verder naar een wizard met drie stappen om precies te definiëren hoe de module wordt geïmplementeerd.

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

   4. Stel [auto-lagen en automatisch verlopen](#configure-auto-tiering-and-auto-expiration-via-azure-portal) in de gewenste eigenschappen. Lijst met [automatisch opslaglagen](#auto-tiering-properties) en [automatisch verlopen](#auto-expiration-properties) eigenschappen en hun mogelijke waarden. 

   5. Selecteer **Opslaan**. 

4. Selecteer **Volgende** om door te gaan naar de volgende stap van de wizard.
5. In de **Routes opgeven** stap van de wizard de optie **volgende**.
6. Selecteer in de stap **Implementatie beoordelen** van de wizard de optie **Verzenden**.
7. Nadat u de implementatie hebt ingediend, wordt u terugkeert naar de **IoT Edge** pagina van uw IoT-hub. Selecteer de IoT Edge-apparaat dat u met de implementatie te openen van de details ervan wordt benaderd. 
8. Controleer of dat de blob storage-module wordt vermeld als zowel in de apparaatgegevens **opgegeven in de implementatie** en **gerapporteerd door device**. Het duurt een paar seconden voor de module worden gestart op het apparaat en klikt u vervolgens teruggemeld aan IoT-Hub. Vernieuw de pagina om een bijgewerkte status te bekijken. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code-sjablonen

Azure IoT Edge zorgt voor sjablonen in Visual Studio Code om u te helpen bij het edge-oplossingen te ontwikkelen. Deze stappen vereisen dat u met [Visual Studio Code](https://code.visualstudio.com/) op uw ontwikkelcomputer geïnstalleerd en geconfigureerd met de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

Gebruik de volgende stappen voor het maken van een nieuwe IoT Edge-oplossing met een blob storage-module en configureer het manifest van de implementatie. 

1. Selecteer **weergave** > **Command Palette**. 

2. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
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

5. Configureer [auto-lagen en automatisch verlopen](#configure-auto-tiering-and-auto-expiration-via-vscode). Lijst met [automatisch opslaglagen](#auto-tiering-properties) en [automatisch verlopen](#auto-expiration-properties) eigenschappen

6. Sla het bestand **deployment.template.json** op.

7. Open de **.env** bestand in de werkruimte van uw oplossing. 

8. Het .env-bestand voor het ontvangen van container registerreferenties is ingesteld, maar u niet nodig hebt die voor de installatiekopie van het blob-opslag omdat het is openbaar beschikbaar. In plaats daarvan moet u het bestand vervangen door twee nieuwe omgevingsvariabelen: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

9. Geef een waarde voor `STORAGE_ACCOUNT_NAME`, namen van drie naar 24 tekens lang zijn en kleine letters en cijfers moeten zijn. Geef een 64-byte base64-sleutel voor de `STORAGE_ACCOUNT_KEY`. U kunt een sleutel met de hulpprogramma's zoals genereren [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). U gebruikt deze referenties voor toegang tot de blob-opslag van andere modules. 

   Licentie omvatten geen spaties of aanhalingstekens rond de waarden die u opgeeft. 

10. Sla het **.env**-bestand op. 

11. Met de rechtermuisknop op **deployment.template.json** en selecteer **genereren IoT Edge-implementatie manifest**. 

12. Visual Studio Code wordt de informatie die u in de deployment.template.json en .env en gebruikt voor het maken van een nieuwe implementatie-manifestbestand. Het manifest van de implementatie wordt gemaakt in een nieuwe **config** map in de werkruimte van uw oplossing. Zodra u dat bestand hebt, kunt u de stappen in [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md) of [implementeren Azure IoT Edge-modules met de Azure CLI 2.0](how-to-deploy-modules-cli.md).

## <a name="auto-tiering-and-auto-expiration-properties-and-configuration"></a>Automatische lagen en automatisch verlopen eigenschappen en configuratie

Gebruik de gewenste eigenschappen om in te stellen met automatische lagen voor en automatisch verlopen. Ze kunnen worden ingesteld tijdens de implementatie of later worden gewijzigd door het bewerken van de moduledubbel zonder de noodzaak om opnieuw te implementeren. We raden het "Moduledubbel' controleren voor `reported configuration` en `configurationValidation` om ervoor te zorgen waarden correct zijn doorgegeven.

### <a name="auto-tiering-properties"></a>Eigenschappen van automatische lagen 
De naam van deze instelling is `tieringSettings`
| Veld | Mogelijke waarden | Uitleg |
| ----- | ----- | ---- |
| tieringOn | waar of ONWAAR | Standaard is ingesteld op `false`, als u wilt inschakelen op instellen op `true`|
| backlogPolicy | NewestFirst, OldestFirst | Kunt u kiezen de volgorde waarin de gegevens worden gekopieerd naar Azure. Standaard is ingesteld op `OldestFirst`. De order wordt bepaald door de tijd voor laatst gewijzigd van Blob |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` een verbindingsreeks waarmee u om op te geven van de Azure Storage-account die u wilt dat uw gegevens geüpload. Geef `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Toevoegen van de juiste EndpointSuffix van Azure waar gegevens worden geüpload, dit varieert voor Global Azure, Azure Government en Microsoft Azure Stack. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Hiermee kunt u opgeven van de containernamen van de die u wilt uploaden naar Azure. Deze module kunt u zowel bron- en container-namen opgeven. Als u de naam van de container niet opgeeft, wordt deze automatisch de containernaam van de als toegewezen `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. U kunt de sjabloon van tekenreeksen voor de naam van de doel-container, bekijk de kolom mogelijke waarden maken. <br>* %h -> naam van de IoT-Hub (3-50 tekens). <br>* %d -> IoT-apparaat-Id (1 tot 129 tekens). <br>* %m -> modulenaam (1 tot 64 tekens). <br>* %c -> naam van de Broncontainer (3 tot 63 tekens). <br><br>Maximale grootte van de containernaam van de is 63 tekens, bij het automatisch toewijzen van de naam van de container als de grootte van de container is groter dan 63 tekens lang, die deze wordt trim elke sectie (IoTHubName, IotEdgeDeviceName, modulenaam, ContainerName) tot 15 tekens. |

### <a name="auto-expiration-properties"></a>Eigenschappen van auto-vervaldatum
De naam van deze instelling is `ttlSettings`
| Veld | Mogelijke waarden | Uitleg |
| ----- | ----- | ---- |
| ttlOn | waar of ONWAAR | Standaard is ingesteld op `false`, als u wilt inschakelen op instellen op `true`|
| timeToLiveInMinutes | `<minutes>` | Geef de TTL-waarde in minuten. De module wordt automatisch uw blobs uit de lokale opslag verwijderd wanneer de TTL verloopt |

### <a name="configure-auto-tiering-and-auto-expiration-via-azure-portal"></a>Configureren van automatische-lagen en automatisch verlopen via Azure portal

Stel de gewenste eigenschappen om in te schakelen automatisch opslaglagen en automatisch verlopen, kunt u deze waarden instellen:

- **Tijdens de initiële implementatie**: Kopieer de JSON in **de gewenste eigenschappen van de moduledubbel Set** vak. Elke eigenschap configureren met de juiste waarde, opslaan en doorgaan met de implementatie.

   ```json
   {
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>, 
         "timeToLiveInMinutes": <timeToLiveInMinutes> 
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

   ```

 ![stelt eigenschappen automatisch opslaglagen en automatisch verlopen](./media/how-to-store-data-blob/iotedge_custom_module.png)

- **Nadat de module is geïmplementeerd via de functie "Identiteit Moduledubbel"**: Ga naar 'Een identiteit van een Moduledubbel' van deze module, kopieert u de JSON onder de gewenste eigenschappen en configureren van elke eigenschap met de juiste waarde opslaan. In de Json van de identiteit ' Moduledubbel"Zorg ervoor dat telkens wanneer u toevoegt of een bijwerkt gewenste eigenschap, de `reported configuration` gedeelte worden de wijzigingen en de `configurationValidation` sectie rapporten is geslaagd voor elke eigenschap.

   ```json 
    "ttlSettings": {
        "ttlOn": <true, false>, 
        "timeToLiveInMinutes": <timeToLiveInMinutes> 
    },
    "tieringSettings": {
        "tieringOn": <true, false>,
        "backlogPolicy": "<NewestFirst, OldestFirst>",
        "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
        "tieredContainers": {
            "<source container name1>": {
                "target": "<target container name1>"
            }
        }
    }

   ```

![warmtemeting + ttl module_identity_twin](./media/how-to-store-data-blob/module_identity_twin.png) 

### <a name="configure-auto-tiering-and-auto-expiration-via-vscode"></a>Automatische lagen en automatisch verlopen via VSCode configureren

- **Tijdens de initiële implementatie**: Voeg de onderstaande JSON in uw deployment.template.json voor het definiëren van de gewenste eigenschappen voor deze module. Elke eigenschap configureren met de juiste waarde en sla deze op.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>, 
         "timeToLiveInMinutes": <timeToLiveInMinutes> 
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

   ```

Hier volgt een voorbeeld van de gewenste eigenschappen voor deze module: ![gewenste eigenschappen voor azureblobstorageoniotedge - VS Code instellen](./media/how-to-store-data-blob/tiering_ttl.png)

- **Nadat de module is geïmplementeerd via 'Moduledubbel'**: [Bewerken van de Moduledubbel](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) van deze module, kopieert u de JSON onder de gewenste eigenschappen, elke eigenschap met de juiste waarde configureren en opslaan. In de Json "Moduledubbel" Zorg ervoor dat telkens wanneer u toevoegt of een bijwerkt gewenste eigenschap, de `reported configuration` gedeelte worden de wijzigingen en de `configurationValidation` sectie rapporten is geslaagd voor elke eigenschap.

   ```json 
    "ttlSettings": {
        "ttlOn": <true, false>, 
        "timeToLiveInMinutes": <timeToLiveInMinutes> 
    },
    "tieringSettings": {
        "tieringOn": <true, false>,
        "backlogPolicy": "<NewestFirst, OldestFirst>",
        "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
        "tieredContainers": {
            "<source container name1>": {
                "target": "<target container name1>"
            }
        }
    }

   ```

## <a name="connect-to-your-blob-storage-module"></a>Verbinding maken met uw blob storage-module

U kunt de accountnaam en accountsleutel dat u hebt geconfigureerd voor de module voor toegang tot de blob-opslag op uw IoT Edge-apparaat gebruiken. 

Geef uw IoT Edge-apparaat als de blobeindpunt voor alle opslag aanvragen die u uitvoert. U kunt [maken van een verbindingsreeks voor een expliciete opslageindpunt](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) met behulp van de gegevens van de IoT Edge-apparaat en de accountnaam op die u hebt geconfigureerd. 

1. Voor modules die zijn geïmplementeerd op de dezelfde edge-apparaat waarop de 'Azure Blob-opslag op IoT Edge' wordt uitgevoerd, het eindpunt van blob is: `http://<module name>:11002/<account name>`. 
2. Voor modules die zijn geïmplementeerd op verschillende edge-apparaat, dan het edge-apparaat waar 'Azure Blob-opslag op IoT Edge' wordt uitgevoerd en is afhankelijk van uw installatie van de blob-eindpunt: `http://<device IP >:11002/<account name>` of `http://<IoT Edge device hostname>:11002/<account name>` of `http://<FQDN>:11002/<account name>`

## <a name="deploy-multiple-instances"></a>Meerdere exemplaren implementeren

Als u implementeren meerdere exemplaren van Azure Blob-opslag op IoT Edge wilt, moet u verschillende opslagpad bieden en wijzig de HostPort die de module wordt gebonden aan. De blob-opslag-modules altijd poort 11002 in de container beschikbaar, maar u kunt aangeven welke poort gebonden aan op de host. 

De module bewerken Opties voor het wijzigen van de waarde HostPort maken:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Wanneer u verbinding met aanvullende blob storage-modules maken, wijzigt u het eindpunt om te verwijzen naar de bijgewerkte host-poort. 

## <a name="try-it-out"></a>Uitproberen

### <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Storage-Quickstart-voorbeelden
De Azure Blob Storage-documentatie bevat snelstartgidsen die de voorbeeldcode in verschillende talen. U kunt deze voorbeelden als u wilt testen van Azure Blob-opslag op IoT Edge door het veranderen van de blob-eindpunt om te verwijzen naar uw blob storage-module kunt uitvoeren. Volg de stappen voor het [verbinding maken met uw blob storage-module](#connect-to-your-blob-storage-module)

De volgende quickstarts talen die ook worden ondersteund door de IoT Edge, zodat u ze als IoT Edge-modules samen met de blob storage-module implementeren kunt gebruiken:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md) 

### <a name="azure-storage-explorer"></a>Azure Opslagverkenner
U kunt ook proberen [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) verbinding maken met uw lokale storage-account. We hebben geprobeerd met [vorige versie 1.5.0](https://go.microsoft.com/fwlink/?LinkId=809306&clcid=0x409) van Azure Explorer.
> [!NOTE]
> U kunt er fouten optreden tijdens het uitvoeren van onderstaande stappen te volgen, negeren en vernieuwen. 

1. Download en installeer Azure Storage Explorer
2. Verbinding maken met Azure Storage met behulp van een verbindingsreeks
3. Verbindingsreeks opgeven: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`
4. Doorloop de stappen om verbinding te maken.
5. Container in uw lokale storage-account maken
6. Beginnen met uploaden van bestanden als blok-blobs.
> [!NOTE]
> Schakel het selectievakje uit als u wilt uploaden als pagina-blobs. Deze module biedt geen ondersteuning voor pagina-blobs. U krijgt deze vragen tijdens het uploaden van bestanden, zoals ISO, VHD, vhdx of een grote bestanden.

7. U kunt verbinding maken met Azure storage-accounts waarbij u de gegevens wilt uploaden. Het biedt een weergave voor uw account met lokale opslag en de Azure storage-account

## <a name="supported-storage-operations"></a>Ondersteunde opslagbewerkingen

BLOB storage-modules op IoT Edge gebruikt u de dezelfde Azure Storage-SDK's en consistent zijn met de versie van 17-04-2017 van de API van Azure Storage voor blok-blob-eindpunten. Latere releases zijn afhankelijk van de behoeften van klanten.

Niet alle Azure Blob Storage-bewerkingen worden ondersteund door Azure Blob-opslag op IoT Edge. De volgende sectie vindt u de ondersteunde en niet-ondersteunde bewerkingen.

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
* Ophalen en instellen van de container ACL
* Metagegevens van de container set

Niet-ondersteund:
* Lease-container

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

##<a name="feedback"></a>Feedback:
Uw feedback is zeer belangrijk voor ons, zodat deze module en de bijbehorende functies handig en eenvoudig te gebruiken. Deel uw feedback en laat het ons weten hoe we kunnen verbeteren.

U kunt contact met ons op absiotfeedback@microsoft.com 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md)

