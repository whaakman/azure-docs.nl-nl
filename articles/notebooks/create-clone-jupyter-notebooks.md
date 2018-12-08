---
title: Maken en klonen Jupyter-notebooks in Azure
description: Azure notitieblokken projecten beheren van een verzameling van notitieblokken en -gerelateerde bestanden, die u kunt nieuwe maken of klonen vanaf een andere bron.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 9b6a49e2-1d71-4c0b-9e5d-16e059427e38
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: c4325f0c9de0d945d1612a3ab4171a2bfe1be25b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106512"
---
# <a name="create-and-clone-projects"></a>Projecten maken en klonen

Azure-notitieblokken organiseert uw Jupyter-notebooks en de bijbehorende bestanden in logische groepen met de naam *projecten*. U een project eerst als een container maken en vervolgens maakt of een of meer notitieblokken in een map naast andere projectbestanden klonen. (Dit proces wordt geïllustreerd in de [zelfstudie](tutorial-create-run-jupyter-notebook.md).)

Een project onderhoudt ook metagegevens en andere configuratie-instellingen die invloed hebben op de server op welke notitieblokken uitvoeren, met inbegrip van aangepaste installatiestappen uit en installatie van het pakket. Zie voor meer informatie, [beheren en configureren van projecten](configure-manage-azure-notebooks-projects.md).

## <a name="use-the-my-projects-dashboard"></a>Gebruik het dashboard Mijn projecten

Uw **Mijn projecten** dashboard op `https://notebooks.azure.com/<userID>/projects` is waar u weergeven, beheren en projecten maken:

