---
title: Opmerkingen bij de release van de StorSimple 8000 Series Update 2 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 2.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: ''
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: f23a507ab631be553613e22cafa037291548a8aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60530859"
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 Series Update 2

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en de kritieke open problemen identificeren voor StorSimple 8000 Series Update 2. Ze bevatten ook een lijst van de StorSimple-software, stuurprogramma en updates van de schijffirmware inbegrepen in deze release. 

Update 2 kan worden toegepast op alle StorSimple-apparaat met Release (GA) of Update 0.1 via Update 1.2. De versie van het apparaat dat is gekoppeld met Update 2 is 6.3.9600.17673.

Lees de informatie in de opmerkingen bij de release voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Het duurt ongeveer 4-7 uur om deze update (met inbegrip van de Windows-updates) te installeren. 
> * Update 2 heeft de software, LSI-stuurprogramma en SSD firmware-updates.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk dat er een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze snel weer beschikbaar is.
> 
> 

## <a name="whats-new-in-update-2"></a>Wat is er nieuw in Update 2
Update 2 worden de volgende nieuwe functies geïntroduceerd.

* **Lokaal vastgemaakt volumes** – In eerdere versies van de StorSimple 8000-reeks blokken met gegevens zijn gelaagde in de cloud op basis van gebruik. Er is geen manier om te garanderen dat blokken op lokale zou blijven. Bij Update 2, bij het maken van een volume, kunt u opgeven een volume als lokaal vastgemaakt en primaire uit het volume wordt niet worden gelaagde gegevens in de cloud. Momentopnamen van lokaal vastgemaakte volumes worden nog steeds worden gekopieerd naar de cloud voor back-up zodat de cloud kan worden gebruikt voor gegevenshersteldoeleinden voor mobiliteit en noodherstel. Bovendien kunt u het volumetype (dat wil zeggen, converteren gelaagde volumes naar lokaal vastgemaakte volumes en converteren lokaal vastgemaakt volumes aan lagen). 
* **StorSimple-apparaat verbeteringen** – voorheen de StorSimple 8000-serie van het virtuele apparaat geplaatst als een oplossing voor disaster recovery of ontwikkelen/testen. Er is slechts één model (model 1100) van het virtuele apparaat. Update 2 introduceert twee modellen van het virtuele apparaat: 
  
  * 8010 (voorheen de 1100): Er is geen wijziging; heeft een capaciteit van 30 TB en maakt gebruik van Azure standard-opslag.
  * 8020-heeft een capaciteit van 64 TB en maakt gebruik van Azure Premium storage voor betere prestaties.
    
    Er is één VHD voor beide modellen virtuele apparaten (8010/8020). Wanneer u het virtuele apparaat voor het eerst start, detecteert de parameters van het platform en de juiste versie is van toepassing.
* **Netwerkverbeteringen** – Update 2 bevat de volgende verbeteringen voor netwerken:
  
  * Meerdere NIC's kunnen worden ingeschakeld voor de cloud zodat failover zich voordoen kan als een NIC is mislukt.
  * Routering verbeteringen, met vaste metrische gegevens voor cloud ingeschakeld blokken.
  * Online opnieuw proberen van mislukte resources voordat u een failover.
  * Nieuwe waarschuwingen voor servicefouten.
* **Bijwerken van de verbeteringen** – In Update 1.2 en eerder, de StorSimple 8000-serie is bijgewerkt via twee kanalen: Windows Update voor clustering, iSCSI, en enzovoort, en Microsoft Update voor de binaire bestanden en -firmware.
    Update 2 maakt gebruik van Microsoft Update voor alle updatepakketten. Dit moet leiden tot minder tijd patches of failovers uitvoeren. 
* **Firmware-updates** : de volgende firmware-updates zijn opgenomen:
  
  * LSI: lsi_sas2.sys Product Version 2.00.72.10
  * Alleen SSD (Er zijn geen updates van de harde schijf): XMGG, XGEG, KZ50, F6C2 en VR08
* **Ondersteuning voor proactieve** – Update 2 kan Microsoft voor het ophalen van aanvullende diagnostische gegevens van het apparaat. Bij ons team bewerkingen wordt aangegeven welke apparaten problemen ondervindt, zijn we beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren. **Update 2 accepteert, kunnen we deze proactieve ondersteuning te bieden**.    

## <a name="issues-fixed-in-update-2"></a>Problemen opgelost met Update 2
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Updates 2.    

| Nee. | Functie | Probleem | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Netwerkinterfaces |Na een upgrade naar Update 1, de StorSimple Manager-service heeft gerapporteerd dat de Data2 en Data3 poorten op een domeincontroller is mislukt. Dit probleem is opgelost. |Ja |Nee |
| 2 |Updates |Na een upgrade naar Update 1, hoorbaar alarm waarschuwingen opgetreden in de klassieke Azure portal op meerdere apparaten. Dit probleem is opgelost. |Ja |Nee |
| 3 |Openstack-verificatie |Wanneer u Openstack als uw cloudserviceprovider, kan u een foutbericht weergegeven dat de tekenreeks van uw cloud-verificatie te lang is. Dit probleem is opgelost. |Ja |Nee |

