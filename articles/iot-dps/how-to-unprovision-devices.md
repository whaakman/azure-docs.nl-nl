---
title: Hoe u de inrichting van apparaten die zijn ingericht met Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Inrichting van apparaten die zijn ingericht met Azure IoT Hub Device Provisioning Service
author: wesmc7777
ms.author: wesmc
ms.date: 05/11/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 3c6e2a9006d73d269422292dc959866d3f6d8a82
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522669"
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Hoe u de inrichting van apparaten die zijn eerder automatisch ingericht 

U kunt vinden die nodig zijn voor de inrichting van apparaten die eerder in de auto is ingericht via de Device Provisioning Service. Bijvoorbeeld, een apparaat kan worden verkocht of verplaatst naar een andere IoT-hub, of deze kan worden verloren, gestolen of anderszins gecompromitteerd is. 

In het algemeen opheffen van inrichting van een apparaat bestaat uit twee stappen:

1. Het apparaat vanaf uw provisioning-service, om te voorkomen dat toekomstige automatische inrichting disenroll. Afhankelijk van of u toegang intrekken tijdelijk of permanent wilt, kunt u een vermelding voor apparaatinschrijving verwijderen of uitschakelen. Voor apparaten die gebruikmaken van X.509-attestation, kunt u een vermelding in de hiërarchie van uw bestaande Registratiegroepen zijn uitschakelen/verwijderen.  
 
   - Zie voor meer informatie over een apparaat disenroll, [hoe u een apparaat bij Azure IoT Hub Device Provisioning Service disenroll](how-to-revoke-device-access-portal.md).
   - Zie voor meer informatie over een apparaat via een programma met behulp van een van de provisioning service-SDK's disenroll, [apparaatregistraties beheren met service-SDK's](how-to-manage-enrollments-sdks.md).

2. Registratie ongedaan maken op het apparaat van uw IoT-Hub, om te voorkomen dat toekomstige communicatie en gegevensoverdracht. U kunt opnieuw tijdelijk uitschakelen of permanent verwijderen van het apparaat vermelding in het id-register voor de IoT-Hub wanneer deze is ingericht. Zie [apparaten uitschakelen](/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) voor meer informatie over deactivering. Zie "Device Management / IoT-apparaten' voor uw IoT-Hub, in de [Azure-portal](https://portal.azure.com).

De exacte stappen die u ondernemen om de inrichting van een apparaat ongedaan maken, is afhankelijk van de attestation-mechanisme en de toepasselijke inschrijvingsvermelding met de provisioning-service. De volgende secties bevatten een overzicht van het proces, op basis van de registratie en het attestation-type.

## <a name="individual-enrollments"></a>Afzonderlijke inschrijvingen
Apparaten die gebruikmaken van TPM-attestation of X.509-attestation met een leafcertificaat zijn ingericht via een afzonderlijke inschrijvingsvermelding. 

Als u wilt de inrichting van een apparaat ongedaan maken heeft met een afzonderlijke inschrijving: 

1. Het apparaat uit de inrichtingsservice disenroll:

   - Voor apparaten die gebruikmaken van TPM-attestation, verwijdert u de vermelding voor afzonderlijke inschrijving om in te trekken permanent van het apparaat toegang tot de inrichtingsservice of uitschakelen van de vermelding voor de toegang tijdelijk intrekken. 
   - Voor apparaten die gebruikmaken van X.509-attestation, kunt u verwijderen of uitschakelen van de vermelding. Let erop, echter, als u een afzonderlijke inschrijving voor een apparaat dat gebruikmaakt van X.509 verwijdert en een ingeschakelde registratiegroep voor een certificaat voor ondertekening bestaat in de betreffende certificaatketen van het apparaat, het apparaat kan opnieuw worden ingeschreven. Voor dergelijke apparaten, kan het veiliger zijn om uit te schakelen van de vermelding voor apparaatinschrijving. Hierdoor wordt voorkomen dat het apparaat opnieuw te registreren, ongeacht of een ingeschakelde registratiegroep voor een van de certificaten voor tokenondertekening bestaat.

2. Uitschakelen of verwijderen van het apparaat in het id-register van de IoT-hub die is geleverd aan. 


## <a name="enrollment-groups"></a>Registratiegroepen
Met X.509-attestation, kunnen apparaten ook worden ingericht via een registratiegroep zit. Registratiegroepen zijn geconfigureerd met een ondertekend certificaat, ofwel een tussenliggend of basis-CA-certificaat en de toegang tot de inrichtingsservice voor apparaten met een certificaat dat in de certificaatketen. Zie voor meer informatie over het registreren van groepen en X.509-certificaten met de provisioning-service, [X.509-certificaten](concepts-security.md#x509-certificates). 

U ziet een lijst met apparaten die zijn ingericht via een registratiegroep zit, kunt u de registratiegroep details weergeven. Dit is een eenvoudige manier om te begrijpen welke IoT-hub elk apparaat is ingericht voor. De lijst met apparaten weergeven: 

1. Meld u aan bij de Azure-portal en klikt u op **alle resources** in het menu links.
2. Klik op de provisioning-service in de lijst met resources.
3. Klik in de provisioning-service op **registraties beheren**en selecteer vervolgens **Registratiegroepen** tabblad.
4. Klik op de registratiegroep om deze te openen.

   ![Registatiegroepvermelding weergeven in de portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Met het registreren van groepen zijn er twee scenario's:

- Aan de inrichting ongedaan maken op alle apparaten die zijn ingericht via een registratiegroep zit:
  1. Schakel de registratiegroep blokkeringslijst van het certificaat voor ondertekening. 
  2. Gebruik de lijst met ingerichte apparaten voor die inschrijvingsgroep uitschakelen of verwijderen van elk apparaat van de id-register van de respectieve IoT-hub. 
  3. Na het uitschakelen of verwijderen van alle apparaten van hun respectieve IoT-hubs, kunt u eventueel de registratiegroep verwijderen. Let erop, maar dan wel dat, als u de registratiegroep verwijdert en er een ingeschakelde registratiegroep voor een certificaat voor ondertekening eerder in de certificaatketen van één of meer van de apparaten is, die apparaten kunnen opnieuw worden ingeschreven. 

- Aan de inrichting ongedaan maken van een registratiegroep zit één apparaat:
  1. Maken van een uitgeschakelde afzonderlijke inschrijving voor het certificaat (apparaat). Dit trekt u toegang tot de inrichtingsservice voor dat apparaat terwijl nog steeds toegang voor andere apparaten waarvoor de registratiegroep handtekeningcertificaat in hun keten. De uitgeschakelde afzonderlijke registratie voor het apparaat niet verwijderen. In dat geval kan het apparaat opnieuw in te schrijven via de registratiegroep. 
  2. De lijst met ingerichte apparaten voor die inschrijvingsgroep gebruiken de IoT-hub die het apparaat is ingericht om te vinden en uitschakelen of verwijderen van de id-register van de hub. 
  
  










