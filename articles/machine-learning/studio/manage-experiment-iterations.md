---
title: Experimentherhalingen in Machine Learning Studio beheren | Microsoft Docs
description: Experimentherhalingen in Azure Machine Learning Studio beheren
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a53530f-20d5-40ae-9b49-7b499ccb44b7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 062620f2174ecc93c1deb816069e32152dbef636
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Iteraties van experimenten beheren in Azure Machine Learning Studio
Een predictive Analytics-model te ontwikkelen, is een iteratief proces - als u de verschillende functies en parameters van uw experiment wijzigt, worden de resultaten geconvergeerd tot u tevreden bent dat u een getraind en doeltreffend model hebt. Sleutel voor dit proces is het bijhouden van de verschillende herhalingen van uw experiment parameters en configuraties.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

U kunt eerder uitgevoerde uw experimenten bekijken op elk gewenst moment om uitdaging, bezoekt, en uiteindelijk bevestigen of eerdere veronderstellingen verfijnen. Wanneer u een experiment uitvoert, Machine Learning Studio wordt bijgehouden de uitvoeren, waaronder gegevensset, module en poort verbindingen en -parameters. Dit overzicht wordt ook vastgelegd runtime informatie zoals de begin- en stoptijden, logboekberichten en uitvoeringsstatus van resultaten. U kunt bekijken weer een van deze wordt uitgevoerd op elk gewenst moment de volgorde van uw experiment en tussenliggende resultaten bekijken. U kunt zelfs een eerder uitgevoerde van uw experiment gebruiken om te starten in een nieuwe fase van de query en detectie van het pad voor het maken van eenvoudige, complexe of zelfs ensemble modellering oplossingen.

> [!NOTE]
> Wanneer u een experiment eerder werd uitgevoerd, wordt die versie van het experiment is vergrendeld en kan niet worden bewerkt. U kunt echter een kopie opslaan door te klikken op **SAVE AS** en het geven van een nieuwe naam voor de kopie. Machine Learning Studio wordt geopend voor de nieuwe kopie, die vervolgens bewerken en uitvoeren. Dit exemplaar van uw experiment is beschikbaar in de **EXPERIMENTEN** lijst samen met uw experimenten.
> 
> 

## <a name="viewing-the-prior-run"></a>Weergeven van de voorafgaande uitvoeren
Wanneer u een experiment geopend die u hebt ten minste één keer uitgevoerd hebt, kunt u de voorgaande uitvoeren van het experiment weergeven door te klikken op **voorafgaande uitvoeren** in het deelvenster properties.

Stel bijvoorbeeld dat u een experiment maken en uitvoeren van versies van it op 11:23 11:42 en 11:55. Als u de laatste uitvoering van het experiment (11:55) opent en klikt u op **voorafgaande uitvoeren**, de versie die u op 11:42 uitvoerde wordt geopend.

## <a name="viewing-the-run-history"></a>De Uitvoeringsgeschiedenis weergeven
U kunt de vorige uitvoert van een experiment weergeven door te klikken op **weergave uitvoeren geschiedenis** in een open experiment.

Stel bijvoorbeeld dat u maakt een experiment met de [lineaire regressie] [ linear-regression] module en u wilt kijken welk effect van het wijzigen van de waarde van **leertempo** op de resultaten van uw experiment. U uitvoeren het experiment meerdere keren met verschillende waarden voor deze parameter als volgt:

| De waarde voor Learning | Begintijd uitvoering |
| --- | --- |
| 0.1 |11-9-2014 4:18:58 pm |
| 0.2 |11-9-2014 4:24:33 pm |
| 0.4 |11-9-2014 4:28:36 uur |
| 0.5 |11-9-2014 4:33:31 pm |

Als u op **weergave uitvoeren geschiedenis**, ziet u een lijst van deze wordt uitgevoerd:

![Voorbeeld van de geschiedenis uitvoeren][runhistory]

Klik op een van deze wordt uitgevoerd om weer te geven van een momentopname van het experiment op het moment dat u deze hebt uitgevoerd. De configuratie, parameterwaarden, opmerkingen en resultaten alle behouden zodat u een volledig overzicht van uw experiment die wordt uitgevoerd.

> [!TIP]
> Als u wilt uw herhalingen van het experiment document, kunt u de titel telkens wanneer u deze uitvoeren, kunt u bijwerken de **samenvatting** van het experiment in de eigenschappen deelvenster en u kunt toevoegen of bijwerken van opmerkingen over afzonderlijke modules om uw wijzigingen te noteren. De titel, de samenvatting en de module opmerkingen worden opgeslagen bij elke uitvoering van het experiment.
> 
> 

De lijst met experimenten in de **EXPERIMENTEN** tabblad in Machine Learning Studio altijd de nieuwste versie van een experiment weergegeven. Als u een eerder uitgevoerde van het experiment openen (met behulp van **voorafgaande uitvoeren** of **uitvoeren Historie weergeven**), kunt u terugkeren naar de conceptversie door te klikken op **weergave uitvoeren geschiedenis** en het selecteren van de herhaling met een **status** van **bewerkbaar**.

## <a name="iterating-on-a-previous-run"></a>Op een eerder uitgevoerde doorlopen
Wanneer u klikt op **voorafgaande uitvoeren** of **weergave uitvoeren geschiedenis** en opent een eerder werd uitgevoerd, kunt u een voltooide experiment weergeven in de modus alleen-lezen.

Als u beginnen met een herhaling van uw experiment die beginnen met de manier waarop u het voor een eerder uitgevoerde hebt geconfigureerd wilt, kunt u dit doen door de sessie openen en te klikken op **SAVE AS**. Hiermee maakt u een nieuw experiment met een nieuwe naam, een lege geschiedenis, uitvoeren en alle onderdelen en parameterwaarden van de vorige uitgevoerd. Dit nieuwe experiment wordt vermeld in de **EXPERIMENTEN** tabblad in de startpagina van de Machine Learning Studio, en u kunt wijzigen en uitvoeren, initiëren van een nieuwe uitvoeringsgeschiedenis voor deze herhaling van uw experiment. 

Stel dat u hebt het experiment geschiedenis wordt weergegeven in de vorige sectie uitvoeren. U wilt bekijken wat er gebeurt wanneer u de **leertempo** parameter 0,4 en probeer verschillende waarden voor de **aantal training epoches** parameter.

1. Klik op **weergave uitvoeren geschiedenis** en open de herhaling van het experiment die u hebt uitgevoerd om 4:28:36 uur (waarin u de parameterwaarde instelt op 0,4).
2. Klik op **SAVE AS**.
3. Voer een titel en klik op de **OK** vinkje. Een nieuwe kopie van het experiment is gemaakt.
4. Wijzig de **aantal training epoches** parameter.
5. Klik op **uitvoeren**.

U kunt nu verder wijzigen en uitvoeren van deze versie van uw experiment bouwen van een nieuwe uitvoeringsgeschiedenis om vast te leggen van uw werk.

<!-- Images -->
[runhistory]:./media/manage-experiment-iterations/viewrunhistory.jpg


<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
