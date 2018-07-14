---
title: Een transparante gateway maken met Azure IoT Edge - Windows | Microsoft Docs
description: Azure IoT Edge gebruiken om te maken van een transparante gateway waarmee informatie voor meerdere apparaten kan worden verwerkt.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 96a0443a66bb826496c6af42fe6479c0a53ac964
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036081"
---
# <a name="create-a-windows-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Een Windows IoT Edge-apparaat die als een transparante gateway fungeert maken

In dit artikel vindt u gedetailleerde instructies voor het gebruik van een IoT Edge-apparaat als een transparante gateway. Voor de rest van dit artikel wordt de term *IoT Edge-gateway* verwijst naar een IoT Edge-apparaat gebruikt als een transparante gateway. Zie voor meer informatie, [hoe een IoT Edge-apparaat kan worden gebruikt als een gateway][lnk-edge-as-gateway], waardoor een conceptueel overzicht.

>[!NOTE]
>Op dit moment:
> * Als de gateway niet met IoT Hub verbonden is, kunnen niet downstream apparaten worden geverifieerd met de gateway.
> * Edge-apparaten kunnen geen verbinding maken met IoT Edge-gateways. 
> * Aangesloten apparaten niet uploaden van bestanden gebruiken.

Het moeilijke gedeelte over het maken van een transparante gateway is de gateway naar downstream apparaten veilig verbinding wordt gemaakt. Azure IoT Edge kunt u gebruikmaken van PKI-infrastructuur voor het instellen van beveiligde TLS-verbindingen tussen deze apparaten. In dit geval toestemming we een downstream apparaat verbinding maakt met een IoT Edge-apparaat als een transparante gateway fungeert.  Redelijke om beveiliging te handhaven, moet het downstream apparaat de identiteit van het Edge-apparaat bevestigen omdat u wilt dat alleen de apparaten die verbinding maken met uw gateways en niet een mogelijk schadelijke gateway.

U kunt een certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat-instellingen die u gebruiken zou om in te schakelen [x.509-CA-beveiliging] [ lnk-iothub-x509] in IoT-Hub, die bestaat uit een X.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub (IoT hub de eigenaar van de CA ), en een reeks van certificaten die zijn ondertekend met deze CA en een CA voor het Edge-apparaat.

![Gateway-installatie][1]

De gateway geeft het Edge-apparaat CA-certificaat naar het downstream apparaat tijdens de initialisatie van de verbinding. Het downstream apparaat controleert om ervoor te zorgen dat het Edge-apparaat CA-certificaat is ondertekend door de eigenaar van CA-certificaat. Dit proces kan de downstream-apparaat om te bevestigen van dat de gateway van een vertrouwde bron afkomstig is.

De volgende stappen helpen u bij het proces van het maken van de certificaten en ze te installeren in de juiste plaatsen worden uitgevoerd.

## <a name="prerequisites"></a>Vereisten
1.  [Installeer de Azure IoT Edge-runtime] [ lnk-install-windows-x64] op een Windows-apparaat dat u wilt gebruiken als de transparante gateway.

