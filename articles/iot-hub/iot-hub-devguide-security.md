---
title: Azure IoT Hub beveiliging begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - toegang met IoT Hub voor apparaat-apps en apps van de back-end beheren. Bevat informatie over beveiligingstokens en ondersteuning voor X.509-certificaten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 45631e70-865b-4e06-bb1d-aae1175a52ba
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: e4fe5400ffcf4446392015aada031dd4dfbf238a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="control-access-to-iot-hub"></a>Toegang tot IoT Hub regelen

In dit artikel beschrijft de opties voor het beveiligen van uw IoT-hub. IoT Hub gebruikt *machtigingen* toegang verlenen tot elk IoT hub-eindpunt. Machtigingen beperken de toegang tot een IoT-hub die op basis van functionaliteit.

Dit artikel wordt beschreven:

* De andere machtigingen die u kunt verlenen aan een apparaat of back-end-app voor toegang tot uw IoT-hub.
* Het verificatieproces en de tokens die wordt gebruikt om machtigingen te controleren.
* Het bereik van referenties om toegang tot specifieke bronnen te beperken.
* IoT Hub-ondersteuning voor het X.509-certificaten.
* Aangepast apparaat verificatiemechanismen die gebruikmaken van bestaande apparaten identiteit registers of verificatieschema's.

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

U moet de juiste machtigingen voor toegang tot een van de IoT Hub-eindpunten hebben. Een apparaat moet bijvoorbeeld een token met beveiligingsreferenties samen met elk bericht dat naar IoT Hub verzendt.

## <a name="access-control-and-permissions"></a>Toegangsbeheer en machtigingen

