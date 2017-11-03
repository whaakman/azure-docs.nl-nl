---
title: StorSimple virtuele matrix Updates release-opmerkingen | Microsoft Docs
description: Hierin wordt beschreven kritieke open problemen en oplossingen voor het virtuele StorSimple-matrix met Update 0.3.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: fe9d4f6b232e9abcf1fe9fc5657044b6c72fedb8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple virtuele matrix Update 0,3 release-opmerkingen
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen Identificeer de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Voordat u uw virtuele StorSimple-matrix implementeert, zorgvuldig door de gegevens in de release-opmerkingen.

Update 0.3 komt overeen met de softwareversie **10.0.10288.0**.

> [!NOTE]
> Updates kunnen verstoren, start het apparaat. Als i/o's uitgevoerd worden, leidt uitvaltijd ertoe dat het apparaat.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Wat is er nieuw in de Update 0.3
Update 0.3 is voornamelijk een bug fix build. In deze versie, is enkele fouten, wat leidt tot mislukte back-ups in de vorige versie behandeld.

## <a name="issues-fixed-in-the-update-03"></a>Problemen in de Update 0.3 opgelost
De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Back-ups |Een probleem is gedetecteerd in de eerdere versie waarop de back-ups mislukken om voor een bestandsshare te voltooien. Als dit probleem is opgetreden, mislukken de back-uptaak en een kritieke waarschuwing is gegenereerd op de StorSimple Manager-service moet de gebruiker waarschuwen. Dit probleem heeft geen invloed op de gegevens op de shares of toegang tot de gegevens. De hoofdoorzaak is geïdentificeerd en opgelost in deze release. <br></br> De oplossing niet met terugwerkende kracht van shares die al dit probleem ziet van toepassing. Klanten die dit probleem ziet moeten eerst Update 0.3 toe te passen en vervolgens contact opnemen met Microsoft Support een volledige back-up als het probleem wilt oplossen. In plaats van het contact opneemt met Microsoft Support, kunnen klanten ook herstellen naar een nieuwe share vanuit een goede back-up voor de betrokken shares. |
| 2 |iSCSI |Een probleem is gedetecteerd in de eerdere versie waarop de volumes verdwijnen zou bij het kopiëren van gegevens op een volume op de virtuele StorSimple-matrix. Dit probleem is verholpen in deze release. <br></br> De oplossingen te laten treden alleen op nieuwe volumes gemaakt. De oplossingen niet van toepassing met terugwerkende kracht op volumes die al dit probleem ziet. Klanten wordt aangeraden om te zetten van de betrokken volumes online via de klassieke Azure portal, het uitvoeren van een back-up voor deze volumes en herstel vervolgens deze volumes op nieuwe volumes. |

## <a name="known-issues-in-the-update-03"></a>Bekende problemen in de Update 0.3
De volgende tabel bevat een samenvatting van bekende problemen voor de virtuele StorSimple-matrix en omvat problemen met de release genoteerd uit de vorige versies. 

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele apparaten die zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. |Deze virtuele apparaten moeten failover worden uitgevoerd voor de algemene beschikbaarheid release met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Als u een gegevensschijf van een bepaalde opgegeven omvang hebt ingericht en de bijbehorende virtuele StorSimple-apparaat hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Wilt doen resulteert in een verlies van alle gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat lid van een domein is, toepassen van een Groepsbeleid kunt nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale webgebruikersinterface |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige gebruikersinterface lokale webpagina's zoals probleemoplossing of onderhoud niet correct werkt. Knoppen op deze pagina's ook werkt mogelijk niet. |Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale webgebruikersinterface |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-gebruikersinterface worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. 10 Gbps voor virtuele netwerkadapters worden altijd weergegeven in Hyper-V. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bereik aan bytes is vergrendeld in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing in lokaal vastgemaakte volumes in plaats van gelaagde volumes plaatsen.<br></br>*Hoewel*: wanneer met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, lokaal vastgemaakt volume online kan worden voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens wachten op de herstelbewerking is voltooid. |
| **7.** |Gelaagde shares |Werken met grote bestanden kan leiden tot een trage laag. |Wanneer u werkt met grote bestanden, raden we aan dat het grootste bestand kleiner dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteit voor shares gebruikt |Mogelijk ziet u verbruik delen wanneer er geen gegevens op de share zijn. Dit is omdat het gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen op een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere release. |
| **10.** |Azure PowerShell |Het virtuele StorSimple-apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden uitgevoerd via de klassieke Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de indeling en-US. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de CHAP-referenties wijzigt, moet u offline zetten van de volumes en brengt u ze online om de wijziging door te voeren. |Dit probleem is verholpen in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kan niet verschillen in de StorSimple Manager-service en de iSCSI-host. |De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map heeft een alternatieve gegevens stroom (ADS) gekoppeld, wordt de ADVERTENTIES geen back-up gemaakt of hersteld via herstel na noodgevallen, klonen en herstel Item. | |

## <a name="next-step"></a>Volgende stap
[Installeer Update 0.3](storsimple-ova-install-update-01.md) op uw virtuele StorSimple-matrix.

## <a name="references"></a>Verwijzingen
Zoekt u een oudere versie Opmerking? Ga naar: 

* [StorSimple virtuele matrix Update 0,1-0,2 Release-opmerkingen](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuele matrix algemene beschikbaarheid Release-opmerkingen](storsimple-ova-pp-release-notes.md)

