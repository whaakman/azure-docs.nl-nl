---
title: Bandbreedtesjablonen voor StorSimple 8000-serie beheren | Microsoft Docs
description: Beschrijft hoe u StorSimple bandbreedtesjablonen, waarmee u kunt voor het beheren van bandbreedtegebruik beheren.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 13a3e57bb27c075fc045e87790dbe13369ed9f8e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699452"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>De StorSimple Device Manager-service gebruiken voor het StorSimple-bandbreedtesjablonen beheren

## <a name="overview"></a>Overzicht

Bandbreedtesjablonen kunnen u gebruik van netwerkbandbreedte tussen meerdere van de dag planningen om de gegevens van de StorSimple-apparaat naar de cloud in lagen configureren.

Met schema's voor bandbreedteregeling kunt u het volgende doen:

* Geef aangepaste bandbreedte schema's, afhankelijk van het gebruik van de werkbelasting netwerk.
* Centraliseer het beheer en de planning op meerdere apparaten in een eenvoudig en naadloze wijze opnieuw gebruiken.

> [!NOTE]
> Deze functie is alleen beschikbaar voor fysieke StorSimple-apparaten (modellen 8100- en 8600) en niet voor StorSimple Cloud Appliances (modellen 8010 en 8020).


## <a name="the-bandwidth-templates-blade"></a>De blade sjabloon voor bandbreedte

De **bandbreedtesjablonen** blade de bandbreedtesjablonen voor uw service in tabelvorm en bevat de volgende informatie:

* **Naam** : een unieke naam die aan de bandbreedtesjabloon wordt toegewezen wanneer deze is gemaakt.
* **Planning** : het aantal planningen die deel uitmaken van een bepaalde bandbreedte-sjabloon.
* **Gebruikt door** : het aantal volumes met behulp van de bandbreedtesjablonen.

U vindt hier ook aanvullende informatie over het configureren van bandbreedtesjablonen in:

