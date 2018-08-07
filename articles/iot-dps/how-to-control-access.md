---
title: Beveiliging-eindpunten in IoT Device Provisioning Service | Microsoft Docs
description: Concepten - toegang tot IoT Device Provisioning Service voor back-end-apps beheren. Bevat informatie over beveiligingstokens.
author: wesmc7777
manager: timlt
ms.service: iot-dps
services: iot-dps
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: wesmc
ms.openlocfilehash: b4776ef3589d994fff692e450d252c491c20f7b2
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522863"
---
# <a name="control-access-to-azure-iot-hub-device-provisioning-service"></a>Toegang tot Azure IoT Hub Device Provisioning Service beheren

In dit artikel beschrijft de opties voor het beveiligen van uw IoT device provisioning service. De provisioning-service wordt gebruikt *machtigingen* toegang verlenen tot elk eindpunt. Machtigingen beperken de toegang tot een service-exemplaar op basis van functionaliteit.

Dit artikel wordt beschreven:

* De andere machtigingen die u aan een back-end-app verlenen kunt voor toegang tot uw provisioning-service.
* Het verificatieproces en de tokens die wordt gebruikt om machtigingen te controleren.

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

U moet de juiste machtigingen voor toegang tot een van de provisioning service-eindpunten hebben. Een back-end-app moet bijvoorbeeld een token met beveiligingsreferenties samen met elk bericht dat wordt verzonden naar de service.

## <a name="access-control-and-permissions"></a>Access control en machtigingen

