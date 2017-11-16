---
title: Beveiliging-eindpunten in IoT Device inrichtingsservice | Microsoft Docs
description: Concepten - toegang tot IoT Device inrichtingsservice voor back-end-apps beheren. Bevat informatie over beveiligingstokens.
services: iot-dps
documentationcenter: .net
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-dps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: dkshir,rajeevmv
ms.openlocfilehash: 718fe9b3ca449f8f7b1420080ea75716e8badcf5
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="control-access-to-azure-iot-hub-device-provisioning-service"></a>Toegang tot Azure IoT Hub apparaat inrichtingsservice beheren

In dit artikel beschrijft de opties voor het beveiligen van uw IoT-apparaat inrichting service. De inrichting service wordt gebruikt *machtigingen* toegang verlenen tot elk eindpunt. Machtigingen beperken de toegang tot een service-exemplaar op basis van functionaliteit.

Dit artikel wordt beschreven:

* De andere machtigingen die u kunt verlenen aan een back-end-app voor toegang tot uw provisioning-service.
* Het verificatieproces en de tokens die wordt gebruikt om machtigingen te controleren.

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

U moet de juiste machtigingen voor toegang tot een van de inrichting service-eindpunten hebben. Een back-endserver voor apps moet bijvoorbeeld een token met beveiligingsreferenties samen met elk bericht die wordt verzonden naar de service.

## <a name="access-control-and-permissions"></a>Toegangsbeheer en machtigingen

