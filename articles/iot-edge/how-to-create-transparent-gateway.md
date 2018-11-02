---
title: Een transparante gateway maken met Azure IoT Edge | Microsoft Docs
description: Een Azure IoT Edge-apparaat gebruiken als een transparante gateway waarmee informatie voor meerdere apparaten kan worden verwerkt.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/01/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a48699507fbba18b20cb94e404c4814f25d31f44
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50915292"
---
# <a name="configure-an-iot-edge-device-to-act-as-a-transparent-gateway"></a>Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren

In dit artikel vindt u gedetailleerde instructies voor het configureren van IoT Edge-apparaten functioneren als een transparante gateway voor andere apparaten om te communiceren met IoT Hub. In dit artikel wordt de term *IoT Edge-gateway* verwijst naar een IoT Edge-apparaat gebruikt als een transparante gateway. Zie voor meer informatie, [hoe een IoT Edge-apparaat kan worden gebruikt als een gateway](./iot-edge-as-gateway.md), waardoor een conceptueel overzicht.

>[!NOTE]
>Op dit moment:
> * Als de gateway niet met IoT Hub verbonden is, kunnen niet downstream apparaten worden geverifieerd met de gateway.
> * Edge-apparaten kunnen geen verbinding maken met IoT Edge-gateways. 
> * Aangesloten apparaten niet uploaden van bestanden gebruiken.

Een apparaat moet worden gebruikt als een gateway, moet kunnen veilig verbinding maken met downstream-apparaten. Azure IoT Edge kunt u een openbare-sleutelinfrastructuur (PKI) gebruiken voor het instellen van beveiligde verbindingen tussen apparaten. In dit geval toestemming we een downstream apparaat verbinding maakt met een IoT Edge-apparaat als een transparante gateway fungeert. Redelijke om beveiliging te handhaven, moet het downstream apparaat de identiteit van het Edge-apparaat bevestigen omdat u wilt dat alleen de apparaten die verbinding maken met uw gateways en niet een mogelijk schadelijke gateway.

Een downstream-apparaat mag een toepassing of het platform waarvoor een identiteit die is gemaakt met de [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub) cloudservice. In veel gevallen gebruikt u deze toepassingen de [Azure IoT device-SDK](../iot-hub/iot-hub-devguide-sdks.md). Praktische overwegingen, een downstream apparaat mogelijk ook een toepassing die wordt uitgevoerd op het IoT Edge-gateway-apparaat zelf. 

U kunt een certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat-instellingen die u gebruiken zou om in te schakelen [x.509-CA-beveiliging](../iot-hub/iot-hub-x509ca-overview.md) in IoT Hub, waarbij een X.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub (de IoT hub eigenaar CA) en een reeks van certificaten geregistreerd bij deze Certificeringsinstantie, en een CA voor het Edge-apparaat.

![Gateway-installatie](./media/how-to-create-transparent-gateway/gateway-setup.png)

De gateway geeft het Edge-apparaat CA-certificaat naar het downstream apparaat tijdens de initialisatie van de verbinding. Het downstream apparaat controleert om ervoor te zorgen dat het Edge-apparaat CA-certificaat is ondertekend door de eigenaar van CA-certificaat. Dit proces kan de downstream-apparaat om te bevestigen van dat de gateway van een vertrouwde bron afkomstig is.

De volgende stappen helpen u bij het proces van het maken van de certificaten en ze te installeren in de juiste plaatsen worden uitgevoerd.

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat configureren als een gateway. U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een IoT Edge-apparaat met de stappen voor de volgende besturingssystemen:
* [Windows](./how-to-install-iot-edge-windows-with-windows.md)
* [Linux x64](./how-to-install-iot-edge-linux.md)
* [Linux ARM32](./how-to-install-iot-edge-linux-arm.md)

U kunt elke virtuele machine gebruiken voor het genereren van de certificaten, en kopieer deze vervolgens naar uw IoT Edge-apparaat. 

## <a name="generate-certificates-with-windows"></a>Genereren van certificaten met Windows

