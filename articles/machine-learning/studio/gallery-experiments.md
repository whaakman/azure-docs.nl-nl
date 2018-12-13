---
title: Azure AI Gallery-experimenten - Azure Machine Learning Studio | Microsoft Docs
description: Detecteren en experimenten in Azure AI Gallery delen. Een experiment is een canvas in Machine Learning Studio die u gebruiken kunt om samen te stellen van een predictive Analytics-model
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: f4248922-c961-4d3a-9e1b-aec743210166
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.openlocfilehash: 12fb45b00848e5e34760ccb0691998c1f72237b8
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269281"
---
# <a name="discover-azure-machine-learning-studio-experiments-in-azure-ai-gallery"></a>Azure Machine Learning Studio-experimenten in Azure AI Gallery detecteren

[Azure AI Gallery](http://gallery.cortanaintelligence.com) heeft een aantal [experimenten](https://gallery.cortanaintelligence.com/experiments) die zijn ontwikkeld [Azure Machine Learning Studio](https://studio.azureml.net). Experimenten variëren van snelle proof of concept experimenten die een specifieke machine learning-technieken, naar volledig ontwikkelde oplossingen voor problemen met complexe machine learning laten zien.

> [!NOTE]
> Een ***experimenteren*** is van een canvas in Machine Learning Studio die u gebruiken kunt om samen te stellen van een predictive Analytics-model. U kunt het model maken door verbinding te maken van gegevens met verschillende analytische modules. U kunt verschillende ideeën probeert, proefuitvoeringen doen en uiteindelijk uw model als een webservice in Azure implementeren. Zie voor een voorbeeld van het maken van een eenvoudige experiment [Machine learning-zelfstudie: Uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md). Zie voor een uitgebreider overzicht van het maken van een predictive analytics-oplossing, [scenario: Predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).
>
>

## <a name="discover"></a>Ontdekken
Om te bladeren door experimenten [in de galerie](http://gallery.cortanaintelligence.com), aan de bovenkant van de startpagina van de galerie, selecteer **experimenten**.

De **[experimenten](https://gallery.cortanaintelligence.com/experiments)** pagina geeft een lijst met onlangs toegevoegde en populaire experimenten. Als u wilt zien van alle experimenten, selecteer de **alle** knop. Als u wilt zoeken naar een specifieke experiment, selecteer **alle**, en selecteer vervolgens filtercriteria. Ook kunt u zoektermen op in de **zoeken** vak aan de bovenkant van de pagina van de galerie.

Hier krijgt u meer informatie over een experiment op de pagina met details van het experiment. Als de pagina met details van een experiment, schakelt u het experiment. Op een experiment details pagina, in de **opmerkingen** sectie, u kunt opmerkingen, feedback geven of vragen over het experiment. U kunt ook het experiment delen met vrienden en collega's op Twitter of LinkedIn. U kunt ook een koppeling naar de detailpagina experiment aan andere gebruikers uitnodigen voor het weergeven van de pagina e-mail.

![Dit item delen met vrienden](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Uw eigen opmerkingen toevoegen](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="download"></a>Downloaden
U kunt een kopie van een experiment in de galerie downloaden naar uw Machine Learning Studio-werkruimte. Vervolgens kunt u uw exemplaar voor het maken van uw eigen oplossingen.

Azure AI Gallery biedt twee methoden voor het importeren van een kopie van een experiment:

* **In de galerie**. Als u een experiment dat u in de galerie wilt vinden, kunt u een kopie downloaden en opent u het in uw Machine Learning Studio-werkruimte.
* **Vanuit Machine Learning Studio**. In Machine Learning Studio, kunt u een experiment in de galerie als een sjabloon om een nieuw experiment te maken.

### <a name="from-the-gallery"></a>In de galerie

1. Open in de galerie, de detailpagina van het experiment.
2. Selecteer **Open in Studio**.

    ![Open experiment uit de galerie](./media/gallery-experiments/open-experiment-from-gallery.png)

Wanneer u selecteert **Open in Studio**, het experiment wordt geopend in uw Machine Learning Studio-werkruimte. (Als u nog niet bent aangemeld bij Machine Learning Studio, u wordt gevraagd naar de eerst aanmeldt met uw Microsoft-account.)

### <a name="from-within-machine-learning-studio"></a>Vanuit Machine Learning Studio

1. Selecteer in Machine Learning Studio, **nieuw**.
2. Selecteer **Experiment**. U kunt kiezen uit een lijst van de galerie experimenten of een specifieke experiment zoeken met behulp van de **zoeken** vak.
3. Wijs met de muis op een experiment en selecteer vervolgens **Open in Studio**. (Als u informatie over het experiment, selecteer **weergave in de galerie**. Hiermee gaat u naar de pagina met details van experiment in de galerie.)

    ![Open galerie experimenteren uit in Machine Learning Studio](./media/gallery-experiments/open-experiment-from-studio.png)

U kunt aanpassen, herhalen en een gedownloade experiment implementeren zoals elke andere experiment dat u in Machine Learning Studio maakt.

![Experiment in Studio geopend](./media/gallery-experiments/experiment-open-in-studio.png)

## <a name="contribute"></a>Bijdragen
Wanneer u zich bij de galerie aanmelden, worden u lid zijn van de galerie-community. Als een lid van de community, kunt u uw eigen experimenten bijdragen zodat andere gebruikers kunnen profiteren van de oplossingen die u hebt ontdekt.

### <a name="publish-your-experiment-to-the-gallery"></a>Uw experiment publiceren naar de galerie

1. Meld u met behulp van uw Microsoft-account voor Machine Learning Studio.
2. Uw experiment maken en vervolgens uit te voeren.
3. Wanneer u bent klaar om te publiceren van uw experiment in de galerie, in de lijst met acties onder het experimentcanvas selecteert **publiceren naar de galerie**.

    ![Selecteer 'Publiceren naar de galerie'](./media/gallery-experiments/publish-experiment-to-gallery.png)
4. Op de **Experiment beschrijving** pagina, voer een titel en tags. Controleer de titel en tags beschrijvende. Markeer de technieken die u hebt gebruikt of het werkelijke probleem oplossen. Een voorbeeld van een experiment beschrijvende titel is ' binaire classificatie: Twitter-Sentimentanalyse."

    ![Voer de titel en labels voor publicatie](./media/gallery-experiments/experiment-description.png)
5. In de **samenvatting** voert u een overzicht van uw experiment. Een korte beschrijving van het probleem dat het experiment is opgelost en hoe u nadert.
6. In de **gedetailleerde beschrijving** vak, worden de stappen beschreven die u hebt genomen in elk onderdeel van uw experiment. Er zijn enkele nuttige onderwerpen om op te nemen:
   * Schermafbeelding van de grafiek experiment
   * Uitleg bij en gegevensbronnen
   * Gegevensverwerking
   * Functie-engineering
   * Modelbeschrijving
   * Resultaten en evaluatie van de modelprestaties van

   U kunt markdown gebruiken om de opmaak van uw beschrijving. Als u wilt zien hoe uw gegevens op de pagina van de beschrijving experiment eruit ziet wanneer het experiment wordt gepubliceerd, selecteert u **Preview**.

   ![Selecteer 'Preview' om te zien hoe uw tekst eruit](./media/gallery-experiments/preview-markdown-text.png)

   > [!TIP]
   > De tekstvakken voor markdown bewerken en voorbeeld van klein zijn. U wordt aangeraden dat u de documentatie van uw experiment schrijven in een markdown-editor, kopieert u het en plakt u de voltooide documentatie in het tekstvak in de galerie. Nadat u uw experiment hebt gepubliceerd, kunt u de wijzigingen met behulp van de standaard web-hulpprogramma's die markdown gebruiken voor het bewerken en voorbeelden.

7. Op de **selectie van een installatiekopie** pagina, kies een miniatuurafbeelding voor uw experiment. De miniatuur wordt weergegeven aan de bovenkant van de pagina met details van het experiment en in de tegel experiment. Andere gebruikers zien de miniatuurafbeelding wanneer ze door de galerie bladeren. U kunt een installatiekopie uploaden vanaf uw computer of een slot installatiekopie in de galerie selecteren.
    </br>
    ![Uploaden of een afbeelding voor de galerie selecteren](./media/gallery-experiments/select-gallery-image.png)
8. Op de **instellingen** pagina onder **zichtbaarheid**, kies of u uw inhoud openbaar publiceren (**openbare**) of dat deze alleen toegankelijk voor mensen met een koppeling naar de pagina ( **Niet-vermelde**).

    ![Kies of u wilt publiceren, openbaar of als niet-vermelde](./media/gallery-experiments/choose-public-or-unlisted.png)

    <!-- -->

   > [!TIP]
   > Als u controleren of uw documentatie is gelukt wilt voordat u deze openbaar release, kunt u eerst het experiment als publiceren **niet-weergegeven**. Later, kunt u de van zichtbaarheidsinstelling wijzigen **openbare** op de detailpagina van het experiment.
   >
   >
9. Voor het publiceren van het experiment naar de galerie, selecteer de **OK** selectievakje is ingeschakeld.

    ![Selecteer het vinkje OK voor het publiceren van het experiment](./media/gallery-experiments/ok-checkmark.png)

Raadpleeg voor tips over het publiceren van een experiment van hoge kwaliteit galerie [Tips voor het documenteren en publiceren van uw experiment](#tips-for-documenting-and-publishing-your-experiment).

Dat is alles--u bent klaar.

U kunt nu uw experiment weergeven in de galerie en de koppeling delen met anderen. Als u uw experiment gepubliceerd met behulp van de **openbare** zichtbaarheid instelt, het experiment wordt weergegeven in de resultaten voor bladeren en zoeken in de galerie. U kunt uw experiment-documentatie op de pagina met details van het experiment telkens wanneer die u bent aangemeld bij de Galerie bewerken.

De lijst van uw bijdragen wilt bekijken, selecteert u uw installatiekopie in de rechterbovenhoek van een willekeurige galeriepagina. Selecteer vervolgens de naam van uw om uw accountpagina te openen.

![Selecteer de accountnaam van uw](./media/gallery-experiments/click-account-name.png)

### <a name="update-your-experiment"></a>Bijwerken van uw experiment
Als u nodig hebt, kunt u wijzigingen aanbrengen aan de werkstroom in een experiment die u hebt gepubliceerd naar de galerie (modules, parameters, enzovoort). Breng wijzigingen wilt aanbrengen in het experiment en vervolgens opnieuw publiceren in Machine Learning Studio. Uw gepubliceerde experiment wordt met uw wijzigingen bijgewerkt.

U kunt een van de volgende informatie voor uw experiment rechtstreeks in de galerie wijzigen:

* Naam van experiment
* Samenvatting of beschrijving
* Tags
* Installatiekopie
* Zichtbaarheidsinstelling (**openbare** of **niet-weergegeven**)

U kunt ook het experiment verwijderen uit de galerie.

U kunt deze wijzigingen aanbrengt, of het experiment verwijderd uit de pagina met details van het experiment of op uw profielpagina in de galerie.


#### <a name="from-the-experiment-details-page"></a>Vanaf de pagina met details van experiment
Op de pagina met details van experiment, om de details voor uw experiment wijzigen, selecteer **bewerken**.

![Selecteer Bewerken om te bewerken van uw experiment](./media/gallery-experiments/edit-button.png)

De pagina met details krijgt de bewerkingsmodus. Als u wilt wijzigen, selecteert u **bewerken** naast de naam van experiment, samenvatting of tags. Wanneer u klaar bent bent, selecteert u **gedaan**.

![Selecteer 'Bewerken' als gegevens wilt bewerken, en selecteer 'Voltooid' wanneer u klaar bent](./media/gallery-experiments/edit-details-page.png)

De instellingen van de zichtbaarheid van het experiment wijzigen (**openbare** of **niet-weergegeven**), of selecteer wilt verwijderen van het experiment uit de galerie de **instellingen** pictogram.

![Selecteer 'Instellingen' om zichtbaarheid te wijzigen of verwijderen van het experiment](./media/gallery-experiments/settings-button.png)

#### <a name="from-your-profile-page"></a>Op uw profielpagina
Selecteer de pijl-omlaag voor het experiment op uw profielpagina en selecteer vervolgens **bewerken**. Hiermee gaat u naar de pagina met details voor uw experiment in de bewerkingsmodus. Wanneer u klaar bent bent, selecteert u **gedaan**.

Selecteer wilt verwijderen van het experiment uit de galerie **verwijderen**.

![Selecteer 'Bewerken' of "Delete"](./media/gallery-experiments/edit-delete-buttons.png)

### <a name="tips-for-documenting-and-publishing-your-experiment"></a>Tips voor het documenteren en publiceren van uw experiment
* U kunt ervan uitgaan dat de lezer voorgaande datatechnologie is-ervaring, maar kan het handig is voor gebruik van eenvoudige taal zijn. Wordt uitgelegd wat in detail indien mogelijk.
* Cortana Intelligence Suite is relatief nieuw zijn. Niet alle lezers bent bekend met het gebruik ervan. Geef voldoende informatie en stapsgewijze uitleg voor de lezers van uw experiment te navigeren.
* Visuele elementen kunnen nuttig zijn voor lezers het interpreteren en correct gebruik van de documentatie van uw experiment. Visuele elementen bevatten experimentele grafieken en schermopnamen van gegevens. Zie voor meer informatie over het opnemen van afbeeldingen in de documentatie van uw experiment de [Publicatierichtlijnen en voorbeelden verzameling](https://gallery.cortanaintelligence.com/Collection/Publishing-Guidelines-and-Examples-1).
* Als u een set gegevens in uw experiment opneemt (dat wil zeggen, u bent niet importeren van de gegevensset via de module gegevens importeren), de gegevensset is onderdeel van uw experiment en in de galerie wordt gepubliceerd. Zorg ervoor dat de gegevensset die u publiceert heeft licentievoorwaarden die delen en te downloaden door iedereen toestaan. Galeriebijdragen vallen onder de Azure [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**Wat zijn de vereisten voor het verzenden of bewerken van een afbeelding voor mijn experiment?**

Installatiekopieën die u met uw experiment verzenden worden gebruikt voor het maken van een experiment tegel voor uw bijdragen. Het is raadzaam dat installatiekopieën niet kleiner zijn dan 500 KB, met een aspect ratio van 3:2, en een resolutie van 960 &#215; 640.

**Wat gebeurt er met de gegevensset die ik in het experiment gebruikt? Wordt de gegevensset ook gepubliceerd in de galerie?**

Als uw gegevensset onderdeel van uw experiment is en niet via de Import-module wordt geïmporteerd, wordt de gegevensset gepubliceerd in de galerie als onderdeel van uw experiment. Zorg ervoor dat de gegevensset die u met uw experiment publiceert de juiste licentievoorwaarden heeft. De licentievoorwaarden moeten toestaan iedereen te delen en de gegevens te downloaden.

**Ik heb een experiment die gebruikmaakt van een module gegevens importeren gegevens wilt halen uit Azure HDInsight of SQL Server. Mijn referenties wordt gebruikt om de gegevens te halen. Kan ik dit soort experiment publiceren? Hoe kan ik er zeker van zijn dat mijn referenties niet wordt gedeeld?**

U kunt een experiment dat referenties in de galerie gebruikt op dit moment niet publiceren.

**Hoe voer ik meerdere labels?**

Druk op de Tab-toets nadat u een label, om in te voeren van een andere code invoeren.

**[Ga naar de galerie](http://gallery.cortanaintelligence.com)**


