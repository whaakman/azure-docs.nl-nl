---
title: Opmerkingen bij de release van de StorSimple 8000 Series Update 2 | Microsoft Docs
description: Beschrijft de nieuwe functies, problemen en tijdelijke oplossingen voor StorSimple 8000 Series Update 2.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/03/2017
ms.author: v-sharos
ms.openlocfilehash: 285c5abf574544737f3d30981a6c5b8f9548922a
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>Opmerkingen bij de release van de StorSimple 8000 Series Update 2
> [!NOTE]
> De klassieke portal voor StorSimple is afgeschaft. Uw Managers StorSimple-apparaat wordt automatisch verplaatst naar de nieuwe Azure portal aan de hand van de planning afschaffing. U ontvangt een e-mailbericht en een portal melding voor deze verplaatsen. Dit document wordt ook snel worden ingetrokken. Zie voor vragen met betrekking tot de verplaatsing, [Veelgestelde vragen over: verplaatsen naar Azure-portal](storsimple-8000-move-azure-portal-faq.md).

## <a name="overview"></a>Overzicht
De volgende releaseopmerkingen beschrijven de nieuwe functies en problemen met de kritieke open voor StorSimple 8000 Series Update 2. Ze bevatten ook een lijst van de StorSimple-software, stuurprogramma en schijf firmware-updates is opgenomen in deze release. 

Update 2 kan worden toegepast op een StorSimple-apparaat met Release (GA) of Update 0.1 via Update 1.2. De versie van het apparaat dat is gekoppeld met Update 2 is 6.3.9600.17673.

Controleer de informatie in de release-opmerkingen voordat u de update in uw StorSimple-oplossing implementeert.

> [!IMPORTANT]
> * Het duurt ongeveer 4-7 uur deze update (met inbegrip van de Windows-updates) te installeren. 
> * Update 2 heeft de software, LSI stuurprogramma en SSD-firmware-updates.
> * Voor nieuwe releases, kunnen er geen updates onmiddellijk omdat we een gefaseerde implementatie van de updates. Wacht een paar dagen en vervolgens zoeken naar updates opnieuw als deze is binnenkort beschikbaar.
> 
> 

## <a name="whats-new-in-update-2"></a>Wat is er nieuw in Update 2
Update 2 introduceert de volgende nieuwe functies.

* **Lokaal vastgemaakte volumes** – blokken met gegevens waren In eerdere versies van de StorSimple 8000-serie lagen naar de cloud op basis van gebruik. Er is geen manier om te garanderen dat blokken op de lokale zou blijven. Bij Update 2 bij het maken van een volume, kunt u aanwijzen een volume lokaal vastgemaakt en primaire gegevens van dat volume wordt niet in tiers worden verdeeld in de cloud. Momentopnamen van lokaal vastgemaakte volumes nog steeds worden gekopieerd naar de cloud voor back-up, zodat de cloud kan worden gebruikt voor hersteldoeleinden gegevens mobiliteit en noodherstel. Bovendien kunt u het volumetype (dat wil zeggen, converteren gelaagde volumes tot lokaal vastgemaakte volumes en gelaagde volumes te converteren lokaal vastgemaakt). 
* **StorSimple-apparaat verbeteringen** – voorheen de serie StorSimple 8000 het virtuele apparaat geplaatst als een oplossing voor disaster recovery of ontwikkeling en testen. Er is slechts één model van het virtuele apparaat (model 1100). Update 2 introduceert twee modellen van het virtuele apparaat: 
  
  * 8010 (voorheen de 1100) – geen wijziging; heeft een capaciteit van 30 TB en maakt gebruik van Azure standard-opslag.
  * 8020 – heeft een capaciteit van 64 TB en maakt gebruik van Azure Premium-opslag voor verbeterde prestaties.
    
    Er is een enkel VHD voor beide modellen virtuele apparaat (8010/8020). Wanneer u het virtuele apparaat voor het eerst start, detecteert de platform-parameters en de versie van het juiste van toepassing is.
