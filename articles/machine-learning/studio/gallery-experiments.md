---
title: Azure AI-galerie experimenten | Microsoft Docs
description: Detecteren en experimenten in de galerie van Azure AI delen.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: f4248922-c961-4d3a-9e1b-aec743210166
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.openlocfilehash: d6de7aa6d16196f415688d68251e8c1b0a4b9aee
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="discover-experiments-in-azure-ai-gallery"></a>Experimenten in de galerie van Azure AI detecteren
[!INCLUDE [machine-learning-gallery-item-selector](../../../includes/machine-learning-gallery-item-selector.md)]

## <a name="experiments-for-machine-learning-studio"></a>Experimenten voor Machine Learning Studio
De galerie heeft een groot aantal [experimenten](https://gallery.cortanaintelligence.com/experiments) die zijn ontwikkeld [Azure Machine Learning Studio](https://studio.azureml.net). Het bereik van de experimenten van snelle bewijs van concept experimenten die een specifieke machine learning techniek volledig ontwikkelde oplossingen voor problemen met complexe machine learning te demonstreren.

> [!NOTE]
> Een ***experimenteren*** is een tekenpapier in Machine Learning Studio die u gebruiken kunt voor het maken van een predictive Analytics-model. U kunt het model maken door gegevens te koppelen aan verschillende analytische modules. U kunt verschillende ideeën probeert, doen evaluatieversie wordt uitgevoerd en uiteindelijk uw model te implementeren als een webservice in Azure. Zie voor een voorbeeld van hoe u een eenvoudige experiment maken [Machine learning-zelfstudie: uw eerste experiment maken in Azure Machine Learning Studio](create-experiment.md). Zie voor een uitgebreider overzicht van het maken van een predictive analytics-oplossing [scenario: een predictive analytics-oplossing voor kredietrisicobeoordeling in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md).
>
>

## <a name="discover"></a>Ontdekken
Om te bladeren door experimenten [in de galerie](http://gallery.cortanaintelligence.com), aan de bovenkant van de startpagina van de galerie selecteren **experimenten**.

De **[experimenten](https://gallery.cortanaintelligence.com/experiments)** pagina geeft een lijst met onlangs toegevoegde en populaire experimenten. Als u wilt zien alle experimenten, selecteer de **alle** knop. Als u wilt zoeken naar een specifieke experiment, selecteer **alle**, en selecteer vervolgens filtercriteria. Ook kunt u zoektermen op in de **Search** vak aan de bovenkant van de pagina Galerie.

U kunt meer informatie over een experiment op de pagina experiment details ophalen. Als de pagina met details van een experiment, schakelt u het experiment. Op een experiment details pagina in de **opmerkingen** sectie, u kunt opmerkingen, feedback geven of vragen hebt over het experiment. U kunt zelfs het experiment delen met vrienden of collega's op Twitter of LinkedIn. U kunt ook een koppeling naar de pagina experiment details om uit te nodigen van andere gebruikers naar de pagina mail.

![Dit item delen met vrienden](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Uw eigen opmerkingen toevoegen](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="download"></a>Downloaden
U kunt een kopie van een experiment in de galerie downloaden naar uw werkruimte van Machine Learning Studio. Vervolgens kunt u uw exemplaar voor het maken van uw eigen oplossingen wijzigen.

Azure AI-galerie biedt twee methoden voor het importeren van een kopie van een experiment:

* **In de galerie**. Als u een experiment die u in de galerie, zoals vinden, kunt u een kopie downloaden en opent u het in uw werkruimte van Machine Learning Studio.
* **Vanuit Machine Learning Studio**. In Machine Learning Studio, kunt u een experiment in de galerie als een sjabloon voor het maken van een nieuw experiment.

### <a name="from-the-gallery"></a>In de galerie

1. Open in de galerie, de detailpagina experiment.
2. Selecteer **openen in Studio**.

    ![Open experiment uit de galerie](./media/gallery-experiments/open-experiment-from-gallery.png)

Wanneer u selecteert **openen in Studio**, het experiment in uw werkruimte van Machine Learning Studio wordt geopend. (Als u nog niet bent aangemeld bij Machine Learning Studio, u gevraagd om de eerste te ondertekenen in met behulp van uw Microsoft-account.)

### <a name="from-within-machine-learning-studio"></a>Vanuit Machine Learning Studio

1. Selecteer in Machine Learning Studio **nieuw**.
2. Selecteer **Experiment**. U kunt kiezen uit een lijst van de galerie experimenten of een specifieke experiment zoeken met behulp van de **Search** vak.
3. Wijs met de muis op een experiment en selecteer vervolgens **openen in Studio**. (Als u informatie over het experiment, selecteer **weergave in de galerie**. Hiermee gaat u naar de detailpagina experiment in de galerie.)

    ![Open galerie experimenteren uit in Machine Learning Studio](./media/gallery-experiments/open-experiment-from-studio.png)

U kunt aanpassen, herhalen en implementeren van een experiment gedownloade zoals andere experiment die u in Machine Learning Studio maakt.

![Experiment geopend in Studio](./media/gallery-experiments/experiment-open-in-studio.png)

## <a name="contribute"></a>Bijdragen
Wanneer u zich aanmeldt bij de galerie, worden lid van de Gallery-community. Als een lid van de community kunt u uw eigen experimenten bijdragen zodat andere gebruikers kunnen profiteren van de oplossingen die u hebt ontdekt.

### <a name="publish-your-experiment-to-the-gallery"></a>Uw experiment publiceren naar de galerie

1. Meld u aan Machine Learning Studio via uw Microsoft-account.
2. Uw experiment maken en vervolgens uit te voeren.
3. Wanneer u klaar bent voor het publiceren van uw experiment in de galerie, in de lijst met acties onder het experimentcanvas selecteren **publiceren naar de galerie**.

    ![Selecteer 'Publiceren naar de galerie'](./media/gallery-experiments/publish-experiment-to-gallery.png)
4. Op de **Experiment beschrijving** pagina, voer een titel en labels. Controleer de titel en labels beschrijvende. Markeer de technieken die u hebt gebruikt of het werkelijke probleem oplossen. Een voorbeeld van een experiment beschrijvende titel is ' binaire classificatie: Twitter-gevoel Analysis. "

    ![Voer titel en labels voor publicatie](./media/gallery-experiments/experiment-description.png)
5. In de **samenvatting** Geef een samenvatting van uw experiment. Een korte beschrijving van het probleem dat het experiment is opgelost en hoe u nadert.
6. In de **gedetailleerde beschrijving** vak, worden de stappen beschreven die u hebt gemaakt in elk deel van uw experiment. Sommige onderwerpen handig om op te nemen zijn:
   * Schermafbeelding van de grafiek experiment
   * Uitleg bij en gegevensbronnen
   * Gegevensverwerking
   * Functie-engineering
   * Modelbeschrijving van het
   * Resultaten en evaluatie van de model-prestaties

   U kunt markdown je de beschrijving. Selecteer hoe de gegevens op de pagina van de beschrijving experiment wilt bekijken wanneer het experiment wordt gepubliceerd, **Preview**.

   ![Selecteer 'Preview' om te zien hoe de tekst eruit](./media/gallery-experiments/preview-markdown-text.png)

   > [!TIP]
   > De tekstvakken opgegeven voor markdown bewerken en voorbeeld van klein zijn. U wordt aangeraden dat u de documentatie van uw experiment schrijven in een markdown-editor kopiëren en plak de voltooide documentatie in het tekstvak in de galerie. Nadat u uw experiment publiceert, kunt u de wijzigingen met behulp van standaard web gebaseerde hulpprogramma's die gebruik markdown voor edit- and -preview.

7. Op de **selectie van een installatiekopie** pagina, kiest u een miniatuur voor uw experiment. De miniatuur wordt weergegeven aan de bovenkant van de detailpagina experiment en in de tegel experiment. Andere gebruikers zien de miniatuurafbeelding wanneer ze door de galerie bladeren. U kunt een installatiekopie van het uploaden van uw computer of een voorraad installatiekopie in de galerie selecteren.
    </br>
    ![Uploaden of Selecteer een afbeelding voor de galerij](./media/gallery-experiments/select-gallery-image.png)
8. Op de **instellingen** pagina onder **zichtbaarheid**, kies of u voor het publiceren van uw inhoud openbaar (**openbare**) of zodat deze alleen toegankelijk voor mensen met een koppeling naar de pagina (**niet-weergegeven**).

    ![Kies of u wilt publiceren openbaar of als een niet-vermelde](./media/gallery-experiments/choose-public-or-unlisted.png)

    <!-- -->

   > [!TIP]
   > Als u controleren of de documentatie van uw er goed uitziet wilt voordat u het openbaar verstuurt, kunt u eerst het experiment als publiceren **niet-weergegeven**. Later, kunt u de instellingen voor zichtbaarheid te wijzigen **openbare** op de detailpagina experiment.
   >
   >
9. Als u wilt het experiment publiceren naar de galerie, selecteer de **OK** selectievakje is ingeschakeld.

    ![Selecteer het vinkje OK voor het publiceren van het experiment](./media/gallery-experiments/ok-checkmark.png)

Zie voor meer tips voor het uitgeven van een experiment van hoge kwaliteit galerie [Tips voor het documenteren en publiceren van uw experiment](#tips-for-documenting-and-publishing-your-experiment).

Dat is alles--u alle klaar bent.

U kunt nu uw experiment weergeven in de galerie en de koppeling met anderen delen. Als u uw experiment gepubliceerd met behulp van de **openbare** zichtbaarheid instelt, het experiment wordt weergegeven in de resultaten van bladeren en zoek in de galerie. U kunt uw experiment-documentatie op de detailpagina experiment elk gewenst moment dat u bent aangemeld bij de Galerie bewerken.

De lijst van uw bijdragen wilt bekijken, selecteert u uw installatiekopie in de rechterbovenhoek van galeriepagina's. Selecteer vervolgens de naam van uw om uw accountpagina te openen.

![Selecteer de accountnaam van uw](./media/gallery-experiments/click-account-name.png)

### <a name="update-your-experiment"></a>Bijwerken van uw experiment
Als u wilt, kunt u wijzigingen aanbrengen aan de werkstroom in een experiment die u hebt gepubliceerd naar de galerie (modules, parameters, enzovoort). Breng wijzigingen wilt aanbrengen in het experiment en vervolgens opnieuw te publiceren in Machine Learning Studio. Uw gepubliceerde experiment wordt bijgewerkt met uw wijzigingen.

U kunt een van de volgende informatie voor uw experiment rechtstreeks in de galerie wijzigen:

* Naam van experiment
* Samenvatting of beschrijving
* Tags
* Installatiekopie
* Zichtbaarheidsinstelling (**openbare** of **niet-weergegeven**)

U kunt ook het experiment verwijderen uit de galerie.

U kunt deze wijzigingen aanbrengen, of het experiment verwijderen van de detailpagina experiment of van de profielpagina in de galerie.


#### <a name="from-the-experiment-details-page"></a>Uit de detailpagina experiment
Op de detailpagina experiment wilt wijzigen de details voor uw experiment, selecteert u **bewerken**.

![Selecteer Bewerken om te bewerken van uw experiment](./media/gallery-experiments/edit-button.png)

De detailpagina krijgt de bewerkingsmodus. Als u wilt wijzigen, selecteert u **bewerken** naast de naam van het experiment, samenvatting of labels. Wanneer u klaar bent u wijzigingen aanbrengt, selecteert u **gedaan**.

![Selecteer 'Bewerken' als gegevens wilt bewerken en selecteer 'Uitgevoerd' als voltooid](./media/gallery-experiments/edit-details-page.png)

Wijzigen van de instellingen voor zichtbaarheid voor het experiment (**openbare** of **niet-weergegeven**), of selecteert u het experiment uit de galerie verwijderen door de **instellingen** pictogram.

![Selecteer 'Instellingen' om zichtbaarheid te wijzigen of verwijderen van het experiment](./media/gallery-experiments/settings-button.png)

#### <a name="from-your-profile-page"></a>Op de profielpagina
Selecteer op de profielpagina voor het experiment de pijl-omlaag en selecteer vervolgens **bewerken**. Hiermee gaat u naar de detailpagina voor uw experiment, in de bewerkingsmodus. Wanneer u klaar bent bent, selecteert u **gedaan**.

Selecteer wilt verwijderen van het experiment uit de galerie **verwijderen**.

![Selecteer 'Bewerken' of 'Verwijderen'](./media/gallery-experiments/edit-delete-buttons.png)

### <a name="tips-for-documenting-and-publishing-your-experiment"></a>Tips voor het documenteren en publiceren van uw experiment
* U kunt wordt ervan uitgegaan dat de lezer heeft eerdere gegevenswetenschap ervaring, maar kan het handig zijn voor gebruik van eenvoudige taal zijn. Uitgelegd dingen toegelicht indien mogelijk.
* Cortana Intelligence Suite is relatief nieuw. Niet alle lezers bent bekend met het gebruik ervan. Geef voldoende informatie en stapsgewijze uitleg zodat de lezer uw experiment navigeren.
* Visuele elementen is handig voor lezers interpreteren en gebruikt u de documentatie van uw experiment correct zijn. Visuele elementen bevatten experimentele grafieken en schermopnamen van gegevens. Zie voor meer informatie over het opnemen van afbeeldingen in de documentatie van uw experiment de [Publishing richtlijnen en voorbeelden verzameling](https://gallery.cortanaintelligence.com/Collection/Publishing-Guidelines-and-Examples-1).
* Als u een set gegevens in uw experiment opnemen (dat wil zeggen, u niet de importeert de gegevensset via de module gegevens importeren), de gegevensset is onderdeel van uw experiment en is gepubliceerd in de galerie. Zorg ervoor dat het licentievoorwaarden waarmee delen en het downloaden door iedereen die de gegevensset die u publiceert. Bijdragen vanuit de galerie vallen onder het Azure [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
**Wat zijn de vereisten voor het verzenden van of het bewerken van een afbeelding voor mijn experiment?**

Installatiekopieën die u met uw experiment indienen worden gebruikt voor het maken van een experiment tegel voor uw bijdrage. Het is raadzaam dat afbeeldingen niet kleiner zijn dan 500 KB, met een aspect ratio van 3:2 en een resolutie van 960 &#215; 640.

**Wat gebeurt er in de gegevensset die ik in het experiment gebruikt? Is de set gegevens ook in de galerie gepubliceerd?**

Als uw gegevensset onderdeel van uw experiment is en niet via de module gegevens importeren wordt geïmporteerd, wordt de gegevensset is gepubliceerd in de galerie als onderdeel van uw experiment. Zorg ervoor dat de gegevensset die u met uw experiment publiceert de relevante licentievoorwaarden heeft. De licentievoorwaarden mag iedereen te delen en de gegevens te downloaden.

**Ik heb een experiment die gebruikmaakt van een module gegevens importeren naar pull-gegevens uit Azure HDInsight of SQL Server. Mijn referenties wordt gebruikt voor het ophalen van de gegevens. Kan ik dit soort experiment publiceren? Hoe kan ik er zeker van zijn dat mijn referenties niet wordt gedeeld?**

Op dit moment kunt publiceren u een experiment dat gebruikmaakt van referenties in de galerie niet.

**Hoe voer ik meerdere labels**

Druk op de Tab-toets nadat u een label, om in te voeren van een andere code invoeren.

**[Ga naar de galerie](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
