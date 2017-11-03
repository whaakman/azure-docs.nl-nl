---
title: Virtuele StorSimple-matrix web UI beheer | Microsoft Docs
description: Beschrijft hoe u eenvoudige beheertaken via de webgebruikersinterface virtuele StorSimple-matrix.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 989e7b697f9b527df549fb32be18edd1d3c8d224
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>De Webgebruikersinterface gebruiken voor het beheren van uw virtuele StorSimple-matrix
![Processtroom](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Overzicht
De zelfstudies in dit artikel van toepassing op de Microsoft Azure StorSimple virtuele matrix (ook wel bekend als de StorSimple on-premises virtuele apparaat) versie van maart 2016 algemene beschikbaarheid (GA). Dit artikel worden enkele van de werkstromen en beheertaken uitvoeren die kunnen worden uitgevoerd op de virtuele StorSimple-matrix. U kunt de virtuele StorSimple-matrix met de StorSimple Manager beheren service UI (aangeduid als de gebruikersinterface van de portal) en de lokale webgebruikersinterface voor het apparaat. Dit artikel is gericht op de taken die u kunt uitvoeren met behulp van de web-UI.

In dit artikel bevat de volgende zelfstudies:

* Ophalen van de gegevensversleutelingssleutel van service
* Web UI setup fouten oplossen
* Genereren van een logboek-pakket
* Afsluiten of opnieuw opstarten van uw apparaat

## <a name="get-the-service-data-encryption-key"></a>Ophalen van de gegevensversleutelingssleutel van service
Een service voor de gegevensversleutelingssleutel wordt gegenereerd wanneer u uw eerste apparaat bij de StorSimple Manager-service registreren. Deze sleutel is vereist met de serviceregistratiesleutel extra apparaten registreren bij de StorSimple Manager-service.

Als u hebt uw gegevensversleutelingssleutel van service en is nodig voor het ophalen van het geraakt, voert u de volgende stappen in de lokale webgebruikersinterface van het apparaat geregistreerd bij uw service.

#### <a name="to-get-the-service-data-encryption-key"></a>Ophalen van de gegevensversleutelingssleutel van service
1. Verbinding maken met de lokale webgebruikersinterface. Ga naar **configuratie** > **Cloudinstellingen**.
2. Klik onder aan de pagina op **Get service gegevensversleutelingssleutel**. Een sleutel wordt weergegeven. Kopieer en sla deze sleutel.
   
    ![ophalen van de gegevensversleutelingssleutel van service 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Web UI setup fouten oplossen
In sommige gevallen wanneer u het apparaat via de lokale webgebruikersinterface, configureert kunt u tegenkomen fouten. Voor het diagnosticeren en oplossen van dergelijke fouten, kunt u de diagnostische tests uitvoeren.

#### <a name="to-run-the-diagnostic-tests"></a>De diagnostische tests uitvoeren
1. Ga in de lokale webgebruikersinterface naar **probleemoplossing** > **diagnostische tests**.
   
    ![diagnose 1 uitvoeren](./media/storsimple-ova-web-ui-admin/image29.png)
2. Klik onder aan de pagina op **diagnostische Tests uitvoeren**. Er wordt nu tests uit om het onderzoeken van mogelijke problemen met uw netwerk, het apparaat, de webproxy, tijd of instellingen voor cloud. U wordt gewaarschuwd dat het apparaat tests wordt uitgevoerd.
3. Nadat de tests zijn voltooid, kunt u de resultaten worden weergegeven. Het volgende voorbeeld ziet de resultaten van diagnostische tests. Houd er rekening mee dat de web proxy-instellingen zijn niet geconfigureerd op dit apparaat en de proxy WebTest is daarom niet uitgevoerd. Alle andere tests voor netwerkinstellingen, DNS-server en tijdinstellingen zijn gelukt.
   
    ![de diagnostische gegevens van 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Genereren van een logboek-pakket
Een pakket logboek bestaat uit de relevante logboeken die Microsoft Support u helpen kunnen bij het oplossen van eventuele problemen apparaat. In deze release, kan een pakket logboek via de lokale webgebruikersinterface worden gegenereerd.

#### <a name="to-generate-the-log-package"></a>De logboek-pakket te genereren
1. Ga in de lokale webgebruikersinterface naar **probleemoplossing** > **systeemlogboeken**.
   
    ![logboek pakket, 1 genereren](./media/storsimple-ova-web-ui-admin/image31.png)
2. Klik onder aan de pagina op **maken logboek pakket**. Een pakket van de systeemlogboeken wordt gemaakt. Dit duurt enkele minuten duren.
   
    ![logboek pakket 2 genereren](./media/storsimple-ova-web-ui-admin/image32.png)
   
    U wordt gewaarschuwd wanneer het pakket is gemaakt en de pagina bijgewerkt wordt om aan te geven van de tijd en datum waarop het pakket is gemaakt.
   
    ![logboek pakket, 3 genereren](./media/storsimple-ova-web-ui-admin/image33.png)
3. Klik op **logboek downloadpakket**. Een ZIP-pakket worden gedownload op uw systeem.
   
    ![logboek pakket 4 genereren](./media/storsimple-ova-web-ui-admin/image34.png)
4. U kunt Pak het gedownloade logboek-pakket en de Systeemlogboekbestanden weergeven.

## <a name="shut-down-and-restart-your-device"></a>Afsluiten en opnieuw opstarten van uw apparaat
U kunt afsluiten of opnieuw opstarten van uw virtuele apparaat met de lokale webgebruikersinterface. We aanbevelen die voordat u opnieuw start, neemt u de volumes of shares op de host en klik vervolgens op het apparaat. Hierdoor minimaliseert u iedere mogelijkheid van beschadigde gegevens. 

#### <a name="to-shut-down-your-virtual-device"></a>Uw virtuele apparaat afsluiten
1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **energie-instellingen**.
2. Klik onder aan de pagina op **afsluiten**.
   
    ![Afsluitgebeurtenis apparaat 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Een waarschuwing wordt weergegeven waarin staat dat afsluiten van het apparaat een i/o die zijn uitgevoerd, wat resulteert in een uitvaltijd wordt onderbroken. Klik op het vinkje ![vinkje](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Waarschuwing voor apparaat afsluiten](./media/storsimple-ova-web-ui-admin/image37.png)
   
    U wordt gewaarschuwd dat het afsluiten is gestart.
   
    ![apparaat afsluiten is gestart](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Het apparaat wordt nu afgesloten. Als u uw apparaat wilt, moet u doen via de Hyper-V-beheer.

#### <a name="to-restart-your-virtual-device"></a>Opnieuw opstarten van uw virtuele apparaat
1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **energie-instellingen**.
2. Klik onder aan de pagina op **opnieuw**.
   
    ![herstart van het apparaat](./media/storsimple-ova-web-ui-admin/image36.png)
3. Er wordt een waarschuwing weergegeven waarin staat dat het opstarten van het apparaat een IOs die zijn uitgevoerd, wat resulteert in een uitvaltijd wordt onderbroken. Klik op het vinkje ![vinkje](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![opnieuw opstarten van waarschuwing](./media/storsimple-ova-web-ui-admin/image37.png)
   
    U wordt gewaarschuwd dat de computer opnieuw is gestart.
   
    ![opnieuw opstarten geïnitieerd](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Terwijl de computer opnieuw uitgevoerd wordt, wordt de verbinding met de gebruikersinterface wordt verbroken. U kunt het opnieuw opstarten door het vernieuwen van de gebruikersinterface periodiek controleren. U kunt ook de status van het apparaat opnieuw opstarten via de Hyper-V Manager controleren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw apparaat](storsimple-virtual-array-manager-service-administration.md).

