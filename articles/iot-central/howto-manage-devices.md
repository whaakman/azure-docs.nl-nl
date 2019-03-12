---
title: Beheren van apparaten in uw Azure IoT Central-toepassing | Microsoft Docs
description: Als een operator informatie over het beheren van apparaten in uw Azure IoT Central-toepassing.
author: ellenfosborne
ms.author: elfarber
ms.date: 01/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 59f4dffb64b0e72e8847f0b1baf947ad1d732063
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766567"
---
# <a name="manage-devices-in-your-azure-iot-central-application-new-ui-design"></a>Beheer van apparaten in uw Azure IoT Central-toepassing (nieuwe gebruikersinterface ontwerp)

Dit artikel wordt beschreven hoe u, als operator voor het beheren van apparaten in uw Azure IoT Central-toepassing. Als operator kunt u het volgende doen:

- Gebruik de **Device Explorer** pagina weergeven, toevoegen en verwijderen van apparaten die zijn verbonden met uw Azure IoT Central-toepassing.
- Onderhouden een actuele inventaris van uw apparaten.
- De metagegevens van uw apparaten up-to-date houden door de waarden die zijn opgeslagen in de apparaateigenschappen te wijzigen.
- De werking van uw apparaten beheren door het bijwerken van een instelling op een specifiek apparaat uit de **instellingen** pagina.


## <a name="view-your-devices"></a>Uw apparaten weergeven

Een afzonderlijk apparaat weergeven:

1. Kies **Device Explorer** in het navigatiemenu links. Hier ziet u een lijst van uw [apparaatsjablonen](howto-set-up-template.md).

1. Kies een sjabloon van het apparaat in de **sjablonen** lijst.

1. In het rechter deelvenster van de **Device Explorer** pagina, ziet u een lijst met apparaten die zijn gemaakt op basis van een sjabloon voor dat apparaat. Kies een afzonderlijk apparaat om te zien van de pagina met details van het apparaat voor dat apparaat:

    ![Pagina met Details van apparaat](./media/howto-manage-devices/devicelist.png)

## <a name="add-a-device"></a>Een apparaat toevoegen

Een apparaat toevoegen aan uw Azure IoT Central-toepassing:

1. Kies **Device Explorer** in het navigatiemenu links.

1. Kies de apparaat-sjabloon van waaruit u wilt maken van een apparaat.

1. Kies + **nieuwe**.

1. Kies **echte** of **gesimuleerde**. Er is een echt apparaat voor een fysiek apparaat waarmee u verbinding met uw Azure IoT Central-toepassing maken. Een gesimuleerd apparaat heeft voorbeeldgegevens die voor u worden gegenereerd door Azure IoT Central.

## <a name="import-devices"></a>Apparaten importeren

Voor grote aantallen apparaten verbinding met uw toepassing, kunt u bulksgewijs apparaten importeren uit een CSV-bestand. Het CSV-bestand moet de volgende kolommen en headers hebben:

* **IOTC_DeviceID** -de apparaat-ID moet alleen kleine letters bevatten.
* **IOTC_DeviceName** -in deze kolom is optioneel.

Bulk-apparaten in uw toepassing registreren:

1. Kies **Device Explorer** in het navigatiemenu links.

1. Kies de apparaat-sjabloon die u bulksgewijs wilt maken de apparaten in het linkerdeelvenster.

    > [!NOTE]
    > Als u de sjabloon voor een apparaat niet hebben, maar vervolgens kunt u apparaten onder importeren **niet-gekoppelde apparaten** en registreer ze zonder een sjabloon. Nadat apparaten zijn geïmporteerd, kunt u deze vervolgens koppelen met een sjabloon.

1. Selecteer **importeren**.

    ![Actie importeren](./media/howto-manage-devices/BulkImport1.png)

1. Selecteer het CSV-bestand met de lijst van apparaat-id's moeten worden geïmporteerd.

1. Apparaten importeren begint zodra het bestand is geüpload. U kunt de status importeren aan de bovenkant van het apparaat raster volgen.

1. Nadat het importeren is voltooid, wordt een bericht wordt weergegeven op het raster apparaat.

    ![Importeren geslaagd](./media/howto-manage-devices/BulkImport3.png)

Als het apparaat mislukt importeren, ziet u een foutbericht weergegeven in het raster apparaat. Er wordt een logboekbestand voor het vastleggen van alle fouten gegenereerd die u kunt downloaden.

**Apparaten koppelen met een sjabloon**

Als u apparaten registreren met het starten van de import onder **niet-gekoppelde apparaten**, en vervolgens de apparaten worden gemaakt zonder een sjabloon apparaatkoppeling. Apparaten moeten zijn gekoppeld aan een sjabloon voor het verkennen van de gegevens en andere details over het apparaat. Volg deze stappen voor het koppelen van apparaten met een sjabloon:

1. Kies **Device Explorer** in het navigatiemenu links.

1. Kies in het linkerdeelvenster **niet-gekoppelde apparaten**:

    ![Niet-gekoppelde apparaten](./media/howto-manage-devices/UnassociatedDevices1.png)

