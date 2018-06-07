---
title: Hoe apparaten die zijn ingericht met Azure IoT Hub apparaat inrichtingsservice inrichting ervan ongedaan | Microsoft Docs
description: Deprovision-apparaten die zijn ingericht met Azure IoT Hub apparaat inrichten van Service
author: bryanla
ms.author: bryanla
ms.date: 05/11/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 56d9c4c057e226f6cd93551380436cdc52d79169
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630354"
---
# <a name="how-to-deprovision-devices-that-were-previously-auto-provisioned"></a>Hoe apparaten die zijn eerder automatisch ingerichte inrichting ervan ongedaan 

Soms is het nodig zijn voor de identiteitsgegevens van apparaten die eerder in de automatische is ingericht via de Service voor het inrichten van apparaten. Bijvoorbeeld, een apparaat kan worden verkocht of verplaatst naar een nieuwe iothub of mogelijk verloren, gestolen, of anderszins gecompromitteerd. 

In het algemeen een apparaat opheffen van inrichting bestaat uit twee stappen:

1. Het apparaat van uw provisioning-service, om te voorkomen dat toekomstige automatisch inrichten van disenroll. Afhankelijk van of u toegang intrekken wilt, tijdelijk of permanent, kunt u een registratie-item verwijderen of uitschakelen. Voor apparaten die gebruikmaken van X.509 attestation, kan u een vermelding in de hiërarchie van uw bestaande inschrijving groepen zijn uitschakelen/verwijderen.  
 
   - Zie voor meer informatie over hoe u een apparaat disenroll, [hoe een apparaat uit Azure IoT Hub apparaat-inrichtingsservice disenroll](how-to-revoke-device-access-portal.md).
   - Zie voor meer informatie over hoe u een apparaat programmatisch met behulp van een van de inrichting service-SDK's disenroll, [apparaatinschrijvingen met service-SDK's beheren](how-to-manage-enrollments-sdks.md).

2. Het apparaat uit uw IoT-Hub, om te voorkomen dat toekomstige communicatie en gegevensoverdracht opgeheven. U kunt opnieuw tijdelijk uitschakelen of permanent verwijderen van het apparaat vermelding in het identiteitenregister voor de IoT-Hub waar het inrichten. Zie [apparaten uitschakelen](/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) voor meer informatie over deactivering. Zie 'Device Management / IoT apparaten' voor uw resource IoT-Hub in de [Azure-portal](https://portal.azure.com).

De exacte stappen waarmee u naar de inrichting ervan ongedaan maakt een apparaat, is afhankelijk van de attestation-mechanisme en de toepasselijke inschrijving vermelding met uw provisioning-service. De volgende secties geven een overzicht van het proces, op basis van de inschrijving en de attestation-type.

## <a name="individual-enrollments"></a>Afzonderlijke inschrijvingen
Apparaten die gebruikmaken van TPM attestation of X.509 attestation met een leaf-certificaat zijn ingericht via een afzonderlijke inschrijving-vermelding. 

Aan de inrichting ervan ongedaan maakt een apparaat met een afzonderlijke inschrijving: 

1. Het apparaat van de inrichting service disenroll:

   - Voor apparaten die gebruikmaken van TPM attestation, de invoer afzonderlijke inschrijving om in te trekken permanent van het apparaat toegang tot de inrichting service verwijderen of uitschakelen van de vermelding voor het tijdelijk de toegang intrekken. 
   - Voor apparaten die gebruikmaken van X.509 attestation, kunt u verwijderen of uitschakelen van de vermelding. Vergeet echter als u een afzonderlijke inschrijving voor een apparaat dat gebruikmaakt van X.509 verwijdert en een inschrijvingsgroep ingeschakeld voor voor een handtekeningcertificaat bestaat in dat de certificaatketen van het apparaat, het apparaat opnieuw kunt inschrijven. Voor dergelijke apparaten mogelijk veiliger uitschakelen van de vermelding voor de inschrijving. Hierdoor wordt dus voorkomen dat het apparaat opnieuw te registreren, ongeacht of er een ingeschakelde inschrijvingsgroep bestaat voor een van de handtekeningcertificaten.

2. Uitschakelen of het apparaat in het identiteitenregister van de IoT-hub die het inrichten te verwijderen. 


## <a name="enrollment-groups"></a>Groepen voor inschrijving
Met X.509-attestation worden apparaten ook ingericht via een registratie-groep. Inschrijving groepen zijn geconfigureerd met een handtekeningcertificaat, ofwel een tussenliggende of basis-CA-certificaat en de toegang tot de inrichting service voor apparaten met een certificaat dat in de certificaatketen. Zie voor meer informatie over inschrijving groepen en X.509-certificaten met de inrichting service, [X.509-certificaten](concepts-security.md#x509-certificates). 

Een overzicht van de apparaten die zijn ingericht via een inschrijvingsgroep-kunt u de registratie-groep details weergeven. Dit is een eenvoudige manier om te begrijpen welke IoT-hub aan elk apparaat is ingericht. De lijst met apparaten weergeven: 

1. Aanmelden bij de Azure-portal en klikt u op **alle resources** op in het menu links.
2. Klik op uw provisioning-service in de lijst met resources.
3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **inschrijving groepen** tabblad.
4. Klik op de inschrijvingsgroep om deze te openen.

   ![Inschrijving groepsvermelding weergeven in de portal](./media/how-to-unprovision-devices/view-enrollment-group.png)

Met groepen van de inschrijving zijn er twee scenario's te overwegen:

- Aan de inrichting ervan ongedaan alle apparaten die zijn ingericht via een inschrijvingsgroep:
  1. De registratie-groep voor het handtekeningcertificaat van de zwarte uitschakelen. 
  2. Gebruik de lijst met ingerichte apparaten voor die groep registratie uit te schakelen of elk apparaat verwijderen uit het identiteitenregister van de respectieve IoT-hub. 
  3. Na het uitschakelen of verwijderen van alle apparaten van hun respectieve IoT hubs, kunt u eventueel de registratie-groep verwijderen. Bedenk wel dat, als u de inschrijvingsgroep verwijdert en er een inschrijvingsgroep ingeschakeld voor voor een handtekeningcertificaat hoger staan in de certificaatketen van een of meer van de apparaten is, die apparaten opnieuw kunnen inschrijven. 

- Aan de inrichting ervan ongedaan één apparaat uit een inschrijvingsgroep:
  1. Maak een uitgeschakelde afzonderlijke inschrijving voor het certificaat leaf (apparaat). Dit trekt u toegang tot de inrichting service voor dat apparaat terwijl nog steeds toegang voor andere apparaten waarop het handtekeningcertificaat van de registratie-groep in de keten. De uitgeschakelde afzonderlijke registratie voor het apparaat niet verwijderen. In dat geval kunt het apparaat opnieuw in te schrijven via de registratie-groep. 
  2. Gebruik de lijst met ingerichte apparaten voor die inschrijvingsgroep de IoT-hub die het apparaat is ingericht om te vinden en uitschakelen of verwijderen uit het register-id's van de hub. 
  
  