1. Haal OpenSSL voor Windows. Er zijn veel manieren kunt u OpenSSL installeren:

   >[!NOTE]
   >Als u al OpenSSL geïnstalleerd op uw Windows-apparaat hebt, kunt u deze stap overslaan maar zorg ervoor dat `openssl.exe` is beschikbaar in uw `%PATH%` omgevingsvariabele.

   * Download en installeer een [OpenSSL binaire bestanden van derden](https://wiki.openssl.org/index.php/Binaries), bijvoorbeeld van [dit project op SourceForge](https://sourceforge.net/projects/openssl/).
   
   * De OpenSSL-broncode downloaden en bouwen van de binaire bestanden op uw computer door uzelf of hiervoor via [vcpkg](https://github.com/Microsoft/vcpkg). De onderstaande instructies gebruiken vcpkg broncode downloaden, compileren en OpenSSL installeren op uw Windows-computer in stappen van zeer eenvoudig te gebruiken.

      1. Ga naar een map waar u vcpkg installeren. Vanaf nu verwijzen we naar dit als $VCPKGDIR. Volg de instructies voor het downloaden en installeren [vcpkg](https://github.com/Microsoft/vcpkg).
   
      1. Wanneer vcpkg is geïnstalleerd, een powershell-opdrachtprompt en voer de volgende opdracht voor het installeren van de OpenSSL-pakket voor Windows x64. Dit vindt meestal plaats ongeveer 5 minuten.

         ```PowerShell
         .\vcpkg install openssl:x64-windows
         ```
      1. Toevoegen `$VCPKGDIR\vcpkg\packages\openssl_x64-windows\tools\openssl` aan uw `PATH` omgevingsvariabele zodat de `openssl.exe` bestand is beschikbaar voor de aanroep.

1. Ga naar de map waarin u wilt werken. Vanaf nu verwijzen we naar dit als $WRKDIR.  Alle bestanden wordt in deze map gemaakt.
   
   cd $WRKDIR

1.  Ophalen van de scripts voor het genereren van de vereiste certificaten voor niet-productie met de volgende opdracht. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken.

      ```PowerShell
      git clone https://github.com/Azure/azure-iot-sdk-c.git
      ```

1. Configuratie en het script-bestanden kopiëren naar uw werkmap. Bovendien env variabele OPENSSL_CONF gebruik van het configuratiebestand openssl_root_ca.cnf instellen.

   ```PowerShell
   copy azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
   ```

1. PowerShell om uit te voeren van de scripts met de volgende opdracht inschakelen

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted
   ```

1. Breng de functies die worden gebruikt door de scripts globale naamruimte door dotsourcing met de volgende opdracht van PowerShell
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

1. Controleer of dat openssl correct is geïnstalleerd en zorg ervoor dat er niet naamconflicten met bestaande certificaten door de volgende opdracht uit. Als er problemen zijn, moet het script voor het oplossen van deze op uw systeem te beschrijven.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

## <a name="certificate-creation"></a>Het maken van certificaten
1.  De eigenaar van CA-certificaat en een tussenliggende certificaat maken. Dit zijn alle geplaatst `$WRKDIR`.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

1.  De CA-certificaat voor Edge-apparaat en de persoonlijke sleutel maken met de onderstaande opdracht.

   >[!NOTE]
   > **GEEN** gebruik een naam die is hetzelfde als de DNS-hostnaam van de gateway. In dat geval zorgt ervoor dat clientcertificaten op basis van deze certificaten mislukken.

   ```PowerShell
   New-CACertsEdgeDevice "<gateway device name>"
   ```

## <a name="certificate-chain-creation"></a>Maken van de keten van certificaten
Een certificaatketen van de eigenaar van CA-certificaat, tussenliggende certificaten en Edge device CA-certificaat met de onderstaande opdracht maken. Plaatsen in een keten-bestand, kunt u gemakkelijk installeren op uw Edge-apparaat als een transparante gateway fungeert.

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway device name>"
   ```

   De uitvoer van de uitvoering van het script worden de volgende certificaten en de sleutel:
   * `$WRKDIR\certs\new-edge-device.*`
   * `$WRKDIR\private\new-edge-device.key.pem`
   * `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="installation-on-the-gateway"></a>Installatie op de gateway
1.  Kopieer de volgende bestanden vanuit $WRKDIR overal op uw Edge-apparaat, verwijzen we naar die als $CERTDIR. Deze stap overslaan als u de certificaten op uw Edge-apparaat gegenereerd.

   * Device CA-certificaat-  `$WRKDIR\certs\new-edge-device-full-chain.cert.pem`
   * De persoonlijke sleutel Device CA- `$WRKDIR\private\new-edge-device.key.pem`
   * De eigenaar van CA- `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

2.  Stel de `certificate` eigenschappen in de Security-Daemon config yaml-bestand naar het pad waar u de bestanden van het certificaat en de sleutel hebt geplaatst.

```yaml
certificates:
  device_ca_cert: "$CERTDIR\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR\\certs\\azure-iot-test-only.root.ca.cert.pem"
```
## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub naar de gateway implementeren
Een van de belangrijkste mogelijkheden van Azure IoT Edge is dat u er modules voor uw IoT Edge-apparaten mee kunt implementeren vanuit de cloud. In deze sectie heeft u bij het maken van een schijnbaar lege implementatie; Edge Hub is echter automatcially toegevoegd aan alle implementaties, zelfs als er geen andere modules die aanwezig zijn. Edge Hub is de enige module die u op een Edge-apparaat beschikken over het fungeren als een transparante gateway moet, zodat het maken van een lege implementatie voldoende is. 
1. Ga in Azure Portal naar uw IoT-hub.
2. Ga naar **IoT Edge** en selecteert u uw IoT Edge-apparaat dat u wilt gebruiken als een gateway.
3. Selecteer **Modules instellen**.
4. Selecteer **Volgende**.
5. In de stap **Routes opgeven** moet u beschikken over een standaardroute waarmee alle berichten van alle modules naar IoT Hub worden verzonden. Voeg anders de volgende code toe en selecteer **Volgende**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. Selecteer in de sjabloon controlestap **indienen**.

## <a name="installation-on-the-downstream-device"></a>Installatie op de downstream-apparaat
Een downstream-apparaat mag elke toepassing met behulp van de [Azure IoT device-SDK][lnk-devicesdk], zoals de eenvoudige één dat wordt beschreven in [uw apparaat aansluiten op uw IoT-hub met behulp van .NET] [ lnk-iothub-getstarted]. Een downstream-device-toepassing heeft vertrouwt de **eigenaar CA** certificaat om te kunnen valideren van de TLS-verbindingen met de gatewayapparaten. Deze stap kan doorgaans worden uitgevoerd op twee manieren: op het niveau van het besturingssysteem, of (voor bepaalde talen) op het toepassingsniveau van de.

### <a name="os-level"></a>Het niveau van
Dit certificaat te installeren in het certificaatarchief van het besturingssysteem kunnen alle toepassingen kunnen gebruikmaken van de eigenaar van de CA-certificaat als een vertrouwd certificaat.

* Ubuntu - als volgt een voorbeeld van een CA-certificaat installeren op een Ubuntu-host.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    U ziet een bericht, 'bijwerken certificaten in /etc/ssl/certs... 1 heeft toegevoegd, 0 verwijderd; gedaan."

* Windows - als volgt een voorbeeld van een CA-certificaat installeren op een Windows-host.
  * Op het menu starttype in 'Certificaten beheren computer'. Dit hulpprogramma moet tevoorschijn `certlm`.
  * Navigeer naar certificaten lokale Computer--> vertrouwde basiscertificaten--> certificaten--> rechts Klik--> alle taken--> importeren om te starten van de wizard Certificaat importeren.
  * Volg de stappen te gaan en certificaat bestand $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem importeren.
  * Wanneer dit is voltooid, kunt u een "Geïmporteerd" bericht moet zien.

### <a name="application-level"></a>Niveau van de toepassing
Voor .NET-toepassingen, kunt u het volgende codefragment om te vertrouwen van een certificaat in de PEM-indeling toevoegen. De variabele initialiseren `certPath` met `$CERTDIR\certs\azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>De downstream apparaat verbinden met de gateway
U moet de sdk van de IoT Hub device initialiseren met een verbindingsreeks die verwijst naar de hostnaam van het gateway-apparaat. Dit wordt gedaan door toe te voegen de `GatewayHostName` eigenschap in de verbindingstekenreeks voor uw apparaat. Bijvoorbeeld, als volgt een voorbeeld van apparaat-verbindingsreeks voor een apparaat, waaraan we toegevoegd de `GatewayHostName` eigenschap:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Dit is een voorbeeldopdracht instellen voor welke tests dat alles is correct. U sohuld een bericht weergegeven dat 'OK geverifieerd'.
   >
   >openssl s_client-mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts verbinding

## <a name="routing-messages-from-downstream-devices"></a>Berichten routeren van downstream-apparaten
IoT Edge-runtime kunt versturen berichten worden verzonden van downstream apparaten net als bij berichten die worden verzonden door modules. Hiermee kunt u analyses uitvoeren in een module die wordt uitgevoerd op de gateway voor het verzenden van gegevens naar de cloud. De onderstaande route moet worden gebruikt voor het verzenden van berichten van een downstream-apparaat met de naam `sensor` op de naam van een module `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Raadpleeg de [module samenstelling artikel] [ lnk-module-composition] voor meer informatie over het routeren van berichten.

## <a name="next-steps"></a>Volgende stappen
[Informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-windows-x64]: ./how-to-install-iot-edge-windows-with-windows.md
[lnk-module-composition]: ./module-composition.md
[lnk-devicesdk]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-edge-as-gateway]: ./iot-edge-as-gateway.md
[lnk-module-dev]: module-development.md
[lnk-iothub-getstarted]: ../iot-hub/iot-hub-csharp-csharp-getstarted.md
[lnk-iothub-x509]: ../iot-hub/iot-hub-x509ca-overview.md
[lnk-iothub-secure-deployment]: ../iot-hub/iot-hub-security-deployment.md
[lnk-iothub-tokens]: ../iot-hub/iot-hub-devguide-security.md#security-tokens
[lnk-iothub-throttles-quotas]: ../iot-hub/iot-hub-devguide-quotas-throttling.md
[lnk-iothub-devicetwins]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-iothub-c2d]: ../iot-hub/iot-hub-devguide-messages-c2d.md
[lnk-ca-scripts]: https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md
[lnk-modbus-module]: https://github.com/Azure/iot-edge-modbus
