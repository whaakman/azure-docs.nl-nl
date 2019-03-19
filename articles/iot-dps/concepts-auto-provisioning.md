---
title: IoT Hub Device Provisioning Service - concepten voor automatische inrichting
description: In dit artikel biedt een conceptueel overzicht van de fasen van het apparaat automatisch inrichten met IoT Device Provisioning Service, IoT-Hub en client-SDK's.
author: wesmc7777
ms.author: wesmc
ms.date: 06/01/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 6ba21e29673da176c380d6b367e84b2ea7536955
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58080028"
---
# <a name="auto-provisioning-concepts"></a>Concepten voor automatische inrichting

Zoals beschreven in de [overzicht](about-iot-dps.md), de Device Provisioning Service is een helper-service die just-in-time inrichting van apparaten naar een IoT-hub zonder menselijke tussenkomst. Inrichting is voltooid, worden apparaten verbinding rechtstreeks met hun aangewezen IoT-Hub. Dit proces wordt aangeduid als automatische inrichting, en biedt dat een out-of-the-box-registratie en eerste configuratie-ervaring voor apparaten.

## <a name="overview"></a>Overzicht

Azure IoT automatische inrichting kan worden onderverdeeld in drie fasen:

1. **Configuratie van de service** -een eenmalige configuratie van de Azure IoT Hub en IoT Hub Device Provisioning Service-exemplaren, tot stand brengen van ze en opeenvolging van koppelingen tussen hen te maken.

   > [!NOTE]
   > Ongeacht de grootte van uw IoT-oplossing, zelfs als u van plan bent voor de ondersteuning van miljoenen apparaten, dit is een **eenmalige configuratie**.

