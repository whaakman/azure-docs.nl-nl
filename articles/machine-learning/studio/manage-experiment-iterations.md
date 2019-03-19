---
title: Experimentherhalingen beheren
titleSuffix: Azure Machine Learning Studio
description: Over het beheren van experimentherhalingen in Azure Machine Learning Studio. U kunt eerdere uitvoeringen van uw experimenten bekijken op elk gewenst moment om te kunnen verstrekken, bezoekt, en uiteindelijk bevestigen of vorige veronderstellingen verfijnen.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/20/2017
ms.openlocfilehash: 34a72f2e7b6be90654c0f053d5b8978b0283d56c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57890515"
---
# <a name="manage-experiment-iterations-in-azure-machine-learning-studio"></a>Iteraties van experimenten beheren in Azure Machine Learning Studio
Het ontwikkelen van een predictive Analytics-model is in een iteratief proces - als u de verschillende functies en parameters van uw experiment wijzigt, worden de resultaten geconvergeerd tot u tevreden bent u een getraind en doeltreffend model hebt. Sleutel voor dit proces is het bijhouden van de verschillende iteraties van uw experiment parameters en configuraties.



U kunt eerdere uitvoeringen van uw experimenten bekijken op elk gewenst moment om te kunnen verstrekken, bezoekt, en uiteindelijk bevestigen of vorige veronderstellingen verfijnen. Wanneer u een experiment uitvoert, Machine Learning Studio de houdt een geschiedenis bij van de uitvoering, met inbegrip van de gegevensset, module en verbinding met de poort en parameters. Dit overzicht bevat ook resultaten, runtime-gegevens, zoals de begin- en stoptijden, logboekberichten en uitvoeringsstatus. U kunt kijken weer een van deze wordt uitgevoerd op elk gewenst moment om te controleren van de tijdlijn van uw experiment en tussenliggende resultaten genereren. U kunt zelfs een vorige uitvoering van uw experiment gebruiken om te starten in een nieuwe fase van de query en detectie van het pad voor het maken van eenvoudige, complexe of zelfs ensembles modellering van oplossingen.

> [!NOTE]
> Wanneer u een experiment dat eerder is uitgevoerd, wordt deze versie van het experiment is vergrendeld en kan niet worden bewerkt. U kunt echter een kopie van het opslaan door te klikken op **SAVE AS** en het geven van een nieuwe naam voor de kopie. Machine Learning Studio wordt geopend de nieuwe kopie, die u kunt vervolgens bewerken en uitvoeren. Deze kopie van uw experiment is beschikbaar in de **EXPERIMENTEN** lijst samen met uw experimenten.
> 
> 

## <a name="viewing-the-prior-run"></a>De vorige uitvoering weergeven
Wanneer u een experiment geopend die u hebt ten minste één keer uitgevoerd hebt, kunt u de vorige uitvoering van het experiment weergeven door te klikken op **voorafgaande uitvoeren** in het deelvenster met eigenschappen.

Stel bijvoorbeeld dat u een experiment maken en uitvoeren van versie ervan op 11:23, 11:42 en 11:55. Als u de laatste uitvoering van het experiment (11:55) opent en klikt u op **voorafgaande uitvoeren**, de versie die u op 11:42 uitvoerde wordt geopend.

## <a name="viewing-the-run-history"></a>De Uitvoeringsgeschiedenis weergeven
U kunt alle eerdere uitvoeringen van een experiment weergeven door te klikken op **View Run History** in een open experiment.

Stel bijvoorbeeld dat u een experiment met maakt de [lineaire regressie] [ linear-regression] -module en u wilt kijken welk effect van het wijzigen van de waarde van **leertempo** op uw experiment resultaten. U voert het experiment meerdere keren met verschillende waarden voor deze parameter als volgt uit:

| De waarde voor Learning | Begintijd uitvoering |
| --- | --- |
| 0.1 |9/11/2014 4:18:58 uur |
| 0.2 |9/11/2014 4:24:33 pm |
| 0.4 |9/11/2014 4:28:36 uur |
| 0.5 |9/11/2014 4:33:31 uur |

Als u klikt op **VIEW RUN HISTORY**, ziet u een lijst van alle deze wordt uitgevoerd:

![Voorbeeld van de geschiedenis uitvoeren](./media/manage-experiment-iterations/viewrunhistory.jpg)

Klik op een van deze uitvoeringen om weer te geven van een momentopname van het experiment op het moment dat u deze hebt uitgevoerd. De configuratie, parameterwaarden, opmerkingen en resultaten blijven alle behouden zodat u een volledige gegevensrecord van deze uitvoering van uw experiment.

> [!TIP]
> Als u wilt uw herhalingen van het experiment document, kunt u de titel telkens wanneer u deze uitvoeren, kunt u bijwerken de **samenvatting** van het experiment in de eigenschappen van deelvenster en u kunt toevoegen of bijwerken opmerkingen over afzonderlijke modules om vast te leggen uw wijzigingen. De titel, overzicht en module opmerkingen worden opgeslagen bij elke uitvoering van het experiment.
> 
> 

De lijst met experimenten in de **EXPERIMENTEN** tabblad in Machine Learning Studio geeft altijd de nieuwste versie van een experiment. Als u een vorige uitvoering van het experiment wordt geopend (met behulp van **voorafgaande uitvoeren** of **VIEW RUN HISTORY**), kunt u terugkeren naar de conceptversie door te klikken op **VIEW RUN HISTORY** en selecteren van de iteratie waarvoor een **status** van **bewerkbaar**.

## <a name="iterating-on-a-previous-run"></a>Doorlopen van een vorige uitvoering
Wanneer u klikt op **voorafgaande uitvoeren** of **VIEW RUN HISTORY** en opent u een vorige uitvoering, vindt u een experiment is voltooid in de modus alleen-lezen.

Als u wilt om te beginnen met een iteratie van uw experiment beginnen met de manier waarop u deze hebt geconfigureerd voor het uitvoeren van eerdere, kunt u dit doen door te klikken en openen van de uitvoering **SAVE AS**. Hiermee maakt u een nieuw experiment, met een nieuwe titel, een lege geschiedenis, uitvoeren en de onderdelen en parameterwaarden van de vorige uitvoeren. Dit nieuwe experiment wordt vermeld in de **EXPERIMENTEN** tabblad in de startpagina van Machine Learning Studio, en u kunt wijzigen en uitvoeren, starten van een nieuwe uitvoeringsgeschiedenis voor deze herhaling van uw experiment. 

Stel bijvoorbeeld dat u het experiment uitvoeringsgeschiedenis wordt weergegeven in de vorige sectie hebt. U wilt zien wat er gebeurt wanneer u de **leertempo** parameter 0.4 en probeer verschillende waarden voor de **aantal training epoches** parameter.

1. Klik op **VIEW RUN HISTORY** en opent u de herhaling van het experiment dat u hebt uitgevoerd: 28:36:00 uur (waarin u de parameterwaarde instelt op 0.4).
2. Klik op **SAVE AS**.
3. Voer een titel en klik op de **OK** vinkje. Een nieuwe kopie van het experiment wordt gemaakt.
4. Wijzig de **aantal training epoches** parameter.
5. Klik op **uitvoeren**.

U kunt nu met de blijven om te wijzigen en uitvoeren van deze versie van uw experiment, het bouwen van een nieuwe uitvoeringsgeschiedenis om vast te leggen van uw werk.

<!-- Module References -->
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
