---
title: bestand opnemen
description: bestand opnemen
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 6179086c6a2cf187c976ff23bf24180257023d28
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289169"
---
# <a name="secure-your-internet-of-things-iot-deployment"></a>Uw Internet of Things (IoT)-implementatie beveiligen

Dit artikel bevat het volgende niveau van de gegevens voor het beveiligen van de infrastructuur op basis van Azure IoT-Internet of Things (IoT). Deze koppelingen naar gedetailleerde implementatiegegevens voor het configureren en implementeren van elk onderdeel. Het biedt ook vergelijkingen en keuzes te bieden tussen verschillende concurrerende methoden.

Beveiligen van de Azure IoT-implementatie kan worden onderverdeeld in de volgende aspecten van de drie beveiliging:

* **Apparaatbeveiliging**: de IoT-apparaat beveiligen terwijl deze wordt geïmplementeerd tijdens het gebruik.

* **Verbindingsbeveiliging**: ervoor te zorgen dat alle gegevens die worden verzonden tussen de IoT-apparaat en IoT Hub is vertrouwelijk en fraudebestendig.

* **Cloud Security**: de mogelijkheid om gegevens te beveiligen terwijl deze doorloopt, en wordt opgeslagen in de cloud.

![Drie aspecten van de beveiliging](./media/iot-secure-your-deployment/overview.png)

## <a name="secure-device-provisioning-and-authentication"></a>Beveiligde apparaten inrichten en verificatie

De IoT-oplossingsversnellers beveiligde IoT-apparaten met behulp van de volgende twee methoden:

* Door een unieke id-sleutel (beveiligingstokens) voor elk apparaat, die door het apparaat kan worden gebruikt om te communiceren met de IoT-Hub.

