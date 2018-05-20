---
title: Beheren van de apparaten in uw Azure IoT centrale toepassing | Microsoft Docs
description: Informatie over het beheren van apparaten in uw Azure IoT centrale toepassing als operator.
services: iot-central
author: ellenfosborne
ms.author: elfarber
ms.date: 01/21/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 75472d701160e7cfd331d01efcdc1a19ae20fb2d
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="manage-devices-in-your-azure-iot-central-application"></a>Beheer van apparaten in uw Azure IoT centrale toepassing

Dit artikel wordt beschreven hoe u, als operator voor het beheren van apparaten in uw Microsoft Azure IoT centrale toepassing. Als operator kunt u:

- Gebruik de **Explorer** pagina weergeven, toevoegen en verwijderen van apparaten die zijn verbonden met uw Azure IoT centrale toepassing.
- Een actuele inventaris van uw apparaten onderhouden.
- De metagegevens van uw apparaten up-to-date houden door de waarden die zijn opgeslagen in de apparaateigenschappen te wijzigen.
- Het gedrag van uw apparaten beheren met het bijwerken van een instelling op een specifiek apparaat van de **instellingen** pagina.

## <a name="view-your-devices"></a>Uw apparaten weergeven

Een afzonderlijk apparaat weergeven:

1. Kies **Explorer** in het navigatiemenu links. Hier ziet u een lijst van uw [apparaat sjablonen](howto-set-up-template.md).

1. Kies een **apparaat sjabloon** in het linkerdeelvenster.

1. In het rechterdeelvenster ziet u een lijst met apparaten die zijn gemaakt op basis van die sjabloon apparaat. Kies een afzonderlijk apparaat om te zien de **Apparaatdetails** pagina voor dat apparaat:

    [![De pagina Details apparaat](./media/howto-manage-devices/image1.png)](./media/howto-manage-devices/image1.png#lightbox)

## <a name="add-a-device"></a>Een apparaat toevoegen

Een apparaat toevoegen aan uw Azure IoT centrale toepassing:

1. Kies **Explorer** in het navigatiemenu links.

1. Kies een sjabloon voor het apparaat van waaruit u wilt maken van een apparaat.

1. Kies + **nieuwe**.

1. Kies **echte** of **gesimuleerde**. Een echte apparaat is voor een fysiek apparaat waarmee u verbinding met uw Azure IoT centrale toepassing maken. Een gesimuleerd apparaat heeft voor u gegenereerd door Azure IoT centrale voorbeeldgegevens. In dit voorbeeld wordt een echte apparaat. Kies **echte** om te navigeren naar de **Apparaatdetails** pagina voor het nieuwe apparaat.


## <a name="bulk-import-devices"></a>Bulkimport apparaten

Groot aantal apparaten verbinding van uw toepassing, Azure IoT centrale Bulksgewijs aanbiedingen importeren apparaten via een CSV-bestand. 

Vereisten voor CSV-bestand:
1. Het CSV-bestand mag slechts één kolom met de apparaat-id's hebben.

1. Bestand mag geen hebben een koptekst.


Bulksgewijs registreren apparaten in uw toepassing:

1. Kies **Explorer** in het navigatiemenu links.

1. Kies de apparaat-sjabloon waarvoor u bulksgewijs wilt maken de apparaten in het linkerdeelvenster.

1. Kies **nieuw** en selecteer **bulkimport**.

    [![Bulksgewijs importactie](./media/howto-manage-devices/BulkImport1.png)](./media/howto-manage-devices/BulkImport1.png#lightbox)

1. Selecteer het CSV-bestand met de lijst met apparaat-id's moeten worden geïmporteerd.

1. Apparaat importeren begint zodra het bestand is geüpload. U kunt de status importeren aan de bovenkant van het raster apparaat volgen.

1. Nadat het importeren is voltooid, wordt een bericht op het raster apparaat weergegeven.

    [![Bulksgewijs importeren geslaagd](./media/howto-manage-devices/BulkImport3.png)](./media/howto-manage-devices/BulkImport3.png#lightbox)

Als het apparaat mislukt importeert, wordt er een fout weergegeven in het raster apparaat. Een logboekbestand voor het vastleggen van alle fouten gegenereerd en door te klikken op het foutbericht kan worden gedownload.



## <a name="delete-a-device"></a>Een apparaat verwijderen

Ofwel een echte of gesimuleerde apparaat verwijderen uit uw Azure IoT centrale toepassing:

1. Kies **Explorer** in het navigatiemenu.

1. Kies een sjabloon voor de apparaten van het apparaat dat u wilt verwijderen.

1. Schakel het selectievakje naast het apparaat te verwijderen.

1. Kies **verwijderen**.

## <a name="change-a-device-setting"></a>De Apparaatinstelling van een wijzigen

Instellingen bepalen het gedrag van een apparaat. Met andere woorden: Hiermee kunt u informatie invoeren op uw apparaat. U kunt weergeven en bijwerken van instellingen voor apparaten op de **Apparaatdetails** pagina.

1. Kies **Explorer** in het navigatiemenu.

1. Kies een sjabloon voor de apparaten van het apparaat waarvan u de instellingen wilt wijzigen.

1. Kies de **instellingen** tabblad. Hier ziet u alle instellingen voor die uw apparaat heeft en de huidige waarden. U kunt voor elke instelling zien als het apparaat nog steeds wordt gesynchroniseerd.

1. Wijzig de instellingen op de gewenste waarden. U kunt in één keer meerdere instellingen wijzigen en ze allemaal op hetzelfde moment bijwerken.

1. Kies **Update**. De waarden worden verzonden naar het apparaat. Wanneer het apparaat, de gewijzigde instelling bevestigt, de status van de instelling terug naar **gesynchroniseerd**.

## <a name="change-a-property"></a>Een eigenschap wijzigt

Eigenschappen zijn de metagegevens van apparaten die zijn gekoppeld aan het apparaat, zoals plaats en serienummer. U kunt weergeven en bijwerken van eigenschappen op de **Apparaatdetails** pagina.

1. Kies **Explorer** op navigatiemenu.

1. Kies een sjabloon voor de apparaten van het apparaat waarvan u de eigenschappen u wilt wijzigen.

1. Kies de **eigenschappen** tabblad waar u de eigenschappen zien.

1. De eigenschappen op de gewenste waarden wijzigen. U kunt meerdere eigenschappen in één keer wijzigen en ze allemaal op hetzelfde moment bijwerken.

1. Kies **Update**.

> [!NOTE]
> U kunt de waarde van wijzigen _apparaateigenschappen_. Apparaateigenschappen worden ingesteld door het apparaat en zijn alleen-lezen in de Azure IoT centrale toepassing.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u apparaten in uw Azure IoT centrale toepassing beheert, wordt hier de voorgestelde volgende stap:

> [!div class="nextstepaction"]
> [Het gebruik van Apparaatsets](howto-use-device-sets.md)

<!-- Next how-tos in the sequence -->
