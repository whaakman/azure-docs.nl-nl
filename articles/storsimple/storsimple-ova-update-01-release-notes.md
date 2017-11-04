---
title: StorSimple virtuele matrix Updates release-opmerkingen | Microsoft Docs
description: Kritieke open problemen en oplossingen beschrijft voor de virtuele StorSimple-matrix met Update 0.2 en 0,1.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: c4ccde9635b3874864baa9d4d262ff5ddcf2a425
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Opmerkingen bij de release van de StorSimple virtuele matrix Update 0.2 en 0,1
## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen Identificeer de kritieke problemen openen en de problemen opgelost voor updates van Microsoft Azure StorSimple virtuele matrix. (Microsoft Azure StorSimple virtuele matrix is ook bekend als de lokale virtueel StorSimple-apparaat of het virtuele StorSimple-apparaat.) 

De release-opmerkingen worden voortdurend bijgewerkt en wanneer er kritieke problemen waarvoor een tijdelijke oplossing nodig worden ontdekt, worden ze toegevoegd. Voordat u uw virtuele StorSimple-apparaat implementeert, zorgvuldig door de gegevens in de release-opmerkingen.

Update 0.2 komt overeen met de softwareversie **10.0.10280.0**; Update 0.1 is versie **10.0.10279.0**. De onderstaande secties worden de wijzigingen voor elke update. 

> [!NOTE]
> Updates kunnen verstoren, wordt uw apparaat opnieuw opstarten. Als i/o's uitgevoerd worden, wordt het apparaat uitvaltijd in rekening gebracht.
> 
> 

## <a name="issues-fixed-in-the-update-02"></a>Problemen in de Update 0.2 opgelost
Update 0.2 bevat alle wijzigingen van de Update 0.1 naast de oplossing in de volgende tabel beschreven:

| Functie | Probleem |
| --- | --- |
| Updates |In de laatste versie zijn niet updates automatisch gedetecteerd in de klassieke Azure portal, zodat moest u de lokale Webgebruikersinterface gebruiken om updates te installeren. Dit probleem is opgelost in deze release. U kunt na de installatie van Update 0.2 toekomstige updates met de klassieke Azure portal installeren. |

## <a name="whats-new-in-the-update-01"></a>Wat is er nieuw in de Update 0.1
Update 0.1 bevat de volgende oplossingen voor problemen en verbeteringen. 

* **Verbeterde tolerantie voor cloud uitval**: deze release heeft verschillende oplossingen voor problemen rond herstel na noodgevallen, back-up, herstel en lagen in het geval van een onderbreking van de cloud-connectiviteit. 
* **Verbeterde prestaties terugzetten**: deze versie bevat oplossingen voor problemen die u hebt de voltooiingstijd van de hersteltaken aanzienlijk verminderen.
* **Ruimte vrijmaken optimalisatie geautomatiseerde**: wanneer gegevens worden verwijderd op dun ingerichte volumes, de blokken ongebruikte opslag moeten worden vrijgemaakt. Deze release is het proces voor het vrijmaken van ruimte uit de cloud waardoor de ongebruikte ruimte wordt sneller beschikbaar in vergelijking met vorige versies worden verbeterd.
* **Installatiekopieën van het nieuwe virtuele schijf**: nieuwe VHD en VHDX VMDK zijn nu beschikbaar via de klassieke Azure portal. U kunt deze installatiekopieën voor het inrichten van nieuwe Update 0.1 apparaten downloaden.
* **Verbetering van de nauwkeurigheid van de status van taken in de portal**: In de eerdere versie van software, taakstatus rapportage in de portal is niet gedetailleerde. Dit probleem wordt opgelost in deze release.
* **Domain join-ervaring**: oplossingen voor problemen die zijn gerelateerd aan lid te worden van domein en het hernoemen van het apparaat.

## <a name="issues-fixed-in-the-update-01"></a>Problemen in de Update 0.1 opgelost
De volgende tabel bevat een samenvatting van problemen in deze release worden opgelost.

| Nee. | Functie | Probleem |
| --- | --- | --- |
| 1 |VMDK |De OS-schijf is in sommige versies van VMware gezien als sparse die waarschuwingen veroorzaken en normale bewerkingen verstoren. Dit is opgelost in deze release. |
| 2 |iSCSI-server |De gebruiker is in de laatste versie vereist voor het opgeven van een gateway voor elke ingeschakelde netwerkinterface van uw virtuele StorSimple-apparaat. Dit gedrag is gewijzigd in deze release, zodat de gebruiker heeft ten minste één gateway voor de ingeschakelde netwerkinterfaces configureren. |
| 3 |Ondersteuningspakket |Ondersteuning voor verzameling van pakket mislukt als de grootte van het pakket groter dan 1 GB zijn in de eerdere versie van software. Dit probleem is opgelost in deze release. |
| 4 |Toegang tot de cloud |In de laatste versie als de virtuele StorSimple-matrix is geen verbinding met het netwerk en opnieuw is opgestart, heeft de gebruikersinterface van de lokale problemen met de netwerkverbinding. Dit probleem is opgelost in deze release. |
| 5 |Bewaking van grafieken |De cloud capaciteit gebruik grafieken weergegeven in de vorige release, na een failover apparaat onjuiste waarden in de klassieke Azure portal. Dit probleem wordt opgelost in de huidige release. |

