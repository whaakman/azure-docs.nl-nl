---
title: Transparante gateway-apparaat - Azure IoT Edge maken | Microsoft Docs
description: Een Azure IoT Edge-apparaat gebruiken als een transparante gateway waarmee gegevens uit de downstream-apparaten kan worden verwerkt.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 5881adb7e2fc0d52cc2037d3d4a9e986b3e29d74
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058382"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren

In dit artikel vindt u gedetailleerde instructies voor het configureren van een IoT Edge-apparaat werken als een transparante gateway voor andere apparaten om te communiceren met IoT Hub. In dit artikel wordt de term *IoT Edge-gateway* verwijst naar een IoT Edge-apparaat gebruikt als een transparante gateway. Zie voor meer informatie, [hoe een IoT Edge-apparaat kan worden gebruikt als een gateway](./iot-edge-as-gateway.md).

>[!NOTE]
>Op dit moment:
> * Edge-apparaten kunnen geen verbinding maken met IoT Edge-gateways. 
> * Aangesloten apparaten niet uploaden van bestanden gebruiken.

Er zijn drie algemene stappen voor het instellen van een geslaagde transparante gateway-verbinding. In dit artikel bevat informatie over de eerste stap:

1. **Het gateway-apparaat moet kunnen veilig verbinding maken met downstream-apparaten, ontvangen een bericht van downstream-apparaten en het routeren van berichten naar de juiste bestemming.**
2. De downstream moet een apparaat-id te kunnen verifiëren met IoT Hub en weten om te communiceren via de gateway-apparaat. Zie voor meer informatie, [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).
3. De downstream-apparaat moet kunnen veilig verbinding maken met de gateway-apparaat. Zie voor meer informatie, [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md).


Een apparaat moet worden gebruikt als een gateway, moet kunnen veilig verbinding maken met de downstream-apparaten. Azure IoT Edge kunt u een openbare-sleutelinfrastructuur (PKI) gebruiken voor het instellen van beveiligde verbindingen tussen apparaten. In dit geval toestemming we een downstream apparaat verbinding maakt met een IoT Edge-apparaat als een transparante gateway fungeert. Redelijke om beveiliging te handhaven, moet het downstream apparaat de identiteit van de gateway-apparaat bevestigen. Deze identiteitscontrole voorkomt u dat uw apparaten verbinding maakt met mogelijk schadelijke gateways.

Een downstream-apparaat mag een toepassing of het platform waarvoor een identiteit die is gemaakt met de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) cloudservice. In veel gevallen gebruikt u deze toepassingen de [Azure IoT device-SDK](../iot-hub/iot-hub-devguide-sdks.md). Praktische overwegingen, een downstream apparaat mogelijk ook een toepassing die wordt uitgevoerd op het IoT Edge-gateway-apparaat zelf. 

U kunt een certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat-instellingen die u gebruiken zou om in te schakelen [x.509-CA-beveiliging](../iot-hub/iot-hub-x509ca-overview.md) in IoT-Hub, die bestaat uit een X.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub (de IoT hub basis-CA), een reeks van certificaten die zijn ondertekend Met deze CA, en een Certificeringsinstantie voor de IoT Edge-apparaat.

![Certificaat-gatewayupgrade](./media/how-to-create-transparent-gateway/gateway-setup.png)

>[!NOTE]
>De term 'basis-CA' gebruikt in dit artikel verwijst naar het openbare certificaat van de bovenste certificeringsinstantie van de PKI-certificaatketen en niet per se de hoofdmap van het certificaat van een gepubliceerde certificeringsinstantie. In veel gevallen is eigenlijk een tussenliggende CA openbaar certificaat. 

De gateway geeft de IoT Edge device CA-certificaat aan het downstream apparaat tijdens de initialisatie van de verbinding. Het downstream apparaat controleert om ervoor te zorgen dat het IoT Edge device CA-certificaat is ondertekend door de CA-basiscertificaat. Dit proces kan de downstream-apparaat om te bevestigen dat de gateway van een vertrouwde bron afkomstig is.

