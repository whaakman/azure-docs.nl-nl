---
title: Update 1.0 installeren op de virtuele StorSimple-matrix | Microsoft Docs
description: Beschrijft hoe u de virtuele StorSimple-matrix-webgebruikersinterface toepassen van updates via de Azure portal en de hotfix-methode
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: a85290f3f56eb1e1bf57524c43c6d4fea36129f7
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="install-update-10-on-your-storsimple-virtual-array"></a>Update 1.0 installeren op uw virtuele StorSimple-matrix

## <a name="overview"></a>Overzicht

In dit artikel beschrijft de stappen voor het installeren van Update 1.0 op uw virtuele StorSimple-matrix via de lokale webgebruikersinterface en via de Azure-portal.

U toepassen de software-updates of hotfixes up-to-date houden van uw virtuele StorSimple-matrix. Voordat u een update hebt toegepast, wordt u aangeraden dat u rekening houden met de volumes of shares offline op de host eerst en klik vervolgens op het apparaat. Dit verkleint dat beschadigde gegevens. Nadat u de volumes of shares die offline is, u moet ook rekening houden met een handmatige back-up van het apparaat.

> [!IMPORTANT]
> - Update 1.0 komt overeen met **10.0.10296.0** softwareversie van de op uw apparaat. Voor informatie over wat is er nieuw in deze update, gaat u naar [Release-opmerkingen voor bijwerken 1.0](storsimple-virtual-array-update-1-release-notes.md).
>
> - Houd er rekening mee dat voor het installeren van een update of hotfix uw apparaat opnieuw wordt opgestart. Gezien het feit dat het virtuele StorSimple-matrix een apparaat met één knooppunt is, een i/o uitgevoerd wordt onderbroken en uw apparaat ervaringen uitvaltijd.
>
> - 1 update is beschikbaar in de Azure-portal alleen als de virtuele matrix Update 0,6 wordt uitgevoerd. Voor virtuele matrices met 0,6 versies vóór het bijwerken, moet u eerst Update 0,6 installeren en installeer vervolgens Update 1.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Als Update 0,2 en hoger, raden wij aan dat u updates via de Azure portal installeert. De portal procedure moet de gebruiker om te scannen, downloaden en installeer vervolgens de updates. Afhankelijk van de versie van de software die uw virtuele matrix wordt uitgevoerd, is toepassen van de update via de Azure-portal anders.

 - Als uw virtuele matrix wordt Update 0,6 uitgevoerd, installeert de Azure-portal Update 1 (10.0.10296.0) rechtstreeks op uw apparaat. Deze procedure duurt ongeveer 7 minuten is voltooid.
 - Als uw virtuele matrix een versie voorafgaand aan 0,6-Update wordt uitgevoerd, de update wordt uitgevoerd in twee fase. De Azure-portal eerst Update 0,6 (10.0.10293.0) is geïnstalleerd op uw apparaat. De virtuele matrix opnieuw wordt opgestart en de portal Update 1 (10.0.10296.0) installeert op uw apparaat. Deze procedure duurt ongeveer 15 minuten te voltooien.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-1.md)]

Nadat de installatie voltooid is, gaat u naar uw StorSimple-apparaat Manager-service. Selecteer **apparaten** en selecteer en klik op het apparaat dat u zojuist hebt bijgewerkt. Ga naar **instellingen > beheren > apparaatupdates**. De weergegeven software-versie moet **10.0.10296.0**.

![De versie van de software na de update](./media/storsimple-virtual-array-install-update-1/azupdate17m1.png)

## <a name="use-the-local-web-ui"></a>Gebruik de lokale webgebruikersinterface

Er zijn twee stappen wanneer u de lokale-webgebruikersinterface:

* Download de update of hotfix
* Installeer de update of de hotfix

> [!IMPORTANT] 
> **Met deze update alleen doorgaan als u Update 0,6 (10.0.10293.0) worden uitgevoerd. Als u een eerdere versie uitvoert [installeert u Update 0,6](storsimple-virtual-array-install-update-06.md) op uw apparaat eerst en installeer vervolgens Update 1.**

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Als uw virtuele matrix wordt Update 0,6 uitgevoerd, voert u de volgende stappen uit om de Update 1 downloaden vanaf Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>De update of de hotfix te downloaden