* **Netwerkverbeteringen** – Update 2 bevat de volgende verbeteringen voor netwerken:
  
  * Meerdere NIC's kunnen worden ingeschakeld voor de cloud zodat failover zich voordoen kan als een NIC is mislukt.
  * Routering verbeteringen, met vaste metrische gegevens voor cloud blokken ingeschakeld.
  * Online opnieuw uit met mislukte resources voordat u een failover.
  * Nieuwe waarschuwingen voor servicefouten.
* **Bijwerken van verbeteringen** : In bijwerken 1.2 en eerder, de StorSimple 8000-serie is bijgewerkt via twee kanalen: Windows Update voor clustering, iSCSI, en enzovoort en Microsoft Update voor binaire bestanden en -firmware.
    Update 2 gebruikt Microsoft Update voor alle updatepakketten. Deze moet leiden tot minder tijd patchen of failovers doet. 
* **Firmware-updates** – de volgende firmware-updates zijn opgenomen:
  
  * LSI: lsi_sas2.sys productversie 2.00.72.10
  * Alleen SSD (Er zijn geen updates HDD): XMGG, XGEG KZ50, F6C2 en VR08
* **Proactieve ondersteuning** – Update 2 kan Microsoft voor het ophalen van aanvullende diagnostische gegevens van het apparaat. Als onze teamleden identificeert de apparaten die problemen ondervindt, zijn er beter ingericht voor het verzamelen van informatie van het apparaat en problemen diagnosticeren. **Update 2 accepteert, kunnen we deze proactieve ondersteuning bieden**.    

## <a name="issues-fixed-in-update-2"></a>Problemen opgelost met Update 2
De volgende tabel bevat een samenvatting van problemen die zijn verholpen in Updates 2.    

| Nee. | Functie | Probleem | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- |
| 1 |Netwerkinterfaces |Na een upgrade naar Update 1, de StorSimple Manager-service heeft gerapporteerd dat de Data2 en Data3 poorten op een domeincontroller is mislukt. Dit probleem is opgelost. |Ja |Nee |
| 2 |Updates |Na een upgrade naar Update 1, akoestisch alarm waarschuwingen opgetreden in de klassieke Azure portal op meerdere apparaten. Dit probleem is opgelost. |Ja |Nee |
| 3 |Openstack verificatie |Wanneer u Openstack als uw cloudserviceprovider, kan een foutbericht dat uw cloud-verificatie-tekenreeks te lang is. Dit probleem is opgelost. |Ja |Nee |

## <a name="known-issues-in-update-2"></a>Bekende problemen in Update 2
De volgende tabel bevat een samenvatting van bekende problemen in deze release.

