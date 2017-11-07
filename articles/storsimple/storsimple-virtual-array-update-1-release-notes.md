---
title: StorSimple virtuele matrix Update 1.0 release-opmerkingen | Microsoft Docs
description: Hierin wordt beschreven kritieke open problemen en oplossingen voor de virtuele StorSimple-matrix met Update 1.0.
services: storsimple
documentationcenter: 
author: alkohli
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: alkohli
ms.openlocfilehash: 777718c4893f1edab3613be5dc941e2716d4c972
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="storsimple-virtual-array-update-10-release-notes"></a>StorSimple virtuele matrix Update 1.0 release-opmerkingen

## <a name="overview"></a>Overzicht

De volgende releaseopmerkingen Identificeer de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix.

De release-opmerkingen worden voortdurend bijgewerkt en wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Voordat u uw virtuele StorSimple-matrix implementeert, zorgvuldig door de gegevens in de release-opmerkingen.

Update 1.0 komt overeen met de softwareversie **10.0.10296.0**.

> [!IMPORTANT]
> - Updates kunnen verstoren, start het apparaat. Als i/o's uitgevoerd worden, leidt uitvaltijd ertoe dat het apparaat. Ga voor gedetailleerde instructies voor het toepassen van de update naar [Update 1.0 installeren](storsimple-virtual-array-install-update-1.md).
>
> - Update 1 is alleen beschikbaar voor u via de Azure-portal als uw apparaat Update 0,6 wordt uitgevoerd.

## <a name="whats-new-in-update-10"></a>Wat is er nieuw in Update 1.0

**Update 1.0 bevat wijzigingen die verband houden met verificatie van Apparaatbeheer StorSimple-service en moeten worden geïmplementeerd op de vroegste.** Deze update bevat de volgende verbeteringen en oplossingen voor problemen:

 - **Gebruik van Azure Active Directory (AAD) voor verificatie met service voor Apparaatbeheer StorSimple** – van Update 1.0 of hoger, Azure Active Directory wordt gebruikt voor verificatie met de service Manager voor StorSimple-apparaat. Het oude verificatiemechanisme wordt afgeschaft per December 2017. Alle gebruikers moeten de verificatie-URL's opnemen in hun firewallregels. Voor meer informatie gaat u naar de verificatie-URL's die worden vermeld in de [netwerkvereisten voor uw virtuele StorSimple-matrix](storsimple-ova-system-requirements.md).
 
    Als de verificatie-URL niet in de firewall-regels opgenomen is, ziet de gebruiker een kritieke waarschuwing voor die hun StorSimple-apparaat kan niet worden geverifieerd met de service. Als gebruikers deze waarschuwing wordt weergegeven, moeten deze de nieuwe URL voor webverificatie opnemen. Ga voor meer informatie naar [StorSimple networking waarschuwingen](storsimple-virtual-array-manage-alerts.md).

 - **Prestatieverbeteringen** -verschillende oplossingen voor problemen zijn gedaan om de snelheid van leesbewerkingen van de cloud, laag-modules en laag-outs verbeteren. Hierdoor kunnen verbeterd de back-up- en terugzetten prestaties voor apparaten met iSCSI en file server.

 - **Garbagecollection verzameling verbetering** -deze release bevat oplossingen voor problemen die de prestaties van garbagecollection Verzamelfase bestanden wanneer het apparaat en storage-account zich in twee verafgelegen regio's verbeteren.

 - **Verbetering van de logboekregistratie** -deze release bevat verbeteringen in logboekregistratie die betrekking hebben op garbagecollection en i/o-pad.


## <a name="issues-fixed-in-update-10"></a>Problemen in Update 1.0 opgelost

De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |Verificatie op basis van AAD| Deze release bevat wijzigingen waarmee AAD om te verifiëren met de StorSimple-Apparaatbeheer.|
| 2 |Garbagecollection| Dit probleem is opgetreden bij de klant waarbij de apparaat- en storage-accounts zijn in verschillende regio's en de klant gerapporteerd onregelmatige netwerkfouten, waardoor die invloed hebben op de facturering. In deze release, is dit probleem opgelost. |
| 3 |Prestaties| Deze release bevat wijzigingen die resulteren in terugzetten/cloud leesbewerkingen/laag in / uit prestatieverbeteringen laag.|
| 4 |Update| Er is een probleem met de update in de eerdere versie dat heeft geresulteerd in een mislukte back-ups op een klantsite. Dit probleem is opgelost in deze release.|

## <a name="known-issues-in-update-10"></a>Bekende problemen in Update 1.0