1. Start Internet Explorer en blader naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Als u de Microsoft Update Catalog voor het eerst op deze computer gebruikt, klikt u op **installeren** wanneer u wordt gevraagd voor het installeren van de invoegtoepassing voor Microsoft Update-catalogus.

3. Voer in het zoekvak van Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u wilt downloaden. Voer **4047203** voor Update 1.0 en klik vervolgens op **Search**.
   
    De hotfix-wordt weergegeven, bijvoorbeeld **StorSimple virtuele matrix Update 1.0**.
   
    ![Catalogus doorzoeken](./media/storsimple-virtual-array-install-update-1/download1.png)

4. Klik op **Downloaden**.

5. De twee bestanden downloaden naar een map. U kunt ook de map kopiëren naar een netwerkshare zijn die bereikbaar is vanaf het apparaat.

6. Open de map waar de bestanden zich bevinden.

    ![Bestanden in het pakket](./media/storsimple-virtual-array-install-update-1/update01folder.png)

    Ziet u twee bestanden:
    -  Een pakketbestand van Microsoft Update zelfstandige `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt voor het bijwerken van de software van het apparaat.
    - Een bestand met de cumulatieve updates voor augustus `windows8.1-kb4034681-x64`. Voor meer informatie over wat is opgenomen in deze rollup, gaat u naar [totalisering van de augustus maandelijkse beveiliging](https://support.microsoft.com/help/4034681/windows-8-1-windows-server-2012-r2-update-kb40346810).

### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Voordat u de update of hotfix, zorg ervoor dat:

 - U hebt de update of de hotfix die is gedownload lokaal op de host of toegankelijk via een netwerkshare.
 - Uw virtuele matrix kan Update 0,6 (10.0.10293.0) wordt uitgevoerd. Als u een versie vóór Update 0,6, [installeert u Update 0,6](storsimple-virtual-array-install-update-06.md) eerste en installeer vervolgens Update 1.

Deze procedure duurt ongeveer 4 minuten is voltooid. Voer de volgende stappen uit voor het installeren van de update of hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **Software-Update**. Noteer de softwareversie die u uitvoert. **Met deze update alleen doorgaan als u Update 0,6 (10.0.10293.0) worden uitgevoerd. Als u een eerdere versie uitvoert [installeert u Update 0,6](storsimple-virtual-array-install-update-06.md) op uw apparaat eerst en installeer vervolgens Update 1.**
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update1m.png)

2. In **Update bestandspad**, geef de bestandsnaam voor de update of hotfix. U kunt ook bladeren naar het installatiebestand update of hotfix als geplaatst op een netwerkshare. Klik op **Toepassen**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update2m.png)

3. Een waarschuwing wordt weergegeven. Gegeven de virtuele matrix is een apparaat met één knooppunt, nadat de update is toegepast, het apparaat opnieuw wordt opgestart en er uitvaltijd is. Klik op het vinkje.
   
   ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update3m.png)

4. De update wordt gestart. Nadat het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale gebruikersinterface is niet toegankelijk in deze duur.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update5m.png)

5. Nadat het opnieuw opstarten voltooid is, gaat u naar de **aanmelden** pagina. Om te controleren of software van het apparaat is bijgewerkt, in de lokale webgebruikersinterface, gaat u naar **onderhoud** > **Software-Update**. De weergegeven software-versie moet **10.0.0.0.0.10296** voor Update 1.0.
   
   > [!NOTE]
   > We rapport de softwareversies in een iets andere manier in de lokale webgebruikersinterface en de Azure portal. Bijvoorbeeld: de lokale webgebruikersinterface rapporten **10.0.0.0.0.10296** en de Azure portal rapporten **10.0.10296.0** voor dezelfde versie.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update6m.png)

6. Herhaal stappen 2-4 voor het installeren van de Windows-security-oplossing met behulp van bestand `windows8.1-kb4012213-x64`. De virtuele matrix wordt opnieuw opgestart na de installatie en moet u zich aanmeldt bij de lokale webgebruikersinterface.

> [!NOTE]
> Als u Update 1 direct worden toegepast op een apparaat met een eerdere versie dan Update 0,6, kunt u sommige updates ontbreken. Neem contact op met Microsoft Support voor volgende stappen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheer van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).
