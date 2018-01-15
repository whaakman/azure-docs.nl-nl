---
title: Apparaattoegang beheren voor Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Het Apparaattoegang tot uw service DP's in de Azure Portal te trekken
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 12/22/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6f1dad648b228163219c8f722eed3897f4ba4d22
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Het Apparaattoegang tot uw provisioning-service in de Azure Portal te trekken

Goed beheer van het apparaat is van cruciaal belang voor hoogwaardig systemen zoals IoT-oplossingen. Er is een best practice voor dergelijke systemen een duidelijke plan voor het intrekken van toegang voor apparaten in gevallen waar hun referenties of een SAS-token of een X.509-certificaat kunnen worden geknoeid. In dit artikel wordt beschreven hoe apparaten toegang op de stap inrichting in te trekken.

Voor meer informatie over het Apparaattoegang tot een IoT-hub intrekken nadat het apparaat is ingericht. Zie [apparaten uitschakelen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Let op het beleid voor opnieuw proberen van apparaten die u toegang voor intrekken. Bijvoorbeeld, probeert een apparaat met een oneindige herhaling beleid continu te registreren bij de inrichting service, service-bronnen en mogelijk van invloed op de prestaties.

## <a name="blacklist-devices-with-an-individual-enrollment-entry"></a>Apparaten met een vermelding in afzonderlijke inschrijving blacklist

Afzonderlijke inschrijvingen van toepassing op één apparaat en x.509-certificaten of SAS-tokens (in een echt of virtueel TPM) kunnen gebruiken als het mechanisme voor attestation. (Dat wil zeggen: apparaten die gebruikmaken van SAS-tokens als hun mechanisme attestation kan alleen worden ingericht via een afzonderlijke inschrijvingsprofielkolom.) Als u wilt een apparaat met een afzonderlijke inschrijving afgekeurde, kunt u uitschakelen of verwijder de vermelding van de inschrijving: 

- Als u wilt tijdelijk afgekeurde het apparaat, kunt u de vermelding van de inschrijving uitschakelen. 

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.
    2. Klik op de inrichting service die u wilt afgekeurde van uw apparaat bij in de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **afzonderlijke inschrijvingen** tabblad.
    4. Klik op de vermelding voor inschrijving voor het apparaat dat u wilt afgekeurde om deze te openen. 
    5. Klik op **uitschakelen** aan de onderkant van de inschrijving lijst vermelding klikt u vervolgens op **opslaan**.  

        ![Uitschakelen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
- Om het apparaat permanent afgekeurde, kunt u de vermelding van de inschrijving te verwijderen.

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.
    2. Klik op de inrichting service die u wilt afgekeurde van uw apparaat bij in de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **afzonderlijke inschrijvingen** tabblad.
    4. Schakel het selectievakje naast de vermelding voor inschrijving voor het apparaat dat u wilt afgekeurde. 
    5. Klik op **verwijderen** aan de bovenkant van het venster, klikt u vervolgens op **Ja** om te bevestigen dat u wilt verwijderen van de inschrijving. 

        ![Verwijderen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
    6. Zodra de actie is voltooid, ziet u de vermelding is verwijderd uit de lijst met afzonderlijke inschrijvingen.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-using-an-enrollment-group"></a>Een tussenliggende x.509- of CA-basiscertificaat met behulp van een inschrijvingsgroep afgekeurde

X.509-certificaten worden meestal in een certificaatvertrouwensketen gerangschikt. Als er een certificaat in elk stadium in een keten wordt geknoeid, broken trust plaatsvindt en het certificaat moet worden gebeurd om te voorkomen dat apparaten omlaag stream in de keten met dat certificaat uit door de Service voor het inrichten van apparaat wordt ingericht. Zie voor meer informatie over het X.509-certificaten en hoe ze worden gebruikt met de inrichting service, [X.509-certificaten](./concepts-security.md#x509-certificates). 

Een registratie-groep is een vermelding voor apparaten die een gemeenschappelijk attestation-mechanisme van X.509-certificaten die zijn ondertekend door dezelfde tussenliggende delen of basis-CA. De vermelding van de groep inschrijving is geconfigureerd met het X.509-certificaat dat is gekoppeld aan de tussenliggende of basis-CA, evenals alle configuratiewaarden zoals twin status en IoT hub-verbinding, die worden gedeeld door apparaten met een certificaat dat in hun certificaat keten. Om het certificaat afgekeurde, kunt u uitschakelen of verwijderen van de registratie-groep:

- Als u wilt tijdelijk afgekeurde het certificaat, kunt u de registratie-groep uitschakelen. 

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.
    2. Klik op de inrichting service die u wilt afgekeurde van het handtekeningcertificaat van in de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **inschrijving groepen** tabblad.
    4. Klik op de inschrijvingsgroep voor het certificaat dat u wilt afgekeurde om deze te openen.
    5. Klik op **groep bewerken** op de linkerbovenhoek van de vermelding van de groep inschrijving.
    6. Schakel de vermelding voor inschrijving selecteren **uitschakelen** op de **invoer inschakelen** schakelen en klik vervolgens op **opslaan**.  

        ![Schakel inschrijving groepsvermelding in de portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
- Als u wilt permanent afgekeurde het certificaat, kunt u de registratie-groep verwijderen.

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.
    2. Klik op de inrichting service die u wilt afgekeurde van uw apparaat bij in de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **inschrijving groepen** tabblad.
    4. Schakel het selectievakje naast de inschrijvingsgroep voor het certificaat dat u wilt afgekeurde. 
    5. Klik op **verwijderen** aan de bovenkant van het venster, klikt u vervolgens op **Ja** om te bevestigen dat u wilt verwijderen van de registratie-groep. 

        ![Inschrijving groepsvermelding in de portal verwijderen](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

    6. Zodra de actie is voltooid, ziet u de vermelding is verwijderd uit de lijst met groepen van de inschrijving.  

> [!NOTE]
> Als u een inschrijvingsgroep voor een certificaat voor verwijdert, apparaten waarvoor dit certificaat in de certificaatketen nog steeds mogelijk om in te schrijven als een inschrijvingsgroep ingeschakeld voor voor het basiscertificaat of een ander tussenliggende certificaat hoger staan in hun certificaat keten bestaat.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Blacklist specifieke apparaten in een inschrijvingsgroep voor

Apparaten die het X.509-mechanisme voor attestation implementeren certificaatketen van het apparaat en de persoonlijke sleutel voor verificatie gebruiken. Wanneer een apparaat verbinding maakt en geverifieerd met de Service voor het inrichten van apparaten, wordt de service eerst gezocht naar een afzonderlijke inschrijving die overeenkomt met de referenties van het apparaat voordat het zoeken van inschrijving groepen om te bepalen of het apparaat kan worden ingericht. Als de service vindt een uitgeschakelde afzonderlijke inschrijving voor het apparaat, het voorkomt dat het apparaat verbinding maakt, zelfs als er een ingeschakelde inschrijvingsgroep voor een tussenliggende of de basis-CA in de certificaatketen van het apparaat bestaat. Als u wilt een afzonderlijk apparaat in een inschrijvingsgroep afgekeurde, de volgende stappen uit:

1. Aanmelden bij de Azure-portal en klikt u op **alle resources** in het menu links.
2. Klik op de inrichting service met de registratie-groep voor het apparaat dat u wilt afgekeurde uit de lijst met resources.
3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **afzonderlijke inschrijvingen** tabblad.
4. Klik op de **toevoegen** bovenaan op de knop. 
5. Selecteer **X.509** als het beveiligingsmechanisme voor voor het apparaat en het uploaden van het certificaat voor apparaten. Dit is de ondertekende Eindentiteit certificaat geïnstalleerd op het apparaat dat wordt gebruikt voor het genereren van certificaten voor clientverificatie.
6. Voer de **IoT Hub apparaat-ID** voor het apparaat. 
7. Schakel de vermelding voor inschrijving selecteren **uitschakelen** op de **invoer inschakelen** overschakelen. 
8. Klik op **Opslaan**. Op het is gemaakt van uw inschrijving voor ziet u het apparaat worden weergegeven onder de **afzonderlijke inschrijvingen** tabblad. 

    ![Uitschakelen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)