## <a name="known-issues-in-update-2"></a>Bekende problemen in Update 2
De volgende tabel bevat een overzicht van de bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Is van toepassing op het fysieke apparaat | Is van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat zijn verbroken, wat resulteert in geen quorum schijf gaat vervolgens de opslaggroep offline. Dit wordt offline blijven, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, kunt u contact op met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Wanneer een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de installatiekopie wordt geladen vanuit het peer-knooppunt kan de besturings-ID weergegeven in eerste instantie als de peer-controller-ID. In zeldzame gevallen kan dit gedrag ook worden weergegeven na het systeem opnieuw is opgestart. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de netwerkcontroller te vervangen voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Met de Storage-service voor het verwijderen van het opslagaccount is een niet-ondersteund scenario. Dit zal leiden tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van het apparaat |Meerdere failovers van een volumecontainer van het bronapparaat dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één dead apparaat naar meerdere apparaten wordt de volumecontainers van het eerste apparaat een failover geen eigenaar van gegevens maken. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u ze in de klassieke Azure portal weergeven. | |Ja |Nee |
| 5 |Installatie |Tijdens de StorSimple-Adapter voor SharePoint-installatie moet u voor een IP-adres van het apparaat in de volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de apparaat-naar-servicecommunicatie worden beïnvloed en het apparaat gaat offline. Voor ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de URL van proxy HTTP als het opgegeven protocol heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureren en webproxy op een geregistreerd apparaat inschakelt, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en hoge i/o-werkbelasting tegenkomt, de apparaatvolumes gaat u in een gedegradeerde status en de i/o's mislukken mogelijk vanwege een fout 'het apparaat is niet gereed'. |U moet handmatig opnieuw opstarten Apparaatcontrollers of uitvoeren van een failover van het apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Bij het gebruik van de StorSimple-cmdlet **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - Wait** het eerste object selecteren, zodat u kunt een nieuwe maken **Upopslagaccount** object, de cmdlet retourneert alle objecten. |De cmdlet als volgt tussen haakjes verpakken: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object -First 1 -Wait** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de verwachte Aankomsttijd voor de meest recente back-up nauwkeurige alleen voor de eerste volumecontainer. Bovendien begint parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer worden gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het herstel, zijn de volumes niet toegevoegd aan het back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet de 5000/7000-serie-apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijderen nadat de migratie voltooid en wordt doorgevoerd is. |Ja |Nee |
| 13 |Kloon en DR |Een StorSimple-apparaat met Update 1 kan klonen of uitvoeren van herstel na noodgevallen naar een apparaat met 1 software vóór het bijwerken. |U moet het doelapparaat bijwerken op 1 om toe te staan van deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Back-up van configuratie voor de migratie mislukken op een apparaat uit de 5000-7000 serie wanneer er volumegroepen met geen gekoppelde volumes. |Verwijderen van alle groepen van een leeg volume met geen gekoppelde volumes en voer vervolgens de configuratie van back-up opnieuw uit. |Ja |Nee |
| 15 |Azure PowerShell-cmdlets en lokaal vastgemaakte volumes |U kunt een lokaal vastgemaakt volume via Azure PowerShell-cmdlets kan niet maken. (Alle volumes die u via Azure PowerShell maakt worden gelaagd.) |Gebruik altijd de StorSimple Manager-service voor het configureren van lokaal vastgemaakte volumes. |Ja |Nee |
| 16 |Beschikbare schijfruimte voor het lokaal vastgemaakte volumes |Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-service-updates de lokale ruimte die beschikbaar ongeveer elk uur. |Wacht een uur voordat u probeert te maken van het nieuwe volume. |Ja |Nee |
| 17 |Lokaal vastgemaakte volumes |De hersteltaak wordt aangegeven dat de tijdelijke momentopnameback-up in de back-catalogus, maar alleen voor de duur van de hersteltaak. Bovendien het wordt aangegeven dat een groep virtuele schijf met het voorvoegsel **tmpCollection** op de **back-upbeleid** pagina, maar alleen voor de duur van de hersteltaak. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 18 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleren en een failover van de controller gebeurt onmiddellijk daarna wordt de hersteltaak wordt weergegeven **mislukt** in plaats van **geannuleerd**. Als een hersteltaak mislukt en er treedt een failover van de controller op direct daarna de hersteltaak wordt weergegeven **geannuleerd** in plaats van **mislukt**. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 19 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleert of als een herstelpunt is mislukt en treedt er een failover van de controller op, een extra hersteltaak wordt weergegeven op de **taken** pagina. |Dit probleem kan optreden als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. Als de hersteltaak alleen gelaagde volumes bevat, wordt klikt u vervolgens dit gedrag niet uitgevoerd. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 20 |Lokaal vastgemaakte volumes |Als u probeert te converteren van een gelaagd volume (gemaakt en gekloonde met Update 1.2 of eerder) naar een lokaal vastgemaakt volume en uw apparaat bijna vol is of er een storing in de cloud is, kunnen de clone(s) worden beschadigd. |Dit probleem treedt alleen op volumes die gemaakt en gekloonde met vooraf Update 2-software zijn. Dit moet een onregelmatige scenario. | | |
| 21 |Volumeconversie |De ACR's die zijn gekoppeld aan een volume, terwijl de volumeconversie van een uitgevoerd wordt, niet bijwerken (gelaagde in lokaal vastgemaakt of omgekeerd). Bijwerken van de ACR's kan leiden tot beschadigde gegevens. |Indien nodig, bijwerken van de ACR's voordat u de volumeconversie en maak geen verdere ACR updates tijdens de conversie uitgevoerd wordt. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller en firmware-updates in Update 2
Deze versie werkt het stuurprogramma en de schijffirmware op uw apparaat.

* Voor meer informatie over de LSI-firmware bijwerken, Zie Microsoft Knowledge base-artikel 3121900. 
* Voor meer informatie over de schijffirmware bijwerken, Zie Microsoft Knowledge base-artikel 3121899.

## <a name="virtual-device-updates-in-update-2"></a>Updates van de virtuele apparaat in Update 2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over het [Update 2 installeren](storsimple-install-update-2.md) op uw StorSimple-apparaat.