## <a name="known-issues-in-the-update-01"></a>Bekende problemen in de Update 0.1
De volgende tabel bevat een samenvatting van bekende problemen voor de virtuele StorSimple-matrix en omvat problemen met de release genoteerd uit de vorige versies. **De versie van de problemen vermeld in deze release zijn gemarkeerd met een sterretje. Bijna alle problemen in deze lijst hebt overgedragen van de GA-release van het virtuele StorSimple-matrix.**

| Nee. | Functie | Probleem | Tijdelijke oplossing/opmerkingen |
| --- | --- | --- | --- |
| **1.** |Updates |De virtuele apparaten die zijn gemaakt in de preview-versie kunnen niet worden bijgewerkt naar een ondersteunde versie van de algemene beschikbaarheid. |Deze virtuele apparaten moeten failover worden uitgevoerd voor de algemene beschikbaarheid release met behulp van een werkstroom van disaster recovery (DR). |
| **2.** |Ingerichte gegevensschijf |Als u een gegevensschijf van een bepaalde opgegeven omvang hebt ingericht en de bijbehorende virtuele StorSimple-apparaat hebt gemaakt, moet u niet uitbreiden of verkleinen van de gegevensschijf. Poging om dit te doen leidt tot verlies van de gegevens in de lokale lagen van het apparaat. | |
| **3.** |Groepsbeleid |Wanneer een apparaat lid van een domein is, toepassen van een Groepsbeleid kunt nadelige invloed heeft op het apparaat opnieuw. |Zorg ervoor dat uw virtuele matrix in de eigen organisatie-eenheid (OE) voor Active Directory en geen groepsbeleidsobjecten (GPO) worden toegepast. |
| **4.** |Lokale webgebruikersinterface |Als verbeterde beveiligingsfuncties zijn ingeschakeld in Internet Explorer (IE ESC), is het mogelijk dat sommige gebruikersinterface lokale webpagina's zoals probleemoplossing of onderhoud niet correct werkt. Knoppen op deze pagina's ook werkt mogelijk niet. |Verbeterde beveiligingsfuncties in Internet Explorer uitschakelen. |
| **5.** |Lokale webgebruikersinterface |In een Hyper-V virtuele machine, de netwerkinterfaces in de web-gebruikersinterface worden weergegeven als 10 Gbps-interfaces. |Dit gedrag is een weerspiegeling van Hyper-V. 10 Gbps voor virtuele netwerkadapters worden altijd weergegeven in Hyper-V. |
| **6.** |Gelaagde volumes of shares |Bytebereik vergrendelen voor toepassingen die werken met de StorSimple gelaagde volumes wordt niet ondersteund. Als byte bereik vergrendeling is ingeschakeld, werkt StorSimple lagen niet. |Aanbevolen maatregelen zijn onder andere: <br></br>Bereik aan bytes is vergrendeld in uw toepassingslogica uitschakelen.<br></br>Wilt u gegevens voor deze toepassing in lokaal vastgemaakte volumes in plaats van gelaagde volumes plaatsen.<br></br>*Hoewel*: als met behulp van lokaal volumes vastgemaakte en byte bereik vergrendeling is ingeschakeld, worden op de hoogte dat lokaal vastgemaakt volume online worden kan voordat de herstelbewerking voltooid is. In dergelijke gevallen, als een herstelbewerking uitgevoerd wordt, moet vervolgens wachten op de herstelbewerking is voltooid. |
| **7.** |Gelaagde shares |Werken met grote bestanden kan leiden tot een trage laag. |Wanneer u werkt met grote bestanden, raden we aan dat het grootste bestand kleiner dan 3% van de grootte van de bestandsshare is. |
| **8.** |Capaciteit voor shares gebruikt |Mogelijk ziet u weer in de afwezigheid van alle gegevens op de share te delen. Dit is omdat het gebruikte capaciteit voor shares metagegevens bevat. | |
| **9.** |Herstel na noodgeval |U kunt alleen het herstel na noodgevallen van een bestandsserver aan hetzelfde domein als die van het bronapparaat uitvoeren. Herstel na noodgevallen op een doelapparaat in een ander domein wordt niet ondersteund in deze release. |Dit wordt geïmplementeerd in een latere versie. |
| **10.** |Azure PowerShell |Het virtuele StorSimple-apparaten kunnen niet worden beheerd via de Azure PowerShell in deze release. |Het beheer van de virtuele apparaten moet worden uitgevoerd via de klassieke Azure portal en de lokale webgebruikersinterface. |
| **11.** |Wachtwoord wijzigen |De console van het virtuele matrix accepteert alleen invoer in de indeling en-US. | |
| **12.** |CHAP |CHAP-referenties eenmaal is gemaakt, kunnen niet worden verwijderd. Bovendien, als u de referenties CHAP wijzigt, moet u offline zetten van de volumes en brengt u ze online om de wijziging door te voeren. |Deze wordt opgelost in een latere versie. |
| **13.** |iSCSI-server |De 'opslagruimte hebt gebruikt' weergegeven voor een iSCSI-volume kan niet verschillen in de StorSimple Manager-service en de iSCSI-host. |De iSCSI-host heeft de weergave van het bestandssysteem.<br></br>Het apparaat, ziet de blokken toegewezen wanneer het volume bij de maximale grootte is. |
| **14.** |File server * |Als een bestand in een map heeft een alternatieve gegevens stroom (ADS) gekoppeld, wordt de ADVERTENTIES geen back-up gemaakt of hersteld via herstel na noodgevallen, klonen en herstel Item. | |

## <a name="next-step"></a>Volgende stap
[Updates installeren](storsimple-ova-install-update-01.md) op uw virtuele StorSimple-matrix.