| Nee. | Functie | Probleem | Opmerkingen / tijdelijke oplossing | Van toepassing op het fysieke apparaat | Van toepassing op het virtuele apparaat |
| --- | --- | --- | --- | --- | --- |
| 1 |Schijf quorum |In zeldzame gevallen, als het merendeel van de schijven in de behuizing EBOD van een 8600-apparaat niet zijn verbonden met waardoor geen quorum schijf gaat vervolgens de opslaggroep offline. Het veld offline blijft, zelfs als de schijven opnieuw worden verbonden. |U moet het apparaat opnieuw opstarten. Als het probleem zich blijft voordoen, neem contact met Microsoft Support voor de volgende stappen. |Ja |Nee |
| 2 |Onjuiste besturings-ID |Als een vervangende domeincontroller wordt uitgevoerd, kan controller 0 als controller 1 weergegeven. Tijdens de vervanging van de domeincontroller, wanneer de afbeelding is geladen vanuit het knooppunt peer de besturings-ID kan worden weergegeven in eerste instantie als peer-controller-ID. In zeldzame gevallen kan worden dit probleem ook gezien na opnieuw opstarten. |Er is geen gebruikersactie vereist. Deze situatie wordt automatisch opgelost nadat de vervangende domeincontroller voltooid is. |Ja |Nee |
| 3 |Opslagaccounts |Het opslagaccount verwijderen met behulp van de Storage-service is een niet-ondersteund scenario. Dit leidt tot een situatie waarin de gebruikersgegevens kan niet worden opgehaald. | |Ja |Ja |
| 4 |Failover van apparaat |Meerdere failovers van een volumecontainer van het bronapparaat met dezelfde aan verschillende doelapparaten wordt niet ondersteund. Failover van één apparaat inactief naar meerdere apparaten, brengt de volumecontainers op de eerste failover apparaat is Gegevenseigendom gaan verloren. Na een failover wordt deze volumecontainers worden weergegeven of zich anders gedragen wanneer u deze in de klassieke Azure portal bekijken. | |Ja |Nee |
| 5 |Installeren |Tijdens het StorSimple-Adapter voor SharePoint-installatie moet u voor ondersteuning van een apparaat IP in volgorde voor de installatie met succes te voltooien. | |Ja |Nee |
| 6 |Webproxy |Als uw webproxyconfiguratie HTTPS als het opgegeven protocol heeft, de communicatie van uw apparaat-naar-service worden beïnvloed en het apparaat gaat offline. Ondersteuningspakketten wordt ook gegenereerd in het proces, verbruikt behoorlijk aanspraak op uw apparaat. |Zorg ervoor dat de proxy-URL HTTP als protocol opgegeven heeft. Zie [Configure web proxy for your device](storsimple-configure-web-proxy.md) (Webproxy voor uw apparaat configureren) voor meer informatie. |Ja |Nee |
| 7 |Webproxy |Als u configureert en webproxy op een geregistreerd apparaat inschakelen, moet u de actieve controller op uw apparaat opnieuw opstarten. | |Ja |Nee |
| 8 |Cloud hoge latentie en hoge i/o-werkbelasting |Wanneer uw StorSimple-apparaat een combinatie van zeer hoge cloud latenties (volgorde van seconden) en de hoge i/o-belasting tegenkomt, gaat u de volumes van het apparaat in een gedegradeerde status en de i/o's mislukken met een 'apparaat niet gereed'-fout. |U moet handmatig opnieuw opstarten van de Apparaatcontrollers of voer een failover apparaat deze situatie te herstellen. |Ja |Nee |
| 9 |Azure PowerShell |Wanneer u de cmdlet StorSimple gebruiken **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - eerst 1 - wacht** het eerste object selecteren, zodat u kunt een nieuwe maken **VolumeContainer** object, de cmdlet retourneert alle objecten. |Inpakken van de cmdlet tussen haakjes als volgt: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - eerst 1 - wachttijd** |Ja |Ja |
| 10 |Migratie |Wanneer meerdere volumecontainers voor migratie worden doorgegeven, is de ETA voor de meest recente back-up nauwkeurig is alleen voor de eerste volumecontainer. Bovendien start parallelle migratie nadat de eerste 4 back-ups in de eerste volumecontainer zijn gemigreerd. |Het is raadzaam dat u een volumecontainer tegelijk migreren. |Ja |Nee |
| 11 |Migratie |Na het terugzetten zijn volumes niet toegevoegd aan de back-upbeleid of de groep van de virtuele schijf. |U moet deze volumes toevoegen aan een back-upbeleid om te kunnen maken van back-ups. |Ja |Ja |
| 12 |Migratie |Nadat de migratie voltooid is, moet het 5000/7000-serie apparaat geen toegang tot de gemigreerde gegevenscontainers. |Het is raadzaam dat u de gemigreerde gegevenscontainers verwijdert nadat de migratie voltooid en doorgevoerd is. |Ja |Nee |
| 13 |Kloon en Noodherstel |Een StorSimple-apparaat met Update 1 kan niet klonen of herstel na noodgevallen bij een apparaat met 1 software vóór het bijwerken uitvoeren. |U moet het doelapparaat op 1 bijwerken zodat deze bewerkingen bijwerken |Ja |Ja |
| 14 |Migratie |Configuratieback-up voor de migratie kan op een apparaat 5000 7000-serie mislukken wanneer er groepen met geen gekoppelde volumes volume zijn. |Alle lege volume-groepen die geen gekoppelde volumes verwijderen en probeer vervolgens de configuratieback-up. |Ja |Nee |
| 15 |Azure PowerShell-cmdlets en lokaal vastgemaakte volumes |U kunt een lokaal vastgemaakt volume via Azure PowerShell-cmdlets kan niet maken. (Alle volumes die u via Azure PowerShell maakt worden gelaagd.) |Gebruik altijd de StorSimple Manager-service voor het configureren van lokaal vastgemaakte volumes. |Ja |Nee |
| 16 |Beschikbare schijfruimte voor het lokaal vastgemaakte volumes |Als u een lokaal vastgemaakt volume verwijdert, worden de beschikbare schijfruimte voor de nieuwe volumes mogelijk niet meteen bijgewerkt. De StorSimple Manager-service-updates de lokale schijfruimte ongeveer om het uur. |Wacht tot een uur voordat u probeert te maken van het nieuwe volume. |Ja |Nee |
| 17 |Lokaal vastgemaakte volumes |De hersteltaak wordt de momentopname van de tijdelijke back-up in de back-upcatalogus, maar alleen voor de duur van de hersteltaak. Bovendien beschrijft de virtuele schijf wordt een groep met voorvoegsel **tmpCollection** op de **back-upbeleid** pagina, maar alleen voor de duur van de hersteltaak. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 18 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleren en een failover controller optreedt onmiddellijk daarna de hersteltaak ziet **mislukt** in plaats van **geannuleerd**. Als een hersteltaak mislukt en een failover controller optreedt onmiddellijk daarna de hersteltaak ziet **geannuleerd** in plaats van **mislukt**. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 19 |Lokaal vastgemaakte volumes |Als u een hersteltaak annuleert of als een terugzetbewerking uitvalt en vervolgens een controller-failover wordt uitgevoerd, een extra hersteltaak wordt weergegeven op de **taken** pagina. |Dit kan gebeuren als de hersteltaak alleen lokaal volumes of een combinatie van lokaal vastgemaakte en gelaagde volumes vastgemaakte is. De hersteltaak bevat alleen gelaagde volumes, wordt vervolgens dit gedrag niet uitgevoerd als. Er is geen tussenkomst van de gebruiker is vereist. |Ja |Nee |
| 20 |Lokaal vastgemaakte volumes |Als u probeert een gelaagd volume (gemaakt en gekloonde met Update 1.2 of eerder) converteren naar een lokaal vastgemaakt volume en uw apparaat is bijna geen schijfruimte meer of er een storing van de cloud, kan de clone(s) zijn beschadigd. |Dit probleem treedt alleen op volumes die gemaakt en gekloonde met vooraf Update 2-software zijn. Dit moet een incidentele scenario. | | |
| 21 |Volumeconversie |Werk de ACRs gekoppeld aan een volume, terwijl de volumeconversie van een uitgevoerd wordt niet (gelaagd naar lokaal vastgemaakt of andersom). Bijwerken van de ACRs kan leiden tot beschadiging van gegevens. |Indien nodig, werk van de ACRs voorafgaand aan de volumeconversie en maak geen verdere ACR updates terwijl de conversie uitgevoerd wordt. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller en firmware-updates in de Update 2
Deze release werkt het stuurprogramma en de firmware van de schijf op uw apparaat.

* Voor meer informatie over de LSI firmware bijwerken, Zie Microsoft Knowledge base-artikel 3121900. 
* Voor meer informatie over de schijf-firmware bijwerken, Zie Microsoft Knowledge base-artikel 3121899.

## <a name="virtual-device-updates-in-update-2"></a>Virtueel apparaat-updates in de Update 2
Deze update kan niet worden toegepast op het virtuele apparaat. Nieuwe virtuele apparaten moet worden gemaakt. 

## <a name="next-step"></a>Volgende stap
Meer informatie over hoe [installeren Update 2](storsimple-install-update-2.md) op uw StorSimple-apparaat.

