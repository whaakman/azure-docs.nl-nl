---
title: Beveiligen de communicatie van OPC-Client en de OPC-PLC met behulp van Azure IoT OPC UA-Certificaatbeheer | Microsoft Docs
description: Beveiligen de communicatie van OPC-Client en de OPC-PLC hun handtekeningcertificaten met behulp van de CA van de OPC-kluis.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: c437f6db21956d1be5e4f6d3512f325f37ca7308
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759455"
---
# <a name="secure-the-communication-of-opc-client-and-opc-plc"></a>De communicatie van OPC-Client en de OPC-PLC beveiligen

Azure IoT OPC UA Certificaatbeheer, ook weten als OPC-kluis is een micro service die u configureren kunt, registreren, en beheren van de levenscyclus van certificaten voor OPC UA-server en client-toepassingen in de cloud. In dit artikel laat zien hoe de communicatie van OPC-Client en de OPC-PLC beveiligen door het ondertekenen van certificaten met behulp van de CA van de OPC-kluis.

In de volgende instellingen test de OPC-Client de verbinding met de OPC-PLC. Standaard is niet de connectiviteit mogelijk, omdat de beide onderdelen niet met de juiste certificaten zijn ingericht. Als een OPC UA-component nog niet met een certificaat ingericht is, genereert een zelfondertekend certificaat bij het opstarten. Het certificaat kan echter worden ondertekend door een certificeringsinstantie (CA) en worden geïnstalleerd in het OPC UA-onderdeel. Nadat dit is gebeurd voor OPC-Client en de OPC-PLC, wordt de verbinding is ingeschakeld. De volgende werkstroom wordt het proces beschreven. Sommige achtergrondinformatie over OPC UA-beveiliging vindt u [dit document](https://opcfoundation.org/wp-content/uploads/2014/05/OPC-UA_Security_Model_for_Administrators_V1.00.pdf) technisch document. De volledige gegevens vindt u in de OPC UA-specificatie.

Testbed: De volgende omgeving is geconfigureerd voor het testen.

OPC-kluis scripts:
- Beveiligde communicatie van OPC-Client en de OPC-PLC door hun handtekeningcertificaten met behulp van de CA van de OPC-kluis.

> [!NOTE]
> Zie voor meer informatie, de GitHub [opslagplaats](https://github.com/Azure-Samples/iot-edge-industrial-configs#testbeds).

## <a name="generate-a-self-signed-certificate-on-startup"></a>Genereren van een zelfondertekend certificaat bij het opstarten

**Voorbereiding**

- Zorg ervoor dat de omgevingsvariabelen `$env:_PLC_OPT` en `$env:_CLIENT_OPT` bijvoorbeeld niet zijn gedefinieerd, `$env:_PLC_OPT=""` in uw PowerShell.

- Stel de omgevingsvariabele `$env:_OPCVAULTID` naar een tekenreeks waarmee u uw gegevens opnieuw te vinden in de OPC-kluis. Alleen alfanumerieke tekens zijn toegestaan. In ons voorbeeld is '123456' als de waarde gebruikt voor deze variabele.

- Zorg ervoor dat er zijn geen volumes docker `opcclient` of `opcplc`. Neem contact op met `docker volume ls` en verwijder ze met `docker volume rm <volumename>`. U moet mogelijk ook verwijderen van containers met `docker rm <containerid>` als de volumes worden nog steeds worden gebruikt door een container.

**Snelstartgids**

Voer de volgende PowerShell-opdracht in de hoofdmap van de opslagplaats:

```
docker-compose -f connecttest.yml up
```

**Verificatie**

Controleer in het logboek dat er geen certificaten geïnstalleerd op de eerste keer opstarten zijn. Hier de logboekuitvoer van OPC PLC (vergelijkbaar zien voor de OPC-Client):...
```
opcplc-123456 | [20:51:32 INF] Trusted issuer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted issuer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Trusted peer store contains 0 certs
opcplc-123456 | [20:51:32 INF] Trusted peer store has 0 CRLs.
opcplc-123456 | [20:51:32 INF] Rejected certificate store contains 0 certs
```
Als u certificaten die zijn gerapporteerd ziet, volgt u de bovenstaande voorbereidende stappen en verwijderen van de docker-volumes.

Controleer of dat de verbinding met de OPC-PLC is mislukt. Hier ziet u de volgende uitvoer in de OPC-Client zich uitvoer:

```
opcclient-123456 | [20:51:35 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:51:36 ERR] Session creation to endpoint 'opc.tcp://opcplc-123456:50000/' failed 1 time(s). Please verify if server is up and OpcClient configuration is correct.
opcclient-123456 | Opc.Ua.ServiceResultException: Certificate is not trusted.
```
De reden voor de fout is dat het certificaat niet vertrouwd wordt. Dit betekent dat `opc-client` heeft geprobeerd verbinding maken met `opc-plc` en ontvangen van een reactie terug, die aangeeft dat `opc-plc` niet vertrouwt `opc-client`, een resultaat van `opc-plc` kan het certificaat te valideren die `opc-client` heeft geleverd. Dit is een zelf-ondertekend certificaat zonder verdere certificaat-configuratie op `opc-plc`, en dus de verbinding is mislukt.

## <a name="sign-and-install-certificates-in-opc-ua-components"></a>Meld u aan en installeert u certificaten in de OPC UA-onderdelen

**Voorbereiding**
1. Ga naar de logboekuitvoer van stap 1 en ophalen van 'CreateSigningRequest gegevens' voor de OPC-PLC en de OPC-Client. De uitvoer wordt hier alleen voor OPC PLC weergegeven:

    ```
    opcplc-123456 | [20:51:32 INF] ----------------------- CreateSigningRequest information ------------------
    opcplc-123456 | [20:51:32 INF] ApplicationUri: urn:OpcPlc:opcplc-123456
    opcplc-123456 | [20:51:32 INF] ApplicationName: OpcPlc
    opcplc-123456 | [20:51:32 INF] ApplicationType: Server
    opcplc-123456 | [20:51:32 INF] ProductUri: https://github.com/azure-samples/iot-edge-opc-plc
    opcplc-123456 | [20:51:32 INF] DiscoveryUrl[0]: opc.tcp://opcplc-123456:50000
    opcplc-123456 | [20:51:32 INF] ServerCapabilities: DA
    opcplc-123456 | [20:51:32 INF] CSR (base64 encoded):
    opcplc-123456 | MIICmzCCAYMCAQAwETEPMA0GA1UEAwwGT3BjUGxjMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwTvlbinAPWPxR9Lw1ndGsrLGy8GiqVOxyGaDpPUcMchX0k0/ncg28h7xrB2H1PThdiZxUJuUwsNM74HrVgt
    ofmXhA4dLM1cTxZHyJVFjl2L3vK5M58NNf6UNdKcB0x3LyuoT6mAIMXmCioqymFCk1TMzIMzbAe7JVAdUaSRBP1vuqQ1rV/cfNAe35dKQW4aPYgl7pR5f1hqprcDu/oca67X8L4kTl4oN0/bCYTk+Ibcd9cG462oAN+bSwbHn8a2jNky8rGsofA
    o9DOT+0ALPhk6CApCYIP2yxoI/kT188eqUUxzAFF9nyU79AyCkpGPuY8DGMyf56pDofgtGpfY3wQIDAQABoEUwQwYJKoZIhvcNAQkOMTYwNDAyBgNVHREEKzAphhh1cm46T3BjUGxjOm9wY3BsYy0xMjM0NTaCDW9wY3BsYy0xMjM0NTYwDQYJK
    oZIhvcNAQELBQADggEBAAsZLoOLzS2VhDcQRu0QhRbG7CGAxX19l7fDCG2WjU7lTFnCvYVTWTYyaY61ljmrWc7IbCaQdMJM8GRnAnvAzUh/PBDxkOX7NqI2+8F1yQOHgs/AfKuppOd6DIP8EzFAHnc0H85jay6zFdmIDWoWwpy0ACqOVooOTKST
    7uty0mT87bj8Cdy1yf4mvBNQx+nsuTbKgxWCBxGYAyg9dIL2uKL0aeB/ROW5Gkelz5sCEzQ1fFDokUA4oC5QiATQBN3cY7EmvRbPgdToY7CpRN3iiO7J+7bC7BP9YKfuE34E8xOFpskHPHAPf3r002/L0S67HyuVSXLUj1+Jc0LeAAF9Bw0=
    opcplc-123456 | [20:51:32 INF] ---------------------------------------------------------------------------
    ```
    
1. Ga naar de [OPC kluis website](https://opcvault.azurewebsites.net/).

1. Selecteer `Register New`

1. Voer de OPC-PLC informatie uit de uitvoer van het logboek `CreateSigningRequest information` gebied in de velden op de `Register New OPC UA Application` weergeeft, schakelt `Server` als ApplicationType.

1. Selecteer `Register`.

1. Op de volgende pagina `Request New Certificate for OPC UA Application` Selecteer `Request new Certificate with Signing Request`.

1. Op de volgende pagina `Generate a new Certificate with a Signing Request` plakken de `CSR (base64 encoded)` tekenreeks uit de uitvoer van de logboekbestanden in de `CreateRequest` invoerveld. Zorg ervoor dat u de volledige tekenreeks kopiëren.

1. Selecteer `Generate New Certificate`.

1. U bent nu naar voren verplaatsen naar `View Certificate Request Details`. Op deze pagina kunt u alle vereiste gegevens voor het inrichten van de certificaatarchieven van downloaden `opc-plc`.

1. Op deze pagina:  
    - Selecteer `Certificate` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<applicationcertbase64-string>` later op. Selecteer `Back`.

    - Selecteer `Issuer` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<addissuercertbase64-string>` later op. Selecteer `Back`.

    - Selecteer `Crl` in `Download as Base64` en kopieert u de tekenreeks die zijn gepresenteerd in de `EncodedBase64` veld en op te slaan voor later gebruik. We verwijzen naar het bestand als `<updatecrlbase64-string>` later op. Selecteer `Back`.

1. Nu is ingesteld in uw PowerShell een variabele met de naam `$env:_PLC_OPT`:

    ```
    `$env:_PLC_OPT="--applicationcertbase64 <applicationcertbase64-string> --addtrustedcertbase64 <addissuercertbase64-string> --addissuercertbase64 <addissuercertbase64-string> --updatecrlbase64 <updatecrlbase64-string>"`  
    ```
    > [!NOTE] 
    > Vervang de tekenreeksen die zijn ingevoerd als optie waarden Base64 tekenreeksen die u opgehaald via de website.

Herhaal het volledige proces beginnen met `Register New` (stap 3 hierboven) voor de OPC-Client. Er zijn alleen de volgende punten u moet rekening mee moet houden:

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

**Verificatie** verifiëren dat de twee onderdelen toepassingscertificaten nu hebt ondertekend. Controleer de logboekuitvoer voor de volgende uitvoer:

```
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Starting to add certificate(s) to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Certificate 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' and thumbprint 'BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Starting to update the current CRL.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted peer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted peer store.
opcplc-123456 | [20:54:38 INF] Remove the current CRL from the trusted issuer store.
opcplc-123456 | [20:54:38 INF] The new CRL issued by 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' was added to the trusted issuer store.
opcplc-123456 | [20:54:38 INF] Start updating the current application certificate.
opcplc-123456 | [20:54:38 INF] The current application certificate has SubjectName 'CN=OpcPlc' and thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] Remove the existing application certificate with thumbprint '8FD43F66479398BDA3AAF5B193199A6657632B49'.
opcplc-123456 | [20:54:39 INF] The new application certificate 'CN=OpcPlc' and thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' was added to the application certificate store.
opcplc-123456 | [20:54:39 INF] Activating the new application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586'.
opcplc-123456 | [20:54:39 INF] Application certificate with thumbprint 'DA6B8B2FB533FBC188F7017BAA8A36FDB77E2586' found in the application certificate store.
opcplc-123456 | [20:54:39 INF] Application certificate is for ApplicationUri 'urn:OpcPlc:opcplc-123456', ApplicationName 'OpcPlc' and Subject is 'OpcPlc'
 ```
Het toepassingscertificaat is er en ondertekend door een CA.

Controleer of in het logboek zijn nu certificaten die zijn geïnstalleerd. Hieronder volgt de logboekuitvoer van OPC PLC en OPC-Client heeft een vergelijkbare uitvoer.
```
opcplc-123456 | [20:54:39 INF] Trusted issuer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted issuer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Trusted peer store contains 1 certs
opcplc-123456 | [20:54:39 INF] 01: Subject 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.' (thumbprint: BC78F1DDC3BB5D2D8795F3D4FF0C430AD7D68E83)
opcplc-123456 | [20:54:39 INF] Trusted peer store has 1 CRLs.
opcplc-123456 | [20:54:39 INF] 01: Issuer 'CN=Azure IoT OPC Vault CA, O=Microsoft Corp.', Next update time '10/19/2019 22:06:46'
opcplc-123456 | [20:54:39 INF] Rejected certificate store contains 0 certs
```
De verlener van het toepassingscertificaat is de CA `CN=Azure IoT OPC Vault CA, O=Microsoft Corp.` en de OPC-PLC ook alle certificaten die zijn ondertekend door deze CA vertrouwt.


Controleer of de verbinding met de OPC-PLC is gemaakt en de OPC-Client gegevens van OPC PLC kan lezen. Hier ziet u de volgende uitvoer in de OPC-Client zich uitvoer:
```
opcclient-123456 | [20:54:42 INF] Create secured session for endpoint URI 'opc.tcp://opcplc-123456:50000/' with timeout of 10000 ms.
opcclient-123456 | [20:54:42 INF] Session successfully created with Id ns=3;i=1085867946.
opcclient-123456 | [20:54:42 INF] The session to endpoint 'opc.tcp://opcplc-123456:50000/' has 4 entries in its namespace array:
opcclient-123456 | [20:54:42 INF] Namespace index 0: http://opcfoundation.org/UA/
opcclient-123456 | [20:54:42 INF] Namespace index 1: urn:OpcPlc:opcplc-123456
opcclient-123456 | [20:54:42 INF] Namespace index 2: http://microsoft.com/Opc/OpcPlc/
opcclient-123456 | [20:54:42 INF] Namespace index 3: http://opcfoundation.org/UA/Diagnostics
opcclient-123456 | [20:54:42 INF] The server on endpoint 'opc.tcp://opcplc-123456:50000/' supports a minimal sampling interval of 0 ms.
opcclient-123456 | [20:54:42 INF] Execute 'OpcClient.OpcTestAction' action on node 'i=2258' on endpoint 'opc.tcp://opcplc-123456:50000/' with security.
opcclient-123456 | [20:54:42 INF] Action (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258') completed successfully
opcclient-123456 | [20:54:42 INF] Value (ActionId: 000 ActionType: 'OpcTestAction', Endpoint: 'opc.tcp://opcplc-123456:50000/' Node 'i=2258'): 10/20/2018 20:54:42
```
Als u deze uitvoer ziet, klikt u vervolgens de OPC-PLC is nu vertrouwen OPC-Client en vice versa, aangezien beide hebben nu certificaten die zijn ondertekend door een Certificeringsinstantie en beide vertrouwde certificaten die door deze CA ondertekend.

> [!NOTE] 
> Hoewel we de eerste twee verificatiestappen alleen voor OPC PLC, moeten de OPC-Client ook worden geverifieerd.


## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u OPC-kluis implementeert in een bestaand project, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [OPC-Twin implementeren op een bestaand project](howto-opc-twin-deploy-existing.md)