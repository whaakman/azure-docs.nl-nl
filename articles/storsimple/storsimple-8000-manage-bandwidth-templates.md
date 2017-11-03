---
title: Beheren van sjablonen voor StorSimple 8000 serie bandbreedte | Microsoft Docs
description: Beschrijft hoe voor het beheren van StorSimple bandbreedte sjablonen waarmee u kunt het gebruik van netwerkbandbreedte te beheren.
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 50d0a920bef097013feddc828d2c37133b9057b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>De service Manager voor StorSimple-apparaat gebruiken voor het beheren van sjablonen voor StorSimple-bandbreedte

## <a name="overview"></a>Overzicht

Bandbreedte-sjablonen kunt u gebruik van de netwerkbandbreedte tussen meerdere tijdgegevens-planningen trapsgewijs van de gegevens van het StorSimple-apparaat naar de cloud te configureren.

Met de schema's voor bandbreedteregeling kunt u:

* Aangepaste bandbreedte's afhankelijk van het gebruik van de werkbelasting netwerk opgeven.
* Management centraliseren en schema's op meerdere apparaten in een eenvoudig en naadloos manier opnieuw gebruiken.

> [!NOTE]
> Deze functie is alleen beschikbaar voor fysieke StorSimple-apparaten (modellen 8100 en 8600) en niet voor StorSimple Cloud toestellen (modellen 8010 als de 8020).


## <a name="the-bandwidth-templates-blade"></a>De blade bandbreedte-sjablonen

De **bandbreedte sjablonen** blade de bandbreedte-sjablonen voor uw service in tabelvorm en bevat de volgende informatie:

* **Naam** : een unieke naam toegewezen aan de bandbreedtesjabloon wanneer deze is gemaakt.
* **Planning** – het nummer van schema's die zijn opgenomen in een bandbreedtesjabloon opgegeven.
* **Gebruikt door** : het aantal volumes met behulp van de bandbreedte-sjablonen.

U kunt ook aanvullende informatie voor het configureren van de bandbreedte-sjablonen in vinden:

* [Vragen en antwoorden over de bandbreedte-sjablonen](#questions-and-answers-about-bandwidth-templates)
* [Aanbevolen procedures voor de bandbreedte-sjablonen](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Voeg een bandbreedtesjabloon

Voer de volgende stappen uit om een nieuwe bandbreedtesjabloon te maken.

#### <a name="to-add-a-bandwidth-template"></a>Een bandbreedtesjabloon toevoegen

1. Ga naar uw StorSimple-apparaat Manager-service, klikt u op **bandbreedte sjablonen** en klik vervolgens op **+ bandbreedtesjabloon toevoegen**.

    ![Klik op + bandbreedtesjabloon toevoegen](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. In de **bandbreedtesjabloon toevoegen** blade, moet u de volgende stappen uitvoeren:
   
    1. Geef een unieke naam voor de bandbreedtesjabloon.
    2. Een schema bandbreedte definiëren. Een planning maken:
   
        1. Kies in de vervolgkeuzelijst de **dagen** van de week voor de planning is geconfigureerd. U kunt meerdere dagen selecteren.        
        
        2. Voer een **begintijd** in _uu: mm_ indeling. Dit is bij de planning begint.

        3. Voer een **eindtijd** in _uu: mm_ indeling. Dit is bij de planning wordt gestopt.
      
           > [!NOTE]
           > Overlappende schema's zijn niet toegestaan. Als de begin- en eindtijden in een overlappende planning resulteren zal, ziet u een foutbericht toe.

        4. Geef de **bandbreedte snelheid**. Dit is de bandbreedte in Megabits per seconde (Mbps) die wordt gebruikt door uw StorSimple-apparaat in bewerkingen met betrekking tot de cloud (uploads en downloads). Geef een getal tussen 1 en 1000 voor dit veld.

            ![Bandbreedte-schema definiëren](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Herhaal de bovenstaande stappen voor het definiëren van meerdere planningen voor de sjabloon totdat u klaar bent.

        5. Klik op **toevoegen** om te beginnen met het maken van een bandbreedtesjabloon. De gemaakte sjabloon is toegevoegd aan de lijst met sjablonen van de bandbreedte.
      

## <a name="edit-a-bandwidth-template"></a>Een bandbreedtesjabloon bewerken

De volgende stappen uitvoeren om te bewerken van een bandbreedtesjabloon.

### <a name="to-edit-a-bandwidth-template"></a>Een bandbreedtesjabloon bewerken

1. Ga naar uw StorSimple-Apparaatbeheer service en klik op **bandbreedte sjablonen**.
2. Selecteer in de lijst met sjablonen van bandbreedte, de sjabloon die u wilt verwijderen. Met de rechtermuisknop en selecteer in het contextmenu **verwijderen**.
3. Wanneer u om bevestiging gevraagd, klikt u op **OK**. Dit moet de bandbreedtesjabloon verwijderen. 
4. De lijst met updates van de bandbreedte-sjablonen in overeenstemming met de verwijdering.

> [!NOTE]
> U kunt uw wijzigingen niet opslaan als de bewerkte planning overlapt met een bestaand schema in de bandbreedtesjabloon die u wilt wijzigen.

## <a name="delete-a-bandwidth-template"></a>Een bandbreedtesjabloon verwijderen

De volgende stappen uitvoeren om te verwijderen van een bandbreedtesjabloon.

#### <a name="to-delete-a-bandwidth-template"></a>Om een bandbreedtesjabloon te verwijderen

1. Ga naar uw StorSimple-Apparaatbeheer service en klik op **bandbreedte sjablonen**.
2. Selecteer in de lijst met sjablonen van bandbreedte, de sjabloon die u wilt verwijderen. Klik met de rechtermuisknop en selecteer in het contextmenu verwijderen.
3. Wanneer u om bevestiging gevraagd, klikt u op **OK**. Dit moet de bandbreedtesjabloon verwijderen.
4. De lijst met updates van de bandbreedte-sjablonen in overeenstemming met de verwijdering.

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

**EEN**. Stel dat u een apparaat met 3 volumecontainers hebben. De schema's die zijn gekoppeld aan deze containers volledig overlappen. Voor elk van deze containers zijn de bandbreedte gebruikt 5, 10 en 15 Mbps respectievelijk. Wanneer de i/o plaatsvinden op al deze containers op hetzelfde moment, het minimum van 3-bandbreedtelimieten kan worden toegepast: in dit geval 5 Mbps als deze i/o-aanvragen voor uitgaande delen dezelfde wachtrij.

## <a name="best-practices-for-bandwidth-templates"></a>Aanbevolen procedures voor de bandbreedte-sjablonen

Volg deze aanbevolen procedures voor uw StorSimple-apparaat:

* Bandbreedte-sjablonen configureren op uw apparaat om in te schakelen variabele beperken van de doorvoer van het netwerk door het apparaat op verschillende tijdstippen van de dag. Deze sjablonen bandbreedte gebruikt in combinatie met de back-upschema's kunnen effectief gebruikmaken van aanvullende netwerkbandbreedte voor cloud-bewerkingen tijdens de daluren.
* De werkelijke bandbreedte die vereist zijn voor een bepaalde implementatie op basis van de grootte van de implementatie en de vereiste beoogde hersteltijd (RTO) berekenen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [de service Manager voor StorSimple-apparaat gebruiken voor het beheer van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

