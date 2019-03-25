---
title: Azure Data Box Edge rekenbeheer | Microsoft Docs
description: Beschrijft hoe u de Edge-compute-instellingen zoals de trigger, modules en configuratie van compute beheren, verwijdert u configuratie via de Azure-portal voor uw Azure Data Box-edge-apparaten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/24/2019
ms.author: alkohli
ms.openlocfilehash: de8ddd0d2886cd7798160e830094b295c62e17e9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400686"
---
# <a name="manage-compute-on-your-azure-data-box-edge"></a>Compute voor uw Azure Data Box-edge-apparaten beheren

In dit artikel wordt beschreven hoe u compute voor uw Azure Data Box-edge-apparaten beheren. U kunt beheren van de compute via Azure portal of via de lokale webgebruikersinterface. De Azure-portal gebruiken voor het beheren van modules, triggers en compute-configuratie en de lokale web-UI voor het beheren van instellingen voor compute.

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.


In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Triggers beheren
> * Rekenintensieve configuratie beheren


## <a name="manage-triggers"></a>Triggers beheren

Gebeurtenissen zijn dingen die plaatsvinden binnen uw cloudomgeving of op uw apparaat die u wilt mogelijk actie ondernemen. Bijvoorbeeld, wanneer een bestand wordt gemaakt in een share, is er een gebeurtenis. Triggers zijn reactie op deze gebeurtenissen. Triggers kunnen helpen bij het uitvoeren van een functie wanneer de gebeurtenis wordt geactiveerd. Triggers kunnen voor uw Data Box-Edge zijn reactie op gebeurtenissen van bestand of een schema.

- **Bestand**: Deze triggers zijn in reactie op gebeurtenissen bestand, zoals het maken van een bestand, het wijzigen van een bestand.
- **Geplande**: Deze triggers zijn in reactie op een schema dat u met een startdatum, begintijd en het herhalingsinterval kunt definiëren.


### <a name="add-a-trigger"></a>Een trigger toevoegen

De volgende stappen uitvoeren in Azure portal om te maken van een trigger.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Edge compute > Trigger**. Selecteer **+ toevoegen trigger** op de opdrachtbalk.

    ![Selecteer de trigger toevoegen](media/data-box-edge-manage-compute/add-trigger-1.png)

2. In **toevoegen trigger** blade, Geef een unieke naam voor de trigger.
    
    <!--Trigger names can only contain numbers, lowercase letters, and hyphens. The share name must be between 3 and 63 characters long and begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character.-->

3. Selecteer een **Type** voor de trigger. Kies **bestand** wanneer de trigger is in reactie op een bestandgebeurtenis. Selecteer **geplande** als u wilt dat de trigger starten op een gedefinieerde periode en uitvoeren met een bepaald interval herhalen. Afhankelijk van uw keuze, wordt een andere set met opties weergegeven.

    - **Bestand trigger** -kiezen uit de vervolgkeuzelijst een gekoppelde share. Wanneer een bestandgebeurtenis wordt geactiveerd in deze share, zou de trigger in een Azure-functie.

        ![SMB-share toevoegen](media/data-box-edge-manage-compute/add-file-trigger.png)

    - **Geplande trigger** : Geef de startdatum /-tijd, en het herhalingsinterval in uren, minuten of seconden. Geef ook de naam voor een onderwerp. Een onderwerp krijgt u de flexibiliteit voor het routeren van de trigger met een module die is geïmplementeerd op het apparaat.

        Een voorbeeld van de route-tekenreeks is: `"route3": "FROM /* WHERE topic = 'topicname' INTO BrokeredEndpoint("modules/modulename/inputs/input1")"`.

        ![NFS-share toevoegen](media/data-box-edge-manage-compute/add-scheduled-trigger.png)

4. Selecteer **toevoegen** te maken van de trigger. Een melding ziet dat het maken van de trigger uitgevoerd wordt. Nadat de trigger is gemaakt, wordt de blade bijgewerkt om de nieuwe trigger weer te geven.
 
    ![Triggerlijst met bijgewerkte](media/data-box-edge-manage-compute/add-trigger-2.png)

### <a name="delete-a-trigger"></a>Een trigger verwijderen

De volgende stappen uitvoeren in Azure portal om te verwijderen van een trigger.

1. Selecteer de trigger die u wilt verwijderen uit de lijst met triggers.

    ![Trigger selecteren](media/data-box-edge-manage-compute/add-trigger-1.png)

2. Met de rechtermuisknop op en selecteer vervolgens **verwijderen**.

    ![Selecteer verwijderen](media/data-box-edge-manage-compute/add-trigger-1.png)

3. Klik op **Ja** als u om bevestiging wordt gevraagd.

    ![De verwijdering bevestigen](media/data-box-edge-manage-compute/add-trigger-1.png)

De lijst met updates van triggers in overeenstemming met het verwijderen.

## <a name="manage-compute-configuration"></a>Rekenintensieve configuratie beheren

