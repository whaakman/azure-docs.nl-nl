---
title: Klonen van een Jupyter-notebook vanuit GitHub met Azure-laptops
description: Snel een Jupyter-notebook vanuit een GitHub-opslagplaats te klonen en voer dit in uw Azure-notitieblokken-account.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: d7122b78-6daa-4bea-883b-ff832cfecef3
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: e180192ed8375e00464ed9ec96080cde10295e5c
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754052"
---
# <a name="quickstart-clone-a-notebook"></a>Quickstart: Een notebook klonen

Veel gegevensanalisten en ontwikkelaars slaan hun notitieblokken in [GitHub-opslagplaatsen](https://github.com), een gratis opslag en versiecontrole biedt voor veel andere projecttypen. GitHub wordt vaak gebruikt als middel om samen te werken op Jupyter-notebooks die lokaal worden uitgevoerd. In dergelijke gevallen kan elke samenwerker onderhoudt een lokale kopie van de opslagplaats en de notebooks op dat exemplaar wordt uitgevoerd.

Klonen wordt een kopie gemaakt van een GitHub-notebook in uw Azure-notitieblokken-account in plaats daarvan. De kloon is onafhankelijk van de oorspronkelijke opslagplaats; wijzigingen worden opgeslagen in alleen de account van uw Azure-notitieblokken en hebben geen invloed op de oorspronkelijke. Omdat de kloon in de cloud, kunt u het project delen met andere medewerkers die niet moeten een lokale kopieën te maken of zelfs automatisch laten Jupyter op hun eigen computers geïnstalleerd. U kunt een laptop ook klonen gewoon als een beginpunt voor een project van uw eigen of te downloaden van gegevensbestanden.

## <a name="clone-azure-cognitive-services-notebooks"></a>Klonen van Azure Cognitive Services-laptops

1. Ga naar [Azure notitieblokken](https://notebooks.azure.com) en meld u aan. (Zie voor meer informatie, [-Snelstart: aanmelden bij Azure-notitieblokken](quickstart-sign-in-azure-notebooks.md)).

1. Selecteer in de pagina van uw openbare profiel **Mijn projecten** aan de bovenkant van de pagina:

    ![Mijn projecten koppeling boven aan het browservenster](media/quickstarts/my-projects-link.png)

1. Op de **Mijn projecten** pagina, selecteert u de pijl-omhoog (sneltoets: U; de knop wordt weergegeven als **GitHub-opslagplaats uploaden** wanneer het browservenster wordt breed genoeg):

    ![Uploaden van GitHub-opslagplaats opdracht op de pagina Mijn projecten](media/quickstarts/upload-github-repo-command.png)

1. In de **GitHub-opslagplaats uploaden** die wordt weergegeven, invoeren of stel de volgende details en selecteer vervolgens **importeren**:

   - **GitHub-opslagplaats**: Microsoft/cognitieve-services-notitieblokken (deze naam wordt de Jupyter-notebooks voor Azure Cognitive Services op gekloond [ https://github.com/Microsoft/cognitive-services-notebooks ](https://github.com/Microsoft/cognitive-services-notebooks)).
   - **Klonen recursief**: (uitgeschakeld)
   - **Naam van het project**: Kloon van de cognitive Services
   - **Project-ID**: cognitive services-kloon
   - **Openbare**: (uitgeschakeld)

     ![GitHub-opslagplaats pop-upvenster voor het verzamelen van informatie van de opslagplaats uploaden](media/quickstarts/upload-github-repo-popup.png)

1. Niet beschikbaar is even geduld terwijl het proces is voltooid; een opslagplaats klonen, kan een paar minuten duren.

1. Als het klonen is voltooid, gaat-laptops Azure u naar het nieuwe project waar u kunt zien de kopieën van alle bestanden.

    [![](media/quickstarts/completed-clone.png "Weergave van een kloon voltooid")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Een notitieblok delen

1. Als u wilt uw exemplaar van de gekloonde project delen, gebruikt u de **delen** bepalen of een koppeling verkrijgen, HTML of Markdown-code die bevat een koppeling verkrijgen of maken van een e-mailbericht met de koppeling:

    ![Opdracht voor project delen](media/quickstarts/share-project-command.png)

1. Omdat u gewist, de **openbare** optie bij het klonen van het project, de kloon is privé. Als u uw exemplaar openbaar, schakelt u **projectinstellingen**, stel de **openbare project** in het pop-upvenster de optie en selecteer vervolgens **opslaan**.

1. Selecteer een notitieblok in het project uit te voeren. Elk notitieblok in de opslagplaats voor Azure Cognitive Services, is bijvoorbeeld een eigen onafhankelijke Quick Start. De onderstaande afbeelding toont het resultaat van het gebruik van de notebook BingImageSearchAPI na het toevoegen van een abonnementssleutel Cognitive Services-API en wijzigen van de zoekterm 'Puppy's ' in 'bunnies':

    ![Jupyter-notebook uitgevoerd gekloond vanuit GitHub](media/quickstarts/clone-notebook-result.png)

1. Wanneer u klaar bent met het notitieblok, selecteer **bestand** > **sluiten en stoppen** om het notitieblok en een browservenster te sluiten.

1. Met de rechtermuisknop op de notebook voor het delen van een afzonderlijke notitieblok in het project en selecteer **koppeling kopiëren** (sneltoets: y):

    ![Opdracht in het contextmenu een koppeling naar een afzonderlijke notitieblok kopiëren](media/quickstarts/copy-link-to-individual-notebook.png)

1. Als u wilt bewerken bestanden dan laptops, met de rechtermuisknop op het bestand in het project en selecteer **bestand bewerken** (sneltoets: ik). De standaardactie **uitvoeren** (sneltoets: r), alleen inhoud van het bestand bevat en niet toestaan.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: een run maken een Jupyter-notebook te doen, lineaire regressie](tutorial-create-run-jupyter-notebook.md)
