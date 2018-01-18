# <a name="secure-your-iot-deployment"></a>Uw IoT-implementatie beveiligen

In dit artikel biedt de volgende detailniveau voor het beveiligen van de infrastructuur op basis van Azure IoT-Internet der dingen (IoT). Deze koppelingen naar gedetailleerde gegevens van de implementatie voor het configureren en implementeren van elk onderdeel. Het bevat ook vergelijkingen en -keuzes tussen verschillende concurrerende methoden.

Beveiliging van de Azure IoT-implementatie kan worden onderverdeeld in de volgende drie beveiligingsgebieden:

* **Apparaatbeveiliging**: beveiliging van de IoT-apparaat, terwijl deze is geïmplementeerd in het wild.
* **Verbindingsbeveiliging**: alle gegevens die worden overgedragen tussen de IoT-apparaat en IoT Hub gezorgd is vertrouwelijk en fraudebestendig.
* **Cloud Security**: de mogelijkheid om gegevens te beveiligen terwijl deze doorloopt en wordt opgeslagen in de cloud.

![Drie beveiligingsgebieden][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Beveiligen van mobiele apparaten inrichten en verificatie

De Azure IoT Suite beveiligt IoT-apparaten door de volgende twee methoden:

* Dankzij een unieke id van de sleutel (beveiligingstokens) voor elk apparaat, die kan worden gebruikt door het apparaat om te communiceren met de IoT-Hub.
* Met behulp van een apparaat op [X.509-certificaat] [ lnk-x509] en de persoonlijke sleutel als een manier om het apparaat met de IoT Hub te verifiëren. Deze verificatiemethode zorgt ervoor dat de persoonlijke sleutel op het apparaat niet bekend is buiten het apparaat op elk gewenst moment een hoger niveau van beveiliging bieden.

De token beveiligingsmethode biedt verificatie voor elke aanroep van het apparaat met IoT Hub door te koppelen van de symmetrische sleutel voor elke aanroep. Verificatie op basis van een X.509 kunt verificatie van een IoT-apparaat op de fysieke laag als onderdeel van de TLS-verbinding tot stand brengen. De methode op basis van beveiligings-tokens kan worden gebruikt zonder de X.509-verificatie een minder veilige patroon is. De keuze tussen de twee methoden is voornamelijk afhankelijk van hoe veilig apparaat verificatie moet worden en beschikbaarheid van beveiligde opslag op het apparaat (voor het veilig opslaan van de persoonlijke sleutel).

## <a name="iot-hub-security-tokens"></a>Beveiligingstokens van IoT Hub

IoT Hub gebruikt de beveiligingstokens om apparaten en services om te voorkomen dat sleutels verzenden via het netwerk te verifiëren. Bovendien zijn beveiligingstokens beperkt in geldigheid en het bereik. Azure IoT SDK's automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's is echter vereist voor de gebruiker te genereren en beveiligingstokens rechtstreeks gebruiken. Deze scenario's omvatten het direct gebruik van het oppervlak MQTT, AMQP of HTTP- of de implementatie van het patroon van de service voor beveiligingstokens.

U vindt meer informatie over de structuur van het beveiligingstoken en het gebruik ervan in de volgende artikelen:

* [Security token structuur][lnk-security-tokens]
* [Met SAS-tokens als een apparaat][lnk-sas-tokens]

Elke IoT-Hub heeft een [identiteitsregister] [ lnk-identity-registry] die kunnen worden gebruikt per apparaat om resources te maken in de service, zoals een wachtrij met onderweg cloud-naar-apparaat-berichten, en dat toegang tot de apparaat gerichte eindpunten. De id-register van IoT Hub biedt een veilige opslag van apparaat-id's en sleutels voor een oplossing. Persoon die of groepen van apparaat-id's kunnen worden toegevoegd aan een lijst met toegestane of een lijst met geblokkeerde volledige controle toegang tot het apparaat inschakelen. De volgende artikelen bevatten meer details over de structuur van de id-register en de ondersteunde bewerkingen.

[IoT Hub zoals MQTT AMQP en HTTP-protocollen ondersteunt][lnk-protocols]. Elk van deze protocollen gebruikt anders beveiligingstokens van de IoT-apparaat met IoT Hub:

* AMQP: SASL zonder opmaak en op basis van Claims AMQP-beveiliging (`{policyName}@sas.root.{iothubName}` met IoT hub-niveau tokens; `{deviceId}` met tokens binnen het bereik van apparaat).
* MQTT: Verbinding maken met het pakket gebruikt `{deviceId}` als de `{ClientId}`, `{IoThubhostname}/{deviceId}` in de **gebruikersnaam** veld en een SAS-token in de **wachtwoord** veld.
* HTTP: Ongeldig token is in de aanvraagheader autorisatie.

ID-register IoT Hub kan worden gebruikt voor het configureren van per apparaat beveiligingsreferenties en toegangscontrole. Echter, als een IoT-oplossing heeft al een aanzienlijke investering een [aangepast apparaat identiteit register en/of verificatie schema][lnk-custom-auth], kunnen worden geïntegreerd in een bestaande infrastructuur met IoT Hub door het maken van een token service.

### <a name="x509-certificate-based-device-authentication"></a>Verificatie van de X.509-apparaten op basis van certificaten

Het gebruik van een [X.509-certificaat op basis van apparaten] [ lnk-use-x509] en de bijbehorende persoonlijke en openbare sleutelpaar staat aanvullende verificatie op de fysieke laag. De persoonlijke sleutel wordt veilig opgeslagen in het apparaat en is niet buiten het apparaat kan worden gedetecteerd. Het X.509-certificaat bevat informatie over het apparaat, zoals de apparaat-ID en de andere organisatie-gegevens. Een handtekening van het certificaat wordt gegenereerd met behulp van de persoonlijke sleutel.

Inrichting stroom op hoog niveau apparaat:

* Koppelen van een id naar een fysiek apparaat – apparaat-id en/of X.509-certificaat dat is gekoppeld aan het apparaat tijdens apparaat productie of bedrijf stellen.
* Overeenkomende identiteit invoer in IoT Hub-apparaat-id en de bijbehorende apparaatgegevens in het identiteitenregister van IoT Hub maken.
* Vingerafdruk van het x.509-certificaat veilig opslaan in het register van IoT Hub-identiteit.

### <a name="root-certificate-on-device"></a>Basiscertificaat op apparaat

Tijdens het maken van een beveiligde TLS-verbinding met IoT Hub, verifieert het IoT-apparaat een basiscertificaat dat deel uitmaakt van het apparaat-SDK met IoT-Hub. Voor de client C SDK het certificaat bevindt zich onder de map '\\c\\certificaten ' onder de hoofdmap van de opslagplaats. Deze basiscertificaten lange levensduur hebben, maar ze nog steeds mogelijk verlopen of worden ingetrokken. Als er geen manier om het certificaat op het apparaat bij te werken is, worden het apparaat niet worden vervolgens verbinding maken met de IoT-Hub (of een andere cloudservice). Een manier om bij te werken van het basiscertificaat zodra de IoT-apparaat daadwerkelijk is geïmplementeerd met verkleint dit risico.

## <a name="securing-the-connection"></a>De verbinding te beveiligen

Internet-verbinding tussen de IoT-apparaat en IoT Hub is beveiligd met de standaard Transport Layer Security (TLS). Azure IoT ondersteunt [TLS 1.2][lnk-tls12], TLS 1.1 en TLS 1.0 in deze volgorde. Ondersteuning voor TLS 1.0 wordt geleverd alleen voor achterwaartse compatibiliteit. Gebruik indien mogelijk, TLS 1.2 aangezien deze de meeste beveiliging biedt.

## <a name="securing-the-cloud"></a>Beveiliging van de cloud

Azure IoT-Hub kunt definitie van [toegangsbeheerbeleid] [ lnk-protocols] voor elke beveiligingssleutel. De volgende set machtigingen wordt gebruikt om toegang aan elk van de eindpunten van IoT-Hub te verlenen. Machtigingen beperken de toegang tot een IoT-Hub die op basis van functionaliteit.

* **RegistryRead**. Verleent leestoegang tot het identiteitsregister. Zie voor meer informatie [identiteitsregister][lnk-identity-registry].
* **RegistryReadWrite**. Verleent lezen en schrijven in het identiteitsregister. Zie voor meer informatie [identiteitsregister][lnk-identity-registry].
* **ServiceConnect**. Verleent toegang tot de cloud service gerichte communicatie en controle-eindpunten. Bijvoorbeeld, geeft het recht met back-end-cloud-services op apparaat-naar-cloud-berichten ontvangen berichten van cloud naar apparaat verzenden en het ophalen van de bijbehorende levering bevestigingen.
* **DeviceConnect**. Verleent toegang tot apparaat gerichte eindpunten. Bijvoorbeeld, geeft het recht voor apparaat-naar-cloud-berichten verzenden en ontvangen berichten van de cloud-naar-apparaat. Deze machtiging wordt gebruikt door apparaten.

Er zijn twee manieren verkrijgen **DeviceConnect** machtigingen met IoT Hub met [beveiligingstokens][lnk-sas-tokens]: met een id-sleutel van het apparaat of een gedeelde toegangssleutel. Bovendien is het belangrijk te weten dat toegankelijk is vanaf de apparaten van alle functionaliteit wordt blootgesteld aan het ontwerp voor eindpunten met voorvoegsel `/devices/{deviceId}`.

[Service-onderdelen kunnen alleen beveiligingstokens worden gegenereerd] [ lnk-service-tokens] gebruik gedeeld toegangsbeleid de juiste machtigingen te verlenen.

Azure IoT Hub en andere services die deel uitmaken van de oplossing wellicht kunnen beheer van gebruikers met de Azure Active Directory.

Gegevens ingenomen door Azure IoT Hub kan worden gebruikt door diverse services zoals Azure Stream Analytics en Azure blob-opslag. Deze services toestaan management toegang. Meer informatie over deze services en de beschikbare opties:

* [Azure Cosmos DB][lnk-cosmosdb]: een schaalbare, volledig geïndexeerd databaseservice voor semi-gestructureerde gegevens die u beheert de metagegevens voor de apparaten die u inricht, zoals kenmerken, configuratie en beveiligingseigenschappen. Azure Cosmos DB biedt hoge prestaties en hoge gegevensdoorvoer verwerking, schema networkdirect indexeren van gegevens en een uitgebreide SQL-QueryInterface.
* [Azure Stream Analytics][lnk-asa]: realtime stroom wordt verwerkt in de cloud waarmee u snel ontwikkelen en implementeren van een analyses voordelige oplossing voor realtime-inzichten verkrijgen van apparaten, sensoren, infrastructuur, onthullen en toepassingen. De gegevens van deze volledig beheerde service kunnen worden geschaald naar een volume en toch veel doorvoer, lage latentie en tolerantie kan bereiken.
* [Azure App Services][lnk-appservices]: een cloudplatform voor het bouwen van krachtige web- en mobiele apps die verbinding met gegevens overal; in de cloud of on-premises maken. Aansprekende mobiele apps bouwen voor iOS, Android en Windows. Geïntegreerd met uw Software als een Service (SaaS) en zakelijke toepassingen met out-of-the-box-connectiviteit naar tientallen cloudservices en bedrijfstoepassingen. De code in uw favoriete taal en IDE (.NET, Node.js, PHP, Python of Java) voor het bouwen van web-apps en API's sneller dan ooit.
* [Logic Apps][lnk-logicapps]: de Logic Apps-functie van Azure App Service helpt bij het integreren van uw IoT-oplossing aan uw bestaande line-of-business-systemen en workflow-processen automatiseren. Logic Apps kan ontwikkelaars werkstromen ontwerpen die na een trigger worden gestart en vervolgens een aantal stappen uitgevoerd: regels en acties die krachtige connectors gebruiken om te integreren met uw bedrijfsprocessen. Logic Apps biedt out-of-the-box-connectiviteit met een enorme ecosysteem van SaaS, op basis van cloud en on-premises toepassingen.
* [Azure-blobopslag][lnk-blob]: betrouwbare en voordelige cloud-opslag voor de gegevens die uw apparaten naar de cloud verzenden.

## <a name="conclusion"></a>Conclusie

Dit artikel bevat de mate van details over het ontwerpen en implementeren van een IoT-infrastructuur met behulp van Azure IoT overzicht van de uitvoering. Configureren van elk onderdeel om te worden beveiligd, is de sleutel in het beveiligen van de algemene IoT-infrastructuur. De ontwerpbeslissingen die beschikbaar zijn in Azure IoT een bepaalde mate van flexibiliteit en keuze; elke keuze hebben echter beveiligingsgebied. Het is raadzaam dat elk van deze keuzes worden geëvalueerd door een/kosten van het risico-evaluatie.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-security-tokens-from-service-components
[lnk-cosmosdb]: https://azure.microsoft.com/services/cosmos-db/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
