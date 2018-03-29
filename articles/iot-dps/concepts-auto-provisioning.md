---
title: IoT Hub apparaat inrichtingsservice - concepten automatische inrichting
description: Dit artikel bevat een overzicht van de fasen van het apparaat automatisch-inrichting, met IoT Device inrichtingsservice, IoT-Hub en client-SDK's.
services: iot-dps
keywords: ''
author: BryanLa
ms.author: bryanla
ms.date: 03/27/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: cd458b1f6d26fbd5f5821a04cd01be5c3a4e4514
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="auto-provisioning-concepts"></a>Automatische inrichting concepten

Zoals beschreven in de [overzicht](about-iot-dps.md), de Service voor het inrichten van apparaten is een helper-service waarmee just in time voor het inrichten van apparaten naar een IoT-hub, zonder menselijke tussenkomst. Apparaten verbinding maken na geslaagde inrichting rechtstreeks met hun aangewezen IoT-Hub. Dit proces wordt aangeduid als automatische inrichting en zorgt voor een out-of-the-box-registratie en de initiële configuratie voor apparaten.

## <a name="overview"></a>Overzicht

Azure IoT-auto-inrichting worden onderverdeeld in drie fasen:

1. **Configuratie van de service** -een eenmalige configuratie van de Azure IoT Hub en IoT Hub apparaat-inrichtingsservice exemplaren tot stand brengen van deze en het maken van de koppeling tussen deze twee.

   > [!NOTE]
   > Ongeacht de grootte van uw IoT-oplossing, zelfs als u van plan bent voor de ondersteuning van miljoenen apparaten, dit is een **eenmalige configuratie**.

2. **Apparaatinschrijving** -het proces van het apparaat wordt ingericht Service-exemplaar op de hoogte van de apparaten die u probeert te registreren in de toekomst te maken. Inschrijving wordt gerealiseerd door het apparaat identiteitsgegevens in de inrichting service configureren als een 'afzonderlijke registratie"tot één apparaat of een 'groepsinschrijving' voor meerdere apparaten. Identiteit is gebaseerd op de [attestation mechanisme](concepts-security.md#attestation-mechanism) het apparaat is ontworpen om te gebruiken, waardoor de inrichting service sta in voor de echtheid van het apparaat tijdens de registratie:

   - **TPM**: geconfigureerd als een 'afzonderlijke inschrijving', de apparaat-id is gebaseerd op de TPM-registratie-ID en de openbare goedkeuringssleutel. Gezien het feit dat de TPM is een [specificatie]((https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)), de service alleen wordt verwacht verklaren volgens de specificatie, ongeacht TPM-implementatie (hardware of software). Zie [apparaten inrichten: identiteit attestation met TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) voor meer informatie over attestation op basis van TPM. 

   - **X509**: geconfigureerd als een 'afzonderlijke inschrijving' of 'groepsinschrijving', de apparaat-id is gebaseerd op een digitale X.509-certificaat op, die is geüpload naar de inschrijving als .pem of .cer-bestand.

   > [!IMPORTANT]  
   > Hoewel niet vereist voor het gebruik van apparaat leveren van Services, wordt aangeraden dat uw apparaat gebruik een Hardware Security Module (HSM) voor het opslaan van identiteitsgegevens gevoelige apparaat, zoals x.509-certificaten en sleutels.

3. **Apparaatregistratie en configuratie van** - gestart bij het opstarten van registratie van software die is gebouwd met behulp van een apparaat inrichtingsservice client SDK die geschikt is voor het apparaat en de attestation-mechanisme. De software maakt een verbinding met de inrichting service voor verificatie van het apparaat en de volgende registratie van de IoT-Hub. Na geslaagde inschrijving wordt wordt het apparaat geleverd met de IoT Hub unieke apparaat-ID en verbinding informatie, zodat het ophalen van de eerste configuratie en te beginnen met de telemetrie. In productieomgevingen, kan deze fase weken of maanden na de vorige twee fasen optreden.

## <a name="roles-and-operations"></a>Rollen en bewerkingen

De fasen beschreven in de vorige sectie kunnen omspannen weken of maanden, vanwege de realiteit van de productie zoals productie tijd, verzending, douane proces, enzovoort. Daarnaast kunnen ze activiteiten omvatten over meerdere functies gegeven van de verschillende entiteiten die betrokken zijn. In deze sectie wordt uitvoerig stil op de verschillende functies en bewerkingen die betrekking hebben op elke fase en illustreert de werkstroom in een reeksdiagram. 

