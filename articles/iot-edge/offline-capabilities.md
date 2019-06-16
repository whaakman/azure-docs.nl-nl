---
title: Offline - apparaten werkt Azure IoT Edge | Microsoft Docs
description: Begrijp hoe IoT Edge-apparaten en -modules kunnen werken zonder internetverbinding gedurende langere tijd wordt opgelost, en hoe normale IoT-apparaten offline te werken op IoT Edge kunt inschakelen.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 4f3e5c1566271573b43e24a1749b42daa7530555
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051950"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Meer informatie over uitgebreide offline mogelijkheden voor IoT Edge-apparaten, modules en onderliggende apparaten

Azure IoT Edge biedt ondersteuning voor uitgebreide offline bewerkingen op uw IoT Edge-apparaten en offline-bewerkingen op IoT Edge onderliggende apparaten te kunnen. Als een IoT Edge-apparaat een mogelijkheid om te verbinden met IoT Hub heeft, kunnen het en eventuele onderliggende-apparaten blijven functie met onregelmatige of geen verbinding met internet. 


## <a name="how-it-works"></a>Hoe werkt het?

Wanneer een IoT Edge-apparaat in de offlinemodus bevindt gaat, neemt de IoT Edge hub drie rollen. Eerst worden opgeslagen berichten die heengaat upstream en slaat ze op totdat het apparaat opnieuw verbinding maakt. Ten tweede, fungeert deze namens IoT Hub voor verificatie van modules en onderliggende apparaten zodat ze kunnen blijven functioneren. Ten slotte maakt communicatie tussen de apparaten van de onderliggende die normaal zou gaan via IoT Hub mogelijk. 

Het volgende voorbeeld ziet u de werking van een IoT Edge-scenario in de offlinemodus:

1. **Apparaten configureren**

   IoT Edge-apparaten hebben automatisch offline-mogelijkheden ingeschakeld. Om uit te breiden die mogelijkheid met andere IoT-apparaten, moet u een bovenliggende / onderliggende relatie tussen de apparaten in IoT Hub declareren. Vervolgens configureert u de onderliggende apparaten hun apparaat toegewezen bovenliggende vertrouwen en de apparaat-naar-cloud-communicatie via de bovenliggende als een gateway routeren. 

2. **Synchronisatie met IoT Hub**

   Ten minste eenmaal na de installatie van de IoT Edge-runtime, de IoT Edge-apparaat moet online zijn om te synchroniseren met IoT Hub. In deze synchronisatie haalt de IoT Edge-apparaat meer informatie over alle onderliggende apparaten zijn toegewezen. De IoT Edge-apparaat wordt ook veilig updates van de lokale cache om in te schakelen van offline-bewerkingen en worden instellingen voor lokale opslag van berichten over telemetrie opgehaald. 

3. **Offline gaan**

   Terwijl niet verbonden met IoT Hub, kunnen de IoT Edge-apparaat, de geïmplementeerde modules en alle onderliggende items IoT-apparaten werken voor onbepaalde tijd. Modules en onderliggende apparaten kunnen starten en opnieuw opstarten door te verifiëren bij de IoT Edge hub tijdens offline. Telemetrie upstream gebonden aan IoT Hub worden lokaal opgeslagen. Communicatie tussen modules of onderliggende IoT-apparaten wordt onderhouden door directe methoden of berichten. 

4. **Opnieuw verbinding maken met en synchroniseren met IoT Hub**

   Zodra de verbinding met IoT Hub is hersteld, wordt de IoT Edge-apparaat weer wordt gesynchroniseerd. Lokaal opgeslagen berichten worden bezorgd in dezelfde volgorde als waarin ze zijn opgeslagen. Eventuele verschillen tussen de gewenste en gerapporteerde eigenschappen van de modules en -apparaten zijn afgestemd. De IoT Edge-apparaat bijwerken eventuele wijzigingen in een set toegewezen onderliggende IoT-apparaten.

## <a name="restrictions-and-limits"></a>Beperkingen en limieten

