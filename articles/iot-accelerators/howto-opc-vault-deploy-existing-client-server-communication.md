---
title: OPC UA-Client en de OPC UA-Server-toepassing met behulp van Azure IoT OPC UA-Certificaatbeheer beveiligen | Microsoft Docs
description: OPC UA-Client en de OPC UA-Server-toepassing met een nieuw sleutelpaar en een certificaat met behulp van de OPC-kluis beveiligen.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: bfa6bdf6a54cb5e54087055988e9682565667105
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759424"
---
# <a name="secure-opc-ua-client-and-opc-ua-server-application"></a>Beveiligen van OPC UA-Client en de OPC UA-servertoepassing 
Azure IoT OPC UA Certificaatbeheer, ook weten als OPC-kluis is een microservice die configureren kunt, registreren, en de levenscyclus van het certificaat voor de OPC UA-server en client-toepassingen in de cloud beheren. In dit artikel wordt beschreven hoe u voor het beveiligen van OPC UA-Client en de OPC UA-Server-toepassing met een nieuw sleutelpaar en het certificaat met behulp van de OPC-kluis.

In de volgende instellingen, is de OPC-Client de verbinding met de OPC-PLC testen. Standaard is niet de connectiviteit mogelijk, omdat beide onderdelen nog niet is ingericht met de juiste certificaten. In deze werkstroom we geen gebruik van de OPC UA-onderdelen zelfondertekende certificaten en ondertekent ze via OPC-kluis. Zie de vorige [testbed](howto-opc-vault-deploy-existing-client-plc-communication.md). Deze testbed bepalingen in plaats daarvan de onderdelen met een nieuw certificaat en met een nieuwe persoonlijke sleutel die zowel door de OPC-kluis wordt gegenereerd. Sommige achtergrondinformatie over OPC UA-beveiliging vindt u in deze [whitepaper](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf). De volledige gegevens vindt u in de OPC UA-specificatie.

Testbed: De volgende omgeving is geconfigureerd voor het testen.

OPC-kluis scripts:
- OPC UA-Client en de OPC UA-Server-toepassing met een nieuw sleutelpaar en een certificaat met behulp van de OPC-kluis beveiligen.

