---
title: Update 0.6 installeren op de StorSimple Virtual Array | Microsoft Docs
description: Wordt beschreven hoe u de web-UI van de StorSimple Virtual Array updates met behulp van de Azure portal en de hotfix-methode toe te passen
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 111976cd684561f5bc63b92f09a20470fe3036d7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38606643"
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>Update 0.6 installeren op uw StorSimple Virtual Array

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de stappen die nodig zijn voor het installeren van Update 0.6 op uw StorSimple Virtual Array via de lokale webgebruikersinterface en via de Azure-portal. U past u de software-updates of hotfixes voor uw StorSimple Virtual Array om up-to-date te houden.

Voordat u een update toepast, wordt aangeraden dat u de volumes of shares offline op de host eerst neemt en klik vervolgens op het apparaat. Dit een mogelijkheid van beschadiging van gegevens wordt geminimaliseerd. Nadat de volumes of shares offline zijn, u moet ook rekening houden met een handmatige back-up van het apparaat.

> [!IMPORTANT]
> - Update 0.6 komt overeen met **10.0.10293.0** softwareversie op uw apparaat. Voor informatie over wat is er nieuw in deze update, gaat u naar [opmerkingen bij de Release van Update 0.6](storsimple-virtual-array-update-06-release-notes.md).
>
> - Als u Update 0.2 uitvoert of hoger, raden wij aan dat installeren u de updates via de Azure-portal. Als u Update 0.1 of GA softwareversies worden uitgevoerd, moet u de hotfix-methode via de lokale webgebruikersinterface Update 0.6 installeren.
>
> - Houd er rekening mee dat een update of hotfix installeert uw apparaat opnieuw wordt opgestart. Gezien het feit dat de StorSimple Virtual Array een apparaat met één knooppunt is, een i/o bezig is onderbroken en uw apparaat ondervindt uitvaltijd.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Als met Update 0,2 en hoger, wordt u aangeraden dat u updates via de Azure-portal installeert. De portal procedure moet de gebruiker om te scannen, downloaden en installeer vervolgens de updates. Deze procedure duurt ongeveer 7 minuten om te voltooien. Voer de volgende stappen uit voor het installeren van de update of hotfix.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Nadat de installatie voltooid is, gaat u naar uw StorSimple Device Manager-service. Selecteer **apparaten** en selecteer en klik op het apparaat dat u zojuist hebt bijgewerkt. Ga naar **instellingen > beheren > apparaatupdates**. De weergegeven software-versie moet **10.0.10293.0**.

## <a name="use-the-local-web-ui"></a>Gebruik de lokale web-UI

Er zijn twee stappen bij het gebruik van de lokale webgebruikersinterface:

* Download de update of hotfix
* Installeer de update of de hotfix

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Voer de volgende stappen uit om de software-update te downloaden uit de Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>De update of de hotfix te downloaden

1. Start Internet Explorer en navigeer naar [ http://catalog.update.microsoft.com ](http://catalog.update.microsoft.com).

2. Als u van de Microsoft Update-catalogus voor de eerste keer op deze computer gebruikmaakt, klikt u op **installeren** wanneer hierom wordt gevraagd voor het installeren van de invoegtoepassing Microsoft Update-catalogus.

3. Voer in het zoekvak van de Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u wilt downloaden. Voer **4023268** voor Update 0.6 en klik vervolgens op **zoeken**.
   
    De lijst met hotfixes wordt weergegeven, bijvoorbeeld **StorSimple Virtual Array Update 0.6**.
   
    ![Catalogus doorzoeken](./media/storsimple-virtual-array-install-update-06/download1.png)

4. Klik op **Downloaden**.

5. U ziet vijf bestanden te downloaden. Elk van deze bestanden downloaden naar een map. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.

6. Open de map waar de bestanden zich bevinden.
    ![Bestanden in het pakket](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    U ziet:
    -  Een pakketbestand van Microsoft Update zelfstandige `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt om bij te werken software van het apparaat.
    - Een pakketbestand Genève Monitoring Agent `GenevaMonitoringAgentPackageInstaller`. Dit bestand wordt gebruikt om bij te werken van de controle en diagnose Services (MDS)-agent. Dubbelklik op het cab-bestand. Een _.msi_ wordt weergegeven. Selecteer het bestand met de rechtermuisknop op, en vervolgens **extraheren** het bestand. U gebruikt de _.msi_ -bestand naar de agent wordt bijgewerkt.

        ![Agentupdate MDS-bestand uitpakken](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > U hoeft niet de MDS-agent bijwerken als waarop StorSimple Update 0.5 (0.0.10293.0) wordt uitgevoerd.

    - Drie bestanden met essentiële updates voor Windows-beveiliging, `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, en `windows8.1-kb4019213-x64`.


### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Voordat u de update of hotfix installeren, zorgen ervoor dat u de update hebt of de hotfix gedownload lokaal op de host of toegankelijk via een netwerkshare.

Gebruik deze methode updates installeren op een apparaat met GA of bijwerken van 0,1 softwareversies. Deze procedure duurt ongeveer 12 minuten om te voltooien. Voer de volgende stappen uit voor het installeren van de update of hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **Software-Update**. Noteer de versie van de software die u uitvoert. Als u werkt met **10.0.10290.0**, u hoeft niet bijwerken van de MDS-agent in stap 6.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. In **Update bestandspad**, voer de naam van de update of hotfix. U kunt ook bladeren naar het installatiebestand van de update of hotfix als geplaatst op een netwerkshare. Klik op **Toepassen**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Een waarschuwing wordt weergegeven. Gezien de virtuele matrix is een apparaat met één knooppunt, nadat de update wordt toegepast, het apparaat opnieuw wordt opgestart en er uitvaltijd is. Klik op het vinkje.
   
   ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. De update wordt gestart. Wanneer het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale UI is niet toegankelijk in deze duur.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Nadat het opnieuw opstarten voltooid is, gaat u naar de **aanmelden** pagina. Als u wilt controleren of software van het apparaat is bijgewerkt, klikt u in de lokale web-UI, gaat u naar **onderhoud** > **Software-Update**. De weergegeven software-versie moet **10.0.0.0.0.10293** voor Update 0.6.
   
   > [!NOTE]
   > We rapport de softwareversies in een iets andere manier in de lokale web-UI en de Azure-portal. Bijvoorbeeld, de lokale webgebruikersinterface gerapporteerd **10.0.0.0.0.10293** en de Azure portal rapporten **10.0.10293.0** voor dezelfde versie.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. Deze stap overslaan als u de StorSimple Virtual Array Update 0.5 (**10.0.10290.0**) voordat u deze update toegepast. U hebt genoteerd een versie van de software in stap 1 voordat u begon met het om bij te werken. Als u Update 0.5 zijn uitgevoerd, is de MDS-agent al bijgewerkt.

    Als u een softwareversie vóór Update 0.5 uitvoert, wordt de volgende stap voor u is de MDS-agent bijwerken. In de **Software-Update** pagina, Ga naar de **Update bestandspad** en blader naar de `GenevaMonitoringAgentPackageInstaller.msi` bestand. Herhaal stappen 2-4. Nadat de virtuele matrix opnieuw is opgestart, meld u aan bij de lokale webgebruikersinterface.

7. Herhaal stap 2-4 voor het installeren van de Windows-beveiliging worden opgelost met behulp van bestanden `windows8.1-kb4012213-x64`,`windows8.1-kb3205400-x64`, en `windows8.1-kb4019213-x64`. De virtuele matrix wordt opnieuw opgestart na elke-installatie en u moet zich aanmelden bij de lokale webgebruikersinterface.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

