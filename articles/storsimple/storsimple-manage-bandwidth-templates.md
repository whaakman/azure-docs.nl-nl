---
title: Beheren van uw StorSimple-bandbreedte-sjablonen | Microsoft Docs
description: Beschrijft hoe voor het beheren van StorSimple bandbreedte sjablonen waarmee u kunt het gebruik van netwerkbandbreedte te beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: df3ae8bf775370432b3648459a7c942afe69fb17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>De StorSimple Manager-service gebruiken om sjablonen voor StorSimple-bandbreedte te beheren
## <a name="overview"></a>Overzicht
Bandbreedte-sjablonen kunt u gebruik van de netwerkbandbreedte tussen meerdere tijdgegevens-planningen trapsgewijs van de gegevens van het StorSimple-apparaat naar de cloud te configureren.

Met de schema's voor bandbreedteregeling kunt u:

* Aangepaste bandbreedte's afhankelijk van het gebruik van de werkbelasting netwerk opgeven.
* Management centraliseren en schema's op meerdere apparaten in een eenvoudig en naadloos manier opnieuw gebruiken.

> [!NOTE]
> Deze functie is alleen beschikbaar voor fysieke StorSimple-apparaten en niet voor virtuele apparaten.
> 
> 

De bandbreedte-sjablonen voor uw service in tabelvorm worden weergegeven en de volgende informatie bevatten:

* **Naam** : een unieke naam toegewezen aan de bandbreedtesjabloon wanneer deze is gemaakt.
* **Planning** – het nummer van schema's die zijn opgenomen in een bandbreedtesjabloon opgegeven.
* **Gebruikt door** : het aantal volumes met behulp van de bandbreedte-sjablonen.

Gebruik van de StorSimple Manager-service **configureren** pagina in de klassieke Azure portal om sjablonen van de bandbreedte te beheren.

U kunt ook aanvullende informatie voor het configureren van de bandbreedte-sjablonen in vinden:

* Vragen en antwoorden over de bandbreedte-sjablonen
* Aanbevolen procedures voor de bandbreedte-sjablonen

## <a name="add-a-bandwidth-template"></a>Voeg een bandbreedtesjabloon
Voer de volgende stappen uit om een nieuwe bandbreedtesjabloon te maken.

#### <a name="to-add-a-bandwidth-template"></a>Een bandbreedtesjabloon toevoegen
1. Op de StorSimple Manager-service **configureren** pagina, klikt u op **bandbreedtesjabloon toevoegen/bewerken**.
2. In de **Bandbreedtesjabloon toevoegen/bewerken** in het dialoogvenster:
   
   1. Van de **sjabloon** vervolgkeuzelijst, selecteer **nieuw** toevoegen van een nieuwe bandbreedtesjabloon.
   2. Geef een unieke naam voor de bandbreedtesjabloon.
