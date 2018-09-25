---
title: Het verwijderen van X.509-certificaten in Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Het verwijderen van X.509-certificaten met uw Device Provisioning service-exemplaar
author: wesmc7777
ms.author: wesmc
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: a40f4489e63c30a101dd708b5a175c25788fb04b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976751"
---
# <a name="how-to-roll-x509-device-certificates"></a>Het verwijderen van certificaten voor x.509-apparaten

Tijdens de levenscyclus van uw IoT-oplossing moet u certificaten implementeren. Twee van de belangrijkste redenen voor het verlengen van certificaten is een schending van de beveiliging en certificaten verlopen. 

Rolling certificaten is een aanbevolen beveiligingsprocedure om te helpen beveiligen van uw systeem in het geval van een inbreuk. Als onderdeel van [wordt ervan uitgegaan dat inbreuk methodologie](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf), Microsoft, wordt de noodzaak van reactieve beveiligingsprocessen die erin samen met preventieve maatregelen nemen. Certificaten voor uw apparaten ongedaan moet zijn opgenomen als onderdeel van deze beveiligingsprocessen. De frequentie in waarmee u uw certificaten implementeert, is afhankelijk van de beveiligingsvereisten van uw oplossing. Klanten met oplossingen met betrekking tot zeer gevoelige gegevens rolt certificaat per dag, terwijl anderen hun certificaten om de paar jaar implementeren.

Rolling apparaatcertificaten omvatten bijwerken van het certificaat dat is opgeslagen op het apparaat en de IoT-hub. Daarna wordt het apparaat opnieuw kunt inrichten zelf met de IoT hub met behulp van normale [automatische inrichting](concepts-auto-provisioning.md) met de Device Provisioning Service.


## <a name="obtain-new-certificates"></a>Nieuwe certificaten verkrijgen

Er zijn veel manieren om nieuwe certificaten voor uw IoT-apparaten te verkrijgen. Deze omvatten het verkrijgen van certificaten van de fabrieksinstellingen van het apparaat, uw eigen certificaten genereren en een derde partij hebben maken van een certificaat voor u beheren. 

