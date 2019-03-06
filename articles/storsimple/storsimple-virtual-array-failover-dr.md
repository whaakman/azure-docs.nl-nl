---
title: StorSimple Virtual Array disaster recovery en apparaat failover | Microsoft Docs
description: Meer informatie over het failover de StorSimple Virtual Array.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: be3d98dc0b3a8119fb853493440c6fc78d65c5a2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57454235"
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Disaster recovery en apparaat failover voor de StorSimple Virtual Array via Azure portal

## <a name="overview"></a>Overzicht
Dit artikel beschrijft het herstel na noodgevallen voor uw Microsoft Azure StorSimple Virtual Array met inbegrip van de gedetailleerde stappen failover naar een andere virtuele matrix. Een failover kunt u verplaatsen van uw gegevens uit een *bron* apparaat in het datacenter naar een *doel* apparaat. Het doelapparaat kan zich bevinden in hetzelfde of een andere geografische locatie. De failover van het apparaat is voor het hele apparaat. Tijdens de failover verandert de cloudgegevens voor het bronapparaat eigendom met die van het doelapparaat.

In dit artikel is alleen van toepassing op virtuele StorSimple-matrices. Als u wilt een 8000-serie-apparaat een failover, gaat u naar [apparaat failover en herstel na noodgevallen van uw StorSimple-apparaat](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Wat is disaster recovery en apparaat-failover?

In een scenario voor disaster recovery (DR), het primaire apparaat niet meer werkt. In dit scenario kunt u de cloudgegevens die is gekoppeld aan het mislukte apparaat naar een ander apparaat verplaatsen. U kunt het primaire apparaat als de *bron* en geef een ander apparaat als de *doel*. Dit proces wordt aangeduid als de *failover*. Tijdens de failover, worden alle volumes of shares van het bronapparaat eigendom wijzigen en naar het doelapparaat worden overgebracht. Er wordt geen filtering van de gegevens is toegestaan.

Herstel na Noodgeval is gemodelleerd als een volledige apparaat herstellen met behulp van de heatmap toewijzing op basis van meerdere lagen en bij te houden. Een heatmap wordt gedefinieerd door een heatmap-waarde toewijzen aan de gegevens die zijn gebaseerd op lezen en schrijven van patronen. Deze heatmap toewijzen vervolgens lagen de laagste segmenten van de heatmap-gegevens naar de cloud eerst terwijl de segmenten van hoge heatmap (meest gebruikte) gegevens in de lokale laag. StorSimple maakt tijdens een herstel na Noodgevallen gebruik van de heatmap terug te zetten en rehydrate van de gegevens vanuit de cloud. Het apparaat worden opgehaald van alle volumes/shares in de laatste recente back-up (zoals intern bepaald) en voert een terugzetbewerking van deze back-up. De virtuele matrix coördineert het hele proces van herstel na Noodgevallen.

> [!IMPORTANT]
> Het bronapparaat aan het einde van de failover van het apparaat wordt verwijderd en kan daarom een failback wordt niet ondersteund.
> 
> 

Herstel na noodgevallen via de failoverfunctie van het apparaat is georganiseerd en wordt gestart vanuit de **apparaten** blade. Deze blade registreert alle StorSimple-apparaten die worden verbonden met uw StorSimple Device Manager-service. Voor elk apparaat ziet u de beschrijvende naam, status, ingericht en maximale capaciteit, type en model.

## <a name="prerequisites-for-device-failover"></a>Vereisten voor failover van het apparaat

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan voor een failover van het apparaat:

* Het bronapparaat moet zich in een **gedeactiveerd** staat.
* Het doelapparaat moet worden weergegeven als **klaar om in te stellen** in Azure portal. Een virtuele doelmatrix van de dezelfde of een hogere capaciteit inrichten. Gebruik de lokale webgebruikersinterface voor het configureren en de doel-virtuele matrix wordt geregistreerd.
  
  > [!IMPORTANT]
  > Probeer niet om de geregistreerde virtuele apparaat via de service te configureren. Er is geen apparaatconfiguratie moet worden uitgevoerd via de service.
  > 
  > 
* Het doelapparaat geen dezelfde naam als het bronapparaat.
* De bron en doel-apparaat moet hetzelfde type zijn. U kunt alleen een failover via een virtuele matrix die is geconfigureerd als een bestandsserver met een andere bestandsserver. Hetzelfde geldt voor een iSCSI-server.
* U wordt aangeraden dat u het doelapparaat aan hetzelfde domein als de bron toevoegen voor een bestandsserver herstel na Noodgevallen. Deze configuratie zorgt ervoor dat de sharemachtigingen automatisch worden opgelost. Alleen de failover naar een doelapparaat in hetzelfde domein.
* De beschikbare doelapparaten voor herstel na Noodgevallen zijn apparaten waarvoor de dezelfde of een grotere capaciteit ten opzichte van het bronapparaat. De apparaten die zijn verbonden met uw service, maar niet voldoen aan de criteria van voldoende ruimte zijn niet beschikbaar als doelapparaten.

### <a name="other-considerations"></a>Andere overwegingen

* Voor een geplande failover 
  
  * Het is raadzaam dat u alle volumes of shares op het bronapparaat offline nemen.
  * U wordt aangeraden dat u een back-up van het apparaat maken en ga daarna verder met de failover om gegevensverlies te minimaliseren. 
* Voor een niet-geplande failover, het apparaat de meest recente back-up gebruikt om de gegevens te herstellen.

### <a name="device-failover-prechecks"></a>Apparaat failover apparaatupdates

Voordat u begint met het herstel na Noodgevallen, voert het apparaat apparaatupdates. Deze controles helpen ervoor te zorgen dat er geen fouten optreden wanneer herstel na Noodgeval wordt gemaakt. De apparaatupdates zijn onder andere:

* Valideren van het storage-account.
* De de cloudconnectiviteit met Azure controleren.
* Controleren op beschikbare ruimte op het doelapparaat.
* Als een iSCSI-server bronvolume apparaat heeft controleren
  
  * de namen van de ACR is ongeldig.
  * geldige IQN (niet van meer dan 220 tekens).
  * geldige CHAP wachtwoorden (12-16 tekens lang).

Als een van de voorgaande prechecks mislukt, kunt u kan niet doorgaan met het herstel na Noodgevallen. Deze problemen oplossen en probeer vervolgens herstel na Noodgevallen.

Nadat het herstel na Noodgeval is voltooid, wordt het eigendom van de cloudgegevens op het apparaat van de gegevensbron overgebracht naar het doelapparaat. Het bronapparaat is vervolgens niet meer beschikbaar in de portal. Toegang tot alle volumes/shares op de Bronapparaat is geblokkeerd en het doelapparaat actief wordt.

> [!IMPORTANT]
> Hoewel het apparaat niet meer beschikbaar is, wordt de virtuele machine die u hebt ingericht op het hostsysteem nog steeds resources verbruikt. Nadat het herstel na Noodgevallen met succes voltooid is, kunt u deze virtuele machine verwijderen uit uw hostsysteem.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Failover naar een virtuele matrix

U wordt aangeraden inrichten, configureren en registreren van een andere StorSimple Virtual Array bij uw StorSimple Device Manager-service voordat u deze procedure uitvoert.

> [!IMPORTANT]
> 
> * U kan geen failover van een StorSimple 8000-apparaat naar een virtueel apparaat 1200.
> * U kunt een failover van een virtueel apparaat Federal Information Processing Standard (FIPS) ingeschakeld op een ander apparaat voor FIPS is ingeschakeld of op een niet-FIPS-apparaat dat is geïmplementeerd in de Government-portal.


De volgende stappen uitvoeren om de apparaat herstellen naar een doel virtuele StorSimple-apparaat.

1. Inrichten en het configureren van een doelapparaat die voldoet aan de [vereisten voor failover van het apparaat](#prerequisites). De apparaatconfiguratie van het via de lokale webgebruikersinterface voltooien en Registreer het bij uw StorSimple Device Manager-service. Als het maken van een bestandsserver, gaat u naar stap 1 van [instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Als het maken van een iSCSI-server, gaat u naar stap 1 van [ingesteld als iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Volumes/shares offline nemen op de host. Als u wilt de volumes/shares offline, verwijzen naar de besturingssysteem-specifieke instructies voor de host. Als deze niet al offline is, moet u alle volumes/shares offline nemen op het apparaat door het volgende te doen.
   
    1. Ga naar **apparaten** blade en selecteer uw apparaat.
   
    2. Ga naar **instellingen > beheren > Shares** (of **instellingen > beheren > Volumes**). 
   
    3. Selecteer een share of het volume, klik met de rechtermuisknop en selecteer **offline zetten**. 
   
    4. Wanneer u hierom wordt gevraagd om bevestiging, controleert u **ik begrijp de gevolgen van het offline halen van deze share.** 
   
    5. Klik op **offline zetten**.

3. In uw StorSimple Device Manager-service, gaat u naar **Management > apparaten**. In de **apparaten** blade, selecteer en klik op uw Bronapparaat.

4. In uw **apparaatdashboard** blade, klikt u op **deactiveren**.

5. In de **deactiveren** blade u om bevestiging wordt gevraagd. Apparaat deactivering is een *permanente* proces dat kan niet ongedaan worden gemaakt. U ook een herinnering aan uw shares /-volumes offline nemen op de host. Typ de naam van het apparaat om te bevestigen en klik op **deactiveren**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. De deactivering wordt gestart. U ontvangt een melding nadat de deactivering is voltooid.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Op de pagina apparaten nu de apparaatstatus wordt gewijzigd in **gedeactiveerd**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. In de **apparaten** blade, selecteer en klik op het bronapparaat gedeactiveerd voor failover. 
9. In de **apparaatdashboard** blade, klikt u op **failover**. 
10. In de **failover van apparaat** blade, doet u het volgende:
    
    1. Het veld van het apparaat bron wordt automatisch ingevuld. Houd er rekening mee de totale grootte van het bronapparaat. Grootte van de gegevens moet kleiner is dan de beschikbare capaciteit op het doelapparaat. Lees de informatie die is gekoppeld aan het bronapparaat zoals de naam van apparaat, totale capaciteit en de namen van shares waarvoor een failover is.

    2. Kies in de vervolgkeuzelijst met beschikbare apparaten een **doelapparaat**. Alleen de apparaten die onvoldoende capaciteit worden weergegeven in de vervolgkeuzelijst.

    3. Controleer of **ik begrijp dat deze bewerking van gegevens naar het doelapparaat mislukt**. 

    4. Klik op **failover**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Een failovertaak wordt gestart en u een melding ontvangen. Ga naar **apparaten > taken** voor het bewaken van de failover.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. In de **taken** blade ziet u een failovertaak gemaakt voor het bronapparaat. Deze taak voert de apparaatupdates herstel na Noodgevallen.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     Nadat de DR-apparaatupdates geslaagd zijn, wordt de failovertaak hersteltaken voor elke share of het volume dat zich op uw Bronapparaat produceren.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Nadat de failover voltooid is, gaat u naar de **apparaten** blade.
    
    1. Selecteer en klik op het StorSimple-apparaat dat is gebruikt als het doelapparaat voor de failoverproces.
    2. Ga naar **instellingen > Beheer > Shares** (of **Volumes** als iSCSI-server). In de **Shares** blade vindt u alle shares (volumes) van het oude apparaat.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. U moet [maken van een DNS-alias](https://support.microsoft.com/kb/168322) zodat alle toepassingen die probeert verbinding te kunnen ophalen omgeleid naar het nieuwe apparaat.

## <a name="errors-during-dr"></a>Fouten tijdens het herstel na Noodgevallen

**Cloud-connectiviteit onderbreking tijdens herstel na Noodgevallen**

Als de cloudconnectiviteit is onderbroken nadat DR is gestart en voordat het apparaat terugzetten voltooid is, mislukt het herstel na Noodgevallen. U ontvangt een melding van de fout. Het doelapparaat voor herstel na Noodgevallen is gemarkeerd als *onbruikbaar.* U kunt hetzelfde apparaat niet gebruiken voor toekomstige DRs.

**Er is geen compatibele apparaten**

Als de beschikbare doelservers-apparaten niet voldoende ruimte hebben, ziet u een fout die naar het effect dat er geen doelapparaten compatibel zijn.

**Controle vooraf fouten**

Als een van de apparaatupdates niet is voldaan, klikt ziet u controle vooraf mislukte tests.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Zakelijke continuïteit noodherstel (BCDR)

Een zakelijke continuïteit (BCDR) noodherstelscenario treedt op wanneer het hele Azure-datacenter niet meer werkt. Dit kan van invloed op uw StorSimple Device Manager-service en de bijbehorende StorSimple-apparaten.

Als er StorSimple-apparaten die zijn geregistreerd, net voordat een noodgeval is opgetreden, kunnen deze StorSimple-apparaten moet worden verwijderd. U kunt na de ramp, opnieuw maken en configureren van deze apparaten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw StorSimple Virtual Array met behulp van de lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md).

