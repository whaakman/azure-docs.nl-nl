---
title: bestand opnemen
description: bestand opnemen
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 780a215b66fec845bc1df639fedda870881b4027
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39189233"
---
> [!div class="op_single_selector"]
> * [C op Windows](../articles/iot-accelerators/iot-accelerators-connecting-devices.md)
> * [C op Linux](../articles/iot-accelerators/iot-accelerators-connecting-devices-linux.md)
> * [Node.js (algemeen)](../articles/iot-accelerators/iot-accelerators-connecting-devices-node.md)
> * [Node.js op Raspberry Pi](../articles/iot-accelerators/iot-accelerators-connecting-pi-node.md)
> * [C op Raspberry Pi](../articles/iot-accelerators/iot-accelerators-connecting-pi-c.md)

In deze zelfstudie implementeert u een **Koelunit** apparaat dat de volgende telemetrie naar de externe controle verzendt [oplossingsverbetering](../articles/iot-accelerators/about-iot-accelerators.md):

* Temperatuur
* Druk
* Vochtigheid

Voor het gemak, genereert de code telemetrie voorbeeldwaarden voor de **Koelunit**. U kunt het voorbeeld kan uitbreiden door echte sensoren verbinding te maken met uw apparaat en echte telemetrie te verzenden.

Het apparaat voorbeeld ook:

* Stuurt metagegevens naar de oplossing voor het beschrijven van de mogelijkheden ervan.
* Reageert op acties die worden geactiveerd in de **apparaten** pagina in de oplossing.
* Reageert op wijzigingen in de configuratie verzenden vanuit de **apparaten** pagina in de oplossing.

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="before-you-start"></a>Voordat u begint

Voordat u code voor uw apparaat schrijven, de oplossingsverbetering voor externe bewaking implementeert en een nieuwe fysiek apparaat toevoegen aan de oplossing.

### <a name="deploy-your-remote-monitoring-solution-accelerator"></a>De oplossingsverbetering voor externe bewaking implementeren

De **Koelunit** apparaat in deze zelfstudie u maakt verzendt gegevens naar een exemplaar van de [bewaking op afstand](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md) oplossingsverbetering. Als u de oplossingsverbetering voor externe controle in uw Azure-account nog niet hebt ingericht, Zie [de oplossingsverbetering voor externe bewaking implementeren](../articles/iot-accelerators/quickstart-remote-monitoring-deploy.md)

Wanneer het implementatieproces voor de oplossing voor externe controle is voltooid, klikt u op **starten** dashboard van de oplossing in uw browser openen.

![Dashboard van de oplossing](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Uw apparaat toevoegt aan de oplossing voor externe controle

> [!NOTE]
> Als u al een apparaat in uw oplossing hebt toegevoegd, kunt u deze stap overslaan. De volgende stap is echter vereist dat de verbindingsreeks van uw apparaat. U kunt ophalen van een apparaat connection string vanuit de [Azure-portal](https://portal.azure.com) of met behulp van de [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI-hulpprogramma.

Voor een apparaat verbinding maakt met de oplossingsversnellers, moet deze zelf identificeren bij IoT Hub met geldige referenties. U hebt de mogelijkheid om op te slaan de apparaatverbindingsreeks met deze referenties wanneer u het apparaat toevoegt de oplossing. U kunt de apparaatverbindingsreeks opnemen in uw clienttoepassing verderop in deze zelfstudie.

Als u wilt een apparaat toevoegt aan uw oplossing voor externe controle, voltooit u de volgende stappen uit op de **apparaten** pagina in de oplossing:

1. Kies **+ nieuwe apparaat**, en kies vervolgens **fysieke** als de **apparaattype**:

    ![Een fysiek apparaat toevoegen](media/iot-suite-selector-connecting/devicesprovision.png)

1. Voer **fysieke Koelunit** als de apparaat-ID. Kies de **symmetrische sleutel** en **automatisch sleutels genereren** opties:

    ![Apparaatopties voor selecteren](media/iot-suite-selector-connecting/devicesoptions.png)

1. Kies **toepassen**. Maak een notitie van de **apparaat-ID**, **primaire sleutel**, en **Connection string primaire sleutel** waarden:

    ![Referenties ophalen](media/iot-suite-selector-connecting/credentials.png)

U hebt nu een fysiek apparaat toegevoegd aan de oplossingsverbetering voor externe controle en de apparaatverbindingsreeks die u hebt genoteerd. In de volgende secties vindt u op de clienttoepassing die gebruikmaakt van de verbindingsreeks van het apparaat verbinding maken met uw oplossing implementeren.

De clienttoepassing implementeert de ingebouwde **Koelunit** Apparaatmodel. Een model solution accelerator apparaat Hiermee geeft u de volgende met betrekking tot een apparaat:

* De eigenschappen van het apparaat rapporteert aan de oplossing. Bijvoorbeeld, een **Koelunit** apparaat rapporteert informatie over de firmware en de locatie.
* De typen telemetrie wordt het apparaat verzendt naar de oplossing. Bijvoorbeeld, een **Koelunit** apparaat verzendt temperatuur, vochtigheid en druk te verlichten waarden.
* De methoden die u vanuit de oplossing plannen kunt om uit te voeren op het apparaat. Bijvoorbeeld, een **Koelunit** apparaat moet worden geïmplementeerd **opnieuw opstarten**, **FirmwareUpdate**, **EmergencyValveRelease**, en  **IncreasePressure** methoden.