1. Selecteer de apparaten die u wilt koppelen aan een sjabloon:

1. Selecteer **koppelen**:

    ![Apparaten koppelen](./media/howto-manage-devices/UnassociatedDevices2.png)

1. Kies de sjabloon uit de lijst met beschikbare sjablonen en selecteer **koppelen**.

1. De geselecteerde apparaten zijn gekoppeld aan de apparaat-sjabloon die u hebt gekozen.

> [!NOTE]
> Nadat een apparaat gekoppeld aan een sjabloon kan niet worden niet-gekoppelde of die zijn gekoppeld aan een andere sjabloon is.

## <a name="export-devices"></a>Apparaten exporteren

Als u wilt een echt apparaat verbinden met IoT Central, moet u de verbindingsreeks. U kunt details van het apparaat in één bulkbewerking om op te halen en verbindingsreeksen en andere eigenschappen exporteren. Het exportproces maakt een CSV-bestand met de apparaat-id, de apparaatnaam en het primaire verbindingsreeks voor de geselecteerde apparaten.

Bulksgewijs export-apparaten van uw toepassing:

1. Kies **Device Explorer** in het navigatiemenu links.

1. Kies de apparaat-sjabloon van waaruit u wilt exporteren van de apparaten in het linkerdeelvenster.

1. Selecteer de apparaten die u wilt exporteren en selecteer vervolgens de **exporteren** actie.

    ![Exporteren](./media/howto-manage-devices/Export1.png)

1. Het exportproces start. U kunt de status aan de bovenkant van het raster volgen.

1. Wanneer de export is voltooid, wordt een bericht weergegeven samen met een koppeling voor het downloaden van het gegenereerde bestand.

1. Selecteer de **bericht** voor het downloaden van het bestand naar een lokale map op de schijf.

    ![Exporteren geslaagd](./media/howto-manage-devices/Export2.png)

1. Het geëxporteerde CSV-bestand bevat de volgende kolommen: apparaat-ID, de naam van apparaat, apparaatsleutels en X509 certificaatvingerafdrukken:

    * IOTC_DEVICEID
    * IOTC_DEVICENAME
    * IOTC_SASKEY_PRIMARY
    * IOTC_SASKEY_SECONDARY
    * IOTC_X509THUMBPRINT_PRIMARY   
    * IOTC_X509THUMBPRINT_SECONDARY

## <a name="delete-a-device"></a>Een apparaat verwijderen

Verwijderen van een echt of gesimuleerd apparaat vanuit uw Azure IoT Central-toepassing:

1. Kies **Device Explorer** in het navigatiemenu.

1. Kies de sjabloon van het apparaat van het apparaat dat u wilt verwijderen.

1. Schakel het selectievakje naast het apparaat te verwijderen.

1. Kies **verwijderen**.

## <a name="change-a-device-setting"></a>De Apparaatinstelling van een wijzigen

Instellingen bepalen het gedrag van een apparaat. Met andere woorden, kunnen ze u informatie invoeren voor uw apparaat. U kunt bekijken en bijwerken van de instellingen op de **Apparaatdetails** pagina.

1. Kies **Device Explorer** in het navigatiemenu.

1. Kies de sjabloon van het apparaat van het apparaat waarvan u de instellingen wilt wijzigen.

1. Kies de **instellingen** tabblad. Hier ziet u alle instellingen voor die uw apparaat heeft en de huidige waarden. Voor elke instelling kunt u zien als het apparaat nog steeds wordt gesynchroniseerd.

1. Wijzig de instellingen op de waarden die u nodig hebt. U kunt meerdere instellingen wijzigen op een tijdstip en ze allemaal op hetzelfde moment bijwerken.

1. Kies **Update**. De waarden worden verzonden naar uw apparaat. Wanneer het apparaat wordt bevestigd de instelling wijzigen dat, de status van de instelling terug naar **gesynchroniseerd**.

## <a name="change-a-property"></a>Een eigenschap wijzigt

Eigenschappen zijn de metagegevens van apparaten die zijn gekoppeld aan het apparaat, zoals stad en serienummer. U kunt bekijken en bijwerken van de eigenschappen op de **Apparaatdetails** pagina.

1. Kies **Device Explorer** in navigatiemenu.

1. Kies de sjabloon van het apparaat van het apparaat waarvan u de eigenschappen u wilt wijzigen.

1. Kies de **eigenschappen** tabblad, waar u alle eigenschappen zien.

1. De toepassingseigenschappen van op de waarden die u moet wijzigen. U kunt meerdere eigenschappen in één keer hebt gewijzigd en ze allemaal op hetzelfde moment bijwerken. Kies **Update**.

> [!NOTE]
> U kunt de waarde niet wijzigen _apparaateigenschappen_. Apparaateigenschappen worden ingesteld door het apparaat en zijn alleen-lezen in de Azure IoT Central-toepassing.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe u apparaten beheert in uw toepassing met Azure IoT Central, volgt de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Het gebruik van Apparaatsets](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
