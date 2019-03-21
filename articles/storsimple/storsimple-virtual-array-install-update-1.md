---
title: Installeer Update 1.0 voor StorSimple Virtual Array | Microsoft Docs
description: Wordt beschreven hoe u de web-UI van de StorSimple Virtual Array updates met behulp van de Azure portal en de hotfix-methode toe te passen
services: storsimple
documentationcenter: NA
author: alkohli
manager: jconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: fa53213e577028628d48db91704578e23888f2a8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58013852"
---
# <a name="install-update-10-on-your-storsimple-virtual-array"></a>Installeer Update 1.0 op uw StorSimple Virtual Array

## <a name="overview"></a>Overzicht

Dit artikel beschrijft de stappen die nodig zijn voor het installeren van Update 1.0 op uw virtuele StorSimple-matrix via de lokale webgebruikersinterface en via de Azure-portal.

U past u de software-updates of hotfixes voor uw StorSimple Virtual Array om up-to-date te houden. Voordat u een update toepast, wordt aangeraden dat u de volumes of shares offline op de host eerst neemt en klik vervolgens op het apparaat. Dit minimaliseert de kans op beschadigde gegevens. Nadat de volumes of shares offline zijn, u moet ook rekening houden met een handmatige back-up van het apparaat.

> [!IMPORTANT]
> - Update 1.0 komt overeen met **10.0.10296.0** softwareversie op uw apparaat. Voor informatie over wat is er nieuw in deze update, gaat u naar [opmerkingen bij de Release van Update 1.0](storsimple-virtual-array-update-1-release-notes.md).
>
> - Houd er rekening mee dat een update of hotfix installeert uw apparaat opnieuw wordt opgestart. Gezien het feit dat de StorSimple Virtual Array een apparaat met één knooppunt is, een i/o bezig is onderbroken en uw apparaat ondervindt uitvaltijd.
>
> - Update 1 is beschikbaar in de Azure-portal alleen als de virtuele matrix Update 0.6 wordt uitgevoerd. Voor virtuele matrices met 0,6 versies vóór het bijwerken, moet u eerst Update 0.6 installeren en installeer vervolgens Update 1.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Als met Update 0,2 en hoger, wordt u aangeraden dat u updates via de Azure-portal installeert. De portal procedure moet de gebruiker om te scannen, downloaden en installeer vervolgens de updates. Afhankelijk van de versie van de software die uw virtuele array wordt uitgevoerd, is toepassen van de update via de Azure-portal anders.

 - Als uw virtuele array wordt Update 0.6 uitgevoerd, installeert de Azure-portal-Update 1 (10.0.10296.0) rechtstreeks op uw apparaat. Deze procedure duurt ongeveer 7 minuten om te voltooien.
 - Als uw virtuele array wordt een eerdere versie dan Update 0.6 uitgevoerd, is bijgewerkt in de fase van de twee. De Azure-portal eerst Update 0.6 (10.0.10293.0) is geïnstalleerd op uw apparaat. De virtuele matrix wordt opnieuw opgestart en de portal-Update 1 (10.0.10296.0) op uw apparaat installeert. Deze procedure duurt ongeveer 15 minuten om te voltooien.


[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-1.md)]

Nadat de installatie voltooid is, gaat u naar uw StorSimple Device Manager-service. Selecteer **apparaten** en selecteer en klik op het apparaat dat u zojuist hebt bijgewerkt. Ga naar **instellingen > beheren > apparaatupdates**. De weergegeven software-versie moet **10.0.10296.0**.

![Softwareversie na-update](./media/storsimple-virtual-array-install-update-1/azupdate17m1.png)

## <a name="use-the-local-web-ui"></a>Gebruik de lokale web-UI

Er zijn twee stappen bij het gebruik van de lokale webgebruikersinterface:

* Download de update of hotfix
* Installeer de update of de hotfix

> [!IMPORTANT] 
> **Met deze update alleen doorgaan als u Update 0.6 (10.0.10293.0) worden uitgevoerd. Als u een eerdere versie uitvoert [Installeer Update 0.6](storsimple-virtual-array-install-update-06.md) op het apparaat eerst, en vervolgens Update 1.**

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Als uw virtuele array wordt Update 0.6 uitgevoerd, de volgende stappen uitvoeren om Update 1 downloaden vanaf Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>De update of de hotfix te downloaden

