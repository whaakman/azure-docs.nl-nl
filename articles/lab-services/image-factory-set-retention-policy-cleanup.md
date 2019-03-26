---
title: De fabrieksinstellingen van een installatiekopie maken in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie-factory in Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 5c1465f31c8b5eb15b6fe63ed61a946e3b32d550
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439851"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>De fabrieksinstellingen van een aangepaste installatiekopie maken in Azure DevTest Labs
In dit artikel bevat informatie over het instellen van een bewaarbeleid, opschonen van de factory en het oude installatiekopieën terug van alle de andere DevTest Labs in de organisatie. 

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u deze artikelen hebt gevolgd voordat u doorgaat:

- [Maak een installatiekopie-factory](image-factory-create.md)
- [Een gegevensfactory installatiekopie uitvoeren vanaf Azure DevOps](image-factory-set-up-devops-lab.md)
- [Aangepaste installatiekopieën opslaan en distribueren naar meerdere labs](image-factory-save-distribute-custom-images.md)

De volgende items moeten al zijn voldaan:

- Een lab voor de factory installatiekopie in Azure DevTest Labs
- Een of meer gericht op Azure DevTest Labs waar de factory golden installatiekopieën wilt distribueren
- Een Azure DevOps-Project wordt gebruikt voor het automatiseren van de factory installatiekopie.
- De locatie van code met de scripts en configuratie (in ons voorbeeld, in het DevOps-Project hierboven hebt gebruikt)
- Een build-definitie voor het indelen van de Azure Powershell-taken
 
## <a name="setting-the-retention-policy"></a>Het bewaarbeleid instellen
Voordat u het opschonen stappen configureert, definieert u het aantal historische installatiekopieën die u wilt behouden in de DevTest Labs. Wanneer u hebt gevolgd de [een factory installatiekopie uitvoeren vanaf Azure DevOps](image-factory-set-up-devops-lab.md) artikel, die u hebt geconfigureerd verschillende variabelen bouwen. Een van beide is **ImageRetention**. U kunt deze variabele instellen op `1`, wat betekent dat de DevTest Labs een geschiedenis van aangepaste installatiekopieën houdt. Alleen de meest recente gedistribueerde afbeeldingen beschikbaar zijn. Als u deze variabele om te wijzigen `2`, de meest recente installatiekopie gedistribueerd plus de vorige die wordt onderhouden. U kunt deze waarde voor het definiëren van het aantal historische afbeeldingen die u wilt behouden in de DevTest Labs kunt instellen.

## <a name="cleaning-up-the-factory"></a>Opschonen van de fabriek
De eerste stap bij het opschonen van de factory is de Gouden installatiekopie van virtuele machines van de factory installatiekopie verwijderen. Er is een script voor deze taak net als onze vorige scripts. De eerste stap is het toevoegen van een andere **Azure Powershell** taak aan de build-definitie, zoals wordt weergegeven in de volgende afbeelding:

![PowerShell-stap](./media/set-retention-policy-cleanup/powershell-step.png)

Zodra u de nieuwe taak in de lijst hebt, selecteert u het item en vul alle gegevens zoals wordt weergegeven in de volgende afbeelding:

![Opschonen van de oude installatiekopieën PowerShell-taak](./media/set-retention-policy-cleanup/configure-powershell-task.png)

De scriptparameters zijn: `-DevTestLabName $(devTestLabName)`.

## <a name="retire-old-images"></a>Oude afbeeldingen buiten gebruik stellen 
Deze taak verwijdert u alle oude installatiekopieën, blijven alleen een geschiedenis die overeenkomt met de **ImageRetention** variabele bouwen. Toevoegen van een extra **Azure Powershell** taak om onze build-definitie te bouwen. Wanneer deze toegevoegd, selecteert u de taak en vul de details, zoals wordt weergegeven in de volgende afbeelding: 

![Oude installatiekopieën PowerShell taak buiten gebruik stellen](./media/set-retention-policy-cleanup/retire-old-image-task.png)

De scriptparameters zijn: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(devTestLabName) -ImagesToSave $(ImageRetention)`

## <a name="queue-the-build"></a>De build-wachtrij
Nu dat u de build-definitie hebt, in de wachtrij een nieuwe build om ervoor te zorgen dat alles werkt. Nadat de build voltooid is de nieuwe aangepaste installatiekopieën te zien in het lab bestemming en als u de installatiekopie factory lab controleren, ziet u geen ingerichte virtuele machines. Als u in de wachtrij plaatsen verdere builds, ziet u bovendien de Opschoningstaken buiten gebruik stellen van oude aangepaste installatiekopieën van de DevTest Labs in overeenstemming aan de waarde voor de bewaarperiode instellen in de build-variabelen.

> [!NOTE]
> Als u de build-pijplijn aan het einde van het laatste artikel hebt uitgevoerd in de reeks, moet u handmatig de virtuele machines die zijn gemaakt in de afbeelding factory testomgeving voordat u een nieuwe build queuing verwijderen.  De stap handmatig opruimen is alleen nodig als we alles instellen en controleren of dat deze werkt.



## <a name="summary"></a>Samenvatting
U hebt nu een actieve installatiekopie factory die u kunt genereren en toewijzen van aangepaste installatiekopieën op uw labs op aanvraag. Op dit punt, het correct ingesteld alleen een kwestie van het ophalen van uw installatiekopieën en identificeren van de doel-labs. Zoals vermeld in het vorige artikel de **Labs.json** bestand zich bevindt in uw **configuratie** map geeft aan welke installatiekopieën beschikbaar zijn in elk van de doel-labs moeten worden gemaakt. Als u andere DevTest Labs aan uw organisatie toevoegt, moet u gewoon een vermelding toevoegen aan de Labs.json voor het nieuwe lab.

Ook is het eenvoudig om een nieuwe installatiekopie toe te voegen aan uw gegevensfactory. Als u wilt opnemen van een nieuwe installatiekopie in de fabriek die u opent de [Azure-portal](https://portal.azure.com), gaat u naar uw gegevensfactory DevTest Labs, selecteer de knop toevoegen van een virtuele machine en kies de gewenste marketplace-installatiekopie en de artefacten. Plaats de **maken** om de nieuwe virtuele machine maken, selecteert u **weergave Azure Resource Manager-sjabloon**' en de sjabloon opslaan als een .json-bestand ergens binnen de **GoldenImages** map in uw opslagplaats. De volgende keer dat u bij het uitvoeren van uw factory-installatiekopie, wordt uw aangepaste installatiekopie gemaakt.


## <a name="next-steps"></a>Volgende stappen
1. [Plannen van uw build en release](/devops/pipelines/build/triggers?view=azure-devops&tabs=designer) de factory installatiekopie periodiek wordt uitgevoerd. Hiermee vernieuwt u uw installatiekopieën factory gegenereerd op gezette tijden.
2. Meer golden afbeeldingen voor uw gegevensfactory maken. U kunt ook overwegen [artefacten maken](devtest-lab-artifact-author.md) extra onderdelen van uw VM-instellingstaken uit een script en de artefacten opnemen in uw factory afbeeldingen.
4. Maak een [scheiden build/release](/devops/pipelines/overview.md?view=azure-devops-2019) om uit te voeren de **DistributeImages** script afzonderlijk. Wanneer u wijzigingen in Labs.json aanbrengen en afbeeldingen die worden gekopieerd naar de doel-labs zonder opnieuw te maken van alle installatiekopieën opnieuw ophalen, kunt u dit script uitvoeren.

