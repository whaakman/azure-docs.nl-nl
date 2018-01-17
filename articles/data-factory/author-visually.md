---
title: Azure data factory's visueel ontwerpen | Microsoft Docs
description: Meer informatie over het Azure Data Factory visueel ontwerpen
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Gegevensfactory's visueel auteur
Gebruikers kunnen met de ervaring met Azure Data Factory UX visueel ontwerpen en implementeren van resources in de gegevensfactory zonder één regel code schrijven. Deze interface zonder code kunt u slepen en neerzetten van activiteiten op het canvas van een pijplijn, testruns uitvoeren, iteratief, foutopsporing en implementeren en bewaken van de pijplijn wordt uitgevoerd. U kunt het hulpprogramma ADF UX op twee manieren gebruiken:

1. Werk rechtstreeks met de Data Factory-service
2. VSTS Git-integratie configureren voor samenwerking, broncodebeheer of versiebeheer

## <a name="authoring-with-data-factory"></a>Met Data Factory ontwerpen
De eerste optie is authoring rechtstreeks met Gegevensfactory-modus. Deze benadering verschilt van authoring via VSTS Code opslagplaats in dat er is geen opslagplaats de JSON-entiteiten van de wijzigingen opslaan en is deze geoptimaliseerd voor samenwerking of versiebeheer niet.

![Data Factory configureren](media/author-visually/configure-data-factory.png)

In de Data Factory-modus, moet u er alleen de modus 'Publiceren' is. Eventuele wijzigingen worden gepubliceerd rechtstreeks op de Data Factory-service.

![Data Factory publiceren](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>Authoring met VSTS Git-integratie
Kan ontwerpen met VSTS Git-integratie voor broncodebeheer en samenwerking tijdens het ontwerpen van uw data factory-pijplijnen. Gebruikers hebben de optie om een gegevensfactory koppelen aan een VSTS Git Account opslagplaats voor broncodebeheer, samenwerking en versiebeheer enzovoort. Een enkel VSTS GIT-account kan meerdere opslagplaatsen hebben. Een VSTS Git-opslagplaats kan echter alleen worden gekoppeld met een enkele data factory. Als u nog geen een VSTS account en de opslagplaats hebt, maakt u een [hier](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student).

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>VSTS Git-opslagplaats met Azure Data Factory configureren
Gebruikers kunnen een VSTS GIT-opslagplaats met een data factory configureren via twee methoden.

#### <a name="method-1-lets-get-started-page"></a>Methode 1: 'aan de slag' pagina

Ga naar de pagina 'Aan de slag' en 'Code opslagplaats configureren' op

![Code-opslagplaats configureren](media/author-visually/configure-repo.png)

Van daaruit is een kant-paneel wordt weergegeven voor het configureren van de instellingen van de opslagplaats.

![Opslagplaats instellingen configureren](media/author-visually/repo-settings.png)
* **Type van de opslagplaats**: Visual Studio Team Services Git (momenteel Github wordt niet ondersteund.)
* **Visual Studio Team Services-Account**: de accountnaam vindt u uit de naam van de https://{account}. visualstudio.com. Aanmelden bij uw account VSTS [hier](https://www.visualstudio.com/team-services/git/) en toegang tot uw Visual Studio-profiel om uw opslagplaatsen en projecten te bekijken
* **ProjectName:** de projectnaam vindt u uit de naam van de name}.visualstudio.com/{project https://{account}
* **RepositoryName:** de naam van de opslagplaats. VSTS projecten bevatten Git-opslagplaatsen voor het beheren van uw broncode wanneer uw project groeit. Maak een nieuwe opslagplaats of een bestaande opslagplaats al in het project, gebruiken.
* **Bestaande resources van de Data Factory naar bibliotheek importeren**: dit selectievakje inschakelt, kunt u uw huidige data factory-resources gemaakt op het canvas UX naar de bijbehorende VSTS GIT-opslagplaats in JSON-indeling importeren. Deze actie exporteert elke resource afzonderlijk (dat wil zeggen, gekoppelde services en gegevenssets worden geëxporteerd naar afzonderlijke JSONs).    Als u dit selectievakje uitschakelt, wordt de bestaande resources zijn niet geïmporteerd in de Git-opslagplaats.

#### <a name="method-2-from-authoring-canvas"></a>Methode 2: Van het Canvas ontwerpen

Klik op de vervolgkeuzelijst 'Data Factory' onder de naam van uw gegevensfactory in het 'Authoring canvas'. Klik vervolgens op 'Configureren Code opslagplaats.' Net als bij **methode 1**, een kant-paneel wordt weergegeven voor het configureren van de instellingen van de opslagplaats. Zie de vorige secties voor informatie over de instellingen.

![Code-opslagplaats 2 configureren](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Versiebeheer
Versiebeheer, ook wel aangeduid aan als broncodebeheer, systemen kunnen ontwikkelaars samenwerken aan code en bijhouden wijzigingen aangebracht in de code base. Verbinding met broncodebeheer is een belangrijk hulpprogramma voor meerdere developer-projecten.

Elke VSTS Git-opslagplaats die is gekoppeld aan een gegevensfactory heeft een hoofdvertakking. Van daaruit elke gebruiker die toegang tot de VSTS Git-opslagplaats heeft heeft twee opties wanneer u wijzigingen aanbrengt: synchroniseren en te publiceren.

![Synchronisatie publiceren](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Sync

Nadat u op 'sync', kunt u wijzigingen van de hoofdvertakking in uw lokale vertakking pull of pushen wijzigingen vanaf uw lokale vertakking naar de hoofdvertakking.

![Wijzigingen worden gesynchroniseerd](media/author-visually/sync-change.png)

#### <a name="publish"></a>Publiceren
 Wijzigingen in de hoofdvertakking voor Data Factory-service gepubliceerd.

> [!NOTE]
> De **hoofdvertakking heeft geen betrekking op wat wordt geïmplementeerd in de Data Factory-service.** De hoofdvertakking *moet* handmatig naar de Data Factory-service worden uitgegeven.




## <a name="expression-language"></a>Expressietaal

Gebruikers kunnen expressies opgeven in eigenschapswaarden definiëren met behulp van de expressietaal wordt ondersteund door Azure Data Factory. Zie [expressies en functies in Azure Data Factory](control-flow-expression-language-functions.md) voor meer expressies waarover worden ondersteund.

Expressies in de eigenschapwaarden in de UX opgeven als volgt te werk.

![Expressietaal](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parameters
Gebruikers kunnen voor pijplijnen en gegevenssets heeft, op het tabblad 'Parameters' parameters opgeven. Bovendien gebruikmaken van parameters in de eigenschappen van eenvoudig door drukken 'Dynamische inhoud toevoegen'.

![Dynamische inhoud](media/author-visually/dynamic-content.png)

Daar kunt u gebruikmaken van een bestaande parameter of geef een nieuwe parameter in de waarde van de eigenschap.

![Parameters](media/author-visually/parameters.png)

## <a name="feedback"></a>Feedback
Klik op het pictogram 'Feedback' om feedback te (Microsoft) op verschillende functies of eventuele problemen die kunnen worden aangesloten.

![Feedback](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het controleren en beheren van pijplijnen, [bewaken en beheren van pijplijnen programmatisch](monitor-programmatically.md) artikel