Gebruik de stappen in deze sectie voor het genereren van testcertificaten op een Windows-apparaat. U kunt genereren van de certificaten op uw IoT Edge-apparaat zelf, of gebruik een afzonderlijke virtuele machine en kopieert u de uiteindelijke certificaten naar elk IoT Edge-apparaat met een ondersteund besturingssysteem. 

De certificaten die zijn gegenereerd in deze sectie zijn bedoeld alleen voor testdoeleinden. 

### <a name="install-openssl"></a>OpenSSL installeren

OpenSSL voor Windows installeren op de computer die u gebruikt voor het genereren van de certificaten. Er zijn verschillende manieren kunt u OpenSSL installeren:

   >[!NOTE]
   >Als u al OpenSSL geïnstalleerd op uw Windows-apparaat hebt, kunt u deze stap overslaan maar ervoor te zorgen dat openssl.exe is beschikbaar in uw padomgevingsvariabele bevindt.

* **Eenvoudiger:** downloaden en installeren van een [OpenSSL binaire bestanden van derden](https://wiki.openssl.org/index.php/Binaries), bijvoorbeeld van [dit project op SourceForge](https://sourceforge.net/projects/openssl/). Het volledige pad toevoegen aan openssl.exe aan de omgevingsvariabele PATH. 
   
* **Aanbevolen:** de OpenSSL-broncode downloaden en bouwen van de binaire bestanden op uw computer zelf of via [vcpkg](https://github.com/Microsoft/vcpkg). De onderstaande instructies gebruiken vcpkg broncode, compileren en OpenSSL installeren op uw Windows-computer met eenvoudige stappen downloadt.

   1. Ga naar een map waar u vcpkg installeren. Verwijzen we naar deze map als  *\<VCPKGDIR >*. Volg de instructies voor het downloaden en installeren [vcpkg](https://github.com/Microsoft/vcpkg).
   
   2. Wanneer vcpkg is geïnstalleerd, een powershell-opdrachtprompt en voer de volgende opdracht voor het installeren van de OpenSSL-pakket voor Windows x64. De installatie duurt meestal ongeveer 5 minuten.

      ```PowerShell
      .\vcpkg install openssl:x64-windows
      ```
   3. Voeg `<VCPKGDIR>\installed\x64-windows\tools\openssl` aan de omgevingsvariabele PATH zodat het bestand openssl.exe beschikbaar voor de aanroep is.

### <a name="prepare-creation-scripts"></a>Voorbereiden van scripts voor het maken

De Azure IoT device-SDK voor C bevat scripts die u gebruiken kunt voor het genereren van testcertificaten. In deze sectie klonen van de SDK en PowerShell configureren.

1. Open een PowerShell-venster in de beheerdersmodus. 

2. Kloon de git-opslagplaats met scripts voor het genereren van certificaten voor niet-productie. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken. Gebruik de `git clone` opdracht of [downloaden van het ZIP-bestand](https://github.com/Azure/azure-iot-sdk-c/archive/master.zip). 

   ```PowerShell
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Ga naar de map waarin u wilt werken. Verwijzen we naar deze map als  *\<WRKDIR >*.  Alle bestanden wordt in deze map gemaakt.

4. Kopieer de bestanden voor configuratie en het script in uw werkmap. 

   ```PowerShell
   copy <path>\azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy <path>\azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   ```

   Als u hebt gedownload van de SDK als ZIP, wordt de mapnaam van de is `azure-iot-sdk-c-master` en de rest van het pad is hetzelfde. 

5. Stel de omgevingsvariabele OPENSSL_CONF het configuratiebestand openssl_root_ca.cnf gebruiken.

    ```PowerShell
    $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
    ```

6. Schakel in PowerShell de scripts uit te voeren.

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
   ```

7. Breng de functies die worden gebruikt door de scripts van PowerShell globale naamruimte.
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

8. Controleren of de OpenSSL correct is geïnstalleerd en controleer of er niet naamconflicten met bestaande certificaten. Als er problemen zijn, moet het script voor het oplossen van deze op uw systeem te beschrijven.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

### <a name="create-certificates"></a>Certificaten maken

In deze sectie maakt u drie certificaten maken en vervolgens verbinding maken in een keten. De certificaten in een keten-bestand plaatsen, kunt u eenvoudig installeren op uw IoT Edge-gateway-apparaat en eventuele downstream-apparaten.  

1. De eigenaar van CA-certificaat maken en deze een tussenliggende certificaat ondertekent. De certificaten worden geplaatst  *\<WRKDIR >*.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

2. De CA-certificaat voor Edge-apparaat en de persoonlijke sleutel maken met de volgende opdracht uit. Geef een naam voor de gateway-apparaat dat wordt gebruikt om de naam van de bestanden en tijdens het genereren van het certificaat. 

   ```PowerShell
   New-CACertsEdgeDevice "<gateway name>"
   ```

3. Maak een certificaatketen van de eigenaar van CA-certificaat, tussenliggende certificaat- en Edge device CA-certificaat met de volgende opdracht. 

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway name>"
   ```

   Het script maakt u de volgende certificaten en de sleutel:
   * `<WRKDIR>\certs\new-edge-device.*`
   * `<WRKDIR>\private\new-edge-device.key.pem`
   * `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="generate-certificates-with-linux"></a>Genereren van certificaten met Linux

Gebruik de stappen in deze sectie voor het genereren van testcertificaten op een Linux-apparaat. U kunt genereren van de certificaten op uw IoT Edge-apparaat zelf, of gebruik een afzonderlijke virtuele machine en kopieert u de uiteindelijke certificaten naar elk IoT Edge-apparaat met een ondersteund besturingssysteem. 

### <a name="prepare-creation-scripts"></a>Voorbereiden van scripts voor het maken

1. Kloon de git-opslagplaats met scripts voor het genereren van certificaten voor niet-productie. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken. 

   ```bash
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

2. Ga naar de map waarin u wilt werken. Verwijzen we naar deze map als  *\<WRKDIR >*.  Alle bestanden wordt in deze map gemaakt.
  
3. Kopieer de bestanden voor configuratie en het script in uw werkmap.

   ```bash
   cp <path>/azure-iot-sdk-c/tools/CACertificates/*.cnf .
   cp <path>/azure-iot-sdk-c/tools/CACertificates/certGen.sh .
   ```

4. OpenSSL voor het genereren van certificaten met behulp van het opgegeven script configureren. 

   ```bash
   chmod 700 certGen.sh 
   ```

### <a name="create-certificates"></a>Certificaten maken

In deze sectie maakt u drie certificaten maken en vervolgens verbinding maken in een keten. Het plaatsen van de certificaten in een keten-bestand kunt eenvoudig installeren op uw IoT Edge-gateway-apparaat en eventuele downstream-apparaten.  

1.  De eigenaar van CA-certificaat en een tussenliggende certificaat maken. Deze certificaten worden geplaatst  *\<WRKDIR >*.

   ```bash
   ./certGen.sh create_root_and_intermediate
   ```

   Het script maakt u de volgende certificaten en sleutels:
   * `<WRKDIR>/certs/azure-iot-test-only.root.ca.cert.pem`
   * `<WRKDIR>/certs/azure-iot-test-only.intermediate.cert.pem`
   * `<WRKDIR>/private/azure-iot-test-only.root.ca.key.pem`
   * `<WRKDIR>/private/azure-iot-test-only.intermediate.key.pem`

2.  De CA-certificaat voor Edge-apparaat en de persoonlijke sleutel maken met de volgende opdracht uit. Geef een naam voor de gateway-apparaat dat wordt gebruikt om de naam van de bestanden en tijdens het genereren van het certificaat. 

   ```bash
   ./certGen.sh create_edge_device_certificate "<gateway name>"
   ```

   Het script maakt u de volgende certificaten en de sleutel:
   * `<WRKDIR>/certs/new-edge-device.*`
   * `<WRKDIR>/private/new-edge-device.key.pem`

3. Maken van een certificaatketen die met de naam **nieuwe-edge-apparaat-full-chain.cert.pem** van de eigenaar van CA-certificaat, tussenliggende certificaat- en Edge device CA-certificaat.

   ```bash
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="install-certificates-on-the-gateway"></a>Certificaten installeren op de gateway

Nu dat u een certificaatketen genomen hebt, moet u deze op het IoT Edge-gateway-apparaat installeren en configureren van de IoT Edge-runtime om te verwijzen naar de nieuwe certificaten. 

1. Kopieer de volgende bestanden uit  *\<WRKDIR >*. Sla deze ergens op uw IoT Edge-apparaat. Verwijzen we naar de doelmap op uw IoT Edge-apparaat als  *\<CERTDIR >*. 

   Als u de certificaten op het Edge-apparaat zelf hebt gemaakt, kunt u deze stap overslaan en het pad naar de werkmap.

   * Device CA-certificaat-  `<WRKDIR>\certs\new-edge-device-full-chain.cert.pem`
   * De persoonlijke sleutel Device CA- `<WRKDIR>\private\new-edge-device.key.pem`
   * De eigenaar van CA- `<WRKDIR>\certs\azure-iot-test-only.root.ca.cert.pem`

2. Open het bestand met IoT Edge security daemon-configuratie. 

   * Windows: `C:\ProgramData\iotedge\config.yaml`
   * Linux: `/etc/iotedge/config.yaml`

3. Stel de **certificaat** eigenschappen in het bestand config.yaml naar het pad waar u de bestanden van het certificaat en sleutel op het IoT Edge-apparaat hebt geplaatst.

```yaml
certificates:
  device_ca_cert: "<CERTDIR>\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "<CERTDIR>\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "<CERTDIR>\\certs\\azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub naar de gateway implementeren

Wanneer u IoT Edge voor het eerst op een apparaat installeert, slechts één Systeemmodule automatisch wordt gestart: de Edge agent. Voor uw apparaat om te werken als een gateway, moet u beide systeemmodules. Als u nog niet hebt alle modules geïmplementeerd voor uw gatewayapparaat hebt, maakt u een implementatie voor uw apparaat te starten van de tweede Systeemmodule, de Edge hub. De implementatie eruit leeg, omdat u niet alle modules in de wizard toevoegen, maar beide systeemmodules worden geïmplementeerd. 

U kunt controleren welke modules worden uitgevoerd op een apparaat met de opdracht `iotedge list`.

1. Ga in Azure Portal naar uw IoT-hub.

2. Ga naar **IoT Edge** en selecteert u uw IoT Edge-apparaat dat u wilt gebruiken als een gateway.

3. Selecteer **Modules instellen**.

4. Selecteer **Volgende**.

5. In de **routes opgeven** pagina, moet u hebt een standaardroute dat alle berichten van alle modules naar IoT Hub verzendt. Voeg anders de volgende code toe en selecteer **Volgende**.

   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```

6. In de **sjabloon controleren** weergeeft, schakelt **indienen**.

## <a name="route-messages-from-downstream-devices"></a>Routeren van berichten van downstream-apparaten
IoT Edge-runtime kunt versturen berichten worden verzonden van downstream apparaten net als bij berichten die worden verzonden door modules. Hiermee kunt u analyses uitvoeren in een module die wordt uitgevoerd op de gateway voor het verzenden van gegevens naar de cloud. De onderstaande route moet worden gebruikt voor het verzenden van berichten van een downstream-apparaat met de naam `sensor` op de naam van een module `ai_insights`.

```json
{
    "routes":{
        "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", 
        "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" 
    } 
}
```

Zie voor meer informatie over het routeren van berichten, [modulesamenstelling](./module-composition.md).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Volgende stappen

Nu u een IoT Edge-apparaat werkt als een transparante gateway hebt, moet u uw downstream-apparaten voor het vertrouwen van de gateway en berichten te verzenden. Zie voor meer informatie, [een downstream-apparaat verbinden met Azure IoT Edge-gateway](how-to-connect-downstream-device.md).