U kunt verlenen [machtigingen](#device-provisioning-service-permissions) in de volgende manieren:

* **Gedeeld toegangsbeleid autorisatie**. Beleid voor gedeelde toegang kunnen verlenen tot een combinatie van [machtigingen](#device-provisioning-service-permissions). U kunt beleid in definiëren de [Azure-portal][lnk-management-portal], of programmatisch met behulp van de [apparaat inrichten Service REST-API's][lnk-resource-provider-apis]. Een nieuwe inrichting service heeft het standaardbeleid voor het volgende:

  * **provisioningserviceowner**: beleid met alle machtigingen.

> [!NOTE]
> Zie [machtigingen](#device-provisioning-service-permissions) voor gedetailleerde informatie.

## <a name="authentication"></a>Authentication

Azure IoT Hub apparaat inrichtingsservice verleent toegang tot eindpunten door te controleren of een token op basis van het beleid voor gedeelde toegang. Beveiligingsreferenties zoals symmetrische sleutels worden nooit verzonden via de kabel.

> [!NOTE]
> De resourceprovider apparaat inrichtingsservice is beveiligd via uw Azure-abonnement, omdat alle providers in het [Azure Resource Manager][lnk-azure-resource-manager].

Zie de volgende sectie voor meer informatie over het maken en gebruiken van beveiligingstokens.

HTTP is het enige ondersteunde protocol en verificatie geïmplementeerd door een geldig token in de **autorisatie** aanvraag-header.

#### <a name="example"></a>Voorbeeld
`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501&skn=provisioningserviceowner`

> [!NOTE]
> De [Azure IoT-apparaat inrichten Service SDK's] [ lnk-sdks] automatisch genereren van tokens bij het verbinden met de service.

## <a name="security-tokens"></a>Beveiligingstokens
De Service voor het inrichten van apparaten gebruikt de beveiligingstokens voor verificatie van services om te voorkomen dat sleutels op de kabel verzonden. Bovendien zijn beveiligingstokens beperkt in geldigheid en het bereik. [Azure IoT-apparaat inrichten Service SDK's] [ lnk-sdks] automatisch genereren van tokens zonder speciale configuratie. Sommige scenario's hoeven te genereren en beveiligingstokens rechtstreeks gebruiken. Dergelijke scenario's omvatten het direct gebruik van de HTTP-aanvallen.

### <a name="security-token-structure"></a>Security token structuur

Beveiligingstokens kunt u tijd begrensd toegang verleend voor de services in de specifieke functionaliteit van de inrichtingsservice van IoT-apparaat. Als u toestemming om verbinding met de service inrichting, moeten de services beveiligingstokens die zijn ondertekend met een gedeelde toegang of de symmetrische sleutel verzenden.

Een token dat is ondertekend met een gedeelde toegang sleutel verleent toegang tot alle functies die zijn gekoppeld aan de gedeelde beleid toegangsmachtigingen. 

Het beveiligingstoken heeft de volgende indeling:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Hier volgen de verwachte waarden:

| Waarde | Beschrijving |
| --- | --- |
| {handtekening} |Een tekenreeks HMAC SHA256 handtekening van het formulier: `{URL-encoded-resourceURI} + "\n" + expiry`. **Belangrijke**: de sleutel is gedecodeerd van base64 en gebruikt als sleutel voor het uitvoeren van de berekening HMAC SHA256.|
| {verstrijken} |UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {{URL-codering-resourceURI} | Case lager URL-codering van de resource-URI van kleine letters. De voorvoegsels van URI (door het segment) van de eindpunten die toegankelijk zijn met dit token, beginnen met de hostnaam van de IoT-apparaat inrichtingsservice (geen protocol). Bijvoorbeeld `mydps.azure-devices-provisioning.net`. |
| {policyName} |De naam van het beleid voor gedeelde toegang waarnaar dit token verwijst. |

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

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires + "&skn="+ policyName;
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
        'se' : str(int(ttl)),
        'skn' : policy_name
    }

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

> [!NOTE]
> Aangezien de geldigheid van het token is gevalideerd op de machines inrichtingsservice van IoT-apparaat, is de afwijking van de klok van de computer die het token genereert moet minimaal.


### <a name="use-security-tokens-from-service-components"></a>Gebruik de beveiligingstokens van de onderdelen van service

Serviceonderdelen kunnen alleen beveiligingstokens die werken met beleid voor gedeelde toegang tot de juiste machtigingen verlenen zoals hierboven is gegenereerd.

Hier volgen de servicefuncties beschikbaar op de eindpunten zijn gemaakt:

| Eindpunt | Functionaliteit |
| --- | --- |
| `{your-service}.azure-devices-provisioning.net/enrollments` |Biedt apparaat registratie bewerkingen met de Service voor het inrichten van apparaten. |
| `{your-service}.azure-devices-provisioning.net/enrollmentGroups` |Bewerkingen voor het beheren van groepen van apparaten inschrijving biedt. |
| `{your-service}.azure-devices-provisioning.net/registrations/{id}` |Bewerkingen voor het ophalen en het beheren van de status van apparaten registraties biedt. |


Als u bijvoorbeeld een service die is gegenereerd met een vooraf gemaakte gedeeld toegangsbeleid aangeroepen **enrollmentread** zou maken van een token met de volgende parameters:

* resource-URI: `{mydps}.azure-devices-provisioning.net`,
* de handtekeningsleutel: een van de sleutels van de `enrollmentread` -beleid
* de naam van beleid: `enrollmentread`,
* elk gewenst moment verlopen.

![Een gedeeld toegangsbeleid voor uw exemplaar van de DP's maken in de portal][img-add-shared-access-policy]

```nodejs
var endpoint ="mydps.azure-devices-provisioning.net";
var policyName = 'enrollmentread'; 
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Het resultaat toegang tot het lezen van alle inschrijvingsrecords verlenen wilt, zou zijn:

`SharedAccessSignature sr=mydps.azure-devices-provisioning.net&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=enrollmentread`

## <a name="reference-topics"></a>De onderwerpen waarnaar wordt verwezen:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het beheren van toegang tot uw IoT-Service voor het inrichten van apparaten.

## <a name="device-provisioning-service-permissions"></a>Apparaat inrichtingsservice machtigingen

De volgende tabel bevat de machtigingen die u kunt toegang tot uw IoT-Service voor het inrichten van apparaat.

| Machtiging | Opmerkingen |
| --- | --- |
| **ServiceConfig** |Verleent toegang tot het wijzigen van de serviceconfiguraties. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **EnrollmentRead** |Verleent leestoegang tot de apparaatinschrijvingen en de inschrijving van groepen. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **EnrollmentWrite** |Verleent-schrijftoegang tot de apparaatinschrijvingen en de inschrijving van groepen. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **RegistrationStatusRead** |Verleent leestoegang tot de status van het apparaat registratie. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |
| **RegistrationStatusWrite**  |Verleent verwijderen toegang tot de status van het apparaat registratie. <br/>Deze machtiging wordt gebruikt door de back-end-cloudservices. |

<!-- links and images -->

[img-add-shared-access-policy]: ./media/how-to-control-access/how-to-add-shared-access-policy.PNG
[lnk-sdks]: ../iot-hub/iot-hub-devguide-sdks.md
[lnk-management-portal]: https://portal.azure.com
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iot-dps/
