---
title: Azure IoT Hub Device Provisioning Service - attestation-symmetrische sleutel
description: In dit artikel biedt een conceptueel overzicht van de symmetrische sleutel attestation met IoT Device Provisioning Service.
author: wesmc7777
ms.author: wesmc
ms.date: 08/18/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 80828876ffe8b58697cfaacad4991354ac070730
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971787"
---
# <a name="symmetric-key-attestation"></a>Attestation-symmetrische sleutel

Dit artikel beschrijft de attestation-proces identiteit als u de symmetrische sleutels voor de Device Provisioning Service. 

Attestation-symmetrische sleutel is een eenvoudige benadering voor het verifiëren van een apparaat met een Device Provisioning Service-exemplaar. Deze methode attestation vertegenwoordigt een 'Hallo wereld'-ervaring voor ontwikkelaars die niet bekend bent met het apparaat wordt ingericht, of geen strikte beveiligingseisen. Apparaten attestation met een [TPM](concepts-tpm-attestation.md) of een [X.509-certificaat](concepts-security.md#x509-certificates) is veiliger zijn en moet worden gebruikt voor striktere beveiligingsvereisten.

Symmetrische sleutel inschrijvingen bieden ook een uitstekende manier voor oudere apparaten met beperkte beveiligingsfunctionaliteit, opstarten naar de cloud via Azure IoT. Zie voor meer informatie over symmetrische sleutel attestation met verouderde apparaten [symmetrische sleutels gebruiken met oudere apparaten](how-to-legacy-device-symm-key.md).


## <a name="symmetric-key-creation"></a>Symmetrische sleutel maken

De Device Provisioning Service maakt standaard nieuwe symmetrische sleutels met een standaardlengte 32 bytes wanneer nieuwe registraties worden opgeslagen met de **automatisch genereren van sleutels** optie is ingeschakeld.

![Automatisch genereren symmetrische sleutels](./media/concepts-symmetric-key-attestation/auto-generate-keys.png)

U kunt ook uw eigen symmetrische sleutels voor inschrijvingen opgeven door deze optie uit te schakelen. Wanneer u uw eigen symmetrische sleutels opgeeft, moeten uw sleutels een sleutellengte tussen 16 bytes en 64 bytes hebben. Symmetrische sleutels moeten ook worden opgegeven in geldige Base 64-indeling.



## <a name="detailed-attestation-process"></a>Gedetailleerde attestation-proces

Attestation-symmetrische sleutel met de Device Provisioning-Service wordt uitgevoerd met behulp van dezelfde [beveiligingstokens](../iot-hub/iot-hub-devguide-security.md#security-token-structure) ondersteund door de IoT-hubs om apparaten te identificeren. Deze beveiligingstokens zijn [Shared Access Signature (SAS)-tokens](../service-bus-messaging/service-bus-sas.md). 

SAS-tokens hebben een hash *handtekening* dat is gemaakt met behulp van de symmetrische sleutel. De handtekening wordt opnieuw gemaakt door de Device Provisioning Service om te controleren of een beveiligingstoken dat wordt weergegeven tijdens de attestation authentiek of niet is.

SAS-tokens hebben de volgende notatie:

`SharedAccessSignature sig={signature}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Hier vindt u de onderdelen van elke token:

| Waarde | Beschrijving |
| --- | --- |
| {handtekening} |Een tekenreeks van de HMAC-SHA256 handtekening. Voor afzonderlijke inschrijvingen, wordt deze handtekening gemaakt met behulp van de symmetrische sleutel (primair of secundair) om uit te voeren van de hash. Voor het registreren van groepen, wordt een sleutel die is afgeleid van de sleutel van de groep registratie gebruikt om uit te voeren van de hash. De hash wordt uitgevoerd op een bericht van het formulier: `URL-encoded-resourceURI + "\n" + expiry`. **Belangrijke**: de sleutel moet worden gedecodeerd op basis van base64 voordat het wordt gebruikt voor het uitvoeren van de HMAC-SHA256-berekening. Het resultaat van de handtekening moet ook zijn URL gecodeerd. |
| {resourceURI} |De URI van het eindpunt voor clientregistratie die toegankelijk zijn met dit token beginnen met bereik-ID voor de Device Provisioning Service-exemplaar. Bijvoorbeeld: `{Scope ID}/registrations/{Registration ID}` |
| {expiry} |UTF8-tekenreeksen voor het aantal seconden sinds de epoche 00:00:00 UTC op 1 januari 1970. |
| {URL-encoded-resourceURI} |Lagere aanvraag-URL-codering van de resource-URI van de kleine letters |
| {policyName} |De naam van het beleid voor gedeelde toegang waarvoor dit token verwijst. De naam van het beleid gebruikt bij het inrichten met de attestation-symmetrische sleutel is **registratie**. |

Wanneer een apparaat te bewijzen met een afzonderlijke inschrijving dat is, het apparaat maakt gebruik van de symmetrische sleutel die is gedefinieerd in de vermelding voor afzonderlijke registratie te maken van de hash-handtekening voor de SAS-token.

Zie voor voorbeelden van code die een SAS-token maken, [beveiligingstokens](../iot-hub/iot-hub-devguide-security.md#security-token-structure).

Het maken van beveiligingstokens voor attestation-symmetrische sleutel wordt ondersteund door de Azure IoT C SDK. Zie voor een voorbeeld met behulp van de Azure IoT C SDK om te bevestigen met een afzonderlijke inschrijving [een gesimuleerd apparaat inrichten met symmetrische sleutels](quick-create-simulated-device-symm-key.md).


## <a name="group-enrollments"></a>Registratiegroepen

De symmetrische sleutels voor groepsinschrijvingen worden niet rechtstreeks door apparaten gebruikt bij het inrichten. Apparaten die deel uitmaken van een inschrijving groep in plaats daarvan inrichten met behulp van een afgeleide apparaatsleutel. 

Een unieke registratie-ID wordt eerst gedefinieerd voor elk apparaat te bewijzen dat met een registratiegroep zit. Geldige tekens voor de registratie-ID zijn kleine alfanumeriek en streepje ('-'). Deze registratie-ID moet een unieke dat het apparaat identificeert. Een ouder apparaat kan bijvoorbeeld geen ondersteuning voor veel beveiligingsfuncties. Het oude apparaat mogelijk alleen een MAC-adres of een serienummer is beschikbaar voor het aanduiden van dat apparaat. In dat geval kan een registratie-ID bestaan uit de MAC-adres en het serienummer is vergelijkbaar met het volgende:

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

In dit voorbeeld exact wordt gebruikt in de [over het inrichten van verouderde apparaten met behulp van symmetrische sleutels](how-to-legacy-device-symm-key.md) artikel.

Zodra een registratie-ID is gedefinieerd voor het apparaat, de symmetrische sleutel voor de registratiegroep wordt gebruikt voor het berekenen van een [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) hash van de registratie-ID voor het produceren van een afgeleide apparaatsleutel. De hash van de registratie-ID kan worden uitgevoerd met de volgende C#-code:

```C#
using System; 
using System.Security.Cryptography; 
using System.Text;  

public static class Utils 
{ 
    public static string ComputeDerivedSymmetricKey(byte[] masterKey, string registrationId) 
    { 
        using (var hmac = new HMACSHA256(masterKey)) 
        { 
            return Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(registrationId))); 
        } 
    } 
} 
```

```C#
String deviceKey = Utils.ComputeDerivedSymmetricKey(Convert.FromBase64String(masterKey), registrationId);
```

De resulterende sleutel voor het apparaat wordt vervolgens gebruikt voor het genereren van een SAS-token moet worden gebruikt voor attestation. Elk apparaat in een registratiegroep zit is vereist om te bevestigen met een beveiligingstoken dat is gegenereerd op basis van een unieke afgeleide sleutel. De symmetrische sleutel voor inschrijving-groep kan niet worden gebruikt voor attestation.

#### <a name="installation-of-the-derived-device-key"></a>Installatie van de afgeleide apparaatsleutel

De apparaatsleutels zijn in het ideale geval afgeleid en geïnstalleerd in de fabriek. Deze methode wordt gegarandeerd dat de groepssleutel is nooit opgenomen in enige software geïmplementeerd op het apparaat. Wanneer het apparaat wordt toegewezen, een MAC-adres of het serienummer wordt geverifieerd, kunt u de sleutel afgeleid en opgenomen in het apparaat echter kiest voor de fabrikant om op te slaan.

Houd rekening met het volgende diagram ziet u een tabel van apparaatsleutels gegenereerd in een fabriek door elk apparaat registratie-ID met de sleutel van de inschrijving groep-hashing (**K**). 

![Apparaatsleutels die zijn toegewezen op basis van een fabriek](./media/concepts-symmetric-key-attestation/key-diversification.png)

De identiteit van elk apparaat dat wordt vertegenwoordigd door de registratie-ID en afgeleide apparaatsleutel die is geïnstalleerd in de fabriek. Sleutel voor het apparaat nooit naar een andere locatie wordt gekopieerd en de groepssleutel is nooit worden opgeslagen op een apparaat.

Als de apparaatsleutels zijn niet geïnstalleerd in de factory, een [HSM hardware security module](concepts-security.md#hardware-security-module) moet worden gebruikt voor het veilig opslaan van de apparaat-id.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een goed begrip van de symmetrische sleutel attestation hebt, bekijkt u de volgende artikelen voor meer informatie:

* [Snelstartgids: Een gesimuleerd apparaat inrichten met symmetrische sleutels](quick-create-simulated-device-symm-key.md)
* [Meer informatie over de concepten in automatische inrichting](./concepts-auto-provisioning.md)
* [Aan de slag met behulp van automatische inrichting](./quick-setup-auto-provision.md) 