U kunt verlenen [machtigingen](#device-provisioning-service-permissions) in de volgende manieren:

* **Gedeeld toegangsbeleid voor autorisatie**. Beleid voor gedeelde toegang kunnen verlenen tot een combinatie van [machtigingen](#device-provisioning-service-permissions). U kunt beleid in definiëren de [Azure-portal][lnk-management-portal], of programmatisch met behulp van de [Device Provisioning Service REST-API's][lnk-resource-provider-apis]. Een nieuwe provisioning-service heeft het standaardbeleid voor het volgende:

  * **provisioningserviceowner**: beleid met alle machtigingen.

> [!NOTE]
> Zie [machtigingen](#device-provisioning-service-permissions) voor gedetailleerde informatie.

## <a name="authentication"></a>Verificatie

Azure IoT Hub Device Provisioning Service verleent toegang tot eindpunten door te controleren of een token op basis van het beleid voor gedeelde toegang. Beveiligingsreferenties, zoals symmetrische sleutels worden nooit via de kabel verzonden.

> [!NOTE]
> De Device Provisioning Service-resourceprovider wordt beveiligd via uw Azure-abonnement, omdat alle providers in het [Azure Resource Manager][lnk-azure-resource-manager].

Zie de volgende sectie voor meer informatie over het maken en gebruiken van beveiligingstokens.

HTTP is de enige ondersteunde protocol en deze verificatie wordt geïmplementeerd door te nemen van een geldig token in de **autorisatie** aanvraagheader.

#### <a name="example"></a>Voorbeeld
`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501&skn=provisioningserviceowner`

> [!NOTE]
> De [Azure IoT Device Provisioning Service-SDK's] [ lnk-sdks] automatisch genereren van tokens bij het verbinden met de service.

## <a name="security-tokens"></a>Beveiligingstokens
De Device Provisioning Service maakt gebruik van beveiligingstokens voor de verificatie van services om te voorkomen dat sleutels op de kabel verzonden. Bovendien zijn beveiligingstokens beperkt in de geldigheid van de tijd en het bereik. [Azure IoT Device Provisioning Service-SDK's] [ lnk-sdks] automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's hoeven te genereren en beveiligingstokens rechtstreeks gebruiken. Dergelijke scenario's omvatten het direct gebruik van de HTTP-aanvallen.

### <a name="security-token-structure"></a>Token beveiligingsstructuur

U beveiligingstokens gebruiken om tijd gebonden toegang voor specifieke functionaliteit in IoT Device Provisioning Service-services te verlenen. Als u autorisatie verbinding maken met de provisioning-service, moeten de services beveiligingstokens die zijn ondertekend met een gedeelde toegang of de symmetrische sleutel verzenden.

Een token is ondertekend met een gedeelde toegang sleutel verleent toegang tot alle functionaliteit die is gekoppeld met de machtigingen van het beleid voor gedeelde toegang. 

Het beveiligingstoken heeft de volgende indeling:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Hier volgen de verwachte waarden:

| Waarde | Beschrijving |
| --- | --- |
| {handtekening} |Een tekenreeks van de HMAC-SHA256 handtekening van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijke**: de sleutel is gedecodeerd op basis van base64 en gebruikt als sleutel voor het uitvoeren van de HMAC-SHA256-berekening.|
| {expiry} |UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {URL-encoded-resourceURI} | Lagere aanvraag-URL-codering van de resource-URI van de kleine letters. URI-voorvoegsel (per segment) van de eindpunten die toegankelijk zijn met dit token, beginnend met de hostnaam van de IoT Device Provisioning Service (Er is geen protocol). Bijvoorbeeld `mydps.azure-devices-provisioning.net`. |
| {policyName} |De naam van het beleid voor gedeelde toegang waarvoor dit token verwijst. |

**Houd er rekening mee op het voorvoegsel van**: de URI-voorvoegsel wordt berekend per segment en niet door het teken. Bijvoorbeeld `/a/b` is een voorvoegsel voor `/a/b/c` , maar niet voor `/a/bc`.

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

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires + "&skn="+ policyName;
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
        'se' : str(int(ttl)),
        'skn' : policy_name
    }

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Aangezien de geldigheid van het token is gevalideerd op IoT Device Provisioning Service-machines, moet de afwijking van de klok van de computer waarmee het token wordt gegenereerd minimaal zijn.


### <a name="use-security-tokens-from-service-components"></a>Beveiligingstokens van de onderdelen van service gebruiken

Serviceonderdelen kunnen alleen beveiligingstokens met behulp van de juiste machtigingen verlenen zoals hierboven is beleid voor gedeelde toegang genereren.

Hier vindt u de servicefuncties beschikbaar gesteld op de eindpunten:

| Eindpunt | Functionaliteit |
| --- | --- |
| `{your-service}.azure-devices-provisioning.net/enrollments` |Biedt bewerkingen voor apparaat-inschrijving met de Device Provisioning Service. |
| `{your-service}.azure-devices-provisioning.net/enrollmentGroups` |Biedt bewerkingen voor het beheren van groepen van de inschrijving van apparaten. |
| `{your-service}.azure-devices-provisioning.net/registrations/{id}` |Biedt bewerkingen voor het ophalen en de status van apparaatregistraties beheren. |


Als u bijvoorbeeld een service die zijn gegenereerd met een vooraf gemaakte gedeeld toegangsbeleid met de naam **enrollmentread** zou maken van een token met de volgende parameters:

* resource-URI: `{mydps}.azure-devices-provisioning.net`,
* ondertekeningssleutel: een van de sleutels van de `enrollmentread` -beleid
* Beleidsnaam: `enrollmentread`,
* elk gewenst moment verlopen.

![Maken van een gedeeld toegangsbeleid voor uw DPS-exemplaar in de portal][img-add-shared-access-policy]

```nodejs
var endpoint ="mydps.azure-devices-provisioning.net";
var policyName = 'enrollmentread'; 
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat toegang tot het lezen van alle inschrijvingsrecords verleent zou, zou zijn:

`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=enrollmentread`

## <a name="reference-topics"></a>Onderwerpen met naslaginformatie:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het beheren van toegang tot uw IoT Device Provisioning Service.

## <a name="device-provisioning-service-permissions"></a>Machtigingen voor Device Provisioning Service

De volgende tabel bevat de machtigingen die u gebruiken kunt voor het beheren van toegang tot uw IoT Device Provisioning Service.

| Machtiging | Opmerkingen |
| --- | --- |
| **ServiceConfig** |Verleent toegang tot het wijzigen van de serviceconfiguraties. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **EnrollmentRead** |Verleent leestoegang heeft tot het apparaatregistraties en Registratiegroepen. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **EnrollmentWrite** |Verleent schrijf-toegang tot het apparaatregistraties en Registratiegroepen. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **RegistrationStatusRead** |Verleent leestoegang heeft tot de status van de apparaatregistratie. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |
| **RegistrationStatusWrite**  |Verleent verwijderen toegang tot de status van de apparaatregistratie. <br/>Deze machtiging wordt gebruikt door de back-end cloudservices. |

<!-- links and images -->

[img-add-shared-access-policy]: ./media/how-to-control-access/how-to-add-shared-access-policy.PNG
[lnk-sdks]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-management-portal]: https://portal.azure.com
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iot-dps/