[![](media/my-projects-dashboard.png "Mijn dashboard projecten in Azure-notitieblokken")](media/my-projects-dashboard.png#lightbox)

Wat u op het dashboard kunt doen, is afhankelijk van of u bent aangemeld met het account dat eigenaar is van de gebruikers-ID:

| Opdracht | Beschikbaar voor | Description |
| --- | --- | --- |
| **Uitvoeren** | Eigenaar | Start de projectserver en de projectmap in Jupyter geopend. (Meer over het algemeen u eerst in een projectmap navigeren en start u een laptop van daaruit.) |
| **Downloaden** | Iedereen | Een kopie van het geselecteerde project gedownload als een ZIP-bestand. |
| **Delen** | Iedereen | Het delen pop-upvenster waarmee u kunt verkrijgen van een URL naar een geselecteerde project, delen op sociale media, stuurt u een e-mail met de URL en verkrijgen van HTML- of Markdown-code voor met een badge 'notebook starten' wordt weergegeven (Zie [verkrijgen van een badge starten](#obtain-a-launch-badge)) met de URL. |
| **Verwijderen** | Eigenaar | Hiermee verwijdert u het geselecteerde project. Deze bewerking kan niet ongedaan worden gemaakt. |
| **Terminal** | Eigenaar | Start de projectserver en vervolgens een nieuw browservenster geopend met de bash, terminal voor die server. |
| **+ Nieuw Project** | Eigenaar | Hiermee maakt u een nieuw project. Zie [een nieuw project maken](#create-a-new-project). |
| **Github-opslagplaats uploaden** | Eigenaar | Een project importeert vanuit GitHub. [Een project importeren vanuit GitHub](#import-a-project-from-github). |
| **Kloon** | Iedereen | Een geselecteerde project worden gekopieerd naar uw eigen account. Vraagt u zich kunt aanmelden in als dit nog niet. Zie [klonen van een project](#clone-a-project). |

### <a name="obtain-a-launch-badge"></a>Verkrijgen van een badge starten

Wanneer u gebruikt de **Share** opdracht en selecteer de **insluiten** tabblad kunt u HTML-code of Markdown waarmee een badge 'notebook starten':

![Laptop-badge starten ](https://notebooks.azure.com/launch.png)

Als u een Azure-notitieblokken-project hebt, kunt u een koppeling die wordt gekloond vanuit GitHub rechtstreeks met behulp van de volgende sjablonen, vervangen door de juiste gebruikersnaam en de namen van de opslagplaats:

```html
<a href="https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>"><img src="https://notebooks.azure.com/launch.png" /></a>
```

```markdown
[![Azure Notebooks](https://notebooks.azure.com/launch.png)](https://notebooks.azure.com/import/gh/<GitHub_username>/<repository_name>)
```

## <a name="create-a-new-project"></a>Een nieuw project maken

Wanneer u gebruikt de **+ nieuw Project** opdracht, Azure-notitieblokken wordt weergegeven een **nieuw Project maken** pop-upvenster. Voer de volgende informatie in dit pop-upvenster en selecteer vervolgens **maken**:

| Veld | Description |
| --- | --- |
| Projectnaam | Een beschrijvende naam voor uw project die gebruikmaakt van Azure-notitieblokken weer te geven. Bijvoorbeeld: 'Ny Notebook Project'. |
| Project-id | Een aangepaste id die deel van de URL die u gebruikt uitmaken voor het delen van een project. Deze ID kunt alleen letters, cijfers en afbreekstreepjes bevatten en is beperkt tot 30 tekens bevatten. Als u niet zeker weet wat u wilt gebruiken, wordt een algemene conventie is het gebruik van een kleine versie van de projectnaam van uw waar spaties in afbreekstreepjes bevatten, zoals 'Mijn-notebook-project' (afgekapt indien nodig aanpassen aan de maximale lengte) zijn ingeschakeld. |
| Public | Als is ingesteld, kan iedereen met de koppeling voor toegang tot het project. Wanneer u een privé-project maakt, moet u deze optie uitschakelen. |
| Initialiseren van dit project met een Leesmij-bestand | Als instelt, maakt u een standaard *README.md* bestand in het project. Een *README.md* bestand is waar het bieden van documentatie voor uw project, indien gewenst. |

## <a name="import-a-project-from-github"></a>Een project importeren vanuit GitHub

U kunt eenvoudig een volledige openbare GitHub-opslagplaats importeren als een project met inbegrip van alle gegevens en *README.md* bestanden. Gebruik de **GitHub-opslagplaats uploaden** opdracht, geef de volgende informatie in het pop-upvenster op en selecteer vervolgens **importeren**:

| Veld | Description |
| --- | --- |
| GitHub-opslagplaats | De naam van de opslagplaats op github.com. Bijvoorbeeld, om te klonen van de Jupyter-notebooks voor Azure Cognitive Services op [ https://github.com/Microsoft/cognitive-services-notebooks ](https://github.com/Microsoft/cognitive-services-notebooks), voer ' Microsoft/cognitieve-services-notitieblokken'.  |
| Kloon-recursief | GitHub-opslagplaatsen kunnen meerdere onderliggende opslagplaatsen bevatten. Stel deze optie als u wilt klonen van de bovenliggende of hoofdopslagplaats en alle onderliggende items. Omdat het is mogelijk dat een opslagplaats te veel onderliggende elementen hebben, laat u deze optie wissen tenzij u zeker weet dat u ze nodig hebt. |
| Projectnaam | Een beschrijvende naam voor uw project die gebruikmaakt van Azure-notitieblokken weer te geven. |
| Project-id | Een aangepaste id die deel van de URL die u gebruikt uitmaken voor het delen van een project. Deze ID kunt gebruiken alleen letters, cijfers en afbreekstreepjes bevatten. |
| Public | Als is ingesteld, kan iedereen met de koppeling voor toegang tot het project. Wanneer u een privé-project maakt, moet u deze optie uitschakelen. |

Importeren van een opslagplaats van GitHub importeert bijbehorende geschiedenis ook. U kunt standard Git-opdrachten uit vanaf de terminal naar de nieuwe wijzigingen doorvoeren, pull-wijzigingen vanuit GitHub, enzovoort.

## <a name="clone-a-project"></a>Een project klonen

Klonen, maakt een kopie van een bestaand project in uw eigen account, waar u kunt uitvoeren en wijzigen van uw laptop of een ander bestand in het project. U kunt klonen ook gebruiken om kopieën van uw eigen projecten waarin u hoeft u geen experimenten of voor andere werkzaamheden te verstoren het oorspronkelijke project te maken.

Voor het klonen van een project:

1. Op de **Mijn projecten** -dashboard met de rechtermuisknop op het gewenste project en selecteer **kloon** (sneltoets: c).

    ![Opdracht in het contextmenu project klonen](media/clone-command.png)

1. In de **kloon Project** pop-upvenster Voer een naam en de ID voor de kloon, en opgeven of de kloon openbaar is. Deze instellingen zijn hetzelfde als voor een [nieuw project](#create-a-new-project).

    ![Kloon Project pop-upvenster](media/clone-project.png)

1. Nadat u hebt geselecteerd de **kloon** knop,-laptops Azure gaat rechtstreeks naar de kopie.

## <a name="next-steps"></a>Volgende stappen

- [Voorbeeldnotitieblokken verkennen](azure-notebooks-samples.md)
- [Hoe: configureren en beheren van projecten](configure-manage-azure-notebooks-projects.md)
- [Hoe: installatie van pakketten van binnen een laptop](install-packages-jupyter-notebook.md)
- [Hoe: een diavoorstelling](present-jupyter-notebooks-slideshow.md)
- [Hoe: werken met bestanden](work-with-project-data-files.md)
- [Hoe: toegang tot gegevensbronnen](access-data-resources-jupyter-notebooks.md)
- [Hoe: Azure Machine Learning-Services gebruiken](use-machine-learning-services-jupyter-notebooks.md)