* Met behulp van een apparaat op [X.509-certificaat](http://www.itu.int/rec/T-REC-X.509-201210-I/en) en de persoonlijke sleutel als een manier om het apparaat naar de IoT Hub te verifiëren. Deze verificatiemethode zorgt ervoor dat de persoonlijke sleutel op het apparaat niet bekend is buiten het apparaat op elk gewenst moment een hoger niveau van beveiliging bieden.

De methode security token biedt verificatie voor elke aanroep van het apparaat naar IoT Hub door te koppelen van de symmetrische sleutel aan elke aanroep. Op basis van X.509-verificatie kunnen verificatie van een IoT-apparaat op de fysieke laag als onderdeel van de TLS-verbinding tot stand brengen. De methode op basis van security-token kan worden gebruikt zonder de X.509-verificatie een minder veilige patroon is. De keuze tussen de twee methoden is voornamelijk afhankelijk van hoe veilig apparaat verificatie moet worden en beschikbaarheid van beveiligde opslag op het apparaat (voor het veilig opslaan van de persoonlijke sleutel).

## <a name="iot-hub-security-tokens"></a>Beveiligingstokens van IoT Hub

IoT Hub maakt gebruik van beveiligingstokens om apparaten en services om te voorkomen dat sleutels verzenden via het netwerk te verifiëren. Bovendien zijn beveiligingstokens beperkt in de geldigheid van de tijd en het bereik. Azure IoT SDK's automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's, moeten echter de gebruiker te genereren en beveiligingstokens rechtstreeks gebruiken. Deze scenario's omvatten het direct gebruik van de oppervlakken MQTT-, AMQP- of HTTP- of de implementatie van het patroon van de service voor beveiligingstokens.

Meer informatie over de structuur van het beveiligingstoken en het gebruik ervan kunnen worden gevonden in de volgende artikelen:

* [Token beveiligingsstructuur](../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure)

* [Met behulp van SAS-tokens als een apparaat](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app)

Elke IoT-Hub is een [id-register](../articles/iot-hub/iot-hub-devguide-identity-registry.md) die kunnen worden gebruikt per apparaat om resources te maken in de service, zoals een wachtrij met berichten die onderweg zijn cloud-naar-apparaat, en voor toegang tot het apparaat gerichte eindpunten. De id-register van IoT Hub biedt een veilige opslag van apparaat-id's en sleutels voor een oplossing. Persoon of groepen van apparaat-id's kunnen worden toegevoegd aan een lijst met toegestane of een lijst met geblokkeerde websites volledige controle over toegang tot het apparaat inschakelen. De volgende artikelen vindt u meer informatie over de structuur van de id-register en de ondersteunde bewerkingen.

[IoT Hub biedt ondersteuning voor protocollen, zoals MQTT, AMQP en HTTP-](../articles//iot-hub/iot-hub-devguide-security.md). Elk van deze protocollen beveiligingstokens van de IoT-apparaat naar IoT Hub anders gebruikt:

* AMQP: SASL zonder opmaak en een AMQP-op basis van Claims-beveiliging (`{policyName}@sas.root.{iothubName}` met IoT hub op serverniveau tokens. `{deviceId}` met behulp van tokens binnen het bereik van apparaat).

* MQTT: Verbinding maken met het pakket gebruikt `{deviceId}` als de `{ClientId}`, `{IoThubhostname}/{deviceId}` in de **gebruikersnaam** veld en een SAS-token in de **wachtwoord** veld.

* HTTP: Ongeldig token is in de aanvraagheader autorisatie.

ID-register van IoT Hub kan worden gebruikt voor het configureren van per apparaat beveiligingsreferenties en toegangscontrole. Echter, als een IoT-oplossing heeft al een aanzienlijke investering een [aangepast apparaat-id-register en/of verificatie schema](../articles/iot-hub/iot-hub-devguide-security.md#custom-device-and-module-authentication), deze kan worden geïntegreerd in een bestaande infrastructuur met IoT Hub met het maken van een service voor beveiligingstokens.

### <a name="x509-certificate-based-device-authentication"></a>Verificatie van X.509-apparaat op basis van certificaten

Het gebruik van een [X.509-certificaat op basis van apparaat](../articles/iot-hub/iot-hub-devguide-security.md) en een bijbehorende persoonlijke en openbare sleutelpaar kunt u aanvullende verificatie bij de fysieke laag. De persoonlijke sleutel veilig opgeslagen in het apparaat en kan niet worden gedetecteerd buiten het apparaat. Het X.509-certificaat bevat informatie over het apparaat, zoals de apparaat-ID en andere organisatie-gegevens. Een handtekening van het certificaat wordt gegenereerd met behulp van de persoonlijke sleutel.

Inrichten werkstroom op hoog niveau apparaat:

* Een op een fysiek apparaat – apparaat-id en/of X.509-certificaat dat is gekoppeld aan het apparaat tijdens de productie- of bedrijf stellen apparaat-id koppelen.

* Maak een bijbehorende vermelding in de identiteit in IoT Hub-apparaat-id en bijbehorende apparaatgegevens in de id-register van IoT Hub.

* Vingerafdruk van het x.509-certificaat veilig opslaan in IoT Hub-identiteitsregister.

### <a name="root-certificate-on-device"></a>Basiscertificaat op apparaat

De IoT-apparaat verifieert tijdens het maken van een beveiligde TLS-verbinding met IoT Hub, IoT-Hub met behulp van een basiscertificaat dat deel uitmaakt van de apparaat-SDK. Voor de client voor C SDK het certificaat bevindt zich onder de map '\\c\\certificaten ' onder de hoofdmap van de opslagplaats. Hoewel deze basiscertificaten lange levensduur hebben, ze nog steeds mogelijk verlopen of worden ingetrokken. Als er geen manier voor het bijwerken van het certificaat op het apparaat, kan het apparaat mogelijk niet vervolgens verbinding maken met de IoT-Hub (of een andere cloudservice). Een manier om bij te werken van het basiscertificaat zodra de IoT-apparaat daadwerkelijk is geïmplementeerd met verkleint dit risico.

## <a name="securing-the-connection"></a>Beveiligen van de verbinding

Internet-verbinding tussen de IoT-apparaat en IoT-Hub is beveiligd met behulp van de standaard Transport Layer Security (TLS). Azure IoT ondersteunt [TLS 1.2](https://tools.ietf.org/html/rfc5246), TLS 1.1 en TLS 1.0 in de aangegeven volgorde. Ondersteuning voor TLS 1.0 wordt aangeboden voor achterwaartse compatibiliteit alleen. Gebruik van TLS 1.2, indien mogelijk, omdat het de meeste beveiliging biedt.

## <a name="securing-the-cloud"></a>De cloud beveiligen

Azure IoT Hub kunnen definitie van [toegangsbeheerbeleid](../articles/iot-hub/iot-hub-devguide-security.md) voor elke beveiligingssleutel. Hierbij de volgende set machtigingen voor toegang verlenen tot elk van de IoT Hub-eindpunten. Machtigingen beperken de toegang tot een IoT-Hub op basis van functionaliteit.

* **RegistryRead**. Verleent leestoegang tot het id-register. Zie voor meer informatie, [id-register](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

* **RegistryReadWrite**. Verleent lezen en schrijven naar het id-register. Zie voor meer informatie, [id-register](../articles/iot-hub/iot-hub-devguide-identity-registry.md).

* **ServiceConnect**. Verleent toegang tot de cloud service gerichte communicatie en -eindpunten worden gecontroleerd. Bijvoorbeeld, er een machtiging verleend voor back-end cloudservices apparaat-naar-cloud-berichten ontvangen, cloud-naar-apparaat-berichten verzenden en ophalen van de bijbehorende delivery-bevestigingen.

* **DeviceConnect**. Verleent toegang tot apparaat gerichte eindpunten. Bijvoorbeeld, verleent deze machtiging voor het apparaat-naar-cloud-berichten verzenden en ontvangen van berichten van cloud-naar-apparaat. Deze machtiging wordt gebruikt door apparaten.

Er zijn twee manieren om op te halen **DeviceConnect** machtigingen met IoT Hub met [beveiligingstokens](../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app): met behulp van een apparaat-id-sleutel of een gedeelde toegangssleutel. Bovendien is het belangrijk te weten dat alle functionaliteit die toegankelijk is vanaf apparaten wordt weergegeven op de eindpunten met het voorvoegsel standaard `/devices/{deviceId}`.

[Onderdelen van de service kunnen alleen genereren voor beveiligingstokens](../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components) met behulp van gedeelde toegangsbeleid dat de juiste machtigingen te verlenen.

Azure IoT Hub en andere services die deel van de oplossing uitmaken kunnen beheer van gebruikers met behulp van de Azure Active Directory.

Gegevens die worden verwerkt door Azure IoT Hub kan worden gebruikt door verschillende services zoals Azure Stream Analytics en Azure blob-opslag. Deze services verlenen management toegang. Meer informatie over deze services en de beschikbare opties:

* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/): een schaalbare, volledig geïndexeerd databaseservice voor de semi-gestructureerde gegevens die worden beheerd metagegevens voor de apparaten die u inricht, zoals kenmerken, configuratie en de eigenschappen voor beveiliging. Azure Cosmos DB biedt hoge prestaties en hoge doorvoer verwerkt, schema-agnostische indexering van gegevens en een geavanceerde SQL-QueryInterface.

* [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/): realtime-verwerking in de cloud waarmee u snel ontwikkelen en implementeren van een goedkope analyseoplossing realtime inzichten kunt van apparaten, sensoren, infrastructuur en toepassingen. De gegevens uit deze volledig beheerde service kan worden geschaald naar een volume behoud van hoge doorvoer, lage latentie en tolerantie.

* [Azure App Services](https://azure.microsoft.com/services/app-service/): een cloudplatform voor het bouwen van krachtige web- en mobiele apps die verbinding met gegevens waar dan ook; in de cloud of on-premises maken. Aansprekende mobiele apps bouwen voor iOS, Android en Windows. Integreren met uw Software as a Service (SaaS) en zakelijke toepassingen met out-of-the-box-connectiviteit met tientallen cloudgebaseerde services en zakelijke toepassingen. Coderen in uw favoriete taal en IDE (.NET, Node.js, PHP, Python of Java) sneller dan ooit webtoepassingen en API's bouwen.

* [Logic Apps](https://azure.microsoft.com/services/app-service/logic/): Logic Apps-functie van Azure App Service helpt uw IoT-oplossing met uw bestaande line-of-business-systemen integreren en automatiseren van werkstroomprocessen worden uitgevoerd. Logic Apps kan ontwikkelaars werkstromen ontwerpen die na een trigger worden gestart en voer vervolgens een reeks stappen, regels en acties die gebruikmaken van krachtige connectors om te integreren in uw bedrijfsprocessen. Logic Apps biedt out-of-the-box verbinding met een uitgebreid ecosysteem van SaaS, op basis van cloud en on-premises toepassingen.

* [Azure Blob-opslag](https://azure.microsoft.com/services/storage/): betrouwbare, betaalbare cloudopslag voor de gegevens die uw apparaten naar de cloud verzenden.

## <a name="conclusion"></a>Conclusie

Dit artikel bevat de gedetailleerde gegevens voor het ontwerpen en implementeren van een IoT-infrastructuur met behulp van Azure IoT overzicht van de uitvoering. Configureren van elk onderdeel om te worden beveiligd, is de sleutel bij het beveiligen van de algemene IoT-infrastructuur. De ontwerpkeuzes die beschikbaar zijn in Azure IoT een bepaalde mate van flexibiliteit en keuze; elke keuze hebben echter invloed op de beveiliging. Het verdient aanbeveling dat elk van deze opties worden geëvalueerd via een evaluatie van de risico's / kosten.