Automatische inrichting ook vereisten op de fabrikant die specifiek zijn voor het inschakelen van het mechanisme voor attestation geplaatst. Productie operations kan zich ook voordoen onafhankelijk van de timing van automatische inrichting fasen, met name in gevallen waar de nieuwe apparaten worden aangeschaft nadat automatische inrichting is gebracht.

Een reeks snelstartgidsen vindt u in de tabel met inhoud aan de linkerkant om uit te leggen automatische inrichting met praktische ervaring. Software wordt gebruikt om het leerproces vergemakkelijken/vereenvoudigen, om te simuleren een fysiek apparaat voor inschrijving en registratie. Sommige snelstartgidsen moet u voldoen aan de bewerkingen voor meerdere rollen, met inbegrip van bewerkingen voor niet-bestaande rollen, vanwege de gesimuleerde aard van de snelstartgidsen.

| Rol | Bewerking | Beschrijving | 
|------| --------- | ------------| 
| Fabrikant | Identiteit en registratie van URL coderen | Op basis van de attestation mechanisme dat wordt gebruikt, is de fabrikant verantwoordelijk voor het coderen van de identiteit apparaatgegevens en de URL van de registratie apparaat inrichtingsservice.<br><br>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, er is geen rol fabrikant. Zie de functie voor ontwikkelaars voor meer informatie over hoe u deze informatie, die wordt gebruikt in een voorbeeldtoepassing registratie code ophalen. |  
| | Apparaat-id opgeven | Als de afzender van de apparaatgegevens voor identiteit is de fabrikant verantwoordelijk voor communiceert naar de operator (of een aangewezen agent) of deze rechtstreeks met de Service apparaat inrichten via API's te registreren.<br><br>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, er is geen rol fabrikant. Zie de rol Operator voor meer informatie over hoe het downloaden van de apparaat-id wordt gebruikt voor het registreren van een gesimuleerd apparaat in uw apparaat inrichtingsservice-exemplaar. | 
| Operator | Automatische inrichting configureren | Deze bewerking, komt overeen met de eerste fase van het automatisch inrichten.<br><br>**Snelstartgidsen**: U de rol Operator uitvoert voor het configureren van de Service voor het inrichten van apparaten en IoT Hub-exemplaren in uw Azure-abonnement. | Automatisch inrichten van installatie |
|  | Apparaat-id registreren | Deze bewerking, komt overeen met de tweede fase van het automatisch inrichten.<br><br>**Snelstartgidsen**: U de rol Operator uitvoeren uw gesimuleerde apparaat inschrijft voor uw apparaat inrichtingsservice-exemplaar. De apparaat-id wordt bepaald door de attestation-methode in de Quick Start (TPM of X.509) wordt gesimuleerd. Zie de Developer-functie voor attestation meer informatie. | 
| Inrichting Device-Service<br>IoT Hub | \<alle bewerkingen\> | Deze rollen voldaan voor zowel een productie-implementatie met fysieke apparaten en snelstartgidsen met gesimuleerde apparaten via de IoT-services die u in uw Azure-abonnement configureren. De rollen/operations werkt precies hetzelfde zijn, omdat de IoT-services onverschillig zijn voor het inrichten van fysieke versus gesimuleerde apparaten. | 
| Developer | Registratie software build/implementeren | Deze bewerking, komt overeen met de derde fase van het automatisch inrichten. De ontwikkelaar is verantwoordelijk voor het bouwen en implementeren van de registratie-software voor het apparaat met de juiste SDK.<br><br>**Snelstartgidsen**: de voorbeeldtoepassing registratie u bouwt simuleert een echte apparaat, voor uw platform/taal keuze, die wordt uitgevoerd op uw werkstation (in plaats van geïmplementeerd op een fysiek apparaat). De aanvraag voert dezelfde bewerkingen als een geïmplementeerd op een fysiek apparaat. U opgeven de attestation-methode (TPM of x.509-certificaat), plus de registratie-URL en het 'bereik ID' die van uw apparaat inrichtingsservice-exemplaar. De apparaat-id wordt bepaald door de logica van de attestation SDK tijdens runtime, op basis van de methode die u opgeeft: <ul><li>**TPM attestation** -uw ontwikkelwerkstation wordt uitgevoerd een [TPM simulator toepassing](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Wanneer uitgevoerd, wordt een afzonderlijke toepassing wordt gebruikt voor het uitpakken van de TPM 'Goedkeuringssleutel' en 'Registratie-ID' voor gebruik in de identiteit van het apparaat inschrijven. De SDK attestation logica gebruikt ook de simulator tijdens de registratie om weer te geven van een ondertekende SAS-token voor verificatie en verificatie van de inschrijving.</li><li>**X509 attestation** -gebruik van een hulpprogramma voor [Genereer een certificaat](how-to-use-sdk-tools.md#x509-certificate-generator). Als gegenereerd, maakt u het certificaatbestand vereist voor gebruik bij de registratie. De SDK attestation logica gebruikt ook het certificaat tijdens de registratie om weer te geven voor verificatie en verificatie van de inschrijving.</li></ul> | 
| Apparaat | Wanneer en registreren | Deze bewerking komt overeen met de derde fase van het automatische inrichting wordt voldaan door gebouwd door de ontwikkelaar van de software voor de registratie van het apparaat. Zie de functie voor ontwikkelaars voor meer informatie. Na de eerste keer wordt opgestart: <ol><li>De toepassing dat is verbonden met het apparaat wordt ingericht Service-exemplaar, per de globale URL en de service 'ID ' opgegeven bereik tijdens de ontwikkeling.</li><li>Eenmaal zijn verbonden, wordt het apparaat wordt geverifieerd met de identiteit die is opgegeven tijdens de inschrijving en de attestation-methode.</li><li>Eenmaal is geverifieerd, wordt het apparaat is geregistreerd bij de IoT Hub-instantie die is opgegeven door het inrichtingsproces service-exemplaar.</li><li>Een unieke apparaat-ID en IoT Hub-eindpunt worden na geslaagde inschrijving wordt geretourneerd naar de toepassing van de registratie voor communicatie met IoT Hub.</li><li> Van daaruit, het apparaat de eerste kan activeren [apparaat twin](~/articles/iot-hub/iot-hub-devguide-device-twins.md) status voor de configuratie en beginnen met de rapportagegegevens voor telemetrie.</li></ol>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, de registratie-software wordt uitgevoerd op uw ontwikkelwerkstation.| 

Het volgende diagram geeft een overzicht van de functies en de volgorde van bewerkingen tijdens het automatisch inrichten van apparaat:
<br><br>
![Automatische inrichting reeks voor een apparaat](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png) 

> [!NOTE]
> Eventueel de fabrikant kan ook de bewerking niet uitvoeren "voor registratie apparaat-id' met apparaat inrichten Service API's (in plaats van via de Operator). Zie voor een gedetailleerde discussie over deze sequentiëren en nog veel meer, de [nul touch-apparaatregistratie met Azure IoT video](https://myignite.microsoft.com/sessions/55087) (te beginnen bij de markering 41:00)

## <a name="next-steps"></a>Volgende stappen

Soms is het handig zijn in dit artikel bladwijzer als verwijzing, terwijl u zich door de bijbehorende automatische inrichting snelstartgidsen werkt. 

Beginnen met het voltooien van een 'Automatische inrichting instellen' Quickstart die het beste aan uw voorkeur management tool, die u bij de "Service" configuratiefase helpt:

- [Automatische inrichting met Azure CLI instellen](quick-setup-auto-provision-cli.md)
- [Automatisch inrichten met de Azure-Portal instellen](quick-setup-auto-provision.md)
- [Instellen van automatische inrichting met Resource Manager-sjabloon](quick-setup-auto-provision-rm.md)

Ga verder met een 'Auto-provision een gesimuleerd apparaat' Quick Start die past bij uw apparaat attestation mechanisme en Device inrichtingsservice SDK/Taalvoorkeur. In deze snelstartgids helpt u stapsgewijs door de fasen 'Apparaatinschrijving' en 'apparaatregistratie en configuratie': 

|  | Gesimuleerde apparaat attestation mechanisme | Quick Start SDK/taal |  |
|--|--|--|--|
|  | Trusted Platform Module (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509-certificaat | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




