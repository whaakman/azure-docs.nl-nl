---
title: Microsoft Translator Hub werkruimte en projecten migreren? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Migreer uw Hub-werkruimte en projecten naar aangepaste Translator.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.date: 11/13/2018
ms.author: v-rada
ms.topic: article
ms.openlocfilehash: e243262a3eedb961bc86cfd167db540c0dece8c3
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856754"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Hub-werkruimte en projecten migreren naar aangepaste Translator

U kunt eenvoudig migreren uw [Microsoft Translator Hub](https://hub.microsofttranslator.com/) werkruimte en projecten met aangepaste Translator. Migratie wordt gestart vanuit Microsoft Hub door een werkruimte of -project te selecteren en vervolgens een werkruimte selecteren in aangepaste Translator en vervolgens te klikken op de trainingen die u wilt overdragen. Nadat de migratie is gestart, wordt de geselecteerde training-instellingen met alle relevante documenten worden overgedragen. Geïmplementeerde modellen kunnen worden getraind en autodeployed na voltooiing.

Deze acties worden uitgevoerd tijdens de migratie:
* Alle documenten en project definities hebben hun namen met de toevoeging van 'hub_' als voorvoegsel toegevoegd aan de naam van de overgedragen. Automatisch gegenereerde test- en afstemmen van gegevens wordt de naam hub_systemtune_\<modelid > of hub_systemtest_\<modelid >.
* Als u kunt de status geïmplementeerd zijn wanneer de migratie plaatsvindt wordt automatisch wordt bijvoorbeeld getraind met behulp van de documenten van de Hub-training. Deze training wordt niet in rekening gebracht aan uw abonnement. Als automatisch wordt geïmplementeerd is geselecteerd voor de migratie, het getrainde model na voltooiing wordt geïmplementeerd. Normale die als host fungeert voor de kosten worden toegepast.
* Alle gemigreerde trainingen die eerder niet in de status van de geïmplementeerde waren wordt in de gemigreerde conceptfase worden geplaatst. In deze staat, hebt u de mogelijkheid van een model met de definitie van de gemigreerde training, maar regelmatig training wordt in rekening gebracht.
* Op elk gewenst moment de score BLEU gemigreerd van de training u op de pagina TrainingDetails van het model in de 'Bleu score in MT Hub vindt' Hub kop.

>[!Note]
>Aangepaste Translator vereist voor een training te voltooien, minimale 10.000 uitgepakte zinnen. Voor een kleiner aantal opgehaalde zinnen dan de [voorgestelde minimale](sentence-alignment.md#suggested-minimum-number-of-extracted-and-aligned-sentences), aangepaste Translator kan niet een trainen.

## <a name="enable-account-migration"></a>Migratie inschakelen

Als u wilt gebruiken van het hulpprogramma voor migratie, moet u hebt de migratie van uw Hub-account is ingeschakeld. U doet dit door e- [ custommt@microsoft.com ](mailto:custommt@microsoft.com) met een lijst van alle-liveid accounts die u graag ingeschakeld. Deze accounts moet het e-mailadressen die u Meld u aan met zijn.

## <a name="find-custom-translator-workspace-id"></a>Aangepaste Translator werkruimte-ID vinden

Voor het migreren van [Microsoft Translator Hub](https://hub.microsofttranslator.com/) werkruimte, moet u de werkruimte-ID in aangepaste Translator bestemming. De doelwerkruimte in aangepaste Translator is waar uw Hub werkruimten en projecten moeten worden gemigreerd naar.

U vindt de bestemming werkruimte-ID op de pagina aangepaste Translator-instellingen:

1. Ga naar de pagina met 'Instellingen' in de portal voor aangepaste Translator.

2. De werkruimte-ID vindt u in de sectie Algemene informatie.

    ![Over het vinden van bestemming werkruimte-ID](media/how-to/how-to-find-destination-ws-id.png)

3. Houd uw doel werkruimte-ID om te verwijzen tijdens het migratieproces.

## <a name="migrate-a-project"></a>Migreren van een project

Als u migreren van uw projecten selectief wilt, geeft u in Microsoft Translator-Hub die mogelijkheid.

Voor het migreren van een project:

1. Aanmelden bij Microsoft Translator-Hub.

2. Ga naar de pagina 'Projecten'.

3. Klik op de koppeling 'Migreren' voor het juiste project.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-from-hub.png)

4. Bij een Tik op de koppeling migreren u krijgt een formulier zodat u kunt:
   * Geef de werkruimte die u overbrengen wilt naar op aangepaste Translator
   * Aangeven of het om over te dragen van alle trainingen met geslaagde trainingen of alleen de geïmplementeerde trainingen. Standaard worden alle geslaagde trainingen worden overgedragen.
   * Aangeven of u uw training automatisch geïmplementeerd wilt wanneer training is voltooid. Uw training worden standaard niet automatisch geïmplementeerd wanneer de installatie voltooid.

5. Klik op "Aanvraag verzenden".

## <a name="migrate-a-workspace"></a>Migreren van een werkruimte

Naast een enkel project migreert, kunt u ook alle projecten met geslaagde trainingen in een werkruimte migreren. Hierdoor wordt elk project in de werkruimte moet worden geëvalueerd, alsof de koppeling migreren had is gedrukt. Deze functie is geschikt voor gebruikers met veel projecten die willen migreren alle mappen met aangepaste Translator met dezelfde instellingen. De migratie van een werkruimte kan worden gestart vanaf de instellingenpagina van Translator-Hub.

Voor het migreren van een werkruimte:

1. Aanmelden bij Microsoft Translator-Hub.

2. Ga naar de pagina 'Instellingen'.

3. Klik op 'Migreren gegevens in de werkruimte met aangepast Translator' op 'Instellingen'-pagina.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-workspace-from-hub.png)

4. Selecteer een van deze twee opties op de volgende pagina:

    a. Alleen geïmplementeerde trainingen: Als u deze optie selecteert, wordt alleen uw geïmplementeerde systemen en gerelateerde documenten migreren.

    b. U kunt alle geslaagde: Als u deze optie selecteert, worden alle geslaagde trainingen en gerelateerde documenten gemigreerd.

    c. Voer uw bestemming werkruimte-ID in aangepaste Translator.

    ![Over het migreren van de Hub](media/how-to/how-to-migrate-from-hub-screen.png)

5. Klik op aanvraag verzenden.

## <a name="migration-history"></a>Migratiegeschiedenis

Wanneer u hebt aangevraagd werkruimte / migratie van de Hub project, vindt u de migratiegeschiedenis van een op de pagina aangepaste Translator-instellingen.

Als u de migratiegeschiedenis, de volgende stappen uit:

1. Ga naar de pagina met 'Instellingen' in de portal voor aangepaste Translator.

2. Klik in de sectie Geschiedenis van de migratie van de pagina instellingen op Migratiegeschiedenis.

    ![Migratiegeschiedenis](media/how-to/how-to-migration-history.png)

Migratie geschiedenispagina bevat de volgende informatie als overzicht voor elke migratie die u hebt aangevraagd.

1. Gemigreerd door: Naam en e-mailadres van de gebruiker deze migratieaanvraag heeft ingediend

2. Op gemigreerd: Datum en een tijdstempel van de migratie

3. Projecten: Aantal projecten aangevraagd voor migratie v/s aantal projecten is gemigreerd.

4. Trainings: Het aantal trainingen aangevraagd voor migratie v/s aantal trainingen gemigreerd.

5. Documents: Het aantal documenten die zijn aangevraagd voor migratie v/s aantal documenten is gemigreerd.

    ![Details van migratie](media/how-to/how-to-migration-history-details.png)

Als u dat meer gedetailleerde migratierapport over uw projecten, trainingen en documenten wilt, hebt u de optie exporteren als CSV-bestand.

## <a name="implementation-notes"></a>Opmerkingen bij de implementatie
* Migreren van een project van de Hub met aangepaste Translator hebben geen invloed op uw Hub trainingen of projecten. We niet verwijderen-projecten of documenten van Hub tijdens een migratie en we modellen kan niet worden geïmplementeerd.
* Alleen is toegestaan voor het migreren van één keer per project. Als u herhalen van een migratie van een project wilt, neem dan contact met ons.
* Op dit moment de aangepaste Translator ondersteunt 36 talen vertalen van en naar het Engels en we werken er hard aan extra talen toevoegen. Hub vereist geen basislijn modellen en daarom enkele duizenden talen worden ondersteund. Maar we alleen de migratie van documenten en definities van project, kunt u een sleutelpaar met een niet-ondersteunde taal migreren. Er is niet mogelijk naar het nieuwe model te trainen. Bovendien deze documenten en projecten worden weergegeven als inactief om aan te geven dat ze op dit moment niet worden gebruikt. Als er is ondersteuning toegevoegd voor deze projecten en/of de documenten, worden ze actief is en trainable.
* Aangepaste Translator ondersteunt momenteel geen ééntalig trainingsgegevens. Als niet-ondersteunde taal paren, kunt u ééntalig documenten migreren, maar ze worden weergegeven als inactief totdat ééntalig wordt ondersteund.
* Aangepaste Translator vereist 10 k parallelle zinnen om te kunnen trainen. Microsoft Hub kunnen trainen op een kleiner aantal gegevens. Als een training is gemigreerd die niet aan deze vereiste voldoet, wordt deze niet getraind.

## <a name="custom-translator-versus-hub"></a>Aangepaste Translator ten opzichte van de Hub

Deze tabel vergelijkt de functies tussen Microsoft Translator-Hub en aangepaste Translator.

|   | Hub | Custom Translator |
|:-----|:----:|:----:|
|Functiestatus aanpassen   | Algemene beschikbaarheid  | Algemene beschikbaarheid |
| Tekst-API-versie  | V2    | V3  |
| SMT aanpassen | Ja   | Nee |
| NMT aanpassen | Nee    | Ja |
| Nieuwe uniforme spraakherkenning services aanpassen | Nee    | Ja |
| Geen tracering | Ja | Ja |

## <a name="next-steps"></a>Volgende stappen

- [Een model te trainen](how-to-train-model.md).
- Start met behulp van het model van uw geïmplementeerde aangepaste vertaling via [Microsoft Translator Text-API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
