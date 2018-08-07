---
title: Hoe u een apparaat bij Azure IoT Hub Device Provisioning Service disenroll
description: Hoe u een apparaat om te voorkomen dat via Azure IoT Hub Device Provisioning Service inrichten disenroll
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: d0720c23e0831b446a92855383fab06b0bfacbc7
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525464"
---
# <a name="how-to-disenroll-a-device-from-azure-iot-hub-device-provisioning-service"></a>Hoe u een apparaat bij Azure IoT Hub Device Provisioning Service disenroll

Goede beheer van de referenties van apparaat is van cruciaal belang voor belangrijke systemen zoals IoT-oplossingen. Er is een best practice voor dergelijke systemen een duidelijke plan van hoe u toegang voor apparaten in te trekken wanneer hun referenties of een token shared access signatures (SAS) of een X.509-certificaat, mogelijk zijn aangetast. 

Inschrijving in de Device Provisioning Service kan een apparaat worden [automatisch ingericht](concepts-auto-provisioning.md). Een ingerichte apparaat is een App die is geregistreerd bij IoT Hub, zodat het voor het ontvangen van de eerste [apparaatdubbel](~/articles/iot-hub/iot-hub-devguide-device-twins.md) status en begin met het melden van telemetrische gegevens. In dit artikel wordt beschreven hoe u een apparaat bij uw provisioning service-exemplaar te voorkomen dat deze wordt opnieuw ingericht in de toekomst disenroll.

> [!NOTE] 
> Houd rekening met het beleid voor opnieuw proberen van apparaten die u toegang kunt intrekken. Een apparaat met een beleid voor oneindig opnieuw proberen kan bijvoorbeeld voortdurend proberen te registreren met de provisioning-service. Deze situatie service-resources verbruikt en mogelijk van invloed op prestaties.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Apparaten met behulp van een afzonderlijke inschrijvingsvermelding blacklist

Afzonderlijke inschrijvingen van toepassing op één apparaat en X.509-certificaten of SAS-tokens (in een echt of virtueel TPM) kunnen gebruiken als het attestation-mechanisme. (Dat wil zeggen: apparaten die gebruikmaken van SAS-tokens als de attestation-mechanisme kan alleen via een afzonderlijke inschrijving worden ingericht.) Een apparaat met een afzonderlijke inschrijving blokkeringslijst, kunt u uitschakelen of verwijderen van de vermelding voor apparaatinschrijving. 

Zwarte tijdelijk het apparaat door de vermelding voor apparaatinschrijving uit te schakelen: 

