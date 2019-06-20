---
title: Zelfstudie voor het instellen van Azure Data Box zware | Microsoft Docs
description: Leer hoe u bekabelen en verbinding maken met uw Azure Data Box zware
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: alkohli
ms.openlocfilehash: d4cdc9b6c31c62080fed5a8528577fef72c01c3f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271718"
---
# <a name="tutorial-cable-and-connect-to-your-azure-data-box-heavy-preview"></a>Zelfstudie: Bekabelen en maak verbinding met uw Azure Data Box zware (preview)


In deze zelfstudie wordt beschreven hoe u bekabelen, verbinden en uw Azure Data Box zware inschakelen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw Data Box-zwaar bekabelen
> * Verbinding maken met uw Data Box-zwaar

## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Azure Data Box-zwaar bestellen](data-box-heavy-deploy-ordered.md).
2. U hebt uw Data Box-zwaar ontvangen en de status van de volgorde in de portal **geleverd**.
3. U hebt bekeken de [Data Box zware veiligheid richtlijnen](data-box-safety.md).
4. U moet toegang hebben tot een platte site in het datacenter met de nabijheid van een beschikbare netwerkverbinding die een apparaat met deze voetafdruk aankan. Dit apparaat kan niet worden geladen op een rek.
5. U hebt vier gevestigd stroomkabels voor gebruik met uw apparaat ontvangen.
6. Er is een hostcomputer verbonden met het datacenternetwerk. Uw Data Box-zwaar kopieert u de gegevens van deze computer. De hostcomputer moet worden uitgevoerd een [ondersteund besturingssysteem](data-box-heavy-system-requirements.md).
7. Uw datacenter moet een netwerk met hoge snelheid hebben. Het wordt aangeraden dat u beschikt over minstens één 10-GbE-verbinding. 
8. U moet een laptop met RJ-45-kabel verbinding maken met de gebruikersinterface van lokale en het apparaat configureren. Gebruik de laptop op elk knooppunt van het apparaat eenmaal configureren.
9. U moet een 40 Gbps-kabel of 10 Gbps-kabel per apparaatknooppunt.
    - Kies kabels die compatibel met zijn de [Mellanox MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) netwerkinterface.
    - Voor de kabel 40 Gbps moet apparaat einde van de kabel QSFP +.
    - Voor de kabel 10 Gbps moet u een SFP +-kabel die in een switch 10 Gbps aan het ene uiteinde, met een QSFP + SFP + adapter (of de adapter QSA) voor het end die wordt aangesloten op het apparaat.

## <a name="cable-your-device-for-power"></a>Uw apparaat voor power bekabelen

Voer de volgende stappen uit op uw apparaat bekabelen.

1. Controleer of er met het apparaat is geknoeid en of het is beschadigd. Als er met het apparaat is geknoeid of het ernstig is beschadigd, gaat u niet verder. [Neem contact op met Microsoft Support](data-box-disk-contact-microsoft-support.md) onmiddellijk te helpen u bij het bepalen of het apparaat zich in goede staat en als ze moeten u een vervangend.
2. Het apparaat naar de installatiesite verplaatsen.

    ![Data Box-zwaar apparaat installatie van site](media/data-box-heavy-deploy-set-up/data-box-heavy-install-site.png)

3. De achterzijde casters vergrendeld op het apparaat, zoals hieronder weergegeven.

    ![Data Box-zwaar apparaat casters vergrendeld](media/data-box-heavy-deploy-set-up/data-box-heavy-casters-locked.png)

4. Ga naar de knoppen die de front-end en de back-deuren van het apparaat te ontgrendelen. Ontgrendelen en verplaats de voordeur totdat deze uitgelijnd met de kant van het apparaat wordt. Herhaal deze procedure met de achterdeur ook.
    Zowel de deuren moeten geopend blijven wanneer het apparaat is operationeel om toe te staan voor optimale naar achteren lucht flow via het apparaat.

    ![Data Box-zwaar deuren openen](media/data-box-heavy-deploy-set-up/data-box-heavy-doors-open.png)

5. De lade aan het einde van het apparaat hebt vier power-kabels. Verwijderen van alle kabels vanuit de lade en plaats deze bewaard.

    ![Data Box-zwaar stroomkabels in de lade](media/data-box-heavy-deploy-set-up/data-box-heavy-power-cords-tray.png)

6. De volgende stap is het identificeren van de verschillende poorten aan het einde van het apparaat. Er zijn twee apparaat knooppunten **knooppunt1** en **Knooppunt2**. Elk knooppunt heeft vier netwerkinterfaces, **MGMT**, **bestand1**, **DATA2**, **DATA3**. **MGMT** beheer configureren tijdens de eerste configuratie van het apparaat wordt gebruikt. **Bestand1**-**DATA3** gegevenspoorten zijn. **MGMT** en **DATA3** poorten zijn 1 Gbps-, terwijl **bestand1**, **DATA2** als 40 Gbps-poorten of 10 Gbps-poorten kunt werken. Aan de onderkant van het apparaat twee knooppunten zijn vier power supply eenheden (PSUs) die worden gedeeld op het apparaat twee knooppunten. Als u dit apparaat te maken krijgt de **PSUs** zijn **PSU1**, **PSU2**, **PSU3**, en **PSU4** van links naar rechts.

    ![Data Box-zwaar poorten](media/data-box-heavy-deploy-set-up/data-box-heavy-ports.png)