Certificaten zijn ondertekend door elkaar in een keten van de vertrouwensrelatie van een basis-CA-certificaat aan een [leaf-certificaat](concepts-security.md#end-entity-leaf-certificate). Een handtekeningcertificaat is het certificaat dat wordt gebruikt voor het ondertekenen van het leaf-certificaat aan het einde van de keten van vertrouwen. Een handtekeningcertificaat mag bestaan uit een basis-CA-certificaat of een tussenliggende certificaat in de keten van vertrouwen. Zie voor meer informatie, [X.509-certificaten](concepts-security.md#x509-certificates).
 
Er zijn twee verschillende manieren om op te halen van een certificaat voor ondertekening. De eerste manier, wordt aanbevolen voor productiesystemen, is het kopen van een certificaat voor ondertekening van een basiscertificeringsinstantie (CA). Op deze manier koppelt beveiliging op een vertrouwde bron. 

De tweede manier is het maken van uw eigen X.509-certificaten met behulp van een hulpprogramma zoals OpenSSL. Deze aanpak is erg handig voor het testen van X.509-certificaten, maar enkele garanties beveiliging biedt. U wordt aangeraden dat u deze methode alleen gebruiken voor het testen, tenzij u voorbereid om te fungeren als uw eigen CA-provider.
 

## <a name="roll-the-certificate-on-the-device"></a>Het certificaat op het apparaat implementeren

Certificaten op een apparaat moeten altijd worden opgeslagen op een veilige plaats, zoals een [hardware security module (HSM)](concepts-device.md#hardware-security-module). De manier waarop u certificaten voor apparaten implementeert, is afhankelijk van hoe ze zijn gemaakt en geïnstalleerd op de apparaten in de eerste plaats. 

Als u uw certificaten van een derde partij hebt ontvangen, moet u bekijken hoe ze hun certificaten implementeren. Het proces kan worden opgenomen in de overeenkomst met hen of is mogelijk een afzonderlijke service die ze bieden. 

Als u uw eigen certificaten voor apparaten beheert, hebt u uw eigen pijplijn voor het bijwerken van certificaten maken. Zorg ervoor dat beide certificaten oude en nieuwe leaf hebben dezelfde algemene naam (CN). Doordat de dezelfde algemene naam, het apparaat kan opnieuw inrichten zelf zonder dat er een dubbele registratierecord gemaakt. 


## <a name="roll-the-certificate-in-the-iot-hub"></a>Het certificaat te rollen in de IoT-hub

Het certificaat voor apparaten kan handmatig worden toegevoegd aan een IoT-hub. Het certificaat kan ook worden geautomatiseerd met behulp van een Device Provisioning service-exemplaar. In dit artikel veronderstellen we dat een Device Provisioning service-exemplaar wordt gebruikt ter ondersteuning van automatische inrichting.

Wanneer een apparaat in eerste instantie is ingericht via automatische inrichting, het Start-up, en neemt contact op met de provisioning-service. De provisioning-service reageert door het uitvoeren van een identiteit te controleren voordat u een apparaat-id maakt in een IoT-hub met behulp van het leaf-certificaat van het apparaat als de referentie. De provisioning-service wordt vervolgens het apparaat toegewezen aan welke IoT-hub en het apparaat gebruikt dan de leaf-certificaat om te verifiëren en verbinding maken met de IoT-hub. 

Zodra een nieuw leafcertificaat is teruggedraaid naar het apparaat, het kan langer verbinding maken met de IoT-hub omdat deze een nieuw certificaat wordt gebruikt om verbinding te maken. Het apparaat met het oude certificaat alleen wordt herkend door de IoT-hub. Het resultaat van de verbindingspoging van het apparaat is de foutmelding 'onbevoegd' verbinding. U kunt deze fout oplossen, moet u de vermelding voor apparaatinschrijving voor het apparaat voor de nieuwe leaf-certificaat van het apparaat bijwerken. De inrichtingsservice werk vervolgens de IoT Hub apparaat registergegevens zo nodig bij het apparaat is ingericht. 

Een mogelijke uitzondering op deze verbindingsfout zou een scenario waarin u hebt gemaakt een [Registratiegroep](concepts-service.md#enrollment-group) voor uw apparaat in de provisioning-service. In dit geval, als u niet zijn de basis- of tussenliggende certificaten in de certificaatketen van het apparaat van de vertrouwensrelatie rolling, wordt klikt u vervolgens het apparaat herkend als het nieuwe certificaat deel uit van de keten van vertrouwen die zijn gedefinieerd in de registratiegroep maakt. Als dit scenario zich als reactie op een schending van de beveiliging voordoet, moet u ten minste de certificaten voor specifieke apparaten in de groep die worden beschouwd als te worden geschonden blokkeringslijst. Zie voor meer informatie, [specifieke apparaten in een registratiegroep zit zwarte](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-revoke-device-access-portal#blacklist-specific-devices-in-an-enrollment-group).

Bijwerken van de vermeldingen voor inschrijving voor certificaten die zijn samengevouwen wordt uitgevoerd op de **registraties beheren** pagina. Voor toegang tot deze pagina, de volgende stappen uit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar de IoT Hub Device Provisioning Service-exemplaar waarvoor de inschrijvingsvermelding voor uw apparaat.

2. Klik op **Inschrijvingen beheren**.

    ![Registraties beheren](./media/how-to-roll-certificates/manage-enrollments-portal.png)


Hoe u omgaan met het bijwerken van de vermelding voor apparaatinschrijving afhankelijk van of u afzonderlijke inschrijvingen of Registratiegroepen. Ook afhankelijk de aanbevolen procedure van of u certificaten bent rolling vanwege een schending van de beveiliging of het certificaat verloopt. De volgende secties wordt beschreven hoe u deze updates worden verwerkt.


## <a name="individual-enrollments-and-security-breaches"></a>Afzonderlijke inschrijvingen en schendingen van de beveiliging

Als u bent certificaten rolling in reactie op een schending van de beveiliging, moet u de volgende benadering die het huidige certificaat onmiddellijk worden verwijderd:

1. Klik op **afzonderlijke inschrijvingen**, en klik op de vermelding van de registratie-ID in de lijst. 

2. Klik op de **verwijderen huidige certificaat** knop en klik vervolgens op het mappictogram om te selecteren van het nieuwe certificaat moet worden geüpload voor de inschrijving. Klik op **opslaan** wanneer u klaar bent.

    Deze stappen moeten worden voltooid voor de primaire en secundaire certificaat, als beide zijn aangetast.

    ![Afzonderlijke registraties beheren](./media/how-to-roll-certificates/manage-individual-enrollments-portal.png)

3. Zodra het certificaat waarmee is geknoeid is verwijderd uit de inrichtingsservice, kan het certificaat nog steeds op het apparaat verbinding met de IoT-hub maken, zolang de apparaatregistratie van een voor het bestaat er worden gebruikt. Hiermee kunt u op twee manieren oplossen: 

    De eerste manier zou worden handmatig navigeren naar uw IoT-hub en de registratie van het apparaat dat is gekoppeld aan het certificaat waarmee is geknoeid onmiddellijk te verwijderen. Klik wanneer het apparaat wordt opnieuw met een bijgewerkt certificaat ingericht, kunt u een nieuwe device Registration service wordt gemaakt.     

    ![IoT hub device Registration service verwijderen](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    De tweede manier zou worden opnieuw inrichten van het apparaat naar de dezelfde IoT-hub met ondersteuning voor beëindiging. Deze methode kan worden gebruikt ter vervanging van het certificaat voor de device Registration service op de IoT-hub. Zie voor meer informatie, [hoe u apparaten opnieuw inrichten](how-to-reprovision.md).

## <a name="individual-enrollments-and-certificate-expiration"></a>Afzonderlijke inschrijvingen en de vervaldatum van certificaat

Als u bent rolling certificaten voor het afhandelen van certificaat verlopen, moet u de configuratie van het secundaire certificaat als volgt om uitvaltijd voor apparaten wordt geprobeerd om in te richten te verlagen.

Later opnieuw wanneer het secundaire certificaat ook bijna is verlopen en moeten worden vernieuwd, kunt u voor het gebruik van de configuratie van de primaire draaien. Draaien tussen de primaire en secundaire certificaten op deze manier hebt u minder uitvaltijd voor apparaten wordt geprobeerd om in te richten.


1. Klik op **afzonderlijke inschrijvingen**, en klik op de vermelding van de registratie-ID in de lijst. 

2. Klik op **secundair certificaat** en klik vervolgens op het mappictogram om te selecteren van het nieuwe certificaat moet worden geüpload voor de inschrijving. Klik op **Opslaan**.

    ![Afzonderlijke inschrijvingen met behulp van het secundaire certificaat beheren](./media/how-to-roll-certificates/manage-individual-enrollments-secondary-portal.png)

3. Later opnieuw als het primaire certificaat is verlopen, keert u terug en verwijdert u dat het primaire certificaat door te klikken op de **verwijderen huidige certificaat** knop.

## <a name="enrollment-groups-and-security-breaches"></a>Registreren van groepen en schendingen van de beveiliging

Voor het bijwerken van de groepsinschrijving van een in reactie op een schending van de beveiliging, moet u een van de volgende methoden die wordt de huidige basis-CA- of tussencertificaat onmiddellijk verwijderen.

#### <a name="update-compromised-root-ca-certificates"></a>Verdachte basis-CA-certificaten bijwerken

1. Klik op de **certificaten** tabblad voor uw Device Provisioning service-exemplaar.

2. Klik op het waarmee is geknoeid certificaat in de lijst en klik vervolgens op de **verwijderen** knop. Bevestig het verwijderen door de certificaatnaam te voeren en op **OK**. Herhaal dit proces voor alle verdachte certificaten.

    ![Basis-CA-certificaat verwijderen](./media/how-to-roll-certificates/delete-root-cert.png)

3. Volg de stappen [configureren CA-certificaten geverifieerd](how-to-verify-certificates.md) toe te voegen en te controleren of de nieuwe basis-CA-certificaten.

4. Klik op de **registraties beheren** tabblad voor uw Device Provisioning service-exemplaar en klik op de **Registratiegroepen** lijst. Klik op de naam van uw inschrijving in de lijst.

5. Klik op **CA-certificaat**, en selecteer het nieuwe basis-CA-certificaat. Klik vervolgens op **Opslaan**. 

    ![De nieuwe basis-CA-certificaat selecteren](./media/how-to-roll-certificates/select-new-root-cert.png)

6. Zodra het certificaat waarmee is geknoeid is verwijderd uit de inrichtingsservice, kan het certificaat nog steeds worden gebruikt op het apparaat verbinding met de IoT-hub maken, zolang het apparaatregistraties voor bestaat. Hiermee kunt u op twee manieren oplossen: 

    De eerste manier zou worden handmatig navigeren naar uw IoT-hub en de registratie van het apparaat dat is gekoppeld aan het certificaat waarmee is geknoeid onmiddellijk te verwijderen. Wanneer uw apparaten opnieuw worden ingericht met bijgewerkte certificaten, wordt vervolgens een nieuwe device Registration service gemaakt voor elk criterium.     

    ![IoT hub device Registration service verwijderen](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    De tweede manier zou zijn met ondersteuning voor beëindiging opnieuw inrichten van uw apparaten op dezelfde IoT hub. Deze methode kan worden gebruikt ter vervanging van certificaten voor het apparaatregistraties op de IoT-hub. Zie voor meer informatie, [hoe u apparaten opnieuw inrichten](how-to-reprovision.md).



#### <a name="update-compromised-intermediate-certificates"></a>Verdachte tussenliggende certificaten bijwerken

1. Klik op **Registratiegroepen**, en klik vervolgens op de groepnaam van de in de lijst. 

2. Klik op **Tussencertificaat**, en **verwijderen huidige certificaat**. Klik op het mappictogram om te navigeren naar het nieuwe tussenliggende certificaat voor de registratiegroep worden geüpload. Klik op **opslaan** wanneer u klaar bent. Deze stappen moeten worden voltooid voor zowel de primaire en secundaire certificaat, als beide zijn aangetast.

    Deze nieuwe tussenliggende certificaat moet worden ondertekend door een geverifieerde CA-basiscertificaat dat al is toegevoegd in de inrichtingsservice. Zie voor meer informatie, [X.509-certificaten](concepts-security.md#x509-certificates).

    ![Afzonderlijke registraties beheren](./media/how-to-roll-certificates/enrollment-group-delete-intermediate-cert.png)


3. Zodra het certificaat waarmee is geknoeid is verwijderd uit de inrichtingsservice, kan het certificaat nog steeds worden gebruikt op het apparaat verbinding met de IoT-hub maken, zolang het apparaatregistraties voor bestaat. Hiermee kunt u op twee manieren oplossen: 

    De eerste manier zou worden handmatig navigeren naar uw IoT-hub en de registratie van het apparaat dat is gekoppeld aan het certificaat waarmee is geknoeid onmiddellijk te verwijderen. Wanneer uw apparaten opnieuw worden ingericht met bijgewerkte certificaten, wordt vervolgens een nieuwe device Registration service gemaakt voor elk criterium.     

    ![IoT hub device Registration service verwijderen](./media/how-to-roll-certificates/remove-hub-device-registration.png)

    De tweede manier zou zijn met ondersteuning voor beëindiging opnieuw inrichten van uw apparaten op dezelfde IoT hub. Deze methode kan worden gebruikt ter vervanging van certificaten voor het apparaatregistraties op de IoT-hub. Zie voor meer informatie, [hoe u apparaten opnieuw inrichten](how-to-reprovision.md).


## <a name="enrollment-groups-and-certificate-expiration"></a>Registreren van groepen en het certificaat verloopt

Als u certificaten voor het afhandelen van certificaat verlopen implementeren, moet u de configuratie van het secundaire certificaat als volgt gebruiken om te controleren of er geen uitvaltijd voor apparaten wordt geprobeerd om in te richten.

Later opnieuw wanneer het secundaire certificaat ook bijna is verlopen en moeten worden vernieuwd, kunt u voor het gebruik van de configuratie van de primaire draaien. Draaien tussen de primaire en secundaire certificaten op deze manier zorgt u ervoor zonder uitvaltijd voor apparaten wordt geprobeerd om in te richten. 

#### <a name="update-expiring-root-ca-certificates"></a>Verlopend basis-CA-certificaten bijwerken

1. Volg de stappen [configureren CA-certificaten geverifieerd](how-to-verify-certificates.md) toe te voegen en te controleren of de nieuwe basis-CA-certificaten.

2. Klik op de **registraties beheren** tabblad voor uw Device Provisioning service-exemplaar en klik op de **Registratiegroepen** lijst. Klik op de naam van uw inschrijving in de lijst.

3. Klik op **CA-certificaat**, en selecteer het nieuwe basis-CA-certificaat onder de **secundair certificaat** configuratie. Klik vervolgens op **Opslaan**. 

    ![De nieuwe basis-CA-certificaat selecteren](./media/how-to-roll-certificates/select-new-root-secondary-cert.png)

4. Later opnieuw als het primaire certificaat is verlopen, klikt u op de **certificaten** tabblad voor uw Device Provisioning service-exemplaar. Klik op het verlopen certificaat in de lijst en klik vervolgens op de **verwijderen** knop. Bevestig het verwijderen door de naam van het certificaat invoeren en op **OK**.

    ![Basis-CA-certificaat verwijderen](./media/how-to-roll-certificates/delete-root-cert.png)



#### <a name="update-expiring-intermediate-certificates"></a>Verlopende tussenliggende certificaten bijwerken


1. Klik op **Registratiegroepen**, en klik op de groepnaam van de in de lijst. 

2. Klik op **secundair certificaat** en klik vervolgens op het mappictogram om te selecteren van het nieuwe certificaat moet worden geüpload voor de inschrijving. Klik op **Opslaan**.

    Deze nieuwe tussenliggende certificaat moet worden ondertekend door een geverifieerde CA-basiscertificaat dat al is toegevoegd in de inrichtingsservice. Zie voor meer informatie, [X.509-certificaten](concepts-security.md#x509-certificates).

   ![Afzonderlijke inschrijvingen met behulp van het secundaire certificaat beheren](./media/how-to-roll-certificates/manage-enrollment-group-secondary-portal.png)

3. Later opnieuw als het primaire certificaat is verlopen, keert u terug en verwijdert u dat het primaire certificaat door te klikken op de **verwijderen huidige certificaat** knop.


## <a name="reprovision-the-device"></a>Het apparaat opnieuw inrichten

Zodra het certificaat wordt geïmplementeerd op het apparaat en de Device Provisioning Service, het apparaat kan opnieuw inrichten zelf contact opnemen met de Device Provisioning service. 

Een eenvoudige manier van programmeren apparaten opnieuw inrichten is om het apparaat te programmeren Neem contact op met de provisioning-service om te gaan via de inrichting stroom als het apparaat de foutmelding 'onbevoegd' ontvangt probeert verbinding maken met de IoT-hub.

Een andere manier is voor zowel de oude als de nieuwe certificaten worden gebruikt voor een korte overlapping en gebruiken van de IoT-hub een opdracht verzenden naar apparaten voordat ze opnieuw worden geregistreerd via de provisioning-service om hun IoT-Hub-verbindingsgegevens bijwerken. Omdat elk apparaat opdrachten anders verwerkt kan, moet u uw apparaat om te weten wat er moet gebeuren wanneer de opdracht wordt aangeroepen programma. Er zijn verschillende manieren kunt u uw apparaat via IoT Hub opdracht, en wordt u aangeraden [directe methoden](../iot-hub/iot-hub-devguide-direct-methods.md) of [taken](../iot-hub/iot-hub-devguide-jobs.md) waarmee de.

Zodra de beëindiging is voltooid, worden apparaten geen verbinding maken met IoT Hub met behulp van de nieuwe certificaten.


## <a name="blacklist-certificates"></a>Zwarte lijst certificaten

In reactie op een schending van de beveiliging moet u mogelijk een certificaat voor apparaten blokkeringslijst. Blokkeringslijst een certificaat voor apparaten, de inschrijvingsvermelding voor het doel apparaat/certificaat uitschakelen Zie voor meer informatie, witte van apparaten in de [uitschrijving beheren](how-to-revoke-device-access-portal.md) artikel.

Wanneer een certificaat opgenomen is als onderdeel van een uitgeschakelde inschrijvingsvermelding, elke poging om u te registreren bij een IoT hub met behulp van die certificaten, mislukken zelfs als deze is ingeschakeld als onderdeel van een andere vermelding voor apparaatinschrijving.
 



## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het x.509-certificaten in de Device Provisioning Service, [beveiliging](concepts-security.md) 
- Zie voor meer informatie over hoe u bewijs van eigendom voor x.509-CA-certificaten met de Azure IoT Hub Device Provisioning Service doet, [certificaten controleren](how-to-verify-certificates.md)
- Zie voor meer informatie over het gebruik van de portal voor het maken van een registratiegroep zit, [apparaatinschrijvingen met Azure portal beheren](how-to-manage-enrollments.md).










