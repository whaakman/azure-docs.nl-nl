---
title: Microsoft Azure Data Box beheren via de lokale webinterface | Microsoft Docs in gegevens
description: Beschrijft hoe u de lokale webinterface gebruikt om uw Data Box-apparaat te beheren.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/11/2018
ms.author: alkohli
ms.openlocfilehash: a5f3f1b4d0d67a12ee0ebaa40935b8494e53dab3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956299"
---
# <a name="use-the-local-web-ui-to-administer-your-data-box"></a>De lokale webinterface gebruiken om uw Data Box beheren

In dit artikel worden enkele van de complexe werkstromen en beheertaken beschreven die kunnen worden uitgevoerd voor de Data Box. U kunt de Data Box beheren via de gebruikersinterface van Azure Portal en de lokale webinterface voor het apparaat. Dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de lokale webinterface.

De lokale webinterface voor de Data Box wordt gebruikt voor de initiële configuratie van het apparaat. U kunt de lokale webinterface ook gebruiken om de Data Box af te sluiten of opnieuw te starten, diagnostische tests uit te voeren, software bij te werken, logboeken te bekijken en een logboekpakket voor Microsoft Ondersteuning te genereren.

Dit artikel bevat de volgende zelfstudies:

- Een ondersteuningspakket genereren
- Apparaat uitschakelen of opnieuw opstarten
- Gegevens veilig wissen van uw apparaat
- Beschikbare capaciteit van het apparaat monitoren
- De controlesomvalidatie overslaan 

## <a name="generate-support-package"></a>Ondersteuningspakket genereren

Als u problemen met het apparaat ondervindt, kunt u een ondersteuningspakket maken vanuit het systeemlogboek. Microsoft Ondersteuning gebruikt dit pakket om het probleem op te lossen. Ga als volgt te werk om een ondersteuningspakket te genereren:

1. Ga in de lokale webinterface naar **Contact opnemen met de ondersteuning**  en klik op **Ondersteuningspakket maken**.

    ![Ondersteuningspakket maken 1](media/data-box-local-web-ui-admin/create-support-package-1.png)

2. Er wordt een ondersteuningspakket verzameld. Deze bewerking duurt enkele minuten.

    ![Ondersteuningspakket maken 2](media/data-box-local-web-ui-admin/create-support-package-2.png)

3. Nadat het maken van het ondersteuningspakket is voltooid, klikt u op **Ondersteuningspakket downloaden**. 

    ![Ondersteuningspakket maken 4](media/data-box-local-web-ui-admin/create-support-package-4.png)

4. Blader naar en kies de downloadlocatie. Open de map om de inhoud weer te geven.

    ![Ondersteuningspakket maken 5](media/data-box-local-web-ui-admin/create-support-package-5.png)


## <a name="shut-down-or-restart-your-device"></a>Apparaat uitschakelen of opnieuw opstarten

U kunt uw Data Box afsluiten of opnieuw starten met de lokale webinterface. We raden u aan de shares vóór de herstart offline te zetten op de host en vervolgens op het apparaat. Dit minimaliseert de kans op beschadigde gegevens. Zorg ervoor dat er geen gegevens worden gekopieerd wanneer u het apparaat afsluit.

Voer de volgende stappen uit om uw Data Box af te sluiten.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Afsluiten**.

    ![Data Box afsluiten 1](media/data-box-local-web-ui-admin/shut-down-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

    ![Data Box afsluiten 2](media/data-box-local-web-ui-admin/shut-down-local-web-ui-2.png)

Nadat het apparaat is uitgeschakeld, gebruikt u de aan-uitknop op het voorpaneel om het apparaat in te schakelen.

Voer de volgende stappen uit om uw Data Box opnieuw op te starten.

1. Ga in de lokale webinterface naar **Afsluiten of opnieuw opstarten**.
2. Klik op **Opnieuw opstarten**.

    ![Data Box opnieuw opstarten 1](media/data-box-local-web-ui-admin/restart-local-web-ui-1.png)

3. Klik op **OK** als u om bevestiging wordt gevraagd.

   Het apparaat wordt afgesloten en opnieuw opgestart.

## <a name="view-available-capacity-of-the-device"></a>Beschikbare capaciteit van het apparaat weergeven

U kunt het apparaatdashboard gebruiken om de beschikbare en gebruikte capaciteit van het apparaat weer te geven. 

1. Ga in de lokale webinterface naar **Dashboard weergeven**.
2. Onder **Verbinding maken en kopiëren** wordt de beschikbare en gebruikte ruimte op het apparaat weergegeven.

    ![Beschikbare capaciteit weergeven](media/data-box-local-web-ui-admin/verify-used-space-dashboard.png)


## <a name="skip-checksum-validation"></a>De controlesomvalidatie overslaan

Een controlesom wordt standaard gegenereerd voor uw gegevens tijdens de voorbereiding voor verzending. In sommige zeldzame gevallen, afhankelijk van het gegevenstype (kleine bestandsgrootten), kan dit erg lang duren. In dergelijke gevallen kunt u de controlesom overslaan. 

We raden u ten zeerste aan om de controlesom niet uit te schakelen, tenzij de prestaties te veel worden beïnvloed.

1. Ga naar Instellingen in de rechterbovenhoek van de lokale webinterface van uw apparaat.

    ![Controlesom uitschakelen](media/data-box-local-web-ui-admin/disable-checksum.png)

2. Controlesomvalidatie **Uitschakelen**
3. Klik op **Toepassen**.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [beheren van de Data Box Disk via Azure Portal](data-box-portal-admin.md).

