---
title: Meer uitleg over Azure IoT Hub-beveiliging | Microsoft Docs
description: Ontwikkelaars begeleiden - toegang tot IoT Hub voor apps op apparaten en back-end-apps beheren. Bevat informatie over de beveiligingstokens en ondersteuning voor X.509-certificaten.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 3b49d568b1ca19a99359314bb181712f56ca5615
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54201054"
---
# <a name="control-access-to-iot-hub"></a>Toegang tot IoT Hub regelen

In dit artikel beschrijft de opties voor het beveiligen van uw IoT-hub. IoT Hub gebruikt *machtigingen* toegang verlenen tot elk IoT hub-eindpunt. Machtigingen beperken de toegang tot een IoT-hub op basis van functionaliteit.

In dit artikel worden geïntroduceerd:

* De andere machtigingen die u aan een apparaat of de back-end-app verlenen kunt voor toegang tot uw IoT-hub.
* Het verificatieproces en de tokens die wordt gebruikt om machtigingen te controleren.
* Klik hier voor meer informatie over het bereik van referenties om toegang tot bepaalde resources te beperken.
* IoT Hub-ondersteuning voor X.509-certificaten.
* Aangepaste apparaat-verificatiemechanismen die gebruikmaken van bestaande identiteit apparaatregisters of verificatiemethoden.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

U moet de juiste machtigingen voor toegang tot een van de IoT Hub-eindpunten hebben. Een apparaat moet bijvoorbeeld een token met beveiligingsreferenties samen met elk bericht dat wordt verzonden naar IoT Hub.

## <a name="access-control-and-permissions"></a>Access control en machtigingen