3. Definieer een **bandbreedte planning**. Een planning maken:
   
   1. Kies in de vervolgkeuzelijst de dagen van de week die voor de planning is geconfigureerd. U kunt meerdere dagen selecteren door de selectievakjes die zich vóór de respectieve dagen in de lijst.
   2. Selecteer de **alle dag** optie als u de planning voor de hele dag wordt afgedwongen. Wanneer deze optie is ingeschakeld, kunt u niet langer opgeven een **begintijd** of een **eindtijd**. De planning wordt uitgevoerd vanaf 00:00 uur tot 11:59 PM liggen.
   3. Selecteer in de vervolgkeuzelijst een **begintijd**. Dit is bij de planning begint.
   4. Selecteer in de vervolgkeuzelijst een **eindtijd**. Dit is bij de planning wordt gestopt.
      
      > [!NOTE]
      > Overlappende schema's zijn niet toegestaan. Als de begin- en eindtijden in een overlappende planning resulteren zal, ziet u een foutbericht toe.
      > 
      > 
   5. Geef de **bandbreedte snelheid**. Dit is de bandbreedte in Megabits per seconde (Mbps) die wordt gebruikt door uw StorSimple-apparaat in bewerkingen met betrekking tot de cloud (uploads en downloads). Geef een getal tussen 1 en 1000 voor dit veld.
   6. Klik op het vinkje ![vinkje](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). De sjabloon die u hebt gemaakt wordt toegevoegd aan de lijst met sjablonen van de bandbreedte op de service **configureren** pagina.
      
      ![Nieuwe bandbreedtesjabloon maken](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Klik op **opslaan** aan de onderkant van de pagina en klik vervolgens op **Ja** wanneer u om bevestiging wordt gevraagd. Hiermee wordt de configuratiewijzigingen die u hebt aangebracht opslaan.

## <a name="edit-a-bandwidth-template"></a>Een bandbreedtesjabloon bewerken
De volgende stappen uitvoeren om te bewerken van een bandbreedtesjabloon.

### <a name="to-edit-a-bandwidth-template"></a>Een bandbreedtesjabloon bewerken
1. Klik op **bandbreedtesjabloon toevoegen/bewerken**.
2. In de **Bandbreedtesjabloon toevoegen/bewerken** in het dialoogvenster:
   
   1. Van de **sjabloon** vervolgkeuzelijst kiest u een bestaande bandbreedtesjabloon voor de die u wilt wijzigen.
   2. Voer de wijzigingen. (U kunt wijzigen van de bestaande instellingen.)
   3. Klik op het vinkje ![Vinkje](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). U ziet de gewijzigde sjabloon in de lijst met sjablonen van de bandbreedte op de pagina service configureren.
3. Sla uw wijzigingen, klikt u op **opslaan** aan de onderkant van de pagina. Klik op **Ja** wanneer u om bevestiging wordt gevraagd.

> [!NOTE]
> U wordt niet toegestaan de wijzigingen wilt opslaan als de bewerkte planning overlapt met een bestaand schema in de bandbreedtesjabloon die u wilt wijzigen.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Een bandbreedtesjabloon verwijderen
De volgende stappen uitvoeren om te verwijderen van een bandbreedtesjabloon.

#### <a name="to-delete-a-bandwidth-template"></a>Om een bandbreedtesjabloon te verwijderen
1. Selecteer de sjabloon die u wilt verwijderen in de lijst in tabelvorm van de bandbreedte-sjablonen voor uw service. Een verwijderpictogram (**x**) uiterst rechts van de geselecteerde sjabloon wordt weergegeven. Klik op de **x** pictogram om de sjabloon te verwijderen.
2. U wordt gevraagd om bevestiging. Klik op **OK** om door te gaan.

Als de sjabloon door een of meer volumes gebruikt is, wordt u niet toegestaan om deze te verwijderen. U ziet een foutbericht met de mededeling dat de sjabloon die gebruikt wordt. Het dialoogvenster met een foutbericht wordt weergegeven waarin wordt gemeld dat alle verwijzingen naar de sjabloon moeten worden verwijderd.

U kunt alle verwijzingen naar de sjabloon verwijderen door het openen van de **Volumecontainers** pagina en wijzigen van de volumecontainers die gebruikmaken van deze sjabloon, zodat ze een andere sjabloon gebruiken of gebruik een aangepaste of onbeperkte bandbreedte-instelling. Als u alle verwijzingen zijn verwijderd, kunt u de sjabloon verwijderen.

## <a name="use-a-default-bandwidth-template"></a>Een standaardsjabloon voor bandbreedte
Een standaardsjabloon voor de bandbreedte wordt geleverd en wordt gebruikt door volumecontainers standaard bandbreedte besturingselementen afdwingen bij het openen van de cloud. De standaardsjabloon fungeert ook als een verwijzing gereed is voor gebruikers die hun eigen sjablonen maken. De details van deze standaardsjabloon zijn:

* **Naam** : onbeperkte nachten en weekends
* **Planning** – één schema van maandag tot vrijdag van die van toepassing een hoeveelheid bandbreedte van 1 Mbps tussen de 8 uur en 17: 00 uur tijd op het apparaat is. De bandbreedte is voor de rest van de week ingesteld op onbeperkt.

De standaardsjabloon kan worden bewerkt. Het gebruik van deze sjabloon (inclusief bewerkte versies) wordt bijgehouden.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Een hele bandbreedtesjabloon maken die bij een opgegeven periode begint
Volg deze procedure voor het maken van een planning die begint bij een opgegeven periode en alle dag wordt uitgevoerd. In het voorbeeld wordt de planning begint bij 9: 00 uur in de ochtend en wordt uitgevoerd tot en met 9: 00 uur van de volgende ochtend. Het is belangrijk te weten dat de begin- en eindtijden voor een opgegeven schema moet beide op dezelfde 24-uurs planning zijn opgenomen en kan geen gegevens voor meerdere dagen. Als u bandbreedte sjablonen meerdere dagen instellen wilt, moet u meerdere planningen gebruiken (zoals weergegeven in het voorbeeld).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Een bandbreedtesjabloon voor de hele dag maken
1. Maak een planning die begint bij 9: 00 uur in de ochtend en tot middernacht uitgevoerd.
2. Voeg een ander schema toe. Configureer de tweede planning vanaf middernacht uitgevoerd tot en met 9: 00 uur in de ochtend.
3. Sla de bandbreedtesjabloon.

De samengestelde planning wordt vervolgens starten op een tijdstip van uw keuze en hele dag worden uitgevoerd.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Vragen en antwoorden over de bandbreedte-sjablonen
**Q**. Wat gebeurt er met bandbreedte besturingselementen wanneer u tussen de schema's bent? (Een schema is beëindigd en een andere is nog niet gestart.)

**EEN**. In dergelijke gevallen wordt geen bandbreedte besturingselementen worden gebruikt. Dit betekent dat het apparaat onbeperkte bandbreedte gebruiken kunt wanneer gegevens in de cloud tiering.

**Q**. Kunt u bandbreedte sjablonen op een apparaat offline wijzigen?

**EEN**. Niet mogelijk om te wijzigen van bandbreedte-sjablonen op volumes containers als het bijbehorende apparaat offline is.

**Q**. Kunt u een bandbreedtesjabloon voor die is gekoppeld aan een volumecontainer wanneer de gekoppelde volumes offline zijn bewerken?

**EEN**. U kunt een bandbreedtesjabloon voor die is gekoppeld aan een volumecontainer waarvan volumes offline zijn wijzigen. Houd er rekening mee dat wanneer volumes offline zijn, geen gegevens worden in tiers worden verdeeld van het apparaat naar de cloud.

**Q**. Kunt u een standaardsjabloon verwijderen?

**EEN**. Hoewel u een standaardsjabloon verwijderen kunt, is het niet een goed idee om dit te doen. Het gebruik van een standaardsjabloon, inclusief bewerkte versies wordt bijgehouden. De traceringsgegevens is geanalyseerd en in de loop van de tijd die wordt gebruikt voor het verbeteren van de standaardsjabloon.

**Q**. Hoe bepaal u dat uw bandbreedte sjablonen moeten worden gewijzigd?

**EEN**. Een van de tekens die u wilt wijzigen van de bandbreedte-sjablonen is bij het starten van het netwerk vertragen of onderdrukking meerdere keren in een dag te zien. Als dit gebeurt, kunt u het opslag- en gebruiksgegevens netwerk bewaken kijken naar de i/o-prestaties en netwerkdoorvoer grafieken.

Identificeren van de gegevens van het netwerk doorvoer, de tijd van de dag en de volumecontainers waarin het knelpunt netwerk zich voordoet. Als dit gebeurt wanneer de gegevens naar de cloud is wordt gelaagd (deze informatie vinden in de i/o-prestaties voor alle volumecontainers voor apparaat in de cloud), moet u de bandbreedte-sjablonen die zijn gekoppeld aan uw volumecontainers wijzigen.

Nadat de gewijzigde sjablonen gebruikt worden, moet u het netwerk voor belangrijke latenties nogmaals controleren. Als deze nog steeds bestaan, moet u terugkeren naar uw sjablonen bandbreedte.

**Q**. Wat gebeurt er als meerdere volumecontainers op mijn apparaat plant die elkaar overlappen, maar andere beperkingen gelden voor elk?

**EEN**. Stel dat u een apparaat met 3 volumecontainers hebben. De schema's die zijn gekoppeld aan deze containers volledig overlappen. Voor elk van deze containers zijn de bandbreedte gebruikt 5, 10 en 15 Mbps respectievelijk. Wanneer de i/o's plaatsvinden op al deze containers op hetzelfde moment, het minimum van 3-bandbreedtelimieten kan worden toegepast: in dit geval 5 Mbps als deze i/o-aanvragen voor uitgaande delen dezelfde wachtrij.

## <a name="best-practices-for-bandwidth-templates"></a>Aanbevolen procedures voor de bandbreedte-sjablonen
Volg deze aanbevolen procedures voor uw StorSimple-apparaat:

* Bandbreedte-sjablonen configureren op uw apparaat om in te schakelen variabele beperken van de doorvoer van het netwerk door het apparaat op verschillende tijdstippen van de dag. Deze sjablonen bandbreedte gebruikt in combinatie met de back-upschema's kunnen effectief gebruikmaken van aanvullende netwerkbandbreedte voor cloud-bewerkingen tijdens de daluren.
* De werkelijke bandbreedte die vereist zijn voor een bepaalde implementatie op basis van de grootte van de implementatie en de vereiste beoogde hersteltijd (RTO) berekenen.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [de StorSimple Manager-service gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