7. Alle vier power-kabels verbinden met de apparaat-voedingen. De groene LED's inschakelen en knipperen.
8. Gebruik de knoppen power in het vlak van front om in te schakelen op de knooppunten van het apparaat. De / uit-knop ingedrukt houden voor een paar seconden totdat de blauwe verlichting op. Nu moet alle groene LED's voor de voedingen aan de achterzijde van het apparaat vast. Het operationele voorpaneel van het apparaat bevat ook fouttolerantie-LED's. Als er een fout die LED wordt belicht, betekent dit een defecte voeding of een fan of een probleem met de schijven.  

    ![Data Box-zwaar front ops deelvenster](media/data-box-heavy-deploy-set-up/data-box-heavy-front-ops-panel.png)

## <a name="cable-first-node-for-network"></a>Eerste knooppunt van de kabel voor netwerk

Op een van de knooppunten van het apparaat, moet u de volgende stappen uitvoeren voor netwerk-kabel.

1. Gebruik een CAT 6 RJ-45-netwerkkabel (blauw kabel in de afbeelding) de computer verbinden met de 1 Gbps beheerpoort aan.
2. Verbinding maken ten minste één netwerkinterface van 40 Gbps (beter dan 1 Gbps) voor gegevens via een kabel QSFP + (fiber of koper). Als u een switch 10 Gbps, verbinding maken met de 40 Gbps-netwerkinterface voor gegevens via een kabel SFP + met een QSFP + SFP + adapter (de adapter QSA).

    ![Data Box-zwaar poorten bekabelde](media/data-box-heavy-deploy-set-up/data-box-heavy-ports-cabled.png)

    > [!IMPORTANT]
    > GEGEVENS 1 en DATA2 worden overgeschakeld en komen niet overeen met wat wordt weergegeven in de lokale webgebruikersinterface.
    > De 40 Gbps-kabel-adapter is verbonden bij het invoegen van de manier zoals hieronder wordt weergegeven.

    ![Data Box-zwaar 40 Gbps-kabel adapter](media/data-box-heavy-deploy-set-up/data-box-heavy-cable-adaptor.png)

## <a name="configure-first-node"></a>Eerste knooppunt configureren

De volgende stappen voor het instellen van uw apparaat met behulp van de lokale configuratie en de Azure-portal.

1. Download de referenties van het apparaat vanuit de portal. Ga naar **Algemeen > Apparaatdetails**. Kopieer het **Apparaatwachtwoord**. Deze wachtwoorden zijn gekoppeld aan een specifieke volgorde in de portal. Overeenkomt met de twee knooppunten in Data Box-zwaar, ziet u de serienummers van de twee apparaten. Het beheerderswachtwoord voor beide knooppunten is hetzelfde.

    ![Data Box-zwaar apparaatreferenties](media/data-box-heavy-deploy-set-up/data-box-heavy-device-credentials.png)

2. Uw clientwerkstation verbinden met het apparaat via een CAT6 RJ-45-netwerkkabel.
3. Configureer de Ethernet-adapter op de computer die u verbinding maken met een apparaat met een statisch IP-adres van `192.168.100.5` en subnet `255.255.255.0`.

    ![Gegevens in het zware maakt verbinding met de lokale web-UI](media/data-box-heavy-deploy-set-up/data-box-heavy-connect-local-web-ui.png)

4. Verbinding maken met de lokale web-UI van het apparaat via de volgende URL: `http://192.168.100.10`. Klik op **Geavanceerd** en klik vervolgens op **gaat u verder met 192.168.100.10 (onveilige)** .
5. U ziet een pagina **Aanmelden** voor de lokale webgebruikersinterface.
    
    - Een van de serienummers knooppunt op deze pagina komt overeen met in de gebruikersinterface van de portal en de lokale webgebruikersinterface. Noteer het aantal knooppunten aan de toewijzing van het serienummer wordt geverifieerd. Er zijn twee knooppunten en twee serienummers van apparaten in de portal. Deze toewijzing helpt u begrijpen welk knooppunt komt overeen met welke serienummer.
    - Het apparaat is op dit moment vergrendeld.
    - Geef het beheerderswachtwoord voor het apparaat dat u hebt verkregen in de vorige stap aan te melden bij het apparaat. Klik op **Aanmelden**.

    ![Aanmelden bij de gegevens in het zware lokale web-UI](media/data-box-heavy-deploy-set-up/data-box-heavy-unlock-device.png)

5. Zorg ervoor dat de netwerkinterfaces zijn geconfigureerd op het Dashboard. Er zijn vier netwerkinterfaces op uw apparaatknooppunt, twee 1 Gbps en twee 40 Gbps. Een van de 1 Gbps-interface een beheerinterface is en kan daarom geen gebruiker worden geconfigureerd. De resterende drie netwerkinterfaces zijn toegewezen aan gegevens en kunnen worden geconfigureerd door de gebruiker.

- Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd.
- Als DHCP niet is ingeschakeld, gaat u naar netwerkinterfaces instellen en statische IP-adressen toewijzen, indien nodig.

    ![Data Box-zwaar dashboard knooppunt 1](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-1.png)

## <a name="configure-second-node"></a>Tweede knooppunt configureren

Voer de stappen die worden beschreven de [configureren van het eerste knooppunt](#configure-first-node) voor het tweede knooppunt van het apparaat.

![Data Box-zwaar dashboard knooppunt 2](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-2.png)

Zodra de installatie van het apparaat is voltooid, kunt u verbinding maken met de apparaatshares en de gegevens van uw computer naar het apparaat kopiëren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over Azure Data Box zware onderwerpen, zoals:

> [!div class="checklist"]
> * Uw Data Box-zwaar bekabelen
> * Verbinding maken met uw Data Box-zwaar

Ga naar de volgende zelfstudie voor informatie over het kopiëren van gegevens op uw Data Box-zwaar.

> [!div class="nextstepaction"]
> [Uw gegevens naar Azure Data Box kopiëren](./data-box-heavy-deploy-copy-data.md)
