---
title: Virtuele StorSimple-matrix disaster recovery en apparaat failover | Microsoft Docs
description: Meer informatie over het failover uw virtuele StorSimple-matrix.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 12079f8dbc409afe5acc274fa08bda878c90b76e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Disaster recovery en apparaat failover voor uw virtuele StorSimple-matrix via Azure portal

## <a name="overview"></a>Overzicht
In dit artikel beschrijft het herstel na noodgevallen voor uw Microsoft Azure StorSimple virtuele matrix met inbegrip van de gedetailleerde stappen failover naar een andere virtuele matrix. Een failover kunt u verplaatsen van uw gegevens uit een *bron* apparaat in het datacenter naar een *doel* apparaat. Het doelapparaat kan zich bevinden in dezelfde of een andere geografische locatie. De failover van het apparaat is voor het hele apparaat. Tijdens de failover verandert de cloudgegevens voor de Bronapparaat eigendom in die van het doelapparaat.

In dit artikel is alleen van toepassing op virtuele StorSimple-matrices. Failover van een apparaat 8000-serie, gaat u naar [apparaat failover en herstel na noodgevallen van uw StorSimple-apparaat](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Wat is disaster recovery en apparaat failover?

In een (DR) noodherstelscenario, het primaire apparaat niet meer werkt. In dit scenario kunt u de cloudgegevens die is gekoppeld aan het mislukte apparaat aan een ander apparaat. U kunt het primaire apparaat als de *bron* en geef een ander apparaat als de *doel*. Dit proces wordt aangeduid als de *failover*. Tijdens de failover, alle volumes of shares van het bronapparaat eigendom wijzigen en worden overgedragen naar het doelapparaat. Er is geen filteren van de gegevens is toegestaan.

Noodherstel is gemodelleerd als een volledige apparaat te herstellen met behulp van de hitte op basis van kaart in lagen en bij te houden. Een heatmap wordt gedefinieerd door het toewijzen van een hitte-waarde in de gegevens die zijn gebaseerd op lezen en schrijven van patronen. Deze hitte toewijzen vervolgens lagen de laagste hitte gegevenssegmenten in de cloud eerst terwijl de gegevenssegmenten hoge hitte (meest gebruikt) in de lokale laag. Tijdens een DR StorSimple de heatmap gebruikt om te zetten en rehydrate van de gegevens vanuit de cloud. Het apparaat haalt alle volumes/shares in de laatste recente back-up (zoals intern bepaald) en voert een herstel van back-up. De virtuele matrix ingedeeld het hele proces voor Noodherstel.

> [!IMPORTANT]
> Het bronapparaat aan het einde van de failover van apparaat verwijderd en wordt daarom een failback wordt niet ondersteund.
> 
> 

Herstel na noodgevallen wordt beheerd via de functie van de failover-apparaat en wordt gestart vanuit de **apparaten** blade. Deze blade registreert alle StorSimple-apparaten die worden verbonden met uw StorSimple-apparaat Manager-service. Voor elk apparaat ziet u de beschrijvende naam, status, ingericht en de maximale capaciteit, type en model.

## <a name="prerequisites-for-device-failover"></a>Vereisten voor failover-apparaat

### <a name="prerequisites"></a>Vereisten

Zorg ervoor dat de volgende vereisten wordt voldaan voor een failover apparaat:

* Het bronapparaat moet zich in een **gedeactiveerd** status.
* Het doelapparaat moet worden weergegeven als **gereed is voor het instellen van** in de Azure portal. Een virtuele doelmatrix van de dezelfde of een hogere capaciteit inrichten. Gebruik de lokale webgebruikersinterface configureren en de virtuele doelmatrix is te registreren.
  
  > [!IMPORTANT]
  > Probeer niet het geregistreerde virtuele apparaat via de service te configureren. Er is geen apparaatconfiguratie moet worden uitgevoerd via de service.
  > 
  > 
* Het doelapparaat kan niet dezelfde naam als het bronapparaat hebben.
* De bron en doel-apparaat moet hetzelfde type zijn. U kunt alleen een virtuele matrix die is geconfigureerd als een bestandsserver naar een andere bestandsserver failover. Hetzelfde geldt voor een iSCSI-server.
* U wordt aangeraden dat u het doelapparaat aan hetzelfde domein als de bron toevoegen voor een bestandsserver Noodherstel. Deze configuratie zorgt ervoor dat de sharemachtigingen automatisch worden opgelost. Alleen de failover naar een doelapparaat in hetzelfde domein.
* De beschikbare doelapparaten voor herstel na Noodgevallen zijn apparaten waarop de dezelfde of een grotere capaciteit ten opzichte van het bronapparaat. De apparaten die zijn verbonden met uw service, maar die niet voldoen aan de criteria van voldoende ruimte zijn niet beschikbaar als doelapparaten.

### <a name="other-considerations"></a>Andere overwegingen

* Voor een geplande failover 
  
  * Het is raadzaam dat u de volumes of shares op het bronapparaat offline neemt.
  * U wordt aangeraden dat u een back-up van het apparaat en ga vervolgens verder met de failover om gegevensverlies te minimaliseren. 
* Voor een niet-geplande failover het apparaat de meest recente back-up gebruikt om de gegevens te herstellen.

### <a name="device-failover-prechecks"></a>Apparaat failover prechecks

Voordat de DR begint, voert het apparaat prechecks. Deze controles zorgen ervoor dat er geen fouten optreden wanneer DR begint. De prechecks zijn onder andere:

* Valideren van het opslagaccount.
* De verbinding met de cloud naar Azure controleren.
* Controleren van de beschikbare ruimte op het doelapparaat.
* Controleren of een iSCSI-server bronvolume apparaat heeft
  
  * geldige ACR-namen.
  * geldige IQN (van ten hoogste 220 tekens).
  * geldige CHAP wachtwoorden (12-16 tekens).

Als een van de voorgaande prechecks mislukken, kunt niet u doorgaan met de DR. Deze problemen oplossen en probeer vervolgens Noodherstel.

Nadat de DR is voltooid, wordt de eigendom van de cloudgegevens op het bronapparaat overgedragen aan het doelapparaat. Het bronapparaat is vervolgens niet meer beschikbaar in de portal. Toegang tot alle volumes/shares op de Bronapparaat is geblokkeerd en het doelapparaat wordt geactiveerd.

> [!IMPORTANT]
> Hoewel het apparaat niet langer beschikbaar is, is de virtuele machine die u hebt ingericht op het hostsysteem nog steeds resources verbruikt. Zodra de DR met succes voltooid is, kunt u deze virtuele machine verwijderen uit uw hostsysteem.
> 
> 

## <a name="fail-over-to-a-virtual-array"></a>Failover naar een virtuele-matrix

U wordt aangeraden inrichten, configureren en registreren van een andere virtuele StorSimple-matrix met uw StorSimple-apparaat Manager-service voordat u deze procedure uitvoert.

> [!IMPORTANT]
> 
> * U kan niet worden overgenomen van een StorSimple 8000 series apparaat naar een virtueel apparaat 1200.
> * U kunt failover van een virtueel apparaat Federal Information Processing Standard (FIPS) is ingeschakeld op een ander apparaat voor FIPS ingeschakeld of op een apparaat niet FIPS is geïmplementeerd in de portal van de overheid.


Voer de volgende stappen uit voor het herstellen van het apparaat op een doel-virtuele StorSimple-apparaat.

1. Inrichten en configureren van een apparaat dat voldoet aan de [vereisten voor failover apparaat](#prerequisites). Voltooi de configuratie van het apparaat via de lokale webgebruikersinterface en registreren met uw StorSimple-apparaat Manager-service. Als u een bestandsserver maakt, gaat u naar stap 1 van [instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Als u een iSCSI-server maakt, gaat u naar stap 1 van [ingesteld als iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Volumes/shares offline nemen op de host. Als u wilt de volumes/shares offline, verwijzen naar de besturingssysteem-specifieke instructies voor de host. Als deze niet al offline is, moet u alle volumes/shares offline nemen op het apparaat als volgt.
   
    1. Ga naar **apparaten** blade en selecteer het apparaat.
   
    2. Ga naar **instellingen > beheren > Shares** (of **instellingen > beheren > Volumes**). 
   
    3. Selecteer een share of het geselecteerde volume, klik met de rechtermuisknop en selecteer **offline zetten**. 
   
    4. Wanneer u wordt gevraagd om bevestiging, controleert u **ik begrijp dat de gevolgen van deze share offline moet worden gezet.** 
   
    5. Klik op **offline zetten**.

3. In uw StorSimple-apparaat Manager-service, gaat u naar **Management > apparaten**. In de **apparaten** blade selecteren en klik op het bronapparaat.

4. In uw **apparaat dashboard** blade, klikt u op **deactiveren**.

5. In de **deactiveren** blade u om bevestiging wordt gevraagd. Deactivering van het apparaat is een *permanente* proces dat kan niet ongedaan worden gemaakt. U ook een herinnering nemen uw offline shares/volumes op de host. Typ de naam van het apparaat om te bevestigen en klikt u op **deactiveren**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. De deactivering wordt gestart. U ontvangt een melding nadat de deactivering is voltooid.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Op de pagina apparaten nu de apparaatstatus wordt gewijzigd in **gedeactiveerd**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. In de **apparaten** blade selecteren en klik op het bronapparaat gedeactiveerde voor failover. 
9. In de **apparaat dashboard** blade, klikt u op **failover**. 
10. In de **apparaat failover** blade het volgende doen:
    
    1. Het veld van het apparaat wordt automatisch gevuld. Let op de totale gegevensgrootte van het bronapparaat. De gegevensgrootte moet kleiner zijn dan de beschikbare capaciteit op het doelapparaat. Lees de informatie die is gekoppeld aan het bronapparaat zoals apparaatnaam totale capaciteit en de namen van de shares die worden overgenomen.

    2. Kies in de vervolgkeuzelijst met beschikbare apparaten een **doelapparaat**. Alleen de apparaten die voldoende capaciteit hebben worden weergegeven in de vervolgkeuzelijst.

    3. Controleer of **ik begrijp dat deze bewerking failover gegevens naar het doelapparaat**. 

    4. Klik op **failover**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Een failover-taak wordt gestart en u ontvangt een melding. Ga naar **apparaten > taken** voor het bewaken van de failover.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. In de **taken** blade ziet u een failover-taak gemaakt voor de Bronapparaat. Deze taak voert de prechecks Noodherstel.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     Nadat de DR-prechecks geslaagd zijn, wordt de failover-taak kan het herstellen van taken voor elk share/volume dat zich op uw Bronapparaat gemaakt.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. Nadat de failover voltooid is, gaat u naar de **apparaten** blade.
    
    1. Selecteer en klik op het StorSimple-apparaat dat is gebruikt als het doelapparaat voor de failoverproces.
    2. Ga naar **instellingen > Beheer > Shares** (of **Volumes** als iSCSI-server). In de **Shares** blade kunt u alle shares (volumes) van het oude apparaat weergeven.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. U moet [maken van een DNS-alias](https://support.microsoft.com/kb/168322) zodat alle toepassingen die verbinding probeert te maken kunnen krijgen omgeleid naar het nieuwe apparaat.

## <a name="errors-during-dr"></a>Fouten tijdens DR

**Cloud-connectiviteit onderbreking tijdens DR**

Als de cloud-connectiviteit wordt onderbroken DR is gestart en voordat het apparaat terugzetten is voltooid, wordt de DR zal mislukken. U ontvangt een melding failore. Het doelapparaat voor herstel na Noodgevallen is gemarkeerd als *onbruikbaar.* U kunt hetzelfde doelapparaat niet gebruiken voor toekomstige DRs.

**Er is geen compatibele doelapparaten**

Als de beschikbare doelapparaten niet voldoende ruimte hebt, ziet u een fout die naar het effect dat er geen doelapparaten compatibel zijn.

**Controle vooraf fouten**

Als een van de prechecks niet is voldaan, ziet u mislukte controle vooraf.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Herstel van zakelijke continuïteit na noodgevallen (BCDR)

Een zakelijke continuïteit (BCDR) noodherstelscenario treedt op wanneer de volledige Azure-datacenter niet meer werkt. Dit kan van invloed op uw StorSimple-apparaat Manager-service en de bijbehorende StorSimple-apparaten.

Als er StorSimple-apparaten die zijn geregistreerd, net voordat een noodgeval is opgetreden, kunnen deze StorSimple-apparaten moet worden verwijderd. U kunt na de noodherstel opnieuw maken en configureren die apparaten.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beheren van uw virtuele StorSimple-matrix met de lokale webgebruikersinterface](storsimple-ova-web-ui-admin.md).