Azure portal gebruiken om te bekijken van de compute-configuratie verwijderen van een bestaande configuratie van de compute, of vernieuwen van de compute-configuratie om te synchroniseren van toegangssleutels voor de IoT-apparaat en IoT Edge-apparaat voor uw Data Box-Edge.

### <a name="view-compute-configuration"></a>Compute-configuratie weergeven

De volgende stappen uitvoeren in Azure portal om de configuratie van de rekenkracht voor uw apparaat weer te geven.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Edge compute > Modules**. Selecteer **weergeven compute** op de opdrachtbalk.

    ![Selecteer Weergave berekenen](media/data-box-edge-manage-compute/view-compute-1.png)

2. Noteer de rekenconfiguratie op uw apparaat. Wanneer u compute hebt geconfigureerd, kunt u een IoT Hub-resource gemaakt. Onder deze IoT Hub-resource, zijn een IoT-apparaat en een IoT Edge-apparaat geconfigureerd. Alleen de Linux-modules worden ondersteund om uit te voeren op het IoT Edge-apparaat.

    ![Configuratie weergeven](media/data-box-edge-manage-compute/view-compute-2.png)


### <a name="remove-compute-configuration"></a>Rekenintensieve configuratie verwijderen

De volgende stappen uitvoeren in Azure portal om te verwijderen van de bestaande Edge Computing-configuratie voor uw apparaat.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Edge compute > aan de slag**. Selecteer **verwijderen compute** op de opdrachtbalk.

    ![Selecteer berekenen verwijderen](media/data-box-edge-manage-compute/remove-compute-1.png)

2. Als u de compute-configuratie verwijderen, moet u uw apparaat configureren als u wilt compute opnieuw gebruiken. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja**.

    ![Selecteer berekenen verwijderen](media/data-box-edge-manage-compute/remove-compute-2.png)

### <a name="sync-up-iot-device-and-iot-edge-device-access-keys"></a>Synchroniseren van IoT-apparaat en IoT Edge-apparaat toegangssleutels

Wanneer u compute voor uw gegevens in het edge-apparaten configureert, worden een IoT-apparaat en een IoT Edge-apparaat gemaakt. Deze apparaten worden automatisch symmetrische toegangssleutels toegewezen. Als een aanbevolen beveiligingsprocedure, deze sleutels worden regelmatig gedraaid via de IoT Hub-service.

U kunt u deze sleutels draaien, gaat u naar de IoT Hub-service die u hebt gemaakt en selecteert u de IoT-apparaat of de IoT Edge-apparaat. Elk apparaat heeft een primaire toegangssleutel en een secundaire toegangssleutel. De primaire toegangssleutel toewijzen aan de secundaire toegangssleutel en vervolgens de primaire toegangssleutel opnieuw genereren.

Als uw IoT-apparaat en de sleutels van IoT Edge-apparaat is gedraaid, moet u de configuratie op de rand van uw gegevens in om op te halen van de meest recente toegangssleutels vernieuwen. De synchronisatie helpt het apparaat de meest recente sleutels ophalen voor uw IoT-apparaat en IoT Edge-apparaat. Data Box Edge maakt gebruik van alleen de primaire toegangssleutel.

De volgende stappen uitvoeren in Azure portal om te synchroniseren van de toegangssleutels voor uw apparaat.

1. In de Azure-portal, gaat u naar uw gegevens in het Edge-resource en ga vervolgens naar **Edge compute > aan de slag**. Selecteer **vernieuwen configuratie** op de opdrachtbalk.

    ![Selecteer vernieuwen configuratie](media/data-box-edge-manage-compute/refresh-configuration-1.png)

2. Selecteer **Ja** wanneer hierom wordt gevraagd om bevestiging.

     ![Selecteer Ja als u wordt gevraagd](media/data-box-edge-manage-compute/refresh-configuration-2.png)

3. Sluit het dialoogvenster wanneer de synchronisatie is voltooid.

## <a name="enable-a-network-interface-for-compute"></a>Inschakelen van een netwerkinterface voor compute

U moet mogelijk toegang tot een module die wordt uitgevoerd op uw gegevens in het Edge-apparaat. Voor toegang tot de module extern, moet u een IP-adres toewijzen aan een netwerkinterface op uw apparaat. U kunt deze beheren compute-instellingen van de lokale webgebruikersinterface.

De volgende stappen op de lokale web-UI compute-instellingen configureren.

1. Ga in de lokale webgebruikersinterface naar **Configuration > Compute instellingen**.  

2. **Schakel** de netwerkinterface die u gebruiken wilt voor verbinding met de compute-modules op het apparaat. 

    - Als u statische IP-adressen gebruikt, voert u een IP-adres voor de netwerkinterface.
    - Als u DHCP gebruikt, wordt de IP-adressen automatisch toegewezen.

3. Selecteer **toepassen** instellingen te kunnen toepassen.

    ![Schakel de compute-instellingen](media/data-box-edge-manage-compute/compute-settings-1.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Gebruikers beheren via Azure Portal](data-box-edge-manage-users.md).