2. **Apparaatinschrijving** -het proces van het maken van de Device Provisioning Service-exemplaar op de hoogte van de apparaten die u probeert te registreren in de toekomst. [Inschrijving](concepts-service.md#enrollment) wordt bereikt door het configureren van informatie over apparaat-id in de provisioning-service als een 'afzonderlijke inschrijving"voor een enkel apparaat of een 'groepsregistratie' voor meerdere apparaten. Identiteit is gebaseerd op de [attestation-mechanisme](concepts-security.md#attestation-mechanism) het apparaat is ontworpen om te gebruiken, waarmee de provisioning-service de echtheid van het apparaat tijdens de registratie bevestigen:

   - **TPM**: geconfigureerd als een 'afzonderlijke inschrijving', de apparaat-id is gebaseerd op de TPM-registratie-ID en de openbare goedkeuringssleutel. Gezien het feit dat de TPM is een [specificatie](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/), de service alleen wordt verwacht dat bevestigen volgens de specificatie, ongeacht de TPM-implementatie (hardware of software). Zie [Device provisioning: Identiteitscontrole met TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) voor meer informatie over op basis van TPM-attestation. 

   - **X509**: geconfigureerd als een 'afzonderlijke inschrijving' of 'groepsregistratie', de apparaat-id is gebaseerd op een x.509-digitaal certificaat, dat is geüpload naar de inschrijving als een PEM- of cer-bestand.

   > [!IMPORTANT]  
   > Hoewel het is geen vereiste voor het gebruik van de Device Provisioning Services, wordt aangeraden dat uw apparaat gebruik een Hardware Security Module (HSM) voor het opslaan van identiteitsgegevens van gevoelige apparaat, zoals sleutels en X.509-certificaten.

3. **Device Registration service en configuratie** - gestart bij het opstarten van de door van registratiesoftware, die is gebouwd met behulp van een Device Provisioning Service client SDK die geschikt is voor het apparaat en de attestation-mechanisme. De software maakt verbinding met de provisioning-service voor verificatie van het apparaat en de registratie van de volgende in de IoT-Hub. Bij registratie is gelukt, wordt het apparaat aangeboden met de IoT Hub unieke apparaat-ID en verbinding informatie, zodat het voor het ophalen van de eerste configuratie en beginnen met de telemetrie. In een productieomgeving, kan deze fase weken of maanden na de vorige twee fasen optreden.

## <a name="roles-and-operations"></a>Rollen en bewerkingen

De fasen in de vorige sectie besproken, kunnen omvatten weken of maanden, vanwege de realiteit van de productie zoals de tijd van de productie, levering, douane proces, enzovoort. Bovendien kunnen deze activiteiten omvatten diverse meerdere rollen gegeven van de verschillende entiteiten die betrokken zijn. In deze sectie wordt een beter overzicht gegeven van de verschillende functies en bewerkingen die betrekking hebben op elke fase en vervolgens ziet u de stroom in een reeksdiagram. 

Vereisten voor automatische inrichting ook worden geplaatst op de fabrikant die specifiek zijn voor het inschakelen van de attestation-mechanisme. Productie-bewerkingen kan zich ook voordoen onafhankelijk van de timing van automatische inrichting fasen, met name in gevallen waar nieuwe apparaten worden verkregen nadat de automatische inrichting is tot stand is gebracht.

Een reeks snelstartgidsen vindt u in de inhoudsopgave aan de linkerkant, voor een verklaring van automatische inrichting tot en met praktische ervaring. Als u wilt het leerproces vergemakkelijken/vereenvoudigen, wordt software gebruikt voor het simuleren van een fysiek apparaat voor apparaatregistratie en registratiestatus. Sommige QuickStart moet u om te voldoen aan de bewerkingen voor meerdere rollen, met inbegrip van bewerkingen voor niet-bestaande rollen, door de gesimuleerde aard van de Quick starts.

| Rol | Bewerking | Description |
|------| --------- | ------------|
| Fabrikant | Identiteits- en registratie-URL coderen | Op basis van de attestation-mechanisme dat wordt gebruikt, is de fabrikant verantwoordelijk voor het coderen van de gegevens van de apparaat-id en de URL van de registratie van Device Provisioning Service.<br><br>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, er is geen rol fabrikant. Zie de rol van ontwikkelaar voor meer informatie over hoe u deze informatie, die wordt gebruikt voor het coderen van een voorbeeldtoepassing voor registratie krijgen. |
| | Apparaat-id opgeven | Als de maker van de gegevens van de apparaat-id is de fabrikant verantwoordelijk voor het naar de operator (of een aangewezen agent) communiceren, of rechtstreeks inschrijft bij Device Provisioning Service via API's.<br><br>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, er is geen rol fabrikant. Zie de rol Operator voor meer informatie over hoe u de apparaat-id, die wordt gebruikt voor het inschrijven van een gesimuleerd apparaat in uw Device Provisioning Service-exemplaar. |
| Operator | Automatische inrichting configureren | Met deze bewerking, komt overeen met de eerste fase van de automatische inrichting.<br><br>**Snelstartgidsen**: U uitvoeren de rol Operator voor het configureren van de Device Provisioning Service en IoT Hub-exemplaren in uw Azure-abonnement. |
|  | Registreren van apparaat-id | Met deze bewerking, komt overeen met de tweede fase van de automatische inrichting.<br><br>**Snelstartgidsen**: U uitvoeren de rol Operator uw gesimuleerde apparaat te registreren in uw Device Provisioning Service-exemplaar. De apparaat-id wordt bepaald door de attestation-methode in de Quick Start (TPM of X.509) wordt gesimuleerd. Zie de rol van ontwikkelaar voor attestation-gegevens. |
| Device Provisioning Service<br>IoT Hub | \<alle bewerkingen\> | Voor beide een productie-implementatie met fysieke apparaten en snelstartgidsen met gesimuleerde apparaten, worden deze rollen vervuld via de IoT-services die u in uw Azure-abonnement configureren. De rollen/bewerkingen werkt precies hetzelfde, zoals de IoT-services onverschillig zijn voor het inrichten van fysieke versus gesimuleerde apparaten. |
| Developer | Software voor apparaatregistratie bouwen/implementeren | Met deze bewerking, komt overeen met de derde fase van de automatische inrichting. De ontwikkelaar is verantwoordelijk voor het bouwen en de software voor apparaatregistratie implementeren op het apparaat met de juiste SDK.<br><br>**Snelstartgidsen**: De voorbeeldtoepassing voor registratie die u bouwt simuleert een echt apparaat, voor uw platform/taal naar keuze in, die wordt uitgevoerd op uw werkstation (in plaats van deze is geïmplementeerd op een fysiek apparaat). De aanvraag voert dezelfde bewerkingen uitvoeren als een geïmplementeerd op een fysiek apparaat. U opgeven de attestation-methode (TPM of x.509-certificaat), plus de registratie-URL en 'ID-bereik' van uw Device Provisioning Service-exemplaar. De apparaat-id wordt bepaald door de logica van de attestation SDK tijdens runtime, op basis van de methode die u opgeeft: <ul><li>**TPM-attestation** -uw ontwikkelwerkstation wordt uitgevoerd een [TPM-simulator toepassing](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Zodra actief is, wordt een afzonderlijke toepassing wordt gebruikt om op te halen van de TPM 'Endorsement Key' en 'registratie-ID"voor gebruik in het registreren van de apparaat-id. De logica van de attestation SDK gebruikt ook de simulator tijdens de registratie, om weer te geven van een ondertekende SAS-token voor verificatie en verificatie voor inschrijving.</li><li>**X509 attestation** -gebruik van een hulpprogramma voor het [een certificaat genereren](how-to-use-sdk-tools.md#x509-certificate-generator). Als die worden gegenereerd, maakt u het certificaatbestand dat vereist is voor gebruik in de inschrijving. De logica van de attestation SDK ook het certificaat wordt gebruikt tijdens de registratie, om weer te geven voor verificatie en verificatie voor inschrijving.</li></ul> |
| Apparaat | Opstartstatus en registreren | Met deze bewerking komt overeen met de derde fase van de automatische inrichting, voldaan door de software voor apparaatregistratie die zijn gemaakt door de ontwikkelaar. Zie de functie voor ontwikkelaars voor meer informatie. Na de eerste keer opstarten: <ol><li>De toepassing verbinding maakt met het Device Provisioning Service-exemplaar per de algemene URL en de service 'Id-bereik' dat is opgegeven tijdens de ontwikkeling.</li><li>Eenmaal verbinding hebben, wordt het apparaat wordt geverifieerd op basis van de attestation-methode en de identiteit die is opgegeven tijdens de inschrijving.</li><li>Eenmaal is geverifieerd, wordt het apparaat is geregistreerd bij de IoT Hub-instantie die is opgegeven door de provisioning service-exemplaar.</li><li>Na de registratie is gelukt, worden een unieke apparaat-ID en IoT Hub-eindpunt geretourneerd naar de registratietoepassing om te communiceren met IoT Hub.</li><li> Van daaruit, het apparaat de eerste kunt halen [apparaatdubbel](~/articles/iot-hub/iot-hub-devguide-device-twins.md) staat voor de configuratie en beginnen met het proces van het melden van telemetrische gegevens.</li></ol>**Snelstartgidsen**: omdat het apparaat wordt gesimuleerd, de registratiesoftware wordt uitgevoerd op uw ontwikkelwerkstation.|

Het volgende diagram geeft een overzicht van de functies en de volgorde van bewerkingen tijdens de automatische inrichting van apparaat:
<br><br>
[![Automatische inrichting reeks voor een apparaat](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png)](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png#lightbox) 

> [!NOTE]
> (Optioneel) de fabrikant kan ook de bewerking niet uitvoeren "voor registratie apparaat-id' met Device Provisioning Service API's (in plaats van via de Operator). Zie voor een gedetailleerde beschrijving van deze sequentiëren en nog veel meer, de [nul touch-apparaatregistratie met Azure IoT-video](https://youtu.be/cSbDRNg72cU?t=2460) (vanaf markering 41:00)

## <a name="roles-and-azure-accounts"></a>Functies en Azure-accounts

Hoe elke rol is toegewezen aan een Azure-account is afhankelijk van het scenario en er zijn een groot aantal scenario's die kunnen worden uitgevoerd. De onderstaande algemene patronen kunnen helpen met het bieden van een algemeen begrip met betrekking tot hoe rollen in het algemeen zijn toegewezen aan een Azure-account.

#### <a name="chip-manufacturer-provides-security-services"></a>-Chipfabrikant biedt security-services

In dit scenario beheert de fabrikant van de beveiliging voor niveau 1-klanten. In dit scenario kan worden voorkeur van deze klanten niveau één als ze geen hebben voor het beheren van uitgebreide beveiliging. 

De fabrikant introduceert beveiliging in Hardware Security Modules (HSM's). Deze beveiliging kan de fabrikant van het verkrijgen van sleutels, certificaten, enzovoort van potentiële klanten die al DPS-exemplaren en inschrijving groepen instellingen hebben bevatten. De fabrikant kan deze beveiligingsgegevens voor de klanten ook genereren.

In dit scenario kunnen er twee Azure-accounts die zijn betrokken:

- **Account #1**: Waarschijnlijk gedeeld tussen de rollen operator en ontwikkelaars tot op zekere hoogte. Deze partij kan de HSM-chips aanschaffen van de fabrikant. Deze chips zijn waarnaar wordt verwezen naar de DPS-exemplaren die zijn gekoppeld aan het Account #1. Met DPS-registraties, kan deze partij apparaten aan meerdere niveau twee klanten lease door de instellingen voor de inschrijving van apparaten in DPS opnieuw in te. Deze partij mogelijk ook IoT-hubs die zijn toegewezen voor eindgebruikers back-endsystemen aan voor toegang tot telemetrie van apparaten enzovoort. In het laatste geval, is een tweede account mogelijk niet nodig.

- **Account #2**: Eindgebruikers, niveau van twee klanten kunnen hun eigen IoT-hubs hebben. De partij die zijn gekoppeld aan Account #1 alleen punten in lease gegeven apparaten naar de juiste hub in dit account. Deze configuratie moet het koppelen van DPS en IoT-hubs in Azure-accounts, die kunnen worden uitgevoerd met Azure Resource Manager-sjablonen.

#### <a name="all-in-one-oem"></a>Alles-in-een door OEM

De fabrikant kan worden "All-in-one OEM" waar slechts één fabrikant-account nodig zou zijn. De fabrikant verwerkt beveiligings- en end-to-end wordt ingericht.

De fabrikant kan een cloud-gebaseerde toepassing naar klanten die apparaten kopen bieden. Deze toepassing zou een interface met de IoT-Hub die is toegewezen door de fabrikant.

Verkoopautomaten of geautomatiseerde koffiezetmachines staan voorbeelden voor dit scenario.




## <a name="next-steps"></a>Volgende stappen

Wellicht vindt u het handig om als verwijzing, maak een bladwijzer voor dit artikel als u uw eigen manier via de bijbehorende snelstartgidsen voor automatische inrichting werkt. 

Beginnen met het voltooien van een 'Automatische inrichting instellen'-snelstartgids die het beste bij uw voorkeur voor het hulpprogramma van management, die u bij de "Service" configuratiefase helpt:

- [Instellen van automatische inrichting met behulp van Azure CLI](quick-setup-auto-provision-cli.md)
- [Instellen van automatische inrichting met behulp van de Azure portal](quick-setup-auto-provision.md)
- [Automatische inrichting met behulp van Resource Manager-sjabloon instellen](quick-setup-auto-provision-rm.md)

Ga vervolgens door met een 'Automatisch inrichten een gesimuleerd apparaat' Quick Start dat aansluit op uw apparaat attestation-mechanisme en SDK/Taalvoorkeur van Device Provisioning Service. In deze snelstartgids hebt doorlopen u de fasen van de 'Apparaatinschrijving' en 'Device Registration service en -configuratie': 

|  | Gesimuleerd apparaat attestation-mechanisme | Snelstartgids SDK/taal |  |
|--|--|--|--|
|  | Trusted Platform Module (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | X.509-certificaat | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




