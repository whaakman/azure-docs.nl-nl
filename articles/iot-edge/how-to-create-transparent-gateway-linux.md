---
title: Maak een transparante gateway met Azure IoT Edge - Linux | Microsoft Docs
description: Azure IoT Edge gebruiken voor het maken van een transparante gateway die informatie voor meerdere apparaten kan worden verwerkt.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5a78d6fb8ee52f0daba80a77cc8a5e75c2e5248d
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37035948"
---
# <a name="create-a-linux-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Een Edge van Linux-IoT-apparaat die als een transparante gateway fungeert maken

In dit artikel vindt u gedetailleerde instructies voor het gebruik van een IoT-randapparaat als een transparante gateway. Voor de rest van dit artikel wordt de term *gateway aan de rand van de IoT* verwijst naar een IoT-randapparaat als een transparante gateway gebruikt. Voor meer informatie gedetailleerde, Zie [hoe een Edge van IoT-apparaat kan worden gebruikt als een gateway][lnk-edge-as-gateway], waardoor een conceptueel overzicht.

>[!NOTE]
>Op dit moment:
> * Als de gateway niet met IoT Hub verbonden is, worden aangesloten apparaten niet verifiëren met de gateway.
> * IoT Edge-apparaten kunnen geen verbinding naar IoT-gateways.
> * Downstream-apparaten kunnen uploaden bestand niet gebruiken.

De moeilijkste over het maken van een transparante gateway maakt de gateway op downstream apparaten veilig verbinding. Azure IoT-rand kunt u PKI-infrastructuur gebruiken om beveiligde TLS-verbindingen tussen deze apparaten te stellen. In dit geval toestemming we een downstream apparaat verbinding maakt met een IoT-randapparaat als een transparante gateway fungeert.  Redelijke om beveiliging te handhaven, moet de downstream-apparaat de identiteit van het apparaat aan de rand bevestigen, omdat u wilt dat alleen de apparaten die verbinding maken met uw gateways en niet een mogelijk schadelijke gateway.

U kunt certificaatinfrastructuur waarmee de vertrouwensrelatie die vereist zijn voor de topologie van uw apparaat-gateway maken. In dit artikel gaan we ervan uit dezelfde certificaat instellingen die u gebruiken wilt om in te schakelen [x.509-CA-beveiliging] [ lnk-iothub-x509] in IoT-Hub die een x.509-CA-certificaat dat is gekoppeld aan een specifieke IoT-hub (IoT hub de eigenaar van de CA omvat ), en een reeks certificaten die zijn ondertekend met deze CA en een CA voor het apparaat aan de rand.

![Gatewayinstallatie][1]

De gateway geeft de rand apparaat CA-certificaat op het apparaat downstream tijdens de initialisatie van de verbinding. Zorg ervoor dat het Edge-apparaat CA-certificaat is ondertekend door de CA-certificaat van de eigenaar van de downstream-apparaat wordt gecontroleerd. Dit proces kan de downstream-apparaat om te bevestigen dat de gateway van een vertrouwde bron afkomstig is.

De volgende stappen maakt u het proces van het maken van de certificaten en ze te installeren op de juiste plaatsen.

## <a name="prerequisites"></a>Vereisten
1.  Azure IoT rand runtime installeren op een Linux-apparaat dat u wilt gebruiken als de transparante gateway.
   * [Linux x64][lnk-install-linux-x64]
   * [Linux-ARM32][lnk-install-linux-arm]

2.  Ophalen van de scripts voor het genereren van de vereiste certificaten voor niet-productieve met de volgende opdracht. Deze scripts kunnen u de benodigde certificaten voor het instellen van een transparante gateway maken. 

   ```cmd
   git clone https://github.com/Azure/azure-iot-sdk-c.git
   ```