U kunt verlenen [machtigingen](#iot-hub-permissions) in de volgende manieren:

* **IoT hub-niveau gedeelde toegangsbeleid**. Beleid voor gedeelde toegang kunnen verlenen tot een combinatie van [machtigingen](#iot-hub-permissions). U kunt beleid in definiëren de [Azure-portal][lnk-management-portal], of programmatisch met behulp van de [resourceprovider IoT Hub REST-API's][lnk-resource-provider-apis]. Een nieuwe iothub heeft de volgende standaardbeleidsregels:

  * **iothubowner**: beleid met alle machtigingen.
  * **service**: beleid met **ServiceConnect** machtiging.
  * **apparaat**: beleid met **DeviceConnect** machtiging.
  * **registryRead**: beleid met **RegistryRead** machtiging.
  * **registryReadWrite**: beleid met **RegistryRead** en RegistryWrite machtigingen.
  * **Beveiligingsreferenties per apparaat**. Elke IoT-Hub bevat een [identiteitsregister][lnk-identity-registry]. U kunt beveiligingsreferenties op die verlenen configureren voor elk apparaat in het identiteitenregister van deze **DeviceConnect** machtigingen binnen het bereik van de bijbehorende apparaat-eindpunten.

Bijvoorbeeld in een typische IoT-oplossing:

* De apparaat-management-component gebruikt de *registryReadWrite* beleid.
* De gebeurtenis processor-component gebruikt de *service* beleid.
* De component runtime-apparaat zakelijke logica gebruikt de *service* beleid.
* Afzonderlijke apparaten verbinding maken met behulp van referenties opgeslagen in het identiteitsregister van de IoT-hub.

> [!NOTE]
> Zie [machtigingen](#iot-hub-permissions) voor gedetailleerde informatie.

## <a name="authentication"></a>Authentication

Azure IoT Hub verleent toegang tot eindpunten door te controleren of een token tegen gedeeld toegangsbeleid en identiteit register beveiligingsreferenties.

Beveiligingsreferenties zoals symmetrische sleutels worden nooit verzonden via de kabel.

> [!NOTE]
> De resourceprovider van Azure IoT Hub is beveiligd via uw Azure-abonnement, omdat alle providers in het [Azure Resource Manager][lnk-azure-resource-manager].

Zie voor meer informatie over het maken en gebruiken van beveiligingstokens [IoT Hub beveiligingstokens][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protocol-specificaties

Elke ondersteunde protocollen, zoals MQTT AMQP en HTTP, transporten tokens op verschillende manieren.

Wanneer u MQTT, het pakket verbinding maken met de apparaat-id heeft als de ClientId, {iothubhostname} / {deviceId} in het veld Username en een SAS-token in het veld wachtwoord. {iothubhostname} moet de volledige CName van de IoT-hub (bijvoorbeeld contoso.azure-devices.net).

Wanneer u [AMQP][lnk-amqp], IoT-Hub ondersteunt [SASL zonder opmaak] [ lnk-sasl-plain] en [AMQP Claims-beveiliging op basis van-] [ lnk-cbs].

Als u AMQP claims-beveiliging op basis van-gebruikt, geeft de standaard geeft aan hoe deze tokens worden verzonden.

Voor SASL zonder opmaak, de **gebruikersnaam** kan zijn:

* `{policyName}@sas.root.{iothubName}`Als met IoT hub-niveau tokens.
* `{deviceId}@sas.{iothubname}`Als het gebruik van tokens binnen het bereik van apparaat.

In beide gevallen wordt het wachtwoordveld bevat het token, zoals beschreven in [IoT Hub beveiligingstokens][lnk-sas-tokens].

HTTP wordt de verificatie geïmplementeerd door een geldig token in de **autorisatie** aanvraag-header.

#### <a name="example"></a>Voorbeeld

Gebruikersnaam (DeviceId is hoofdlettergevoelig):`iothubname.azure-devices.net/DeviceId`

Wachtwoord (genereren van SAS-token met de [apparaat explorer] [ lnk-device-explorer] hulpprogramma):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> De [Azure IoT SDK's] [ lnk-sdks] automatisch genereren van tokens bij het verbinden met de service. In sommige gevallen worden de Azure IoT SDK's bieden geen ondersteuning alle protocollen of de verificatiemethoden.

### <a name="special-considerations-for-sasl-plain"></a>Speciale overwegingen voor SASL zonder opmaak

Als u SASL zonder opmaak met AMQP, kunt een client die verbinding met een IoT-hub-token van een enkele gebruiken voor elke TCP-verbinding. Wanneer het token is verlopen, wordt de TCP-verbinding verbroken van de service en een reconnect activeert. Dit gedrag, raakt terwijl niet problematisch voor een back-endserver voor apps, voor een apparaat-app voor de volgende redenen:

* Gateways verbinding meestal namens veel apparaten. Wanneer u SASL zonder opmaak, hebben ze een afzonderlijke TCP-verbinding voor elk apparaat verbinding maakt met een IoT-hub te maken. Dit scenario aanzienlijk verhoogd het verbruik van kracht en netwerkresources, en de latentie van elk apparaatverbinding.
* Resource beperkte apparaten nadelig worden beïnvloed door het toegenomen gebruik van bronnen te herstellen nadat elke token is verlopen.

## <a name="scope-iot-hub-level-credentials"></a>Bereik IoT hub-niveau referenties

U kunt het beveiligingsbeleid van IoT hub-niveau bereik tokens met een beperkte resource-URI te maken. Het eindpunt op apparaat-naar-cloud-berichten verzenden van een apparaat is bijvoorbeeld **/devices/ {deviceId} / berichten/gebeurtenissen**. U kunt ook een IoT hub-niveau gedeeld toegangsbeleid met **DeviceConnect** machtigingen voor het ondertekenen van een token waarvan resourceURI wordt **/devices/ {deviceId}**. Deze methode maakt u een token die alleen kan worden gebruikt voor het verzenden van berichten namens apparaat **deviceId**.

Deze methode is vergelijkbaar met de [Event Hubs uitgeversbeleid][lnk-event-hubs-publisher-policy], en kunt u aangepaste verificatiemethoden implementeren.

## <a name="security-tokens"></a>Beveiligingstokens

IoT Hub gebruikt de beveiligingstokens voor verificatie van apparaten en services om te voorkomen dat sleutels op de kabel verzonden. Bovendien zijn beveiligingstokens beperkt in geldigheid en het bereik. [Azure IoT SDK's] [ lnk-sdks] automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's hoeven te genereren en beveiligingstokens rechtstreeks gebruiken. Dergelijke scenario's omvatten:

* Het direct gebruik van het oppervlak MQTT, AMQP of HTTP.
* De implementatie van het patroon token service, zoals wordt beschreven in [aangepaste apparaatverificatie][lnk-custom-auth].

IoT Hub kan ook apparaten te verifiëren met IoT Hub met [X.509-certificaten][lnk-x509].

### <a name="security-token-structure"></a>Security token structuur

U beveiligingstokens verlenen tijd begrensd toegang tot apparaten en services met specifieke functionaliteit van IoT-Hub. Als u toestemming om te verbinden met IoT Hub, moeten apparaten en services de beveiligingstokens die zijn ondertekend met een gedeelde toegang of de symmetrische sleutel verzenden. Deze sleutels worden opgeslagen met een apparaat-id in het identiteitsregister.

Een token dat is ondertekend met een gedeelde toegang sleutel verleent toegang tot alle functies die zijn gekoppeld aan de gedeelde beleid toegangsmachtigingen. Een token die is ondertekend met een apparaat-id symmetrische sleutel alleen verleent de **DeviceConnect** machtiging voor het gekoppelde apparaat-id.

Het beveiligingstoken heeft de volgende indeling:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Hier volgen de verwachte waarden:

| Waarde | Beschrijving |
| --- | --- |
| {handtekening} |Een tekenreeks HMAC SHA256 handtekening van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijke**: de sleutel is gedecodeerd van base64 en gebruikt als sleutel voor het uitvoeren van de berekening HMAC SHA256. |
| {resourceURI} |De voorvoegsels van URI (door het segment) van de eindpunten die toegankelijk zijn met dit token, beginnen met de hostnaam van de IoT-hub (geen protocol). Bijvoorbeeld: `myHub.azure-devices.net/devices/device1` |
| {verstrijken} |UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {{URL-codering-resourceURI} |Lagere case URL-codering van de resource-URI van kleine letters |
| {policyName} |De naam van het beleid voor gedeelde toegang waarnaar dit token verwijst. Ontbrekende als verwijst het token naar apparaatregister referenties. |

**Opmerking van het voorvoegsel**: de URI-voorvoegsel wordt berekend door het segment en niet door teken. Bijvoorbeeld `/a/b` is een voorvoegsel voor `/a/b/c` maar niet voor `/a/bc`.

De volgende Node.js-fragment toont een aangeroepen functie **generateSasToken** die het token van de invoer berekent `resourceUri, signingKey, policyName, expiresInMins`. De volgende secties bevatten informatie over het initialiseren van de andere invoer voor de verschillende token gebruiksvoorbeelden.

```nodejs
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct autorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Als een vergelijking is de equivalente Python-code voor het genereren van een beveiligingstoken:

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Aangezien de geldigheid van het token is gevalideerd op de machines IoT Hub, is de afwijking van de klok van de computer die het token genereert moet minimaal.

### <a name="use-sas-tokens-in-a-device-app"></a>SAS-tokens gebruiken in een apparaat-app

Er zijn twee manieren verkrijgen **DeviceConnect** machtigingen met IoT Hub met beveiligingstokens: gebruik een [symmetrische apparaatsleutel uit het identiteitenregister](#use-a-symmetric-key-in-the-identity-registry), of gebruik een [gedeelde toegangssleutel](#use-a-shared-access-policy).

Houd er rekening mee dat alle functionaliteit die toegankelijk is vanaf apparaten wordt blootgesteld aan het ontwerp voor eindpunten met voorvoegsel `/devices/{deviceId}`.

> [!IMPORTANT]
> De enige manier om dat IoT Hub wordt geverifieerd voor een specifiek apparaat maakt gebruik van de symmetrische sleutel voor apparaat-id. In gevallen wanneer een beleid voor gedeelde toegang wordt gebruikt voor toegang tot de functionaliteit van apparaten, de oplossing moet rekening houden met het onderdeel voor het uitgeven van het beveiligingstoken als een vertrouwde subonderdeel.

Het apparaat gerichte eindpunten zijn (onafhankelijk van het protocol):

| Eindpunt | Functionaliteit |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Apparaat-naar-cloud-berichten verzenden. |
| `{iot hub host name}/devices/{deviceId}/devicebound` |Cloud-naar-apparaat-berichten ontvangen. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Een symmetrische sleutel in het identiteitenregister gebruiken

Wanneer u een apparaat-id symmetrische sleutel voor het genereren van een token, de policyName (`skn`) element van het token wordt weggelaten.

Een token dat is gemaakt voor toegang tot alle apparaatfunctionaliteit moet bijvoorbeeld de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* de handtekeningsleutel: een symmetrische sleutel voor de `{device id}` identiteit
* Er is geen naam van beleid
* elk gewenst moment verlopen.

Een voorbeeld met behulp van de voorgaande Node.js-functie is:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Het resultaat toegang tot alle functionaliteit voor device1 verleent, zou zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Het is mogelijk voor het genereren van een SAS-token met behulp van de .NET [apparaat explorer] [ lnk-device-explorer] hulpprogramma of de cross-platform, op basis van een knooppunt [iothub explorer] [ lnk-iothub-explorer]opdrachtregelprogramma.

### <a name="use-a-shared-access-policy"></a>Gebruik een beleid voor gedeelde toegang

Als u een token van een gedeeld toegangsbeleid maakt, stelt de `skn` veld op de naam van het beleid. Dit beleid moet verlenen de **DeviceConnect** machtiging.

De twee belangrijkste scenario's voor het gebruik van beleid voor gedeelde toegang te krijgen tot de functionaliteit voor apparaten zijn:

* [cloud protocol gateways][lnk-endpoints],
* [token services] [ lnk-custom-auth] gebruikt voor het implementeren van aangepaste verificatieschema's.

Omdat het beleid voor gedeelde toegang mogelijk toegang verlenen kunt tot verbinden als een apparaat, is het belangrijk dat u de juiste bron-URI bij het maken van beveiligingstokens. Deze instelling is vooral belangrijk voor token-services, die als bereik voor de token voor een specifiek apparaat met behulp van de bron-URI. Dit punt is minder relevant voor gateways protocol zoals ze zijn verkeer voor alle apparaten al op de volgende manier regelt.

Als u bijvoorbeeld een token service met de vooraf gemaakte gedeeld toegangsbeleid aangeroepen **apparaat** zou maken van een token met de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* de handtekeningsleutel: een van de sleutels van de `device` -beleid
* de naam van beleid: `device`,
* elk gewenst moment verlopen.

Een voorbeeld met behulp van de voorgaande Node.js-functie is:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat toegang tot alle functionaliteit voor device1 verleent, zou zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Een protocolgateway hetzelfde token kan gebruiken voor alle apparaten eenvoudigweg de bron-URI te `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Gebruik de beveiligingstokens van de onderdelen van service

Serviceonderdelen kunnen alleen beveiligingstokens die werken met beleid voor gedeelde toegang tot de juiste machtigingen verlenen zoals hierboven is gegenereerd.

Dit is de servicefuncties beschikbaar op de eindpunten zijn gemaakt:

| Eindpunt | Functionaliteit |
| --- | --- |
| `{iot hub host name}/devices` |Maken, bijwerken, ophalen en verwijderen van apparaat-id's. |
| `{iot hub host name}/messages/events` |Apparaat-naar-cloud-berichten ontvangen. |
| `{iot hub host name}/servicebound/feedback` |Feedback voor cloud-naar-apparaat-berichten ontvangen. |
| `{iot hub host name}/devicebound` |Cloud-naar-apparaat-berichten verzenden. |

Als u bijvoorbeeld een service genereren met behulp van de vooraf gemaakte gedeeld toegangsbeleid aangeroepen **registryRead** zou maken van een token met de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices`,
* de handtekeningsleutel: een van de sleutels van de `registryRead` -beleid
* de naam van beleid: `registryRead`,
* elk gewenst moment verlopen.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat toegang tot het lezen van alle apparaat-id's verlenen wilt, zou zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Ondersteunde X.509-certificaten

U kunt een X.509-certificaat gebruiken voor verificatie van een apparaat met IoT Hub. Certificaten zijn onder andere:

* **Een bestaand X.509-certificaat**. Een apparaat mogelijk al een X.509-certificaat dat is gekoppeld. Het apparaat kan dit certificaat gebruiken om te verifiëren met IoT Hub.
* **Een automatisch gegenereerd en zelfondertekend certificaat X-509**. Een fabrikant of interne-implementatie van deze certificaten genereren en de bijbehorende persoonlijke sleutel (en het certificaat) opslaan op het apparaat. U kunt hulpprogramma's gebruiken zoals [OpenSSL] [ lnk-openssl] en [Windows SelfSignedCertificate] [ lnk-selfsigned] hulpprogramma voor dit doel.
* **X.509-certificaat voor ondertekening van CA**. Als u wilt een apparaat te identificeren en deze kan worden geverifieerd met IoT Hub, kunt u een X.509-certificaat gegenereerd en ondertekend door een certificeringsinstantie (CA). IoT Hub controleert alleen of de geconfigureerde vingerafdruk komt overeen met de vingerafdruk van het weergegeven. IotHub biedt de certificaatketen niet valideren.

Een apparaat kan gebruikmaken van een X.509-certificaat of een beveiligingstoken voor verificatie, maar niet beide.

### <a name="register-an-x509-certificate-for-a-device"></a>Een X.509-certificaat voor een apparaat registreren

De [Azure IoT Service SDK voor C#] [ lnk-service-sdk] (versie 1.0.8+) biedt ondersteuning voor het registreren van een apparaat dat gebruikmaakt van een X.509-certificaat voor verificatie. Andere API's zoals importeren/exporteren van apparaten bieden ook ondersteuning voor X.509-certificaten.

### <a name="c-support"></a>C\# ondersteuning

De **RegistryManager** klasse biedt een programmatische manier kunt u een apparaat registreren. In het bijzonder de **AddDeviceAsync** en **UpdateDeviceAsync** methoden kunnen u registreren en bijwerken van een apparaat in de id-register van IoT Hub. Deze twee methoden maken gebruik van een **apparaat** exemplaar als invoer. De **apparaat** klasse bevat een **verificatie** eigenschap waarmee u primaire en secundaire x.509-certificaatvingerafdrukken opgeven. De vingerafdruk van het vertegenwoordigt een SHA-1-hash van het X.509-certificaat (met behulp van binaire codering DER opgeslagen). U hebt de optie voor het opgeven van de vingerafdruk van een primaire of een secundaire vingerafdruk of beide. Primaire en secundaire vingerafdrukken worden ondersteund voor het afhandelen van certificaat rollover-scenario's.

> [!NOTE]
> IoT Hub geen vereist of het hele X.509-certificaat, alleen de vingerafdruk van het opslaan.

Hier volgt een voorbeeld van C\# codefragment kunt u een apparaat met een X.509-certificaat registreren:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Gebruik van een X.509-certificaat tijdens runtime-bewerkingen

De [Azure IoT-device SDK voor .NET] [ lnk-client-sdk] (versie 1.0.11+) ondersteunt het gebruik van X.509-certificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **DeviceAuthenticationWithX509Certificate** ondersteunt het maken van **DeviceClient** exemplaren met behulp van een X.509-certificaat. Het X.509-certificaat moet zich in de indeling PFX (ook wel PKCS #12) dat de persoonlijke sleutel bevat.

Hier volgt een voorbeeld-codefragment:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Verificatie van aangepast apparaat

U kunt de IoT-Hub [identiteitsregister] [ lnk-identity-registry] beveiligingsreferenties per apparaat configureren en toegang beheren met [tokens][lnk-sas-tokens]. Als een IoT-oplossing al een aangepaste identiteit register en/of de verificatie-schema heeft, kunt u een *token service* deze infrastructuur integreren met IoT Hub. Op deze manier kunt u andere IoT-functies in uw oplossing.

Een service voor beveiligingstokens is een aangepaste cloudservice. Dit maakt gebruik van een IoT-Hub *gedeeld toegangsbeleid* met **DeviceConnect** machtigingen voor het maken *binnen het bereik van apparaat* tokens. Deze tokens inschakelen voor een apparaat verbinding maakt met uw IoT-hub.

![Stappen van de service voor beveiligingstokens patroon][img-tokenservice]

Hier volgen de belangrijkste stappen van de service voor beveiligingstokens patroon:

1. Maken van een Iothub gedeeld toegangsbeleid met **DeviceConnect** machtigingen voor uw IoT-hub. Kunt u dit beleid in de [Azure-portal] [ lnk-management-portal] of via een programma. De Tokenservice die gebruikmaakt van dit beleid voor het ondertekenen van de tokens die wordt gemaakt.
1. Wanneer een apparaat voor toegang tot uw IoT-hub, wordt het token van een ondertekende aanvraagt van uw service voor beveiligingstokens. Het apparaat kunt verifiëren met uw aangepaste identiteit register/verificatieschema om te bepalen van de identiteit van het apparaat dat de token service gebruikt voor het maken van het token.
1. De token-service retourneert een token. Het token wordt gemaakt met behulp van `/devices/{deviceId}` als `resourceURI`, met `deviceId` als het apparaat wordt geverifieerd. De token service gebruikt het beleid voor gedeelde toegang voor het maken van het token.
1. Het apparaat gebruikt het token rechtstreeks met de IoT-hub.

> [!NOTE]
> U kunt de .NET-klasse [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] of de Java-klasse [IotHubServiceSasToken] [ lnk-java-sas] voor het maken van een token in uw service voor beveiligingstokens.

De token-service kan het verlopen van het token naar wens ingesteld. Wanneer het token is verlopen, servers de iothub de apparaatverbinding. Het apparaat moet vervolgens een nieuw token aanvragen van de service voor beveiligingstokens. Een korte verlooptijd verhoogt de belasting van het apparaat en de service voor beveiligingstokens.

Voor een apparaat verbinding maken met uw hub, moet u nog steeds deze toevoegen aan de id-register van IoT Hub, zelfs als het apparaat gebruikmaakt van een token en niet de apparaatsleutel van een verbinding maken. Daarom kunt u blijven gebruiken per apparaat toegangsbeheer door in- of uitschakelen van apparaat-id's in de [identiteitsregister][lnk-identity-registry]. Deze benadering vermindert het risico van het gebruik van tokens met lange verstrijken tijden.

### <a name="comparison-with-a-custom-gateway"></a>Vergelijking met een aangepaste gateway

Het patroon van de service voor beveiligingstokens is de aanbevolen manier voor het implementeren van een aangepaste identiteit register/verificatieschema met IoT Hub. Dit patroon wordt aanbevolen omdat IoT Hub blijft het grootste gedeelte van het verkeer van de oplossing. Echter, als de aangepaste verificatieschema dus met het protocol verweven is, hebt u mogelijk nodig een *aangepaste gateway* alle verkeer verwerken. Het gebruik van een voorbeeld van een dergelijk scenario[Transport Layer Security (TLS) en vooraf gedeelde sleutels (PSKs)][lnk-tls-psk]. Zie voor meer informatie de [protocolgateway] [ lnk-protocols] onderwerp.

## <a name="reference-topics"></a>De onderwerpen waarnaar wordt verwezen:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het beheren van toegang tot uw IoT-hub.

## <a name="iot-hub-permissions"></a>Machtigingen van de IoT-Hub

De volgende tabel bevat de machtigingen die u kunt toegang tot uw IoT-hub.

| Machtiging | Opmerkingen |
| --- | --- |
| **RegistryRead** |Verleent leestoegang tot het identiteitsregister. Zie voor meer informatie [identiteitsregister][lnk-identity-registry]. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **RegistryReadWrite** |Verleent lezen en schrijven in het identiteitsregister. Zie voor meer informatie [identiteitsregister][lnk-identity-registry]. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **ServiceConnect** |Verleent toegang tot de cloud service gerichte communicatie en controle-eindpunten. <br/>Geeft het recht op apparaat-naar-cloud-berichten ontvangen berichten van cloud naar apparaat verzenden en het ophalen van de bijbehorende levering bevestigingen. <br/>Verleent toestemming voor het ophalen van de levering van bevestigingen voor het bestand uploadt. <br/>Verleent toestemming voor toegang tot apparaat horende bijwerken tags en gewenste eigenschappen, gemelde eigenschappen ophalen en uitvoeren van query's. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **DeviceConnect** |Verleent toegang tot apparaat gerichte eindpunten. <br/>Verleent toestemming voor apparaat-naar-cloud-berichten verzenden en ontvangen berichten van de cloud-naar-apparaat. <br/>Verleent toestemming voor het uploaden van het bestand uitvoeren vanaf een apparaat. <br/>Geeft het recht apparaat twin gewenst eigenschap meldingen ontvangen en bijwerken van apparaat twin gerapporteerd eigenschappen. <br/>Verleent toestemming voor het uitvoeren van bestand uploadt. <br/>Deze machtiging wordt gebruikt door apparaten. |

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Er zijn andere onderwerpen waarnaar wordt verwezen in de IoT Hub developer guide:

* [IoT-hubeindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* [Bandbreedtebeperking en quota's] [ lnk-quotas] beschrijving van de quota en beperking gedrag die betrekking hebben op de service IoT Hub.
* [Azure IoT-SDKs voor apparaat en de service] [ lnk-sdks] geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apps voor het apparaat en de service die communiceren met IoT Hub-taal.
* [IoT Hub-querytaal] [ lnk-query] beschrijft de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat horende en taken.
* [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe toegangsbeheer IoT Hub, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Horende apparaten gebruiken om de status en configuraties te synchroniseren][lnk-devguide-device-twins]
* [Een directe methode aangeroepen voor een apparaat][lnk-devguide-directmethods]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende zelfstudies met IoT Hub:

* [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]
* [Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub][lnk-c2d-tutorial]
* [IoT Hub apparaat-naar-cloud-berichten verwerken][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth._iot_hub_service_sas_token
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