U kunt verlenen [machtigingen](#iot-hub-permissions) in de volgende manieren:

* **Niveau van IoT hub gedeelde toegangsbeleid**. Beleid voor gedeelde toegang kunnen verlenen tot een combinatie van [machtigingen](#iot-hub-permissions). Definieert u beleid in de [Azure-portal](https://portal.azure.com), via een programma met behulp van de [REST API's voor Resource van de IoT Hub](/rest/api/iothub/iothubresource), of met behulp van de [az iot hub-beleid](/cli/azure/iot/hub/policy?view=azure-cli-latest) CLI. Een nieuwe IoT-hub heeft de volgende standaard-beleid:
  
  | Beleid voor gedeelde toegang | Machtigingen |
  | -------------------- | ----------- |
  | iothubowner | Alle machtigingen |
  | service | **ServiceConnect** machtigingen |
  | apparaat | **DeviceConnect** machtigingen |
  | registryRead | **RegistryRead** machtigingen |
  | registryReadWrite | **RegistryRead** en **RegistryWrite** machtigingen |

* **Beveiligingsreferenties per apparaat**. Elke IoT-Hub bevat een [id-register](iot-hub-devguide-identity-registry.md) voor elk apparaat in deze id-register, kunt u beveiligingsreferenties op die verlenen **DeviceConnect** machtigingen die zijn afgestemd op het desbetreffende apparaat eindpunten.

Bijvoorbeeld, in een typische IoT-oplossing:

* De beheercomponent apparaat maakt gebruik van de *registryReadWrite* beleid.
* De event processor-component gebruikt de *service* beleid.
* De runtime-apparaat zakelijke logica-component gebruikt de *service* beleid.
* Afzonderlijke apparaten verbinding maken met behulp van referenties die zijn opgeslagen in het id-register van de IoT-hub.

> [!NOTE]
> Zie [machtigingen](#iot-hub-permissions) voor gedetailleerde informatie.

## <a name="authentication"></a>Verificatie

Azure IoT Hub verleent toegang tot eindpunten door te controleren of een token met het beleid voor gedeelde toegang en de beveiligingsreferenties voor id-register.

Beveiligingsreferenties, zoals symmetrische sleutels worden nooit via de kabel verzonden.

> [!NOTE]
> De resourceprovider van Azure IoT Hub wordt beveiligd via uw Azure-abonnement, omdat alle providers in het [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

Zie voor meer informatie over het maken en gebruiken van beveiligingstokens [beveiligingstokens van IoT-Hub](iot-hub-devguide-security.md#security-tokens).

### <a name="protocol-specifics"></a>Protocol-specificaties

Elke ondersteunde protocol, zoals het MQTT-, AMQP- en HTTPS, worden tokens op verschillende manieren filtermodule getransporteerd.

Bij het gebruik van MQTT, het pakket verbinding maken met de apparaat-id als de ClientId, heeft `{iothubhostname}/{deviceId}` in het veld gebruikersnaam en een SAS-token in het veld wachtwoord. `{iothubhostname}` moet u de volledige CName van de IoT-hub (bijvoorbeeld contoso.azure devices.net).

Bij het gebruik van [AMQP](https://www.amqp.org/), IoT Hub worden ondersteund [SASL zonder opmaak](http://tools.ietf.org/html/rfc4616) en [AMQP Claims-beveiliging op basis van-](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc).

Als u AMQP claims op basis van-beveiliging gebruikt, is de standaard Hiermee geeft u het verzenden van deze tokens.

Voor SASL zonder opmaak, de **gebruikersnaam** kan zijn:

* `{policyName}@sas.root.{iothubName}` Als u met behulp van IoT hub op serverniveau tokens.
* `{deviceId}@sas.{iothubname}` Als het gebruik van tokens binnen het bereik van apparaat.

In beide gevallen wordt het wachtwoordveld bevat het token, zoals beschreven in [beveiligingstokens van IoT-Hub](iot-hub-devguide-security.md#security-tokens).

HTTPS wordt de verificatie geïmplementeerd door te nemen van een geldig token in de **autorisatie** aanvraagheader.

#### <a name="example"></a>Voorbeeld

Gebruikersnaam (apparaat-id is hoofdlettergevoelig): `iothubname.azure-devices.net/DeviceId`

Wachtwoord (u kunt genereren van een SAS-token met de [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) hulpprogramma, de CLI-opdracht voor gegevensextensies [az iot hub genereren-sas-token](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token), of de [Azure IoT-Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)):

`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> De [Azure IoT SDK's](iot-hub-devguide-sdks.md) automatisch genereren van tokens bij het verbinden met de service. In sommige gevallen kan ondersteunen de Azure IoT SDK's niet alle protocollen of alle verificatiemethoden.

### <a name="special-considerations-for-sasl-plain"></a>Speciale overwegingen voor SASL zonder opmaak

Wanneer u SASL zonder opmaak met AMQP, kan een client die verbinding maken met een IoT-hub een enkele token gebruiken voor elke TCP-verbinding. Wanneer het token is verlopen, wordt de TCP-verbinding verbreekt u de service en wordt een nieuwe verbindingen wordt geactiveerd. Dit gedrag, is terwijl niet geschikt is voor een back-end-app beschadigen voor een apparaat-app voor de volgende redenen:

* Gateways maken doorgaans verbinding namens een groot aantal apparaten. Wanneer u SASL zonder opmaak, hebben ze geen afzonderlijke TCP-verbinding voor elk apparaat verbinding maakt met een IoT-hub te maken. In dit scenario aanzienlijk verhoogt het verbruik van power- en netwerkresources, en verhoogt de latentie van elke apparaatverbinding.

* Apparaten met beperkte capaciteit nadelig worden beïnvloed door het toegenomen gebruik van resources te herstellen na elke verlopen van het token.

## <a name="scope-iot-hub-level-credentials"></a>Het bereik van IoT hub-niveau van referenties

IoT hub-niveau beveiligingsbeleid kunt u beperken door het maken van tokens met een beperkte resource-URI. Het eindpunt voor het verzenden van apparaat-naar-cloud-berichten vanaf een apparaat is bijvoorbeeld **/devices/ {apparaat-id} / berichten/gebeurtenissen**. U kunt ook een IoT hub-niveau gedeeld toegangsbeleid met **DeviceConnect** machtigingen voor het ondertekenen van een token waarvan resourceURI is **/devices/ {deviceId}**. Deze methode maakt u een token dat kan alleen worden gebruikt voor het verzenden van berichten namens apparaat **deviceId**.

Dit mechanisme is vergelijkbaar met de [Event Hubs uitgeversbeleid](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab), en kunt u voor het implementeren van aangepaste verificatiemethoden.

## <a name="security-tokens"></a>Beveiligingstokens

IoT Hub maakt gebruik van beveiligingstokens voor de verificatie van apparaten en services om te voorkomen dat sleutels op de kabel verzonden. Bovendien zijn beveiligingstokens beperkt in de geldigheid van de tijd en het bereik. [Azure IoT SDK's](iot-hub-devguide-sdks.md) automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's hoeven te genereren en beveiligingstokens rechtstreeks gebruiken. Dergelijke scenario's omvatten:

* Het direct gebruik van de oppervlakken MQTT-, AMQP- of HTTPS.

* De implementatie van de service voor beveiligingstokens patroon, zoals uitgelegd in [aangepaste apparaatverificatie](iot-hub-devguide-security.md#custom-device-and-module-authentication).

IoT Hub ook kunt u apparaten om te verifiëren met IoT Hub met behulp van [X.509-certificaten](iot-hub-devguide-security.md#supported-x509-certificates).

### <a name="security-token-structure"></a>Token beveiligingsstructuur

U beveiligingstokens gebruiken voor toegang tot tijd worden gebonden aan apparaten en services in de specifieke functionaliteit van IoT Hub. Als u toestemming om te verbinden met IoT Hub, moeten apparaten en services beveiligingstokens die zijn ondertekend met een gedeelde toegang of de symmetrische sleutel verzenden. Deze sleutels worden opgeslagen met een apparaat-id in het id-register.

Een token is ondertekend met een gedeelde toegang sleutel verleent toegang tot alle functionaliteit die is gekoppeld met de machtigingen van het beleid voor gedeelde toegang. Een token is ondertekend met een apparaat-id symmetrische sleutel alleen verleent de **DeviceConnect** machtiging voor de identiteit van de gekoppelde op een apparaat.

Het beveiligingstoken heeft de volgende indeling:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Hier volgen de verwachte waarden:

| Waarde | Description |
| --- | --- |
| {handtekening} |Een tekenreeks van de HMAC-SHA256 handtekening van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijke**: De sleutel is gedecodeerd op basis van base64 en gebruikt als sleutel voor het uitvoeren van de HMAC-SHA256-berekening. |
| {resourceURI} |URI-voorvoegsel (per segment) van de eindpunten die toegankelijk zijn met dit token, beginnend met de hostnaam van de IoT-hub (Er is geen protocol). Bijvoorbeeld: `myHub.azure-devices.net/devices/device1` |
| {expiry} |UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {URL-encoded-resourceURI} |Lagere aanvraag-URL-codering van de resource-URI van de kleine letters |
| {policyName} |De naam van het beleid voor gedeelde toegang waarvoor dit token verwijst. Ontbrekende als verwijst het token naar apparaatregister referenties. |

**Houd er rekening mee op het voorvoegsel van**: Het URI-voorvoegsel wordt berekend per segment en niet door het teken. Bijvoorbeeld `/a/b` is een voorvoegsel voor `/a/b/c` , maar niet voor `/a/bc`.

Het volgende Node.js-fragment toont een functie met de naam **generateSasToken** die het token van de invoer berekent `resourceUri, signingKey, policyName, expiresInMins`. De volgende secties bevatten informatie over het initialiseren van de verschillende soorten invoer voor de verschillende token use cases.

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

Als een vergelijking: de equivalente Python-code voor het genereren van een beveiligingstoken is:

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

De functionaliteit in C# voor het genereren van een beveiligingstoken is:

```csharp
using System;
using System.Globalization;
using System.Net;
using System.Net.Http;
using System.Security.Cryptography;
using System.Text;

public static string generateSasToken(string resourceUri, string key, string policyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = WebUtility.UrlEncode(resourceUri) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}", WebUtility.UrlEncode(resourceUri), WebUtility.UrlEncode(signature), expiry);

    if (!String.IsNullOrEmpty(policyName))
    {
        token += "&skn=" + policyName;
    }

    return token;
}

```


> [!NOTE]
> Aangezien de geldigheid van het token is gevalideerd op IoT Hub-machines, moet de afwijking van de klok van de computer waarmee het token wordt gegenereerd minimaal zijn.

### <a name="use-sas-tokens-in-a-device-app"></a>SAS-tokens gebruiken in een apparaat-app

Er zijn twee manieren om op te halen **DeviceConnect** machtigingen met IoT Hub met beveiligingstokens: gebruik een [symmetrische apparaatsleutel vanuit het identiteitsregister](#use-a-symmetric-key-in-the-identity-registry), of gebruik een [gedeelde toegangssleutel](#use-a-shared-access-policy).

Houd er rekening mee dat alle functionaliteit die toegankelijk is vanaf apparaten wordt weergegeven op de eindpunten met het voorvoegsel standaard `/devices/{deviceId}`.

> [!IMPORTANT]
> De enige manier dat een specifiek apparaat wordt geverifieerd door IoT Hub maakt gebruik van de symmetrische sleutel voor apparaat-id. In gevallen wanneer een beleid voor gedeelde toegang wordt gebruikt voor toegang tot de functionaliteit van apparaten, de oplossing moet rekening houden met het uitgeven van het beveiligingstoken als een vertrouwde subonderdeel onderdeel.

Het apparaat gerichte eindpunten zijn (ongeacht het protocol):

| Eindpunt | Functionaliteit |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Apparaat-naar-cloud-berichten worden verzonden. |
| `{iot hub host name}/devices/{deviceId}/messages/devicebound` |Cloud-naar-apparaat-berichten ontvangen. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Gebruik van een symmetrische sleutel in het id-register

Bij het gebruik van een apparaat-id symmetrische sleutel voor het genereren van een token, de naam (`skn`)-element van het token wordt weggelaten.

Een token dat is gemaakt voor toegang tot alle apparaatfunctionaliteit van het moet bijvoorbeeld de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* ondertekeningssleutel: een symmetrische sleutel voor de `{device id}` identiteit
* geen beleidsnaam
* elk gewenst moment verlopen.

Een voorbeeld met behulp van de voorgaande Node.js-functie is:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Het resultaat, waarmee de toegang tot alle functionaliteit voor device1 worden verleend, zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Het is mogelijk voor het genereren van een SAS-token met de [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) hulpprogramma, de CLI-opdracht voor gegevensextensies [az iot hub genereren-sas-token](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token), of de [Azure IoT-Tools voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

### <a name="use-a-shared-access-policy"></a>Een beleid voor gedeelde toegang gebruiken

Als u een token van een beleid voor gedeelde toegang, de `skn` veld aan de naam van het beleid. Dit beleid moet verlenen de **DeviceConnect** machtiging.

De twee belangrijkste scenario's voor het gebruik van beleid voor gedeelde toegang tot functionaliteit voor apparaten zijn:

* [cloud protocolgateways](iot-hub-devguide-endpoints.md),
* [token services](iot-hub-devguide-security.md#custom-device-and-module-authentication) gebruikt voor het implementeren van aangepaste verificatiemethoden.

Omdat het beleid voor gedeelde toegang mogelijk toegang verlenen kan tot verbinding maken als een apparaat, is het belangrijk dat u de juiste resource-URI wordt gebruikt bij het maken van beveiligingstokens. Deze instelling is vooral belangrijk voor token-services, die als bereik voor het token met een bepaald apparaat met behulp van de resource-URI. Dit punt is minder relevant voor protocolgateways zoals ze zijn verkeer voor alle apparaten al op de volgende manier regelt.

Als u bijvoorbeeld een token service met behulp van de vooraf gemaakte gedeeld toegangsbeleid met de naam **apparaat** zou maken van een token met de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* ondertekeningssleutel: een van de sleutels van de `device` -beleid
* Beleidsnaam: `device`,
* elk gewenst moment verlopen.

Een voorbeeld met behulp van de voorgaande Node.js-functie is:

```nodejs
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat, waarmee de toegang tot alle functionaliteit voor device1 worden verleend, zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Een protocolgateway kunt gebruiken om hetzelfde token voor alle apparaten eenvoudig in te stellen de resource-URI op `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Beveiligingstokens van de onderdelen van service gebruiken

Serviceonderdelen kunnen alleen beveiligingstokens met behulp van de juiste machtigingen verlenen zoals hierboven is beleid voor gedeelde toegang genereren.

Hier vindt u de servicefuncties beschikbaar gesteld op de eindpunten:

| Eindpunt | Functionaliteit |
| --- | --- |
| `{iot hub host name}/devices` |Maken, bijwerken, ophalen en verwijderen van apparaat-id's. |
| `{iot hub host name}/messages/events` |Apparaat-naar-cloud-berichten ontvangen. |
| `{iot hub host name}/servicebound/feedback` |Feedback voor cloud-naar-apparaat-berichten ontvangen. |
| `{iot hub host name}/devicebound` |Cloud-naar-apparaat-berichten worden verzonden. |

Als u bijvoorbeeld een service genereren met behulp van de vooraf gemaakte gedeeld toegangsbeleid met de naam **registryRead** zou maken van een token met de volgende parameters:

* resource-URI: `{IoT hub name}.azure-devices.net/devices`,
* ondertekeningssleutel: een van de sleutels van de `registryRead` -beleid
* Beleidsnaam: `registryRead`,
* elk gewenst moment verlopen.

```nodejs
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'registryRead';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat toegang tot het lezen van alle apparaat-id's verleent zou, zijn:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Ondersteunde X.509-certificaten

U kunt een X.509-certificaat gebruiken voor verificatie van een apparaat met IoT Hub door de vingerafdruk van een certificaat of een certificeringsinstantie (CA) uploaden naar Azure IoT Hub. Verificatie met behulp van alleen vingerafdrukken van het certificaat wordt gecontroleerd of de vingerafdruk van het voorgestelde overeenkomt met de geconfigureerde vingerafdruk. Verificatie met behulp van de certificeringsinstantie valideert de certificaatketen. 

Ondersteunde certificaten zijn onder andere:

* **Een bestaand X.509-certificaat**. Een apparaat mogelijk al een X.509-certificaat dat is gekoppeld. Het apparaat kan dit certificaat gebruiken om te verifiëren met IoT Hub. Als u werkt met de vingerafdruk of CA-verificatie. 

* **CA-ondertekend x.509-certificaat**. Als u wilt een apparaat identificeren en verifiëren met IoT Hub, kunt u een X.509-certificaat gegenereerd en ondertekend door een certificeringsinstantie (CA). Als u werkt met de vingerafdruk of CA-verificatie.

* **Een zelf gegenereerd en zelfondertekend certificaat van de x-509**. Een fabrikant van apparaat- of interne implementatie van deze certificaten genereren en opslaan van de bijbehorende persoonlijke sleutel (en het certificaat) op het apparaat. U kunt hulpprogramma's zoals [OpenSSL](https://www.openssl.org/) en [Windows SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) hulpprogramma voor dit doel. Werkt alleen met de vingerafdruk van verificatie. 

Een apparaat kan gebruiken een X.509-certificaat of een beveiligingstoken voor verificatie, maar niet beide.

Zie voor meer informatie over verificatie met behulp van de certificeringsinstantie, [Apparaatverificatie met behulp van x.509-CA-certificaten](iot-hub-x509ca-overview.md).

### <a name="register-an-x509-certificate-for-a-device"></a>Een X.509-certificaat voor een apparaat registreren

De [Azure IoT Service SDK voor C#](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service) (versie 1.0.8+) biedt ondersteuning voor het registreren van een apparaat dat gebruikmaakt van een X.509-certificaat voor verificatie. Andere API's zoals importeren/exporteren van apparaten ondersteunen ook X.509-certificaten.

U kunt ook de CLI-opdracht voor gegevensextensies [az iot hub device-identity](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) x.509-certificaten voor apparaten configureren.

### <a name="c-support"></a>C\# ondersteuning

De **RegistryManager** klasse biedt een programmatische manier om een apparaat te registreren. In het bijzonder de **AddDeviceAsync** en **UpdateDeviceAsync** methoden kunnen u zich registreren en bijwerken van een apparaat in de id-register van IoT Hub. Deze twee methoden maken gebruik van een **apparaat** exemplaar als invoer. De **apparaat** klasse bevat een **verificatie** eigenschap waarmee u om op te geven van de primaire en secundaire x.509-certificaatvingerafdrukken. De vingerafdruk van het vertegenwoordigt een SHA256-hash van het X.509-certificaat (opgeslagen met binaire DER-codering). U hebt de optie voor het opgeven van de vingerafdruk van een primaire of secundaire vingerafdruk van een of beide. Primaire en secundaire vingerafdrukken worden ondersteund voor het afhandelen van certificaat rollover-scenario's.

Hier volgt een voorbeeld van C\# codefragment voor het registreren van een apparaat met de vingerafdruk van een X.509-certificaat:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "B4172AB44C28F3B9E117648C6F7294978A00CDCBA34A46A1B8588B3F7D82C4F1"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Een X.509-certificaat gebruiken tijdens de runtime-bewerkingen

De [Azure IoT device-SDK voor .NET](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device) (versie 1.0.11+) ondersteunt het gebruik van X.509-certificaten.

### <a name="c-support"></a>C\# ondersteuning

De klasse **DeviceAuthenticationWithX509Certificate** ondersteunt het maken van **DeviceClient** machineschaalinstanties met een X.509-certificaat. Het X.509-certificaat moet zich in de PFX (ook wel PKCS #12)-indeling die de persoonlijke sleutel bevat.

Hier volgt een voorbeeld-codefragment:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-and-module-authentication"></a>Aangepaste module-apparaat en -verificatie

U kunt de IoT-Hub [id-register](iot-hub-devguide-identity-registry.md) per-apparaat/module beveiligingsreferenties configureren en beheren met [tokens](iot-hub-devguide-security.md#security-tokens). Als een IoT-oplossing al een aangepaste id-register en/of verificatie-schema heeft, houd rekening met het maken van een *token service* deze infrastructuur integreren met IoT Hub. U kunt op deze manier kunnen andere IoT-functies gebruiken in uw oplossing.

Een service voor beveiligingstokens is een aangepaste cloud-service. Het maakt gebruik van een IoT-Hub *gedeeld toegangsbeleid* met **DeviceConnect** of **ModuleConnect** machtigingen voor het maken *binnen het bereik van apparaat* of *binnen het bereik van module* tokens. Deze tokens kunnen een apparaat en de module verbinding maken met uw IoT-hub.

![Stappen van de service voor beveiligingstokens-patroon](./media/iot-hub-devguide-security/tokenservice.png)

Hier volgen de belangrijkste stappen van de service voor beveiligingstokens-patroon:

1. Maken van een IoT-Hub gedeelde toegangsbeleid met **DeviceConnect** of **ModuleConnect** machtigingen voor uw IoT-hub. Kunt u dit beleid in de [Azure-portal](https://portal.azure.com) of via een programma. De service voor beveiligingstokens gebruikmaakt van dit beleid voor het ondertekenen van de tokens die wordt gemaakt.

2. Wanneer een apparaat/module nodig tot uw IoT-hub heeft, vraagt het een ondertekende tokens van uw service voor beveiligingstokens. Het apparaat kan worden geverifieerd met het schema voor uw aangepaste id-register/verificatie om te bepalen van de identiteit van de apparaatmodule/die de token service gebruikt om het token te maken.

3. De token service retourneert een token. Het token wordt gemaakt met behulp van `/devices/{deviceId}` of `/devices/{deviceId}/module/{moduleId}` als `resourceURI`, met `deviceId` als het apparaat wordt geverifieerd of `moduleId` als de module die wordt geverifieerd. De token service gebruikt het gedeelde toegangsbeleid te maken van het token.

4. De apparaatmodule gebruikt het token rechtstreeks met de IoT-hub.

> [!NOTE]
> U kunt de .NET-klasse [SharedAccessSignatureBuilder](https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx) of de Java-klasse [IotHubServiceSasToken](/java/api/com.microsoft.azure.sdk.iot.service.auth.iothubservicesastoken) voor het maken van een token in uw service voor beveiligingstokens.

De service voor beveiligingstokens kunt het verlopen van het token naar wens instellen. Wanneer het token is verlopen, verbreekt de IoT-hub de apparaatmodule /-verbinding. De apparaatmodule moet vervolgens een nieuw token aanvragen van de service voor beveiligingstokens. Een korte verlooptijd verhoogt de belasting van zowel de apparaatmodule als de service voor beveiligingstokens.

Voor een apparaat/module verbinding maken met uw hub, moet u nog steeds deze toevoegen aan de id-register van IoT Hub, zelfs als er een token en niet een sleutel wordt gebruikt om verbinding te maken. Daarom kunt u blijven gebruikmaken van per-apparaat/per-module toegangsbeheer met in- of uitschakelen van de apparaatmodule /-id's in de [id-register](iot-hub-devguide-identity-registry.md). Deze aanpak vermindert de risico's van het gebruik van tokens met minder lang verlopen.

### <a name="comparison-with-a-custom-gateway"></a>Vergelijking met een aangepaste gateway

Het patroon van de service voor beveiligingstokens is de aanbevolen manier voor het implementeren van een aangepaste id-register/verificatieschema met IoT Hub. Dit patroon wordt aanbevolen omdat de IoT Hub voor het afhandelen van het meeste verkeer oplossing blijft. Echter, als de aangepaste verificatiemethode dus met het protocol verweven wordt, u hebt mogelijk een *aangepaste gateway* al het verkeer verwerken. Een voorbeeld van een dergelijk scenario is het gebruik [Transport Layer Security (TLS) en vooraf-gedeelde sleutels (PSKs)](https://tools.ietf.org/html/rfc4279). Zie voor meer informatie de [protocolgateway](iot-hub-protocol-gateway.md) artikel.

## <a name="reference-topics"></a>Onderwerpen met naslaginformatie:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het beheren van toegang tot uw IoT-hub.

## <a name="iot-hub-permissions"></a>IoT Hub machtigingen

De volgende tabel bevat de machtigingen die u gebruiken kunt voor het beheren van toegang tot uw IoT-hub.

| Machtiging | Opmerkingen |
| --- | --- |
| **registryRead** |Verleent leestoegang tot het id-register. Zie voor meer informatie, [id-register](iot-hub-devguide-identity-registry.md). <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **registryReadWrite** |Verleent lezen en schrijven naar het id-register. Zie voor meer informatie, [id-register](iot-hub-devguide-identity-registry.md). <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **ServiceConnect** |Verleent toegang tot de cloud service gerichte communicatie en -eindpunten worden gecontroleerd. <br/>Een machtiging verleend voor apparaat-naar-cloud-berichten ontvangen, cloud-naar-apparaat-berichten verzenden en ophalen van de bijbehorende delivery-bevestigingen. <br/>Verleent toestemming om op te halen van de levering van bevestigingen voor het bestand wordt geüpload. <br/>Geeft het recht op toegang dubbels bijwerken labels en gewenste eigenschappen, gerapporteerde eigenschappen ophalen en query's uitvoeren. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **DeviceConnect** |Verleent toegang tot apparaat gerichte eindpunten. <br/>Een machtiging verleend voor apparaat-naar-cloud-berichten verzenden en ontvangen van berichten van cloud-naar-apparaat. <br/>Verleent machtiging voor het uploaden van bestanden vanaf een apparaat uitvoeren. <br/>Verleent toestemming voor het apparaat apparaatdubbel-gewenste eigenschap meldingen ontvangen en bijwerken van de apparaatdubbel gerapporteerde eigenschappen. <br/>Verleent machtiging voor het uitvoeren van bestand wordt geüpload. <br/>Deze machtiging wordt gebruikt door apparaten. |

## <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* [Bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) beschrijving van de quota en beperkingen van problemen die betrekking hebben op de IoT Hub-service.

* [Azure IoT-apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* [IoT Hub-querytaal](iot-hub-devguide-query-language.md) beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u voor het beheren van toegang tot IoT Hub, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Apparaatdubbels gebruiken voor het synchroniseren van de status en -configuraties](iot-hub-devguide-device-twins.md)
* [Een rechtstreekse methode aanroepen op een apparaat](iot-hub-devguide-direct-methods.md)
* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md)

Als u wilt voor het uitproberen van enkele concepten die worden beschreven in dit artikel, raadpleegt u de volgende zelfstudies voor IoT-Hub:

* [Aan de slag met Azure IoT Hub](quickstart-send-telemetry-node.md)
* [Over het verzenden van berichten van cloud-naar-apparaat met IoT Hub](iot-hub-csharp-csharp-c2d.md)
* [How to process IoT Hub apparaat-naar-cloud-berichten verwerken](tutorial-routing.md)