3. Deze scripts OpenSSL gebruikt voor het genereren van de vereiste certificaten en OpenSSL moet u instellingen opgeven.
   
   1. Ga naar de map waarin u wilt werken. Vanaf nu verwijzen we naar dit als $WRKDIR.  Alle bestanden wordt in deze map gemaakt.

      cd $WRKDIR
   
   1. Configuratie en het script bestanden kopiëren naar uw werkmap.

      ```cmd
      cp azure-iot-sdk-c/tools/CACertificates/*.cnf .
      cp azure-iot-sdk-c/tools/CACertificates/certGen.sh .
      chmod 700 certGen.sh 
      ```

## <a name="certificate-creation"></a>Maken van het certificaat
1.  Het CA-certificaat van eigenaar en één tussenliggende certificaat maken. Dit zijn alle geplaatst `$WRKDIR`.

   ```cmd
   ./certGen.sh create_root_and_intermediate
   ```

   De uitvoer van de uitvoering van het script zijn de volgende certificaten en sleutels:
   * Certificaten
      * `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`
      * `$WRKDIR/certs/azure-iot-test-only.intermediate.cert.pem`
   * Sleutels
      * `$WRKDIR/private/azure-iot-test-only.root.ca.key.pem`
      * `$WRKDIR/private/azure-iot-test-only.intermediate.key.pem`

2.  Het CA-certificaat van Edge-apparaat en de persoonlijke sleutel maken met de onderstaande opdracht.

   >[!NOTE]
   > **GEEN** gebruik een naam die is hetzelfde als de DNS-hostnaam van de gateway. Hierdoor wordt clientcertificaten op basis van deze certificaten mislukken.

      ```cmd
      ./certGen.sh create_edge_device_certificate "<gateway device name>"
      ```

   De uitvoer van de uitvoering van het script zijn de volgende certificaten en de sleutel:
   * `$WRKDIR/certs/new-edge-device.*`
   * `$WRKDIR/private/new-edge-device.key.pem`

## <a name="certificate-chain-creation"></a>Maken van het certificaatketen
Een certificaatketen van de eigenaar van CA-certificaat, tussenliggende certificaat en rand apparaat CA-certificaat met de onderstaande opdracht maken. Als u deze in een keten-bestand, kunt u eenvoudig te installeren op uw randapparaat als een transparante gateway fungeert.

   ```cmd
   cat ./certs/new-edge-device.cert.pem ./certs/azure-iot-test-only.intermediate.cert.pem ./certs/azure-iot-test-only.root.ca.cert.pem > ./certs/new-edge-device-full-chain.cert.pem
   ```

## <a name="installation-on-the-gateway"></a>Installatie op de gateway
1.  Kopieer de volgende bestanden vanuit $WRKDIR ergens op het apparaat aan de rand, verwijzen we naar die als $CERTDIR. Deze stap overslaan als u de certificaten op uw apparaat rand wordt gegenereerd.

   * Apparaat-CA-certificaat-  `$WRKDIR/certs/new-edge-device-full-chain.cert.pem`
   * De persoonlijke sleutel apparaat CA- `$WRKDIR/private/new-edge-device.key.pem`
   * CA - eigenaar `$WRKDIR/certs/azure-iot-test-only.root.ca.cert.pem`

2.  Stel de `certificate` eigenschappen in de Security-Daemon config yaml-bestand naar het pad waar u de bestanden van het certificaat en de sleutel hebt geplaatst.

```yaml
certificates:
  device_ca_cert: "$CERTDIR/certs/new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR/private/new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem"
```

## <a name="deploy-edgehub-to-the-gateway"></a>EdgeHub implementeren op de gateway
Een van de belangrijkste mogelijkheden van Azure IoT Edge is dat u er modules voor uw IoT Edge-apparaten mee kunt implementeren vanuit de cloud. Deze sectie heeft u bij het maken van een schijnbaar leeg implementatie; Edge-Hub is echter automatcially toegevoegd aan alle implementaties, zelfs als er geen andere modules die aanwezig zijn. Edge-Hub is de enige module u op een randapparaat laten fungeren als een transparante gateway moet, zodat een lege implementatie wordt gemaakt, voldoende is. 
1. Ga in Azure Portal naar uw IoT-hub.
2. Ga naar **IoT rand** en selecteer uw IoT-randapparaat die u wilt gebruiken als een gateway.
3. Selecteer **Modules instellen**.
4. Selecteer **Volgende**.
5. In de **routes opgeven** stap, u moet beschikken over een standaardroute dat alle berichten van alle modules naar IoT Hub verzendt. Als dit niet het geval is, voeg de volgende code en selecteer vervolgens **volgende**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. Selecteer in de stap van de sjabloon revisie **indienen**.