1. Start Internet Explorer en navigeer naar [ https://catalog.update.microsoft.com ](https://catalog.update.microsoft.com).

2. Als u van de Microsoft Update-catalogus voor de eerste keer op deze computer gebruikmaakt, klikt u op **installeren** wanneer hierom wordt gevraagd voor het installeren van de invoegtoepassing Microsoft Update-catalogus.

3. Voer in het zoekvak van de Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u wilt downloaden. Voer **4047203** voor Update 1.0 en klik vervolgens op **zoeken**.
   
    De lijst met hotfixes wordt weergegeven, bijvoorbeeld **StorSimple Virtual Array Update 1.0**.
   
    ![Catalogus doorzoeken](./media/storsimple-virtual-array-install-update-1/download1.png)

4. Klik op **Downloaden**.

5. De twee bestanden downloaden naar een map. U kunt ook de map kopiëren naar een netwerkshare die bereikbaar is vanaf het apparaat.

6. Open de map waar de bestanden zich bevinden.

    ![Bestanden in het pakket](./media/storsimple-virtual-array-install-update-1/update01folder.png)

    U ziet u twee bestanden:
    -  Een pakketbestand van Microsoft Update zelfstandige `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt om bij te werken software van het apparaat.
    - Een bestand met de cumulatieve updates voor augustus `windows8.1-kb4034681-x64`. Voor meer informatie over wat is inbegrepen in deze rollup, gaat u naar [augustus maandelijkse security updatepakket](https://support.microsoft.com/help/4034681/windows-8-1-windows-server-2012-r2-update-kb40346810).

### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Voordat u de update of hotfix installeren, controleert u of:

 - U hebt de update of hotfix gedownload lokaal op uw host of toegankelijk is via een netwerkshare.
 - Uw virtuele array kan Update 0.6 (10.0.10293.0) wordt uitgevoerd. Als u een eerdere versie dan Update 0.6, [Installeer Update 0.6](storsimple-virtual-array-install-update-06.md) eerste en installeer vervolgens Update 1.

Deze procedure duurt ongeveer vier minuten om te voltooien. Voer de volgende stappen uit voor het installeren van de update of hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **Software-Update**. Noteer de versie van de software die u uitvoert. **Met deze update alleen doorgaan als u Update 0.6 (10.0.10293.0) worden uitgevoerd. Als u een eerdere versie uitvoert [Installeer Update 0.6](storsimple-virtual-array-install-update-06.md) op het apparaat eerst, en vervolgens Update 1.**
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update1m.png)

2. In **Update bestandspad**, voer de naam van de update of hotfix. U kunt ook bladeren naar het installatiebestand van de update of hotfix als geplaatst op een netwerkshare. Klik op **Toepassen**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update2m.png)

3. Een waarschuwing wordt weergegeven. Gezien de virtuele matrix is een apparaat met één knooppunt, nadat de update wordt toegepast, het apparaat opnieuw wordt opgestart en er uitvaltijd is. Klik op het vinkje.
   
   ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update3m.png)

4. De update wordt gestart. Wanneer het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale UI is niet toegankelijk in deze duur.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update5m.png)

5. Nadat het opnieuw opstarten voltooid is, gaat u naar de **aanmelden** pagina. Als u wilt controleren of software van het apparaat is bijgewerkt, klikt u in de lokale web-UI, gaat u naar **onderhoud** > **Software-Update**. De weergegeven software-versie moet **10.0.0.0.0.10296** voor Update 1.0.
   
   > [!NOTE]
   > We rapport de softwareversies in een iets andere manier in de lokale web-UI en de Azure-portal. Bijvoorbeeld, de lokale webgebruikersinterface gerapporteerd **10.0.0.0.0.10296** en de Azure portal rapporten **10.0.10296.0** voor dezelfde versie.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update-1/update6m.png)

6. Herhaal stappen 2-4 voor het installeren van de Windows-security-oplossing met behulp van bestand `windows8.1-kb4012213-x64`. De virtuele matrix wordt opnieuw opgestart na de installatie en u moet zich aanmelden bij de lokale webgebruikersinterface.

> [!NOTE]
> Als u Update 1 rechtstreeks op een apparaat met een eerdere versie dan Update 0.6 toegepast, kunt u sommige updates ontbreken. Neem contact op met Microsoft Support voor de volgende stappen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheren van uw StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).