De volgende tabel bevat een samenvatting van bekende problemen voor de virtuele StorSimple-matrix en omvat problemen met de release genoteerd uit de vorige versies.

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele matrices die zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. |Deze virtuele matrices moeten failover worden uitgevoerd voor de algemene beschikbaarheid release met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |U eenmaal hebt ingericht, een gegevensschijf van een bepaalde opgegeven omvang en de bijbehorende virtuele StorSimple-matrix gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Wilt doen resulteert in een verlies van alle gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat lid van een domein is, toepassen van een Groepsbeleid kunt nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale webgebruikersinterface |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige gebruikersinterface lokale webpagina's zoals probleemoplossing of onderhoud niet correct werkt. Knoppen op deze pagina's ook werkt mogelijk niet. |Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale webgebruikersinterface |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-gebruikersinterface worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. 10 Gbps voor virtuele netwerkadapters worden altijd weergegeven in Hyper-V. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bereik aan bytes is vergrendeld in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing in lokaal vastgemaakte volumes in plaats van gelaagde volumes plaatsen.<br></br>*Hoewel*: wanneer met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, lokaal vastgemaakt volume online kan worden voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens wachten op de herstelbewerking is voltooid. |
| **7.** |Gelaagde shares |Werken met grote bestanden kan leiden tot een trage laag. |Wanneer u werkt met grote bestanden, raden we aan dat het grootste bestand kleiner dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteit voor shares gebruikt |Mogelijk ziet u verbruik delen wanneer er geen gegevens op de share zijn. Dit verbruik is omdat het gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen op een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit is geïmplementeerd in een latere release. Ga voor meer informatie naar [Failover en herstel na noodgevallen voor uw virtuele StorSimple-matrix](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |Het virtuele StorSimple-matrices kunnen niet worden beheerd via Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden uitgevoerd via de Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de norm en-us indeling toetsenbord. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Ook als u de CHAP-referenties wijzigt, moet u offline zetten van de volumes en brengt u ze online om de wijziging door te voeren. |Dit probleem is verholpen in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kan niet verschillen in de Apparaatbeheer StorSimple-service en de iSCSI-host. |De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |Bestandsserver |Als een bestand in een map heeft een alternatieve gegevens stroom (ADS) gekoppeld, wordt de ADVERTENTIES geen back-up gemaakt of hersteld via herstel na noodgevallen, klonen en herstel Item. | |
| **15.** |Bestandsserver |Symbolische koppelingen worden niet ondersteund. | |
| **16.** |Bestandsserver |Bestanden beveiligd door Windows Encrypting File System (EFS) wanneer gekopieerd of opgeslagen op het virtuele StorSimple-matrix bestand server resulteren in een niet-ondersteunde configuratie.  | |
| **17.** |Updates |Als u fout ziet code: 2359302 (hex 0x240006) wanneer u probeert een hotfix via de lokale gebruikersinterface installeren, klikt u vervolgens dit betekent dat de hotfix al is geïnstalleerd op uw apparaat.   | |
| **18.** |Updates |Als u de lokale webgebruikersinterface Update 1 installeren op uw virtuele matrix gebruikt, moet u ervoor zorgen dat u Update 0,6 worden uitgevoerd. Als u een versie lager dan 0,6 bijwerken, moet u eerst Update 0,6 installeert en vervolgens toepassen Update 1. Als u Update 1.0 rechtstreeks vanuit een 0,6 versie vóór het bijwerken installeren, vervolgens wordt u bepaalde updates gemist en de bewaking grafieken werkt niet.   | |


## <a name="next-steps"></a>Volgende stappen
[Installeer Update 1.0](storsimple-virtual-array-install-update-1.md) op uw virtuele StorSimple-matrix.

## <a name="references"></a>Verwijzingen
Zoekt u een oudere versie Opmerking? Ga naar:
*  [StorSimple virtuele matrix Update 0,6 Release-opmerkingen](storsimple-virtual-array-update-06-release-notes.md)
* [StorSimple virtuele matrix Update 0,5 Release-opmerkingen](storsimple-virtual-array-update-05-release-notes.md)
* [StorSimple virtuele matrix Update 0,4 Release-opmerkingen](storsimple-virtual-array-update-04-release-notes.md)
* [StorSimple virtuele matrix Update 0,3 Release-opmerkingen](storsimple-ova-update-03-release-notes.md)
* [StorSimple virtuele matrix Update 0,1-0,2 Release-opmerkingen](storsimple-ova-update-01-release-notes.md)
* [StorSimple virtuele matrix algemene beschikbaarheid Release-opmerkingen](storsimple-ova-pp-release-notes.md)
