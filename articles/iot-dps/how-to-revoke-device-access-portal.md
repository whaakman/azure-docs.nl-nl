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
ms.openlocfilehash: 12aebf3a56aa7469a765ab6fc67aa65b254db71a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="revoke-device-access-to-your-provisioning-service-in-the-azure-portal"></a>Apparaattoegang tot uw provisioning-service in de Azure portal intrekken

Goed beheer van het apparaat is van cruciaal belang voor hoogwaardig systemen zoals IoT-oplossingen. Er is een best practice voor dergelijke systemen een duidelijke plan van hoe u toegang voor apparaten in te trekken wanneer hun referenties of een shared access signatures (SAS)-token of een X.509-certificaat is gecompromitteerd. In dit artikel wordt beschreven hoe apparaten toegang op de stap inrichting in te trekken.

Zie voor meer informatie over het Apparaattoegang tot een IoT-hub intrekken nadat het apparaat is ingericht, [apparaten uitschakelen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#disable-devices).

> [!NOTE] 
> Let op het beleid voor opnieuw proberen van apparaten die u toegang voor intrekken. Een apparaat met een oneindige herhaling-beleid kan bijvoorbeeld continu proberen te registreren bij de inrichting service. Deze situatie verbruikt-serviceresources en mogelijk van invloed op prestaties.

## <a name="blacklist-devices-by-using-an-individual-enrollment-entry"></a>Apparaten met behulp van een afzonderlijke inschrijving vermelding blacklist

Afzonderlijke inschrijvingen gelden voor één apparaat en x.509-certificaten of SAS-tokens (in een echt of virtueel TPM) kunnen gebruiken als het mechanisme voor attestation. (Dat wil zeggen: apparaten die gebruikmaken van SAS-tokens als hun mechanisme attestation alleen via een afzonderlijke inschrijving kan worden ingericht.) Als u wilt een apparaat met een afzonderlijke inschrijving afgekeurde, kunt u uitschakelen of verwijderen van de vermelding van de inschrijving. 

Tijdelijk afgekeurde het apparaat door de vermelding van de registratie uit te schakelen: 

1. Aanmelden bij de Azure portal en selecteert u een **alle resources** in het menu links.
2. Selecteer de inrichting service die u wilt uw apparaat bij afgekeurde in de lijst met resources.
3. Selecteer in de inrichting service **inschrijvingen beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer de vermelding van de inschrijving voor het apparaat dat u wilt afgekeurde. 
5. Selecteer **uitschakelen** op de **invoer inschakelen** switch en selecteer vervolgens **opslaan**.  

   ![Uitschakelen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)
    
Aan de zwarte het apparaat permanent door de registratie-item te verwijderen:

1. Aanmelden bij de Azure portal en selecteert u een **alle resources** in het menu links.
2. Selecteer de inrichting service die u wilt uw apparaat bij afgekeurde in de lijst met resources.
3. Selecteer in de inrichting service **inschrijvingen beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer het selectievakje in naast de vermelding van de inschrijving voor het apparaat dat u wilt afgekeurde. 
5. Selecteer **verwijderen** aan de bovenkant van het venster en selecteer vervolgens **Ja** om te bevestigen dat u wilt verwijderen van de inschrijving. 

   ![Verwijderen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/delete-individual-enrollment.png)
    
Nadat u de procedure hebt voltooid, ziet u de vermelding is verwijderd uit de lijst met afzonderlijke inschrijvingen.  

## <a name="blacklist-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group"></a>Een tussenliggende x.509- of CA-basiscertificaat afgekeurde met behulp van een inschrijvingsgroep voor

X.509-certificaten worden meestal in een certificaatvertrouwensketen gerangschikt. Als er een certificaat in elk stadium in een keten wordt geknoeid, plaatsvindt broken trust. Het certificaat moet worden gebeurd om te voorkomen dat apparaat inrichtingsservice downstream inrichting apparaten in de keten met dat certificaat. Zie voor meer informatie over het X.509-certificaten en hoe ze worden gebruikt met de inrichting service, [X.509-certificaten](./concepts-security.md#x509-certificates). 

Een registratie-groep is een vermelding voor apparaten die een gemeenschappelijk attestation-mechanisme van X.509-certificaten die zijn ondertekend door dezelfde tussenliggende delen of basis-CA. De vermelding van de groep inschrijving is geconfigureerd met het X.509-certificaat dat is gekoppeld aan de tussenliggende of basis-CA. De vermelding wordt ook geconfigureerd met een configuratiewaarden, zoals twin status en IoT hub-verbinding, die worden gedeeld door apparaten met een certificaat dat in de certificaatketen. Om het certificaat afgekeurde, kunt u uitschakelen of verwijderen van de registratie-groep.

Tijdelijk afgekeurde het certificaat door de registratie-groep uit te schakelen: 

1. Aanmelden bij de Azure portal en selecteert u een **alle resources** in het menu links.
2. Selecteer de inrichting service die u wilt het handtekeningcertificaat van afgekeurde in de lijst met resources.
3. Selecteer in de inrichting service **inschrijvingen beheren**, en selecteer vervolgens de **inschrijving groepen** tabblad.
4. Selecteer de inschrijvingsgroep voor het certificaat dat u wilt afgekeurde.
5. Selecteer in de vermelding van de groep inschrijving **groep bewerken**.
6. Selecteer **uitschakelen** op de **invoer inschakelen** switch en selecteer vervolgens **opslaan**.  

   ![Schakel inschrijving groepsvermelding in de portal](./media/how-to-revoke-device-access-portal/disable-enrollment-group.png)

    
Aan de zwarte permanent het certificaat door de registratie-groep te verwijderen:

1. Aanmelden bij de Azure portal en selecteert u een **alle resources** in het menu links.
2. Selecteer de inrichting service die u wilt uw apparaat bij afgekeurde in de lijst met resources.
3. Selecteer in de inrichting service **inschrijvingen beheren**, en selecteer vervolgens de **inschrijving groepen** tabblad.
4. Selecteer het selectievakje in naast de inschrijvingsgroep voor het certificaat dat u wilt afgekeurde. 
5. Selecteer **verwijderen** aan de bovenkant van het venster en selecteer vervolgens **Ja** om te bevestigen dat u wilt verwijderen van de registratie-groep. 

   ![Inschrijving groepsvermelding in de portal verwijderen](./media/how-to-revoke-device-access-portal/delete-enrollment-group.png)

Nadat u de procedure hebt voltooid, ziet u de vermelding is verwijderd uit de lijst met groepen van de inschrijving.  

> [!NOTE]
> Als u een inschrijvingsgroep voor een certificaat voor verwijdert, apparaten waarvoor het certificaat in de certificaatketen nog steeds mogelijk om in te schrijven als een inschrijvingsgroep ingeschakeld voor voor het basiscertificaat of een ander tussenliggende certificaat hoger staan in hun certificaat keten bestaat.

## <a name="blacklist-specific-devices-in-an-enrollment-group"></a>Blacklist specifieke apparaten in een inschrijvingsgroep voor

Apparaten die het X.509-mechanisme voor attestation implementeren certificaatketen van het apparaat en de persoonlijke sleutel voor verificatie gebruiken. Wanneer een apparaat verbinding maakt en met inrichtingsservice apparaat verifieert, wordt de service eerst gezocht naar een afzonderlijke inschrijving die overeenkomt met de referenties van het apparaat. De service wordt vervolgens gezocht inschrijving groepen om te bepalen of het apparaat kan worden ingericht. Als de service een uitgeschakelde afzonderlijke inschrijving voor het apparaat wordt gevonden, kunnen verbinding maken met het apparaat. De service wordt voorkomen dat de verbinding zelfs als er een ingeschakelde inschrijvingsgroep voor een tussenliggende of de basis-CA in de certificaatketen van het apparaat bestaat. 

Als u wilt een afzonderlijk apparaat in een inschrijvingsgroep afgekeurde, de volgende stappen uit:

1. Aanmelden bij de Azure portal en selecteert u een **alle resources** in het menu links.
2. Selecteer de inrichting service met de registratie-groep voor het apparaat dat u wilt afgekeurde uit de lijst met resources.
3. Selecteer in de inrichting service **inschrijvingen beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.
4. Selecteer de **toevoegen** bovenaan op de knop. 
5. Selecteer **X.509** als het beveiligingsmechanisme voor voor het apparaat en het uploaden van het certificaat voor apparaten. Dit is de ondertekende Eindentiteit certificaat op het apparaat geïnstalleerd. Het apparaat wordt gebruikt voor het genereren van certificaten voor clientverificatie.
6. Voor **IoT Hub apparaat-ID**, geef de ID voor het apparaat. 
7. Selecteer **uitschakelen** op de **invoer inschakelen** switch en selecteer vervolgens **opslaan**. 

   ![Uitschakelen van afzonderlijke inschrijving vermelding in de portal](./media/how-to-revoke-device-access-portal/disable-individual-enrollment.png)

Wanneer u uw registratie met succes maakt, ziet u het apparaat worden weergegeven op de **afzonderlijke inschrijvingen** tabblad.