De uitgebreide offline-mogelijkheden die worden beschreven in dit artikel zijn beschikbaar in [IoT Edge versie 1.0.4 of hoger](https://github.com/Azure/azure-iotedge/releases). Eerdere versies hebben een subset van de offline functies. IoT Edge bestaande apparaten waarvoor geen uitgebreide mogelijkheden voor offline kunnen niet worden bijgewerkt door het veranderen van de runtimeversie, maar moeten opnieuw worden geconfigureerd met een nieuwe IoT Edge-apparaat-id te krijgen van deze functies. 

Uitgebreide offline ondersteuning is beschikbaar in alle regio's waar de IoT Hub verkrijgbaar is, **behalve** VS-Oost.

Alleen niet - IoT-Edge-apparaten kunnen worden toegevoegd als onderliggende apparaten. 

IoT Edge-apparaten en hun apparaten toegewezen onderliggende kunnen worden gebruikt voor onbepaalde tijd offline na de initiële, eenmalige-synchronisatie. Opslag van berichten is afhankelijk van time to live (TTL) instelling en de beschikbare schijfruimte voor het opslaan van de berichten. 

## <a name="set-up-parent-and-child-devices"></a>Bovenliggende en onderliggende apparaten instellen

Voor een IoT Edge-apparaat uit te breiden zijn uitgebreide offline mogelijkheden voor het onderliggende IoT-apparaten, moet u twee stappen uit te voeren. Eerst declareert de bovenliggende / onderliggende relaties in de Azure-portal. Ten tweede een vertrouwensrelatie tussen het apparaat van de bovenliggende en onderliggende apparaten maken en configureren van apparaat-naar-cloud-communicatie naar de bovenliggende als een gateway doorlopen. 

### <a name="assign-child-devices"></a>Onderliggende apparaten toewijzen

Onderliggende apparaten kunnen elk IoT Edge-apparaat dat is geregistreerd bij dezelfde IoT Hub zijn. Bovenliggende apparaten kunnen meerdere onderliggende apparaten hebben, maar een onderliggend apparaat heeft slechts één bovenliggend object. Er zijn drie opties voor het onderliggende apparaten ingesteld op een edge-apparaat: via de Azure-portal, met behulp van de Azure CLI of met behulp van de IoT Hub-service-SDK. 

De volgende secties vindt u voorbeelden van hoe u de bovenliggende/onderliggende relatie in IoT Hub voor bestaande IoT-apparaten kunt declareren. Als u nieuwe apparaat-id's voor uw kind apparaten maakt, Zie [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md) voor meer informatie.

#### <a name="option-1-iot-hub-portal"></a>Optie 1: IoT Hub-Portal

U kunt de bovenliggende / onderliggende relatie aangeven bij het maken van een nieuw apparaat. Of voor bestaande apparaten, kunt u de relatie van de detailpagina van het apparaat van de bovenliggende IoT Edge-apparaat of de onderliggende IoT-apparaat declareren. 

   ![Onderliggende apparaten beheren vanaf de pagina met details van IoT Edge-apparaat](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Optie 2: Gebruik de `az` opdrachtregel-hulpprogramma

Met behulp van de [Azure-opdrachtregelinterface](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) met [IoT-extensie](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 of hoger), kunt u relaties tussen bovenliggende-onderliggende met beheren de [device-identity](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) subopdrachten. Het volgende voorbeeld wordt een query om toe te wijzen alle IoT Edge-apparaten in de hub zijn onderliggende apparaten van een IoT Edge-apparaat. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

U kunt de [query](../iot-hub/iot-hub-devguide-query-language.md) om een andere subset van apparaten te selecteren. De opdracht kan enkele seconden duren als u een groot aantal apparaten opgeven.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Optie 3: IoT Hub-Service SDK gebruiken 

Ten slotte kunt u beheren via een programma met behulp van relaties tussen bovenliggende onderliggende C#, Java of Service-SDK voor Node.js IoT-Hub. Hier volgt een [voorbeeld van het toewijzen van een onderliggend apparaat](https://aka.ms/set-child-iot-device-c-sharp) met behulp van de C# SDK.

### <a name="set-up-the-parent-device-as-a-gateway"></a>Het bovenliggende apparaat instellen als een gateway

U kunt zien van een bovenliggende/onderliggende relatie als een transparante gateway, waarbij de onderliggende apparaat heeft een eigen identiteit in IoT Hub, maar communiceert via de cloud via het bovenliggende object. Voor veilige communicatie moet het apparaat van de onderliggende kunnen verifiëren dat het apparaat van de bovenliggende afkomstig van een vertrouwde bron is. Anders instellen derden van schadelijke apparaten ouders imiteren en communicatie wordt onderschept. 

Eén manier om u te maken van de vertrouwensrelatie wordt beschreven in de volgende artikelen: 
* [Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren](how-to-create-transparent-gateway.md)
* [Een downstream (onderliggend)-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>DNS-servers opgeven 

Voor een betere stabiliteit, is het raadzaam opgeven van de adressen van de DNS-server in uw omgeving gebruikt. Twee opties om te zien [instellen van de DNS-server in het artikel over probleemoplossing](troubleshoot.md#resolution-7).

## <a name="optional-offline-settings"></a>Optionele offline-instellingen

Als uw apparaten naar offline gaat, worden alle apparaat-naar-cloud-berichten in de bovenliggende IoT Edge-apparaat opgeslagen totdat de verbinding is hersteld. De IoT Edge hub-module beheert de opslag en het doorsturen van offline-berichten. Voor apparaten die gedurende langere tijd offline kunnen gaan, prestaties te optimaliseren door twee IoT Edge hub-instellingen configureren. 

Eerst, verhoogt de tijd aan het live instellen zodat de IoT Edge hub berichten lang genoeg is voor uw apparaat blijven te herstellen. Vervolgens voegt u extra schijfruimte voor de berichtopslag. 

### <a name="time-to-live"></a>Time To Live

Time to live-instelling is de hoeveelheid tijd (in seconden) die een bericht wachten kunt moet worden geleverd voordat deze verloopt. De standaardwaarde is 7200 seconden (twee uur). De maximale waarde wordt alleen beperkt door de maximale waarde van een geheel getal variabele ongeveer 2 miljard is. 

Deze instelling is een gewenste eigenschap van de IoT Edge-hub die is opgeslagen in de moduledubbel. U kunt deze configureren in Azure portal of rechtstreeks in het manifest van de implementatie. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Extra offline opslag

Berichten worden opgeslagen in een van de IoT Edge hub container bestandssysteem standaard. Als deze hoeveelheid opslag niet voldoende voor de behoeften van uw offline is, kunt u speciaal aan lokale opslag op het IoT Edge-apparaat. Maak een omgevingsvariabele voor de IoT Edge hub die naar een opslagmap in de container verwijst. Vervolgens gebruikt u de opties voor het maken die opslagmap binden aan een map op de hostcomputer. 

U kunt omgevingsvariabelen en de opties voor het maken van de IoT Edge hub-module configureren in Azure portal in de **geavanceerde instellingen voor Edge-Runtime configureren** sectie. Of u kunt deze rechtstreeks in het manifest van de implementatie configureren. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Vervang `<HostStoragePath>` en `<ModuleStoragePath>` opslagpad pad; de host en de module moet met uw storage-host en de module een absoluut pad zijn. In de opties voor het maken, de host en de module opslagpaden samen te binden. Vervolgens maakt u een omgevingsvariabele die naar het opslagpad van de module verwijst.  

Bijvoorbeeld, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` betekent dat de map **/etc/iotedge/storage** op uw host systeem is gekoppeld aan de map **/iotedge/opslag/** voor de container. Of een ander voorbeeld voor Windows-systemen, `"Binds":["C:\\temp:C:\\contemp"]` betekent dat de map **C:\\temp** op uw host systeem is gekoppeld aan de map **C:\\contemp** voor de container. 

U vindt ook meer informatie over het maken van opties van [docker docs](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het instellen van een transparante gateway voor uw apparaat bovenliggend/onderliggend verbindingen: 

* [Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren](how-to-create-transparent-gateway.md)
* [Verifiëren met Azure IoT Hub een downstream-apparaat](how-to-authenticate-downstream-device.md)
* [Een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md)
