---
title: Update 0,5 installeren op de virtuele StorSimple-matrix | Microsoft Docs
description: Beschrijft hoe u de virtuele StorSimple-matrix-webgebruikersinterface toepassen van updates via de Azure portal en de hotfix-methode
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/10/2017
ms.author: alkohli
ms.openlocfilehash: c47da5b90c16e2d5b5709e2a6affc026238b9468
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-update-05-on-your-storsimple-virtual-array"></a>Update 0,5 installeren op uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

In dit artikel beschrijft de stappen voor het installeren van Update 0,5 op uw virtuele StorSimple-matrix via de lokale webgebruikersinterface en via de Azure-portal. U moet toepassen van software-updates of hotfixes voor uw virtuele StorSimple-matrix up-to-date te houden.

Voordat u een update hebt toegepast, wordt u aangeraden dat u rekening houden met de volumes of shares offline op de host eerst en klik vervolgens op het apparaat. Dit verkleint dat beschadigde gegevens. Nadat u de volumes of shares die offline is, u moet ook rekening houden met een handmatige back-up van het apparaat.

> [!IMPORTANT]
> - Update 0,5 komt overeen met **10.0.10290.0** softwareversie van de op uw apparaat. Voor informatie over wat is er nieuw in deze update, gaat u naar [Release-opmerkingen voor Update 0,5](storsimple-virtual-array-update-05-release-notes.md).
>
> - Als u Update 0.2 uitvoert of hoger, we raden installeren u de updates via de Azure-portal. Als u Update 0.1 of GA softwareversies uitvoert, moet u de hotfix-methode via de lokale webgebruikersinterface 0,5 Update te installeren.
>
> - Houd er rekening mee dat voor het installeren van een update of hotfix uw apparaat opnieuw wordt opgestart. Gezien het feit dat het virtuele StorSimple-matrix een apparaat met één knooppunt is, een i/o uitgevoerd wordt onderbroken en uw apparaat ervaringen uitvaltijd.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Als Update 0,2 en hoger, raden wij aan dat u updates via de Azure portal installeert. De portal procedure moet de gebruiker om te scannen, downloaden en installeer vervolgens de updates. Deze procedure duurt ongeveer 7 minuten is voltooid. Voer de volgende stappen uit voor het installeren van de update of hotfix.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Nadat de installatie voltooid is, gaat u naar uw StorSimple-apparaat Manager-service. Selecteer **apparaten** en selecteer en klik op het apparaat dat u zojuist hebt bijgewerkt. Ga naar **instellingen > beheren > apparaatupdates**. De weergegeven software-versie moet **10.0.10290.0**.

## <a name="use-the-local-web-ui"></a>Gebruik de lokale webgebruikersinterface

Er zijn twee stappen wanneer u de lokale-webgebruikersinterface:

* Download de update of hotfix
* Installeer de update of de hotfix

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Voer de volgende stappen uit om de software-update te downloaden uit de Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>De update of de hotfix te downloaden

1. Start Internet Explorer en blader naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Als dit de eerste keer is dat u de Microsoft Update-catalogus op deze computer gebruikt, klikt u op **Installeren** wanneer u wordt gevraagd of u de invoegtoepassing voor de Microsoft Update-catalogus wilt installeren.

3. Voer in het zoekvak van Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u wilt downloaden. Voer **4021576** voor Update 0,5 en klik vervolgens op **Search**.
   
    De hotfix-wordt weergegeven, bijvoorbeeld **StorSimple virtuele matrix Update 0,5**.
   
    ![Catalogus doorzoeken](./media/storsimple-virtual-array-install-update-05/download1.png)

4. Klik op **Downloaden**. 

5. Ziet u twee bestanden kunnen downloaden, een *MSU* en een *cab* bestand. Elk van deze bestanden downloaden naar een map. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.

6. Open de map waar de bestanden zich bevinden.
    ![Bestanden in het pakket](./media/storsimple-virtual-array-install-update-05/update05folder.png)

    U ziet:
    -  Een pakketbestand van Microsoft Update zelfstandige `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt voor het bijwerken van de software van het apparaat.
    - Een pakketbestand Geneva Monitoring Agent `GenevaMonitoringAgentPackageInstaller`. Dit bestand wordt gebruikt voor de agent-service (MDS) van de controle en diagnostische gegevens bijwerken. Dubbelklik op het cab-bestand. Een .msi wordt weergegeven. Selecteer het bestand met de rechtermuisknop en vervolgens **extraheren** het bestand. U gebruikt de _.msi_ bestand bijwerken van de agent.

        ![Update van Clientagent MDS-bestand uitpakken](./media/storsimple-virtual-array-install-update-05/extract-geneva-monitoring-agent-installer.png)
        
    

### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Controleer of dat u de update hebt of de hotfix gedownload lokaal op de host of toegankelijk via een netwerkshare voor de installatie van de update of hotfix.

Gebruik deze methode updates installeren op een apparaat met GA of 0,1 softwareversies bijwerken. Deze procedure duurt minder dan 2 minuten. Voer de volgende stappen uit voor het installeren van de update of hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **Software-Update**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. In **Update bestandspad**, geef de bestandsnaam voor de update of hotfix. U kunt ook bladeren naar het installatiebestand update of hotfix als geplaatst op een netwerkshare. Klik op **Toepassen**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Een waarschuwing wordt weergegeven. Gegeven dit is een apparaat met één knooppunt, nadat de update is toegepast, het apparaat opnieuw wordt opgestart en er uitvaltijd is. Klik op het vinkje.
   
   ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. De update wordt gestart. Nadat het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale gebruikersinterface is niet toegankelijk in deze duur.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Nadat het opnieuw opstarten voltooid is, gaat u naar de **aanmelden** pagina. Om te controleren of software van het apparaat is bijgewerkt, in de lokale webgebruikersinterface, gaat u naar **onderhoud** > **Software-Update**. De weergegeven software-versie moet **10.0.0.0.0.10290.0** voor Update 0,5.
   
   > [!NOTE]
   > We rapport de softwareversies in een iets andere manier in de lokale webgebruikersinterface en de Azure portal. Bijvoorbeeld: de lokale webgebruikersinterface rapporten **10.0.0.0.0.10290** en de Azure portal rapporten **10.0.10290.0** voor dezelfde versie.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-05/update6m.png)

6. De volgende stap is de MDS-agent bijwerken. In de **Software-Update** pagina, gaat u naar de **Update bestandspad** en blader naar de `GenevaMonitoringAgentPackageInstaller.msi` bestand. Herhaal stappen 2 tot 4. Nadat de virtuele matrix opnieuw is opgestart, meldt u zich in de lokale webgebruikersinterface.

De update is nu voltooid.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheer van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