De volgende stappen helpen u bij het proces van het maken van de certificaten en ze in de juiste plaatsen worden uitgevoerd op de gateway te installeren. U kunt elke virtuele machine gebruiken voor het genereren van de certificaten, en kopieer deze vervolgens naar uw IoT Edge-apparaat. 

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat configureren als een gateway. Gebruik de IoT Edge-installatiestappen voor een van de volgende besturingssystemen:
  * [Windows](./how-to-install-iot-edge-windows.md)
  * [Linux x64](./how-to-install-iot-edge-linux.md)
  * [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

In dit artikel verwijst naar de *gateway hostnaam* op verschillende tijdstippen. De hostnaam van de gateway is gedeclareerd in de **hostnaam** parameter van het bestand config.yaml op het IoT Edge-gatewayapparaat. Deze wordt gebruikt voor het maken van de certificaten in dit artikel en wordt verwezen in de verbindingsreeks van de downstream-apparaten. De hostnaam van de gateway moet worden omgezet naar een IP-adres, hetzij met behulp van DNS- of een hostbestandvermelding.

## <a name="generate-certificates-with-windows"></a>Genereren van certificaten met Windows

Gebruik de stappen in deze sectie voor het genereren van testcertificaten voor op Windows. U kunt een Windows-machine gebruiken voor het genereren van de certificaten en kopieer deze vervolgens boven aan elk IoT Edge-apparaat waarop een ondersteund besturingssysteem. 

De certificaten die zijn gegenereerd in deze sectie zijn bedoeld alleen voor testdoeleinden. 

### <a name="install-openssl"></a>OpenSSL installeren

OpenSSL voor Windows installeren op de computer die u gebruikt voor het genereren van de certificaten. Als u al OpenSSL geïnstalleerd op uw Windows-apparaat hebt, kan u deze stap overslaan, maar zorg ervoor dat openssl.exe is beschikbaar in uw padomgevingsvariabele bevindt. 

Er zijn verschillende manieren kunt u OpenSSL installeren:

* **Eenvoudiger:** Download en installeer een [OpenSSL binaire bestanden van derden](https://wiki.openssl.org/index.php/Binaries), bijvoorbeeld van [OpenSSL op SourceForge](https://sourceforge.net/projects/openssl/). Het volledige pad toevoegen aan openssl.exe aan de omgevingsvariabele PATH. 
   
* **Aanbevolen:** De OpenSSL-broncode downloaden en bouwen van de binaire bestanden op uw computer zelf of via [vcpkg](https://github.com/Microsoft/vcpkg). De onderstaande instructies gebruiken vcpkg broncode, compileren en OpenSSL installeren op uw Windows-computer met eenvoudige stappen downloadt.

   1. Ga naar een map waar u vcpkg installeren. Verwijzen we naar deze map als  *\<VCPKGDIR >* . Volg de instructies voor het downloaden en installeren [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Eenmaal vcpkg is geïnstalleerd en voer de volgende opdracht vanaf een powershell-prompt de OpenSSL-pakket voor Windows x64 installeren. De installatie duurt meestal ongeveer 5 minuten.

      ```powershell
      .\vcpkg install openssl:x64-windows
      ```
   3. Voeg `<VCPKGDIR>\installed\x64-windows\tools\openssl` aan de omgevingsvariabele PATH zodat het bestand openssl.exe beschikbaar voor de aanroep is.

### <a name="prepare-creation-scripts"></a>Voorbereiden van scripts voor het maken

De git-opslagplaats voor Azure IoT Edge bevat scripts die u gebruiken kunt voor het genereren van testcertificaten. In deze sectie maakt u kloon de opslagplaats van de IoT Edge en de scripts uit te voeren. 

1. Open een PowerShell-venster in de beheerdersmodus. 

2. Kloon de git-opslagplaats met scripts voor het genereren van certificaten voor niet-productie. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken. Gebruik de `git clone` opdracht of [downloaden van het ZIP-bestand](https://github.com/Azure/iotedge/archive/master.zip). 

   ```powershell
   git clone https://github.com/Azure/iotedge.git
   ```

3. Ga naar de map waarin u wilt werken. In dit artikel, noemen we deze map  *\<WRKDIR >* . Alle certificaten en sleutels, wordt in deze werkmap worden gemaakt.

4. Kopieer de bestanden voor configuratie en het script van de gekloonde opslagplaats naar uw werkmap. 

   ```powershell
   copy <path>\iotedge\tools\CACertificates\*.cnf .
   copy <path>\iotedge\tools\CACertificates\ca-certs.ps1 .
   ```

   Als u hebt gedownload van de opslagplaats als ZIP, wordt de mapnaam van de is `iotedge-master` en de rest van het pad is hetzelfde. 
<!--
5. Set environment variable OPENSSL_CONF to use the openssl_root_ca.cnf configuration file.

    ```powershell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```
-->
5. Schakel in PowerShell de scripts uit te voeren.

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Breng de functies die worden gebruikt door de scripts van PowerShell globale naamruimte.
   
   ```powershell
   . .\ca-certs.ps1
   ```

   Het PowerShell-venster wordt een waarschuwing dat de certificaten die worden gegenereerd door dit script worden alleen voor testdoeleinden en mag niet worden gebruikt in productiescenario's weergegeven.

8. Controleren of de OpenSSL correct is geïnstalleerd en zorg ervoor dat er niet naamconflicten met bestaande certificaten. Als er problemen zijn, moet het script voor het oplossen van deze op uw systeem te beschrijven.

   ```powershell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Certificaten maken

In deze sectie maakt u drie certificaten maken en vervolgens verbinding maken in een keten. De certificaten in een keten-bestand plaatsen, kunt u eenvoudig installeren op uw IoT Edge-gateway-apparaat en eventuele downstream-apparaten.  

1. De basis-CA-certificaat maken en deze een tussenliggende certificaat ondertekent. De certificaten worden geplaatst in uw werkmap.

   ```powershell
   New-CACertsCertChain rsa
   ```

   Deze scriptopdracht verschillende certificaat- en sleutelbestanden gemaakt, maar we gaan om te verwijzen naar een in het bijzonder verderop in dit artikel:
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. De IoT Edge device CA-certificaat en de persoonlijke sleutel maken met de volgende opdracht. Geef de hostnaam van de gateway, die kan worden gevonden in het bestand iotedge\config.yaml op de gateway-apparaat. De hostnaam van de gateway wordt gebruikt om de naam van de bestanden en tijdens het genereren van het certificaat. 

   ```powershell
   New-CACertsEdgeDevice "<gateway hostname>"
   ```

   Deze scriptopdracht maakt verschillende certificaat en de sleutelbestanden, waaronder twee die we om te verwijzen naar verderop in dit artikel:
   * `<WRKDIR>\certs\iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * `<WRKDIR>\private\iot-edge-device-<gateway hostname>.key.pem`

Nu dat u de certificaten hebt, gaat u verder met [-certificaten installeren op de gateway](#install-certificates-on-the-gateway)

## <a name="generate-certificates-with-linux"></a>Genereren van certificaten met Linux

Gebruik de stappen in deze sectie voor het genereren van testcertificaten op Linux. U kunt een Linux-machine gebruiken voor het genereren van de certificaten en kopieer deze vervolgens boven aan elk IoT Edge-apparaat waarop een ondersteund besturingssysteem. 

De certificaten die zijn gegenereerd in deze sectie zijn bedoeld alleen voor testdoeleinden. 

### <a name="prepare-creation-scripts"></a>Voorbereiden van scripts voor het maken

De git-opslagplaats voor Azure IoT Edge bevat scripts die u gebruiken kunt voor het genereren van testcertificaten. In deze sectie maakt u kloon de opslagplaats van de IoT Edge en de scripts uit te voeren. 

1. Kloon de git-opslagplaats met scripts voor het genereren van certificaten voor niet-productie. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken. 

   ```bash
   git clone https://github.com/Azure/iotedge.git
   ```

2. Ga naar de map waarin u wilt werken. Verwijzen we naar deze map in het artikel als  *\<WRKDIR >* . Alle bestanden voor certificaat en de sleutel wordt gemaakt in deze map.
  
3. Kopieer de bestanden config en het script van de gekloonde IoT Edge-opslagplaats naar uw werkmap.

   ```bash
   cp <path>/iotedge/tools/CACertificates/*.cnf .
   cp <path>/iotedge/tools/CACertificates/certGen.sh .
   ```

<!--
4. Configure OpenSSL to generate certificates using the provided script. 

   ```bash
   chmod 700 certGen.sh 
   ```
-->

### <a name="create-certificates"></a>Certificaten maken

In deze sectie maakt u drie certificaten maken en vervolgens verbinding maken in een keten. Het plaatsen van de certificaten in een keten-bestand kunt eenvoudig installeren op uw IoT Edge-gateway-apparaat en eventuele downstream-apparaten.  

1. Het basis-CA-certificaat en een tussenliggende certificaat maken. Deze certificaten worden geplaatst  *\<WRKDIR >* .

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Het script maakt u meerdere certificaten en sleutels. Noteer dit we naar in de volgende sectie verwijzen:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`

2. De IoT Edge device CA-certificaat en de persoonlijke sleutel maken met de volgende opdracht. Geef de hostnaam van de gateway, die kan worden gevonden in het bestand iotedge/config.yaml op de gateway-apparaat. De hostnaam van de gateway wordt gebruikt om de naam van de bestanden en tijdens het genereren van het certificaat. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway hostname>"
   ```

   Het script maakt u meerdere certificaten en sleutels. Maak notitie van twee, die we naar in de volgende sectie verwijzen: 
   * `<WRKDIR>/certs/iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * `<WRKDIR>/private/iot-edge-device-<gateway hostname>.key.pem`

## <a name="install-certificates-on-the-gateway"></a>Certificaten installeren op de gateway

Nu dat u een certificaatketen genomen hebt, moet u deze op het IoT Edge-gateway-apparaat installeren en configureren van de IoT Edge-runtime om te verwijzen naar de nieuwe certificaten. 

1. Kopieer de volgende bestanden uit  *\<WRKDIR >* . Sla deze ergens op uw IoT Edge-apparaat. Verwijzen we naar de doelmap op uw IoT Edge-apparaat als  *\<CERTDIR >* . 

   * Device CA-certificaat-  `<WRKDIR>\certs\iot-edge-device-<gateway hostname>-full-chain.cert.pem`
   * De persoonlijke sleutel Device CA- `<WRKDIR>\private\iot-edge-device-<gateway hostname>.key.pem`
   * Basis-CA- `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

   Kunt u een service, zoals [Azure Key Vault](https://docs.microsoft.com/azure/key-vault) of een functie, zoals [beveiligde kopie protocol](https://www.ssh.com/ssh/scp/) de certificaatbestanden te verplaatsen.  Als u de certificaten op het IoT Edge-apparaat zelf hebt gemaakt, kunt u deze stap overslaan en het pad naar de werkmap.

2. Open het bestand met IoT Edge security daemon-configuratie. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Stel de **certificaat** eigenschappen in het bestand config.yaml om het volledige pad naar het certificaat en de sleutel bestanden op het IoT Edge-apparaat. Verwijder de `#` teken voordat u de certificaateigenschappen van het Verwijder opmerkingen bij de vier regels. Houd er rekening mee dat streepjes in yaml twee spaties zijn.

   * Windows:

      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>\\certs\\iot-edge-device-<gateway hostname>-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>\\private\\iot-edge-device-<gateway hostname>.key.pem"
        trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
      ```
   
   * Linux: 
      ```yaml
      certificates:
        device_ca_cert: "<CERTDIR>/certs/iot-edge-device-<gateway hostname>-full-chain.cert.pem"
        device_ca_pk: "<CERTDIR>/private/iot-edge-device-<gateway hostname>.key.pem"
        trusted_ca_certs: "<CERTDIR>/certs/azure-iot-test-only.root.ca.cert.pem"
      ```

4. Op Linux-apparaten, zorg ervoor dat de gebruiker **iotedge** leesmachtigingen heeft voor de map met de certificaten. 

## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub naar de gateway implementeren

Wanneer u IoT Edge voor het eerst op een apparaat installeert, slechts één Systeemmodule automatisch wordt gestart: de IoT Edge-agent. Voor uw apparaat om te werken als een gateway, moet u beide systeemmodules. Als u nog niet hebt alle modules die aan uw gatewayapparaat hebt geïmplementeerd, maakt u een aanvankelijke implementatie voor uw apparaat te starten van de tweede Systeemmodule, IoT Edge hub. De implementatie eruit leeg, omdat u niet alle modules in de wizard toevoegen, maar het zorgt ervoor dat beide systeemmodules worden uitgevoerd. 

U kunt controleren welke modules worden uitgevoerd op een apparaat met de opdracht `iotedge list`. Als de lijst alleen de module retourneert **edgeAgent** zonder **edgeHub**, gebruikt u de volgende stappen uit:

1. Ga in Azure Portal naar uw IoT-hub.

2. Ga naar **IoT Edge** en selecteert u uw IoT Edge-apparaat dat u wilt gebruiken als een gateway.

3. Selecteer **Modules instellen**.

4. Selecteer **Next**.

5. In de **routes opgeven** pagina, moet u hebt een standaardroute dat alle berichten van alle modules naar IoT Hub verzendt. Voeg anders de volgende code toe en selecteer **Volgende**.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. In de **sjabloon controleren** weergeeft, schakelt **indienen**.

## <a name="open-ports-on-gateway-device"></a>Poorten openen op de gateway-apparaat

Standard IoT Edge-apparaten hoeft geen binnenkomende verbindingen aan de functie, omdat alle communicatie met IoT Hub vindt plaats via uitgaande verbindingen. Gateway-apparaten zijn verschillend, omdat ze nodig hebben om berichten te ontvangen van hun downstream-apparaten. Als een firewall tussen de downstream-apparaten en het gateway-apparaat is, moet communicatie mogelijk zijn via de firewall.

Voor het gatewayscenario van een om te werken, moet ten minste één van de ondersteunde protocollen van de IoT Edge hub zijn geopend voor inkomend verkeer van de downstream-apparaten. De ondersteunde protocollen zijn MQTT-, AMQP- en HTTPS. 

| Poort | Protocol |
| ---- | -------- |
| 8883 | MQTT |
| 5671 | AMQP |
| 443 | HTTPS <br> MQTT WS + <br> AMQP+WS | 

## <a name="route-messages-from-downstream-devices"></a>Routeren van berichten van downstream-apparaten
IoT Edge-runtime kunt versturen berichten worden verzonden van downstream apparaten net als bij berichten die worden verzonden door modules. Deze functie kunt u analyses uitvoeren in een module die wordt uitgevoerd op de gateway voor het verzenden van gegevens naar de cloud. 

Op dit moment is de manier waarop u berichten die worden verzonden door downstream-apparaten versturen door ze verschillen van berichten die worden verzonden door modules. Berichten van alle modules bevatten een systeemeigenschap, genaamd **connectionModuleId** maar niet voor berichten die worden verzonden door downstream-apparaten. De WHERE-component van de route kunt u alle berichten met die systeemeigenschap uitsluiten. 

De onderstaande route is een voorbeeld waarin berichten vanaf elk apparaat downstream naar een module met de naam verzenden wilt `ai_insights`, en vervolgens naar `ai_insights` naar IoT Hub.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Zie voor meer informatie over het routeren van berichten, [implementeren modules en routes tot stand brengen](./module-composition.md#declare-routes).


## <a name="enable-extended-offline-operation"></a>Uitgebreide offline bewerking inschakelen

Beginnen met de [gebruikgemaakt van v1.0.4 release](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) van IoT Edge-runtime, de gateway-apparaat en downstream-verbinding te maken met deze apparaten kunnen worden geconfigureerd voor uitgebreide offline bewerking. 

Met deze mogelijkheid kunnen lokale modules of downstream-apparaten opnieuw worden geverifieerd met de IoT Edge-apparaat, indien nodig en communiceren met elkaar met behulp van berichten en -methoden, zelfs wanneer de IoT-hub is verbroken. Zie voor meer informatie, [begrijpen uitgebreid offline-mogelijkheden voor IoT Edge-apparaten, modules en onderliggende apparaten](offline-capabilities.md).

Uitgebreide offline om mogelijkheden te bieden, moet u een bovenliggende / onderliggende relatie tussen een IoT Edge-gateway-apparaat- en downstream-apparaten die verbinding met het instellen. Deze stappen worden beschreven in meer detail in [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat werkt als een transparante gateway hebt, moet u uw downstream-apparaten voor het vertrouwen van de gateway en berichten te verzenden. Zie voor meer informatie, [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md) en [verifiëren van een downstream-apparaat met Azure IoT Hub](how-to-authenticate-downstream-device.md).
