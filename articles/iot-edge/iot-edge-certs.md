---
title: Certificaten voor apparaatbeveiliging - Azure IoT Edge | Microsoft Docs
description: Azure IoT Edge maakt gebruik van certificaten om te valideren van apparaten, modules en leaf-apparaten en beveiligde verbindingen tussen beide tot stand brengen.
author: stevebus
manager: philmea
ms.author: stevebus
ms.date: 09/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 91cde6965f3635d6d2acfaf581f570779020f8ff
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232223"
---
# <a name="azure-iot-edge-certificate-usage-detail"></a>Azure IoT Edge-certificaat-gebruiksdetails

IoT Edge-certificaten worden gebruikt voor de modules en downstream IoT-apparaten om te controleren of de identiteit en de geldigheid van de [IoT Edge hub](iot-edge-runtime.md#iot-edge-hub) -runtimemodule waarmee ze verbinding maken. Deze verificatie inschakelen voor een beveiligde verbinding van TLS (transport layer security) tussen de runtime, de modules en de IoT-apparaten. Zoals IoT-Hub zelf, IoT Edge vereist een veilige en versleutelde verbinding van IoT downstream (bladeren) apparaten en IoT Edge-modules. Als u wilt een veilig TLS-verbinding tot stand brengen, geeft de IoT Edge hub-module een server certificaatketen voor het verbinden van clients zodat ze om de identiteit te verifiëren.

In dit artikel wordt uitgelegd hoe IoT Edge-certificaten kunnen werken in productie, ontwikkeling, en Testscenario's. De scripts zijn verschillende (Powershell versus bash), zijn de concepten hetzelfde tussen Linux en Windows.

## <a name="iot-edge-certificates"></a>IoT Edge-certificaten

Fabrikanten zijn meestal niet de eindgebruikers van een IoT Edge-apparaat. Soms is de enige relatie tussen de twee wanneer de eindgebruiker of operator, koopt een algemeen apparaat gemaakt door de fabrikant. In andere gevallen wordt de fabrikant werkt contractueel aan het bouwen van een aangepast apparaat namens de operator. Het ontwerp van de IoT Edge-certificaat wil rekening gehouden met beide scenario's.

De volgende afbeelding ziet u IoT-Edge-gebruik van certificaten. Er zijn mogelijk nul, een of veel tussenliggende handtekeningcertificaten tussen het basis-CA-certificaat en het CA-certificaat van apparaat, afhankelijk van het aantal entiteiten die betrokken zijn. We zien hier één aanvraag.

![Diagram van typische certificaat relaties](./media/iot-edge-certs/edgeCerts-general.png)

### <a name="certificate-authority"></a>Certificeringsinstantie

De certificeringsinstantie of 'CA' genoemd, is een entiteit die digitale certificaten uitgeeft. Een certificeringsinstantie fungeert als een vertrouwde derde partij tussen de eigenaar en de ontvanger van het certificaat. Een digitaal certificaat verklaart de eigendom van een openbare sleutel door de ontvanger van het certificaat. De certificaatketen van de vertrouwensrelatie werkt met behulp van een basiscertificaat, de basis voor vertrouwen in alle certificaten die door de instantie vormt in eerste instantie. De eigenaar van de kunt daarna het basiscertificaat gebruiken om aanvullende ('leaf-certificaten) in de tussenliggende certificaten te verlenen.

### <a name="root-ca-certificate"></a>Basis-CA-certificaat

Een basis-CA-certificaat is de hoofdmap van de vertrouwensrelatie van het gehele proces. Dit CA-certificaat is in productie scenario's, meestal van een vertrouwde commerciële certificeringsinstantie, zoals Baltimore, Verisign of DigiCert aangeschaft. Hebt u volledige controle over de apparaten die verbinding maken met uw IoT Edge-apparaten, is het mogelijk een zakelijk niveau certificeringsinstantie gebruiken. In beide gevallen de volledige certificaatketen van de IoT Edge hub van genereert een overzicht, zodat de leaf-IoT-apparaten moeten het basiscertificaat vertrouwen. U kunt het basis-CA-certificaat wordt opgeslagen in een instantie voor het vertrouwde Basiscertificaatarchief of geef de details van het certificaat in de code van uw toepassing.

### <a name="intermediate-certificates"></a>Tussenliggende certificaten

In een typische fabricageproces voor het maken van beveiligde apparaten, worden zelden rechtstreeks, basis-CA-certificaten gebruikt voornamelijk vanwege het risico van gegevenslekken en blootstelling. Het basis-CA-certificaat maakt en een of meer tussenliggende CA-certificaten van een digitale handtekening. Er kan alleen worden een of er mogelijk een keten van deze tussenliggende certificaten. Scenario's waarin een keten van tussenliggende certificaten zijn onder andere:

* Een hiërarchie van afdelingen binnen een fabrikant.

* Meerdere bedrijven die betrokken zijn bij de productie van een apparaat serie.

* Een klant aanschaf van een basis-CA en die is afgeleid van een handtekeningcertificaat van de fabrikant voor het ondertekenen van de apparaten die ze maken namens de klant.

De fabrikant gebruikt in elk geval een tussenliggende CA-certificaat aan het einde van deze keten ondertekenen van het CA-certificaat van het apparaat op het apparaat end geplaatst. Deze tussenliggende certificaten zijn over het algemeen goed beveiligde in de fabriek. Ze ondergaan strikt omschreven processen, zowel fysieke als elektronische voor hun gebruik.

### <a name="device-ca-certificate"></a>Device CA-certificaat

Het CA-certificaat van het apparaat is gegenereerd op basis van en ondertekend door het laatste tussenliggende CA-certificaat in het proces. Dit certificaat wordt geïnstalleerd op het IoT Edge-apparaat, bij voorkeur in veilige opslag, zoals een hardware security module (HSM). Bovendien identificatie een certificaat van een apparaat unieke van een IoT Edge-apparaat. Voor IoT Edge, kan het CA-certificaat van het apparaat andere certificaten uitgeven. Bijvoorbeeld, het CA-apparaatcertificaat uitgeeft leaf certificaten voor apparaten die worden gebruikt voor verificatie van apparaten aan de [Azure IoT Device Provisioning Service](../iot-dps/about-iot-dps.md).

### <a name="iot-edge-workload-ca"></a>IoT Edge-werkbelasting CA

De [IoT Edge Security Manager](iot-edge-security-manager.md) de werkbelasting CA-certificaat, de eerste op de 'operator'-zijde van het proces wordt gegenereerd als IoT Edge voor het eerst wordt gestart. Dit certificaat wordt gegenereerd op basis van en ondertekend door het "device CA-certificaat". Dit certificaat, dat gewoon een tussenliggende handtekeningcertificaat is, wordt gebruikt om te genereren en meld u aan alle andere certificaten die worden gebruikt door de IoT Edge-runtime. Vandaag de dag die is voornamelijk IoT Edge hub certificaat van de server die in de volgende sectie wordt besproken, maar in de toekomst kunnen bevatten andere certificaten voor het verifiëren van IoT Edge-onderdelen.

### <a name="iot-edge-hub-server-certificate"></a>IoT Edge hub-servercertificaat

Het certificaat van de IoT Edge hub is het werkelijke certificaat weergegeven voor leaf-apparaten en om uw identiteit te verifiëren tijdens de inrichting van de TLS-verbinding vereist met IoT Edge-modules. Dit certificaat geeft de volledige keten van het ondertekenen van certificaten gebruikt voor het genereren ervan tot aan de basis-CA-certificaat de leaf-IoT-apparaat moet vertrouwen. Wanneer die worden gegenereerd door de IoT Edge Security Manager, de algemene naam (CN) van deze IoT Edge hub is certificaat ingesteld op de eigenschap 'hostname' in het bestand config.yaml na de conversie naar kleine letters. Dit is een veelvoorkomende oorzaak van verwarring met IoT Edge.

## <a name="production-implications"></a>Gevolgen voor productie

Een redelijke vraag mogelijk "Waarom IoT Edge hoeft de 'werkbelasting CA' extra certificaat? Kan dit niet het CA-apparaatcertificaat gebruiken voor het genereren van het certificaat van de IoT Edge hub rechtstreeks? ". In technisch opzicht kunnen kan deze. Het doel van deze "werkbelasting" tussenliggende certificaat is echter voor het scheiden van opmerkingen tussen de fabrikant van het apparaat en de operator apparaat. Stel een scenario waarbij een IoT Edge-apparaat wordt verkocht of naar een andere wordt overgenomen van een klant. U wilt waarschijnlijk het CA-certificaat van het apparaat geleverd door de fabrikant zijn onveranderd. De 'werkbelasting' certificaten die specifiek zijn voor de werking van het apparaat moeten echter worden gewist en opnieuw gemaakt voor de nieuwe implementatie.

Omdat de productie- en bewerking processen worden gescheiden, houd rekening met de volgende gevolgen tijdens het voorbereiden van de productieapparaten:

* Met een proces op basis van certificaten, moeten het basis-CA-certificaat en alle tussenliggende CA-certificaten worden beveiligd en gecontroleerd tijdens het hele proces van het implementeren van een IoT Edge-apparaat. De fabrikant van het IoT Edge-apparaat hebt strenge procedures zijn voor de juiste opslag en het gebruik van de tussenliggende certificaten. Bovendien moet de CA-certificaat van het apparaat worden opgeslagen in als beveiligde opslag mogelijk op het apparaat zelf, bij voorkeur een hardware security module.

* Het certificaat van de IoT Edge hub wordt weergegeven door de IoT Edge hub kunt u de client netwerkapparaten en -modules. De algemene naam (CN) van het CA-apparaatcertificaat **mag geen** gelijk zijn aan de 'hostnaam"die worden gebruikt voor config.yaml op het IoT Edge-apparaat. De naam die wordt gebruikt door clients verbinding maken met IoT Edge (bijvoorbeeld via de parameter GatewayHostName van de verbindingsreeks of met de opdracht CONNECT in MQTT) **kan niet worden** hetzelfde zijn als algemene naam in het device CA-certificaat gebruikt. Deze beperking is omdat de IoT Edge hub de volledige certificaatketen voor verificatie door clients geeft. Als het certificaat van de IoT Edge hub en het CA-apparaatcertificaat hebben de dezelfde algemene naam, u in een lus voor verificatie krijgt en het certificaat ongeldig gemaakt.

* Omdat het CA-certificaat van het apparaat wordt gebruikt door de IoT Edge security-daemon voor het genereren van de laatste IoT Edge-certificaten, moet zelf een handtekeningcertificaat, wat betekent dat er mogelijkheden voor Certificaatondertekening. Stelt u de eigenschappen van de vereiste sleutelgebruik in "V3 Basisbeperkingen CA:True" automatisch toepassen op het CA-apparaatcertificaat.

>[!Tip]
> Als u hebt al doorlopen de installatie van IoT Edge als een transparante gateway in een scenario voor ontwikkelen en testen met behulp van onze "scripts voor uw gemak bedoeld;" (Zie volgende sectie) en dezelfde hostnaam gebruikt bij het maken van het CA-apparaatcertificaat als u dit hebt gedaan voor de hostnaam in config.yaml , u mogelijk vraagt zich misschien af waarom het is gegaan. In een poging voor het vereenvoudigen van de ervaring voor ontwikkelaars, voegt de scripts gemak ".ca' aan het einde van de naam die u in het script doorgeeft. Dus, bijvoorbeeld, als u "mygateway" voor zowel de apparaatnaam in de scripts en hostname in config.yaml gebruikt, de voormalige wordt omgezet in mygateway.ca voordat het wordt gebruikt als de algemene naam voor het apparaat CA-certificaat.

## <a name="devtest-implications"></a>Gevolgen voor ontwikkelen en testen

Voor het vereenvoudigen van de ontwikkeling en testen van scenario's, biedt Microsoft een set [gemak scripts](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) voor het genereren van niet-productie-certificaten die geschikt zijn voor IoT Edge in het scenario voor transparante gateway. Zie voor meer voorbeelden van hoe de scripts werken [configureren van een IoT Edge-apparaat om te fungeren als een transparante gateway](how-to-create-transparent-gateway.md).

Deze scripts genereren van certificaten die de structuur van het certificaat-keten beschreven in dit artikel volgen. De volgende opdrachten de "CA-basiscertificaat' en een enkele"tussenliggende CA-certificaat' genereren.

```bash
./certGen.sh create_root_and_intermediate 
```

```Powershell
New-CACertsCertChain rsa 
```

Deze opdrachten genereren op dezelfde manier het "Device CA-certificaat".

```bash
./certGen.sh create_edge_device_certificate "<gateway device name>"
```

```Powershell
New-CACertsEdgeDevice "<gateway device name>"
```

* De **\<de naam van de gateway-apparaat\>** doorgegeven aan deze scripts **moet niet** niet dezelfde zijn als de parameter 'hostname' in config.yaml. De scripts kunnen u eventuele problemen voorkomen door het toevoegen van een tekenreeks '.ca' aan de **\<de naam van de gateway-apparaat\>** om te voorkomen dat de conflicterende namen in het geval van een gebruiker stelt u IoT Edge met dezelfde naam op beide plaatsen. Het is echter verstandig om te voorkomen dat met dezelfde naam.

>[!Tip]
> Als u wilt verbinding maken met uw apparaat IoT "leaf" apparaten en toepassingen die gebruikmaken van onze IoT-device-SDK via IoT Edge, moet u de optionele parameter GatewayHostName u aan bij het einde van de verbindingsreeks van het apparaat toevoegen. Wanneer het certificaat van de Edge Hub wordt gegenereerd, is gebaseerd op een indeling met een lagere versie van de hostnaam van config.yaml, daarom de namen van de overeenkomst en de verificatie van TLS-certificaat te voltooien, moet u de parameter GatewayHostName in kleine letters.

## <a name="example-of-iot-edge-certificate-hierarchy"></a>Voorbeeld van een certificaathiërarchie IoT Edge

Ter illustratie van een voorbeeld van dit pad naar het certificaat, is de volgende schermafbeelding van een werkende IoT Edge-apparaat instellen als een transparante gateway. OpenSSL wordt gebruikt om verbinding maken met de IoT Edge hub, valideren en de certificaten dump.

![Schermafbeelding van de certificaathiërarchie op elk niveau](./media/iot-edge-certs/iotedge-cert-chain.png)

Hier ziet u de hiërarchie van certificaat diepte weergegeven in de schermafbeelding:

| Basis-CA-certificaat         | Azure IoT Hub-CA-certificaat Test alleen                                                                           |
|-----------------------------|-----------------------------------------------------------------------------------------------------------|
| Tussenliggende CA-certificaat | Azure IoT Hub tussenliggende certificaat alleen testen                                                                 |
| Device CA-certificaat       | iotgateway.CA ("iotgateway" is doorgegeven als de naam < gateway > aan de scripts voor uw gemak bedoeld;)      |
| Workload CA-certificaat     | iotedge werkbelasting ca                                                                                       |
| IoT Edge Hub-servercertificaat | iotedgegw.local (overeenkomt met de 'hostnaam' van config.yaml)                                                |

## <a name="next-steps"></a>Volgende stappen

[Informatie over Azure IoT Edge-modules](iot-edge-modules.md)

[Een IoT Edge-apparaat om te fungeren als een transparante gateway configureren](how-to-create-transparent-gateway.md)