* [Vragen en antwoorden over bandbreedtesjablonen](#questions-and-answers-about-bandwidth-templates)
* [Aanbevolen procedures voor bandbreedtesjablonen](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Een bandbreedtesjabloon toevoegen

Voer de volgende stappen uit voor het maken van een nieuwe bandbreedtesjabloon.

#### <a name="to-add-a-bandwidth-template"></a>Een bandbreedtesjabloon toevoegen

1. Ga naar uw StorSimple Device Manager-service, klikt u op **bandbreedtesjablonen** en klik vervolgens op **+ bandbreedtesjabloon toevoegen**.

    ![Klik op + bandbreedtesjabloon toevoegen](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. In de **bandbreedtesjabloon toevoegen** blade de volgende stappen uit:
   
    1. Geef een unieke naam voor de bandbreedtesjabloon.
    2. Een schema bandbreedte definiëren. Een planning maken:
   
        1. Kies in de vervolgkeuzelijst de **dagen** van de week voor de planning is geconfigureerd. U kunt meerdere dagen selecteren.        
        
        2. Voer een **begintijd** in _uu: mm_ indeling. Dit is bij de planning wordt gestart.

        3. Voer een **eindtijd** in _uu: mm_ indeling. Dit is bij de planning wordt beëindigd.
      
           > [!NOTE]
           > Overlappende schema's zijn niet toegestaan. Als de begin- en eindtijden tot een overlappende schema leiden zal, ziet u een foutbericht weergegeven met deze informatie.

        4. Geef de **Bandbreedtesnelheid**. Dit is de bandbreedte in Megabits per seconde (Mbps) die worden gebruikt door uw StorSimple-apparaat in bewerkingen met betrekking tot de cloud (uploaden en downloaden). Geef voor dit veld een waarde op tussen 1 en 1000.

            ![Bandbreedte-schema definiëren](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Herhaal de bovenstaande stappen voor het definiëren van meerdere planningen voor de sjabloon totdat u klaar bent.

        5. Klik op **toevoegen** om te beginnen met het maken van een bandbreedtesjabloon. De gemaakte sjabloon wordt toegevoegd aan de lijst met bandbreedtesjablonen.
      

## <a name="edit-a-bandwidth-template"></a>Een van de bandbreedtesjabloon bewerken

De volgende stappen uitvoeren om een bandbreedtesjabloon bewerken.

### <a name="to-edit-a-bandwidth-template"></a>Een van de bandbreedtesjabloon bewerken

1. Ga naar uw StorSimple Device Manager-service en klik op **bandbreedtesjablonen**.
2. Selecteer in de lijst met bandbreedtesjablonen, de sjabloon die u wilt verwijderen. Met de rechtermuisknop en selecteer in het contextmenu **verwijderen**.
3. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **OK**. Dit moet de bandbreedtesjabloon verwijderen. 
4. De lijst met updates van de bandbreedte-sjablonen in overeenstemming met het verwijderen.

> [!NOTE]
> U kunt uw wijzigingen niet opslaan als de bewerkte planning overlapt met een bestaand schema in de bandbreedtesjabloon die u wilt wijzigen.

## <a name="delete-a-bandwidth-template"></a>Een bandbreedtesjabloon verwijderen

De volgende stappen uitvoeren om een bandbreedtesjabloon verwijderen.

#### <a name="to-delete-a-bandwidth-template"></a>Een van de bandbreedtesjabloon verwijderen

1. Ga naar uw StorSimple Device Manager-service en klik op **bandbreedtesjablonen**.
2. Selecteer in de lijst met bandbreedtesjablonen, de sjabloon die u wilt verwijderen. Met de rechtermuisknop en selecteer verwijderen in het contextmenu.
3. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **OK**. Dit moet de bandbreedtesjabloon verwijderen.
4. De lijst met updates van de bandbreedte-sjablonen in overeenstemming met het verwijderen.

Als de sjabloon gebruikt door alle volumes wordt, wordt u niet toegestaan om deze te verwijderen. U ziet een foutbericht met de mededeling dat de sjabloon gebruikt wordt. Het dialoogvenster met een foutbericht wordt weergegeven waarin wordt gemeld dat alle verwijzingen naar de sjabloon moeten worden verwijderd.

U kunt alle verwijzingen naar de sjabloon verwijderen door het openen van de **Volumecontainers** pagina en wijzigen van de volumecontainers die gebruikmaken van deze sjabloon, zodat ze een andere sjabloon gebruiken of een aangepaste of onbeperkte bandbreedte-instelling gebruiken. Wanneer alle verwijzingen naar zijn verwijderd, kunt u de sjabloon verwijderen.

## <a name="use-a-default-bandwidth-template"></a>Een standaardsjabloon voor bandbreedte

Een standaardsjabloon voor bandbreedte wordt geleverd en wordt gebruikt door volumecontainers standaard bandbreedte besturingselementen afdwingen bij het openen van de cloud. De standaardsjabloon fungeert ook als uitgangspunt klaar voor gebruikers die hun eigen sjablonen maken. De details van deze sjabloon zijn:

* **Naam** : onbeperkt aantal nachten en weekenden
* **Planning** – één schema van maandag tot en met vrijdag die van toepassing een snelheid van 1 Mbps tussen 8: 00 uur en 17: 00 uur tijd op het apparaat is. De bandbreedte is voor de rest van de week ingesteld op onbeperkt.

De standaardsjabloon kan worden bewerkt. Het gebruik van deze sjabloon (inclusief bewerkte versies) wordt bijgehouden.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Maken van een hele dag duurt bandbreedtesjabloon die bij een opgegeven periode begint

Volg deze procedure voor het maken van een schema dat begint met een opgegeven periode en alle dag wordt uitgevoerd. In het voorbeeld wordt de planning begint om 9 uur's ochtends en wordt tot en met 9: 00 de volgende ochtend wordt uitgevoerd. Het is belangrijk te weten dat de begin- en eindtijden voor een opgegeven schema moet beide op hetzelfde schema voor 24 uur per dag worden opgenomen en kunnen niet meerdere dagen omvatten. Als u nodig hebt voor het instellen van de bandbreedtesjablonen die over meerdere dagen, moet u meerdere planningen gebruiken (zoals weergegeven in het voorbeeld).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Het maken van een hele dag duurt bandbreedtesjabloon

1. Maak een planning die wordt gestart om 9 uur's ochtends en wordt tot middernacht wordt uitgevoerd.
2. Een ander schema toevoegen. Configureer de tweede planning om uit te voeren van middernacht tot en met 9 uur's ochtends.
3. De bandbreedtesjabloon opslaan.

De samengestelde planning wordt vervolgens gestart op een tijdstip van uw keuze en hele dag worden uitgevoerd.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Vragen en antwoorden over bandbreedtesjablonen

**Q**. Wat gebeurt er met bandbreedteregelingen wanneer u tussen de schema's? (Een schema is beëindigd en een andere naam nog niet is gestart.)

**A**. In dergelijke gevallen wordt er geen bandbreedteregelingen worden gebruikt. Dit betekent dat het apparaat onbeperkte bandbreedte gebruiken kunt wanneer meerdere lagen van gegevens naar de cloud.

**Q**. Kunt u bandbreedtesjablonen op een offline-apparaat wijzigen?

**A**. Niet mogelijk om te wijzigen van bandbreedtesjablonen op volumes containers als de bijbehorende apparaat offline is.

**Q**. Kunt u een van de bandbreedtesjabloon die zijn gekoppeld aan een volumecontainer wanneer de gekoppelde volumes offline zijn bewerken?

**A**. Een van de bandbreedtesjabloon die zijn gekoppeld aan een volumecontainer waarvan volumes offline zijn, kunt u wijzigen. Houd er rekening mee dat wanneer de volumes offline zijn, geen zal worden gelaagde gegevens van het apparaat naar de cloud.

**Q**. Kunt u een standaardsjabloon verwijderen?

**A**. Hoewel u een standaardsjabloon verwijderen kunt, is het niet een goed idee om dit te doen. Het gebruik van een standaardsjabloon, met inbegrip van bewerkte versies wordt bijgehouden. De traceringsgegevens worden geanalyseerd en in de loop van de tijd, wordt gebruikt voor het verbeteren van de standaardsjabloon.

**Q**. Hoe bepaal u dat uw bandbreedtesjablonen moeten worden gewijzigd?

**A**. Een van de tekens die u wilt wijzigen van de bandbreedtesjablonen is wanneer u weer te geven van het netwerk vertragen of onderdrukking meerdere keren in een dag. Als dit gebeurt, bewaken van het netwerk opslag en het gebruik door te kijken naar de i/o-prestaties en de netwerkdoorvoer grafieken.

Identificeren van de netwerkgegevens doorvoer, de tijd van de dag en de volumecontainers waarin het knelpunt netwerk zich voordoet. Als dit gebeurt wanneer de gegevens naar de cloud is dat wordt gelaagd (deze informatie ophalen van i/o-prestaties voor alle volumecontainers van apparaat naar de cloud), moet u de bandbreedtesjablonen die zijn gekoppeld aan uw volumecontainers wijzigen.

Nadat de gewijzigde sjablonen gebruikt worden, moet u voor het bewaken van het netwerk opnieuw voor aanzienlijke latentie. Als deze nog steeds aanwezig zijn, moet u terugkeren naar uw bandbreedtesjablonen.

**Q**. Wat gebeurt er als meerdere volumecontainers op mijn apparaat plant die elkaar overlappen, maar andere limieten op elk(e) toepassen?

**A**. Stel dat u een apparaat met 3 volumecontainers hebt. De schema's die volledig zijn gekoppeld aan deze containers overlappen. Voor elk van deze containers zijn de bandbreedtelimieten gebruikt 5, 10, en 15 Mbps respectievelijk. Wanneer de i/o plaatsvinden op al deze containers op hetzelfde moment, het minimum van de 3 bandbreedtelimieten kan worden toegepast: in dit geval de dezelfde wachtrij voor het delen van 5 Mbps als uitgaande i/o-aanvragen.

## <a name="best-practices-for-bandwidth-templates"></a>Aanbevolen procedures voor bandbreedtesjablonen

Volg deze aanbevolen procedures voor uw StorSimple-apparaat:

* Bandbreedtesjablonen configureren op uw apparaat om in te schakelen variabele bandbreedtebeperking van de netwerkdoorvoer door het apparaat op verschillende tijdstippen van de dag. Deze bandbreedtesjablonen gebruikt in combinatie met de back-upschema's kunnen effectief gebruikmaken van extra bandbreedte voor cloudbewerkingen tijdens de daluren.
* Bereken de werkelijke bandbreedte die is vereist voor een bepaalde implementatie op basis van de grootte van de implementatie en de vereiste beoogde hersteltijd (RTO).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [met de StorSimple Device Manager-service voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