1. Aanmelden bij de Azure portal en selecteer **alle resources** in het menu links.
2. Selecteer de inrichtingsservice die u wilt uw apparaat bij zwarte in de lijst met resources.
3. Selecteer in uw provisioning-service, **registraties beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer de vermelding voor apparaatinschrijving voor het apparaat dat u wilt blokkeringslijst. 
5. Schuif naar beneden en selecteer **uitschakelen** op de **vermelding inschakelen** schakelen en selecteer vervolgens **opslaan**.  

   [![Afzonderlijke inschrijvingsvermelding in de portal uitschakelen](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png#lightbox)  

Aan de blokkeringslijst het apparaat permanent te verwijderen van de vermelding voor apparaatinschrijving:

1. Aanmelden bij de Azure portal en selecteer **alle resources** in het menu links.
2. Selecteer de inrichtingsservice die u wilt uw apparaat bij zwarte in de lijst met resources.
3. Selecteer in uw provisioning-service, **registraties beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer het selectievakje in naast de inschrijvingsvermelding voor het apparaat dat u wilt blokkeringslijst. 
5. Selecteer **verwijderen** aan de bovenkant van het venster en selecteer vervolgens **Ja** om te bevestigen dat u wilt verwijderen van de inschrijving. 

   ![Afzonderlijke inschrijvingsvermelding in de portal verwijderen](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Nadat u de procedure hebt voltooid, ziet u de vermelding is verwijderd uit de lijst met afzonderlijke inschrijvingen.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Een x.509-tussenliggend of CA-basiscertificaat met behulp van een registratiegroep zit blokkeringslijst

X.509-certificaten zijn doorgaans gerangschikt in een certificaatketen van de vertrouwensrelatie. Als er een certificaat in elk stadium in een keten wordt geknoeid, plaatsvindt broken trust. Het certificaat moet worden op de blokkeringslijst om te voorkomen dat Device Provisioning Service inrichten apparaten downstream in de keten met dat certificaat. Zie voor meer informatie over het X.509-certificaten en hoe ze worden gebruikt met de provisioning-service, [X.509-certificaten](./concepts-security.md#x509-certificates). 

Een registratiegroep zit, wordt een vermelding voor apparaten die een gemeenschappelijk attestation-mechanisme van X.509-certificaten die zijn ondertekend door de tussenliggende hetzelfde delen of basis-CA. De vermelding voor apparaatinschrijving groep is geconfigureerd met het X.509-certificaat dat is gekoppeld aan de tussenliggende of basis-CA. De vermelding wordt ook geconfigureerd met een configuratiewaarden, zoals de status van de apparaatdubbel en IoT hub-verbinding, die worden gedeeld door apparaten met een certificaat dat in de certificaatketen. Het certificaat blokkeringslijst, kunt u uitschakelen of verwijderen van de registratiegroep.

Zwarte het certificaat door het uitschakelen van de registratiegroep tijdelijk: 

1. Aanmelden bij de Azure portal en selecteer **alle resources** in het menu links.
2. Selecteer de inrichtingsservice die u wilt het certificaat voor ondertekening van zwarte in de lijst met resources.
3. Selecteer in uw provisioning-service, **registraties beheren**, en selecteer vervolgens de **Registratiegroepen** tabblad.
4. Selecteer de registratiegroep met behulp van het certificaat dat u wilt blokkeringslijst.
5. Selecteer **uitschakelen** op de **vermelding inschakelen** schakelen en selecteer vervolgens **opslaan**.  

   ![Groep-vermelding voor apparaatinschrijving in de portal uitschakelen](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Aan de blokkeringslijst permanent het certificaat door het verwijderen van de registratiegroep:

1. Aanmelden bij de Azure portal en selecteer **alle resources** in het menu links.
2. Selecteer de inrichtingsservice die u wilt uw apparaat bij zwarte in de lijst met resources.
3. Selecteer in uw provisioning-service, **registraties beheren**, en selecteer vervolgens de **Registratiegroepen** tabblad.
4. Selecteer het selectievakje in naast de registratiegroep voor het certificaat dat u wilt blokkeringslijst. 
5. Selecteer **verwijderen** aan de bovenkant van het venster en selecteer vervolgens **Ja** om te bevestigen dat u wilt verwijderen van de registratiegroep. 

   ![Verwijderen van groep-vermelding voor apparaatinschrijving in de portal](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Nadat u de procedure hebt voltooid, ziet u de vermelding is verwijderd uit de lijst met Registratiegroepen.  

> [!NOTE]
> Als u een registratiegroep voor een certificaat verwijdert, apparaten waarvoor het certificaat in hun certificaatketen nog steeds mogelijk om in te schrijven als een ingeschakelde registratiegroep voor het basiscertificaat of een ander tussenliggende certificaat eerder in het certificaat keten.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Specifieke apparaten in een registratiegroep zit blacklist

Apparaten die het x.509-attestation-mechanisme implementeren gebruiken de certificaatketen van het apparaat en de persoonlijke sleutel om te verifiëren. Wanneer een apparaat verbinding maakt met een verificatie uitvoert met Device Provisioning Service, wordt de service eerst gezocht naar een afzonderlijke inschrijving die overeenkomt met de referenties van het apparaat. De service wordt vervolgens gezocht Registratiegroepen om te bepalen of het apparaat kan worden ingericht. Als de service een uitgeschakelde afzonderlijke inschrijving voor het apparaat vindt, voorkomt dat het apparaat verbinding te maken. De service wordt voorkomen dat de verbinding, zelfs als er een ingeschakelde registratiegroep voor een gevorderde gebruiker of een basis-CA in de certificaatketen van het apparaat bestaat. 

Blokkeringslijst een afzonderlijk apparaat in een registratiegroep zit, de volgende stappen uit:

1. Aanmelden bij de Azure portal en selecteer **alle resources** in het menu links.
2. Selecteer de provisioning-service die de registratiegroep voor het apparaat dat u wilt zwarte bevat uit de lijst met resources.
3. Selecteer in uw provisioning-service, **registraties beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer de **toevoegen** bovenaan op de knop. 
5. Selecteer **X.509** als het attestation-mechanisme voor het apparaat en upload het apparaatcertificaat. Dit is de ondertekende Eindentiteit-certificaat op het apparaat geïnstalleerd. Het apparaat gebruikt deze voor het genereren van certificaten voor clientverificatie.
6. Voor **IoT Hub apparaat-ID**, geef de ID voor het apparaat. 
7. Selecteer **uitschakelen** op de **vermelding inschakelen** schakelen en selecteer vervolgens **opslaan**. 

    [![Gebruik afzonderlijke inschrijvingsvermelding om uit te schakelen apparaat van de groepsinschrijving, in de portal uitgeschakeld](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png)](./media/how-to-revoke-device-access-portal/disable-individual-enrollment-in-enrollment-group.png#lightbox)

Wanneer u uw inschrijving is gemaakt, ziet u het apparaat worden weergegeven op de **afzonderlijke inschrijvingen** tabblad.

## <a name="next-steps"></a>Volgende stappen

Uitschrijving maakt ook deel uit van het proces voor grotere ongedaan maken. Inrichting van een apparaat bevat zowel uitschrijving van de inrichtingsservice, en als uit IoT hub. Zie voor meer informatie over het hele proces, [hoe u de inrichting van apparaten die zijn eerder automatisch ingericht](how-to-unprovision-devices.md) 