> [!NOTE]
> Zie voor meer informatie, de GitHub [opslagplaats](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-new-certificate-and-private-key"></a>Genereer een nieuw certificaat en de persoonlijke sleutel 
**Voorbereiding**
- Zorg ervoor dat de omgevingsvariabelen `$env:_PLC_OPT` en `$env:_CLIENT_OPT` zijn gedefinieerd. Bijvoorbeeld, `$env:_PLC_OPT=""` in uw PowerShell
- Stel de omgevingsvariabele `$env:_OPCVAULTID` naar een tekenreeks waarmee u uw gegevens opnieuw te vinden in de OPC-kluis. Het is raadzaam instellen op een getal van 6 cijfers. In ons voorbeeld is '123456' als waarde gebruikt voor de variabele.
- Zorg ervoor dat er is geen volume docker `opcclient` of `opcplc`. Neem contact op met `docker volume ls` en verwijder ze met `docker volume rm <volumename>`. U moet mogelijk ook verwijderen van containers met `docker rm <containerid>` als de volumes worden nog steeds worden gebruikt door een container.

**Snelstartgids**
1. Ga naar de [OPC-kluis-website](https://opcvault.azurewebsites.net/)

1. Selecteer `Register New`

1. Voer de gegevens van de OPC PLC deze werd weergegeven in de uitvoer van de vorige testbed `CreateSigningRequest information` gebied in de velden op de `Register New OPC UA Application` weergeeft, schakelt `Server` als ApplicationType.

1. Selecteer `Register`

1. Op de volgende pagina `Request New Certificate for OPC UA Application` selecteren `Request new KeyPair and Certificate`

1. Op de volgende pagina `Generate a new Certificate with a Signing Request` plakken de `CSR (base64 encoded)` tekenreeks uit de uitvoer van de logboekbestanden in de `CreateRequest` invoerveld. Zorg ervoor dat u de volledige tekenreeks kopiëren.

1. Op de volgende pagina `Request New Certificate for OPC UA Application` selecteren `Request new Certificate with Signing Request`

1. Op de volgende pagina `Generate a new KeyPair and for an OPC UA Application` invoeren `CN=OpcPlc` als SubjectName, `opcplc-<_OPCVAULTID>` (Vervang `<_OPCVAULTID>` met uw) als de domeinnaam, selecteer `PEM` als PrivateKeyFormat en voer een wachtwoord (we later hiernaar wordt verwezen als `<certpassword-string>`)

1. Selecteer `Generate New KeyPair`

1. U bent nu naar voren verplaatsen naar `View Certificate Request Details`. Op deze pagina kunt u alle vereiste gegevens voor het inrichten van de certificaatarchieven van downloaden `opc-plc`.

1. Op deze pagina:  
    - Selecteer `Certificate` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<applicationcertbase64-string>` later op. Selecteer `Back`.
    - Selecteer `PrivateKey` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<privatekeybase64-string>` later op. Selecteer `Back`.
    - Selecteer `Issuer` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<addissuercertbase64-string>` later op. Selecteer `Back`.
    - Selecteer `Crl` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<updatecrlbase64-string>` later op. Selecteer `Back`.

1. Nu is ingesteld in uw PowerShell een variabele met de naam `$env:_PLC_OPT`:

   `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --privatekeybase64 <privatekeybase64-string> --certpassword <certpassword-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  

    Vervang de tekenreeksen die zijn ingevoerd als optie waarden Base64 tekenreeksen die u opgehaald via de website.  

1. Herhaal het volledige proces beginnen met `Register New` voor de OPC-Client. Er zijn alleen de volgende punten u moet rekening mee moet houden:
    - Gebruik de logboekuitvoer van de `opcclient`.
    - Selecteer `Client` als ApplicationType tijdens de registratie.
    - Gebruik `$env:_CLIENT_OPT` als naam van de PowerShell-variabele.

    > [!NOTE] 
    > Tijdens het werken met dit scenario, u mogelijk hebt herkend die de `<addissuercertbase64-string>` en `<updatecrlbase64-string>` waarden zijn identiek voor `opcplc` en `opcclient`. Dit geldt voor onze use-case en bespaart u tijd wanneer u de stappen.

**Snelstartgids**

Voer de volgende PowerShell-opdracht in de hoofdmap van de opslagplaats:

```
docker-compose -f connecttest.yml up
```

**Verificatie**

Controleer of de twee onderdelen nog niet een bestaand toepassingscertificaat is. Controleer de logboekuitvoer. Hieronder volgt de uitvoer van de OPC-PLC en OPC-Client heeft een vergelijkbare logboekuitvoer.

```
opcplc-123456 | [13:40:08 INF] There is no existing application certificate.
```
Als er een toepassingscertificaat, verwijdert u de docker-volumes, zoals wordt beschreven in de stappen voor gegevensvoorbereiding.

Controleer in het logboek dat het OPC-kluis CA-certificaat is geïnstalleerd in de verlener certificaatarchief ook zoals in het certificaatarchief van vertrouwde peer. Hieronder volgt de logboekuitvoer van OPC PLC en OPC-Client heeft een vergelijkbare logboekuitvoer. 

```
opcplc-123456 | [13:40:09 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Trusted peer store contains 1 certs
opcplc-123456 | [13:40:09 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [13:40:09 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [13:40:09 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [13:40:09 INF] Rejected certificate store contains 0 certs
```
De OPC-PLC nu alle OPC UA-Clients vertrouwen met certificaten die zijn ondertekend door OPC-kluis.

Controleer in het logboek dat de persoonlijke sleutel indeling wordt herkend als PEM en dat het nieuwe toepassingscertificaat is geïnstalleerd. Hieronder volgt de logboekuitvoer van OPC PLC en OPC-Client heeft een vergelijkbare logboekuitvoer. 

```
opcplc-123456 | [13:40:09 INF] The private key for the new certificate was passed in using PEM format.
opcplc-123456 | [13:40:09 INF] Remove the existing application certificate.
opcplc-123456 | [13:40:09 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C' was added to the application certificate store.
opcplc-123456 | [13:40:09 INF] Activating the new application certificate with thumbprint 'A3CB288FC1D2B7A5C08AACF531CF4A85E44A6C4C'.
```

Het toepassingscertificaat en de persoonlijke sleutel worden nu geïnstalleerd in het certificaatarchief van de toepassing en gebruikt door de OPC UA-toepassing.

Controleer of de verbinding tussen de OPC-Client en OPC PLC is kan worden gemaakt en gegevens is van OPC PLC OPC-Client kan lezen. Hier ziet u de volgende uitvoer in de OPC-Client zich uitvoer:
```
opcclient-123456 | [13:40:12 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [13:40:12 INF] Session successfully created with Id ns=3;i=941910499.
opcclient-123456 | [13:40:12 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [13:40:12 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [13:40:12 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [13:40:12 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [13:40:12 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [13:40:12 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [13:40:12 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [13:40:12 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [13:40:12 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/21/2018 13:40:12
```
Als u deze uitvoer ziet, is de OPC-PLC nu vertrouwen OPC-Client en vice versa, aangezien beide hebben nu de certificaten die zijn ondertekend door een Certificeringsinstantie en beide vertrouwenscertificaten die zijn ondertekend door deze CA.

### <a name="a-testbed-for-opc-publisher"></a>Een testbed voor OPC Publisher ###

**Snelstartgids**

Voer de volgende PowerShell-opdracht in de hoofdmap van de opslagplaats:
```
docker-compose -f testbed.yml up
```

**Verificatie**
- Controleer of dat gegevens worden verzonden naar de IoTHub die u hebt geconfigureerd door in te stellen `_HUB_CS` met behulp van [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) of [iothub-explorer](https://github.com/Azure/iothub-explorer).
- OPC-testclient gaat gebruiken IoTHub directe methode en OPC-methodeaanroepen voor het configureren van OPC Publisher knooppunten uit OPC Testserver publiceren/ongedaan.
- Bekijk de uitvoer voor foutberichten.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u OPC-kluis implementeert in een bestaand project, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC-Twin implementeren op een bestaand project](howto-opc-twin-deploy-existing.md)