## <a name="installation-on-the-downstream-device"></a>Installatie op de downstream-apparaat
Een downstream-apparaat kan worden alle toepassingen met behulp van de [Azure IoT-device SDK][lnk-devicesdk], zoals een eenvoudige beschreven in [uw apparaat aansluit op uw IoT-hub met .NET] [ lnk-iothub-getstarted]. Een downstream-device-toepassing heeft om te vertrouwen de **eigenaar CA** certificaat om te valideren van de TLS-verbindingen met de gatewayapparaten. Deze stap kan doorgaans worden uitgevoerd op twee manieren: op het niveau van het besturingssysteem of (voor bepaalde talen) op het toepassingsniveau van de.

### <a name="os-level"></a>OS-niveau
Dit certificaat installeren in het certificaatarchief van het besturingssysteem kunnen alle toepassingen op het gebruik van de eigenaar van de CA-certificaat als een vertrouwd certificaat.

* Ubuntu - Hier volgt een voorbeeld van een CA-certificaat installeren op een virtuele Ubuntu-host.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    U ziet een bericht weergegeven, 'Updating certificates in /etc/ssl/certs... 1 toegevoegd, 0 verwijderd; gedaan."

* Windows - [dit](https://msdn.microsoft.com/en-us/library/cc750534.aspx) artikel beschrijft hoe u dit doet op een Windows-apparaat met behulp van het certificaat importeren wizzard. 

### <a name="application-level"></a>Niveau van de toepassing
U kunt het volgende fragment voor vertrouwen in een certificaat in PEM-indeling toevoegen voor .NET-toepassingen. De variabele initialiseren `certPath` met `$CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>De downstream-apparaat aansluit op de gateway
U moet de IoT Hub apparaat-sdk initialiseren met een verbindingsreeks verwijst naar de hostnaam van het gateway-apparaat. Dit wordt gedaan door toe te voegen de `GatewayHostName` eigenschap in op uw apparaat-verbindingsreeks. Hier is bijvoorbeeld een apparaat-verbindingsreeks voor het voorbeeld voor een apparaat, waaraan wij toegevoegd de `GatewayHostName` eigenschap:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >Dit is een voorbeeld van een opdracht instellen voor welke tests die alles is correct. U sohuld bericht 'OK geverifieerd'.
   >
   >openssl s_client-mygateway.contoso.com:8883 - CAfile $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts verbinding

## <a name="routing-messages-from-downstream-devices"></a>Routeren van berichten van downstream-apparaten
De runtime IoT rand kan berichten worden verzonden van downstream apparaten net als berichten is verzonden door modules worden gerouteerd. Hiermee kunt u analyses uitvoeren in een module op de gateway uitgevoerd vóór het verzenden van gegevens naar de cloud. De onderstaande route moet worden gebruikt voor het verzenden van berichten van een downstream-apparaat met de naam `sensor` op een modulenaam `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Raadpleeg het [module samenstelling artikel] [lnk--modulesamenstelling] voor meer informatie over het routeren van berichten.

## <a name="next-steps"></a>Volgende stappen
[Overzicht van de vereisten en hulpmiddelen voor het ontwikkelen van IoT rand modules][lnk-module-dev].

<!-- Images -->
[1]: ./media/how-to-create-transparent-gateway/gateway-setup.png

<!-- Links -->
[lnk-install-linux-x64]: ./how-to-install-iot-edge-linux.md
[lnk-install-linux-arm]: ./how-to-install-iot-edge-linux-arm.md
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
