---
title: Updates installeren op een virtuele Microsoft Azure StorSimple-matrix | Microsoft Docs
description: Beschrijft hoe u de virtuele StorSimple-matrix-webgebruikersinterface toepassen van updates via de portal en de hotfix-methode
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 9997a97b-9382-43ed-b56e-61369335c987
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2d081604c0ca01f47c3ff2aab7477859d280963
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-updates-on-your-storsimple-virtual-array---azure-portal"></a>Updates installeren op uw StorSimple virtuele matrix - Azure-portal

## <a name="overview"></a>Overzicht

In dit artikel beschrijft de stappen die nodig zijn om updates te installeren op uw virtuele StorSimple-matrix via de lokale webgebruikersinterface en via de Azure-portal. U moet toepassen van software-updates of hotfixes voor uw virtuele StorSimple-matrix up-to-date te houden. 

Houd er rekening mee dat voor het installeren van een update of hotfix uw apparaat opnieuw wordt opgestart. Gezien het feit dat het virtuele StorSimple-matrix een apparaat met één knooppunt is, een i/o uitgevoerd wordt onderbroken en uw apparaat ervaringen uitvaltijd. 

Voordat u een update hebt toegepast, wordt u aangeraden dat u rekening houden met de volumes of shares offline op de host eerst en klik vervolgens op het apparaat. Dit verkleint dat beschadigde gegevens.

> [!IMPORTANT]
> Als u Update 0.1 of GA softwareversies uitvoert, moet u de hotfix-methode via de lokale webgebruikersinterface voor het installeren van update 0.3. Als u Update 0.2 uitvoert, raden wij aan dat u de updates via de klassieke Azure portal installeert.
 

## <a name="use-the-local-web-ui"></a>Gebruik de lokale webgebruikersinterface

Er zijn twee stappen wanneer u de lokale-webgebruikersinterface:

* Download de update of hotfix
* Installeer de update of de hotfix

### <a name="download-the-update-or-the-hotfix"></a>Download de update of hotfix

Voer de volgende stappen uit om de software-update te downloaden uit de Microsoft Update-catalogus.

#### <a name="to-download-the-update-or-the-hotfix"></a>De update of de hotfix te downloaden

1. Start Internet Explorer en blader naar [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Als dit de eerste keer is dat u de Microsoft Update-catalogus op deze computer gebruikt, klikt u op **Installeren** wanneer u wordt gevraagd of u de invoegtoepassing voor de Microsoft Update-catalogus wilt installeren.

3. Voer in het zoekvak van Microsoft Update-catalogus, het aantal Knowledge Base (KB) van de hotfix die u wilt downloaden. Voer **3182061** voor Update 0.3 en klik vervolgens op **Search**.
   
    De hotfix-wordt weergegeven, bijvoorbeeld **StorSimple virtuele matrix Update 0.3**.
   
    ![Catalogus doorzoeken](./media/storsimple-virtual-array-install-update/download1.png)

4. Klik op **Add**. De update wordt toegevoegd aan het mandje.

5. Klik op **Mandje weergeven**.

6. Klik op **Downloaden**. Typ of **blader naar** een lokale locatie waar u de downloads wilt weergeven. De updates worden naar de opgegeven locatie gedownload en in een submap met dezelfde naam als de update geplaatst. De map kan ook worden gekopieerd naar een netwerkshare die bereikbaar is vanaf het apparaat.

7. De gekopieerde map opent, ziet u een zelfstandige pakket voor Microsoft Update-bestand `WindowsTH-KB3011067-x64`. Dit bestand wordt gebruikt voor het installeren van de update of hotfix.

### <a name="install-the-update-or-the-hotfix"></a>Installeer de update of de hotfix

Controleer of dat u de update hebt of de hotfix gedownload lokaal op de host of toegankelijk via een netwerkshare voor de installatie van de update of hotfix. 

Gebruik deze methode updates installeren op een apparaat met GA of 0,1 softwareversies bijwerken. Deze procedure duurt minder dan 2 minuten. Voer de volgende stappen uit voor het installeren van de update of hotfix.

#### <a name="to-install-the-update-or-the-hotfix"></a>De update of de hotfix te installeren

1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **Software-Update**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update/update1m.png)

2. In **Update bestandspad**, geef de bestandsnaam voor de update of hotfix. U kunt ook bladeren naar het installatiebestand update of hotfix als geplaatst op een netwerkshare. Klik op **Toepassen**.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update/update2m.png)

3. Een waarschuwing wordt weergegeven. Gegeven dit is een apparaat met één knooppunt, nadat de update is toegepast, het apparaat opnieuw wordt opgestart en er uitvaltijd is. Klik op het vinkje.
   
   ![apparaat bijwerken](./media/storsimple-virtual-array-install-update/update3m.png)

4. De update wordt gestart. Nadat het apparaat is bijgewerkt, opnieuw wordt gestart. De lokale gebruikersinterface is niet toegankelijk in deze duur.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update/update5m.png)

5. Nadat het opnieuw opstarten voltooid is, gaat u naar de **aanmelden** pagina. Om te controleren of software van het apparaat is bijgewerkt, in de lokale webgebruikersinterface, gaat u naar **onderhoud** > **Software-Update**. De weergegeven software-versie moet **10.0.0.0.0.10288.0** voor Update 0.3.
   
   > [!NOTE]
   > We rapport de softwareversies in een iets andere manier in de lokale webgebruikersinterface en de Azure portal. Bijvoorbeeld: de lokale webgebruikersinterface rapporten **10.0.0.0.0.10288** en de Azure portal rapporten **10.0.10288.0** voor dezelfde versie.
   
    ![apparaat bijwerken](./media/storsimple-virtual-array-install-update/update6m.png)

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Als u Update 0.2 uitvoert, wordt u aangeraden updates via de Azure portal te installeren. De portal procedure moet de gebruiker om te scannen, downloaden en installeer vervolgens de updates. Deze procedure duurt ongeveer 7 minuten is voltooid. Voer de volgende stappen uit voor het installeren van de update of hotfix.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal.md)]

Nadat de installatie is voltooid (zoals aangegeven door de status van de taak op 100%), gaat u naar uw StorSimple-apparaat Manager-service. Selecteer **apparaten** en selecteer en klik op het apparaat dat u wilt bijwerken in de lijst met apparaten die zijn verbonden met deze service. In de **instellingen** blade, gaat u naar **beheren** sectie en selecteer **apparaatupdates**. De weergegeven software-versie moet **10.0.10288.0**.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [beheer van uw virtuele StorSimple-matrix](storsimple-ova-web-ui-admin.md).

