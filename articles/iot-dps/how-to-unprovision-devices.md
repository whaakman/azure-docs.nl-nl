---
title: Hoe inrichting van apparaten die zijn geregistreerd bij Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Hoe inrichting van apparaten die zijn geregistreerd door uw service DP's in de Azure Portal
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 01/08/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 1d057a4df43cf25e6817672d198207d9a50e462e
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
---
# <a name="how-to-unprovision-devices-enrolled-by-your-provisioning-service"></a>Hoe apparaten die zijn geregistreerd door de inrichting service inrichting

Soms is het nodig zijn om te verwijderen van apparaten die zijn ingericht via de Service voor het inrichten van apparaten. Bijvoorbeeld, een apparaat kan worden verkocht of verplaatst naar een nieuwe iothub of mogelijk verloren, gestolen, of anderszins gecompromitteerd. 

In het algemeen een apparaat ongedaan maken inrichting bestaat uit twee stappen:

1. Toegang voor het apparaat met uw provisioning-service in te trekken. Afhankelijk van of u toegang intrekken wilt, tijdelijk of permanent of in het geval van het x.509-mechanisme voor attestation, op de hiërarchie van uw bestaande groepen voor inschrijving, kunt u een vermelding die registratie verwijderen of uitschakelen. 
 
   - Zie voor meer informatie over het Apparaattoegang via de portal in te trekken, [Apparaattoegang intrekken](how-to-revoke-device-access-portal.md).
   - Zie voor meer informatie over het Apparaattoegang programmatisch met behulp van een van de inrichting service-SDK's in te trekken, [apparaatinschrijvingen met service-SDK's beheren](how-to-manage-enrollments-sdks.md).

2. Uitschakelen of verwijderen van vermelding in het identiteitenregister van het apparaat voor de IoT-Hub waar het inrichten. Zie voor meer informatie, [apparaat identiteiten beheren](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices) in de documentatie van Azure IoT Hub. 

De exacte stappen waarmee u inrichting van een apparaat, is afhankelijk van de attestation-mechanisme en de toepasselijke inschrijving vermelding met uw provisioning-service.

## <a name="individual-enrollments"></a>Afzonderlijke inschrijvingen
Apparaten die gebruikmaken van TPM attestation of X.509 attestation met een leaf-certificaat zijn ingericht via een afzonderlijke inschrijving-vermelding. 

Inrichting van een apparaat met een afzonderlijke inschrijving: 
1. Voor apparaten die gebruikmaken van TPM attestation, verwijdert u de afzonderlijke inschrijving vermelding te permanent van het apparaat toegang tot de inrichting service intrekken of uitschakelen van de vermelding voor het tijdelijk de toegang intrekken. Voor apparaten die gebruikmaken van X.509 attestation, kunt u verwijderen of uitschakelen van de vermelding. Vergeet echter als u een afzonderlijke registratie voor een apparaat verwijdert die gebruikmaakt van X.509 attestation en een inschrijvingsgroep ingeschakeld voor voor een handtekeningcertificaat bestaat in dat de certificaatketen van het apparaat, het apparaat opnieuw kunt inschrijven. Voor dergelijke apparaten mogelijk veiliger uitschakelen van de vermelding voor de inschrijving. Hierdoor wordt dus voorkomen dat het apparaat opnieuw te registreren, ongeacht of er een ingeschakelde inschrijvingsgroep bestaat voor een van de handtekeningcertificaten.
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

- Inrichting alle apparaten die zijn ingericht via een inschrijvingsgroep:
  1. De registratie-groep voor het handtekeningcertificaat van de zwarte uitschakelen. 
  2. Gebruik de lijst met ingerichte apparaten voor die groep registratie uit te schakelen of elk apparaat verwijderen uit het identiteitenregister van de respectieve IoT-hub. 
  3. Na het uitschakelen of verwijderen van alle apparaten van hun respectieve IoT hubs, kunt u eventueel de registratie-groep verwijderen. Bedenk wel dat, als u de inschrijvingsgroep verwijdert en er een inschrijvingsgroep ingeschakeld voor voor een handtekeningcertificaat hoger staan in de certificaatketen van een of meer van de apparaten is, die apparaten opnieuw kunnen inschrijven. 
- Inrichting één apparaat uit een inschrijvingsgroep:
  1. Maak een uitgeschakelde afzonderlijke inschrijving voor het certificaat leaf (apparaat). Dit trekt u toegang tot de inrichting service voor dat apparaat terwijl nog steeds toegang voor andere apparaten waarop het handtekeningcertificaat van de registratie-groep in de keten. De uitgeschakelde afzonderlijke registratie voor het apparaat niet verwijderen. In dat geval kunt het apparaat opnieuw in te schrijven via de registratie-groep. 
  2. Gebruik de lijst met ingerichte apparaten voor die inschrijvingsgroep de IoT-hub die het apparaat is ingericht om te vinden en uitschakelen of verwijderen uit het register-id's van de hub. 
  
  










