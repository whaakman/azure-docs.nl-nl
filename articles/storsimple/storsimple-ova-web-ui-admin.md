---
title: StorSimple Virtual Array web UI beheer | Microsoft Docs
description: Beschrijft hoe u eenvoudige beheertaken via de webgebruikersinterface van de StorSimple Virtual Array uitvoeren.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: ''
ms.assetid: ea65b4c7-a478-43e6-83df-1d9ea62916a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 12/1/2016
ms.author: alkohli
ms.openlocfilehash: 92671206a4171ca838423f55b526191ef30e5c35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60630356"
---
# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>De Web-UI gebruiken voor het beheren van uw StorSimple Virtual Array
![Processtroom voor Setup](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Overzicht
De zelfstudies in dit artikel zijn van toepassing op Microsoft Azure StorSimple Virtual Array (ook wel bekend als de StorSimple on-premises virtuele apparaat) met algemene beschikbaarheid (GA) versie van maart 2016. In dit artikel worden enkele van de complexe werkstromen en beheertaken uitvoeren die kunnen worden uitgevoerd op de StorSimple Virtual Array beschreven. U kunt de StorSimple Virtual Array met behulp van de StorSimple Manager beheren service UI (aangeduid als de gebruikersinterface van de portal) en de lokale webgebruikersinterface voor het apparaat. In dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de web-UI.

Dit artikel bevat de volgende zelfstudies:

* De versleutelingssleutel voor servicegegevens ophalen
* Web UI setup fouten oplossen
* Genereer een logboek-pakket
* Apparaat uitschakelen of opnieuw opstarten

## <a name="get-the-service-data-encryption-key"></a>De versleutelingssleutel voor servicegegevens ophalen
Een versleutelingssleutel voor servicegegevens wordt gegenereerd wanneer u uw eerste apparaat bij de StorSimple Manager-service registreren. Deze sleutel is vereist bij de serviceregistratiesleutel extra apparaten registreren bij de StorSimple Manager-service.

Als u de versleutelingssleutel voor servicegegevens en hoeft te halen hebt geraakt, voert u de volgende stappen in de lokale web-UI van het apparaat geregistreerd bij uw service.

#### <a name="to-get-the-service-data-encryption-key"></a>Om op te halen van de versleutelingssleutel voor servicegegevens
1. Verbinding maken met de lokale webgebruikersinterface. Ga naar **configuratie** > **Cloudinstellingen**.
2. Aan de onderkant van de pagina, klikt u op **versleutelingssleutel voor servicegegevens Get**. Een sleutel wordt weergegeven. Kopieer en bewaar deze sleutel.
   
    ![ophalen van de versleutelingssleutel voor servicegegevens 1](./media/storsimple-ova-web-ui-admin/image27.png)

## <a name="troubleshoot-web-ui-setup-errors"></a>Web UI setup fouten oplossen
In sommige gevallen wanneer u het apparaat wordt via de lokale web-UI, configureert u mogelijk ondervindt fouten. Als u wilt vaststellen en oplossen van dergelijke fouten, kunt u de diagnostische test uitvoeren.

#### <a name="to-run-the-diagnostic-tests"></a>De diagnostische tests uitvoeren
1. Ga in de lokale webgebruikersinterface naar **probleemoplossing** > **diagnostische tests**.
   
    ![Diagnostische gegevens van 1](./media/storsimple-ova-web-ui-admin/image29.png)
2. Aan de onderkant van de pagina, klikt u op **diagnostische Tests uitvoeren**. Er wordt nu tests uit om te onderzoeken van mogelijke problemen met uw netwerk, het apparaat, de webproxy, tijd of instellingen voor cloud. U wordt gewaarschuwd dat het apparaat tests wordt uitgevoerd.
3. Nadat de tests zijn voltooid, worden de resultaten worden weergegeven. Het volgende voorbeeld toont de resultaten van de diagnostische tests uit. Houd er rekening mee dat de webproxy-instellingen zijn niet geconfigureerd op dit apparaat, en daarom de WebTest proxy niet kan worden uitgevoerd. Alle andere tests voor netwerkinstellingen, DNS-server en tijdinstellingen zijn voltooid.
   
    ![Diagnostische gegevens van 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Genereer een logboek-pakket
Een pakket logboek bestaat uit alle relevante logboeken die Microsoft Support helpen kunnen bij het oplossen van eventuele problemen met het apparaat. In deze release, kan een pakket van het logboek worden gegenereerd via de lokale webgebruikersinterface.

#### <a name="to-generate-the-log-package"></a>Voor het genereren van het logboek-pakket
1. Ga in de lokale webgebruikersinterface naar **probleemoplossing** > **systeemlogboeken**.
   
    ![logboek pakket, 1 genereren](./media/storsimple-ova-web-ui-admin/image31.png)
2. Aan de onderkant van de pagina, klikt u op **maken log pakket**. Een pakket van de systeemlogboeken wordt gemaakt. Dit kan enkele minuten duren.
   
    ![logboek pakket 2 genereren](./media/storsimple-ova-web-ui-admin/image32.png)
   
    U wordt een melding nadat het pakket is gemaakt en de pagina bijgewerkt wordt om aan te geven van de tijd en datum waarop het pakket is gemaakt.
   
    ![logboek pakket, 3 genereren](./media/storsimple-ova-web-ui-admin/image33.png)
3. Klik op **log downloadpakket**. Een ZIP-pakket worden gedownload op uw systeem.
   
    ![logboek pakket 4 genereren](./media/storsimple-ova-web-ui-admin/image34.png)
4. U kunt Pak het gedownloade log-pakket en de Systeemlogboekbestanden weergeven.

## <a name="shut-down-and-restart-your-device"></a>Afsluiten en opnieuw opstarten van het apparaat
U kunt afsluiten of opnieuw opstarten van uw virtuele apparaat met behulp van de lokale webgebruikersinterface. We aanbevolen die u opnieuw start, neemt u even voordat de volumes of shares op de host en klik vervolgens op het apparaat. Hierdoor minimaliseert u een mogelijkheid van beschadiging van gegevens. 

#### <a name="to-shut-down-your-virtual-device"></a>Om uw virtuele apparaat af te sluiten
1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **energie-instellingen**.
2. Aan de onderkant van de pagina, klikt u op **afsluiten**.
   
    ![apparaat afsluiten 1](./media/storsimple-ova-web-ui-admin/image36.png)
3. Een waarschuwing wordt weergegeven waarin staat dat een afsluiten van het apparaat IO's die zijn uitgevoerd, wat resulteert in een uitvaltijd wordt onderbroken. Klik op het vinkje ![vinkje](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![Waarschuwing voor apparaat afsluiten](./media/storsimple-ova-web-ui-admin/image37.png)
   
    U wordt gewaarschuwd dat het afsluiten is gestart.
   
    ![afsluiten van het apparaat aan de slag](./media/storsimple-ova-web-ui-admin/image38.png)
   
    Het apparaat wordt nu afgesloten. Als u beginnen uw apparaat wilt, moet u dit doen via de Hyper-V-beheer.

#### <a name="to-restart-your-virtual-device"></a>Opnieuw opstarten van uw virtuele apparaat
1. Ga in de lokale webgebruikersinterface naar **onderhoud** > **energie-instellingen**.
2. Aan de onderkant van de pagina, klikt u op **opnieuw**.
   
    ![herstart van het apparaat](./media/storsimple-ova-web-ui-admin/image36.png)
3. Een waarschuwing wordt weergegeven waarin staat dat het opnieuw starten van het apparaat een IOs die zijn uitgevoerd, wat resulteert in een uitvaltijd wordt onderbroken. Klik op het vinkje ![vinkje](./media/storsimple-ova-web-ui-admin/image3.png).
   
    ![waarschuwing opnieuw starten](./media/storsimple-ova-web-ui-admin/image37.png)
   
    U wordt gewaarschuwd dat de computer opnieuw is gestart.
   
    ![opnieuw opstarten geïnitieerd](./media/storsimple-ova-web-ui-admin/image39.png)
   
    Wanneer de computer opnieuw uitgevoerd wordt, verliest u de verbinding in de gebruikersinterface. U kunt het opnieuw opstarten controleren door het periodiek vernieuwen van de gebruikersinterface. U kunt ook de status van het apparaat opnieuw opstarten via de Hyper-V-beheer controleren.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [de StorSimple Manager-service gebruiken voor het beheren van uw apparaat](storsimple-virtual-array-manager-service-administration.md).

