---
title: Visual ontwerpen in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van visual ontwerpen in Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: bce74c80f53eda654bb0289a1ff8a6cb88fd13f5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visual ontwerpen in Azure Data Factory
De Azure Data Factory gebruikersinterface ervaring (UX) kunt u visueel ontwerpen en implementeren van resources voor uw data factory zonder code te schrijven. U kunt activiteiten naar een pipeline-canvas te slepen, testruns uitvoeren, iteratief, foutopsporing en implementeren en controleren de pijplijn wordt uitgevoerd. Er zijn twee methoden voor het gebruik van de UX voor het uitvoeren van de visual ontwerpen:

- Auteur rechtstreeks met de Data Factory-service.
- Auteur met Visual Studio Team Services (VSTS) Git-integratie voor samenwerking, broncodebeheer of versiebeheer.

## <a name="author-directly-with-the-data-factory-service"></a>Auteur rechtstreeks met de Data Factory-service
Visual ontwerpen met de Data Factory-service verschilt van het visual ontwerpen met VSTS op twee manieren:

- De Data Factory-service omvat een opslagplaats voor het opslaan van de JSON-entiteiten voor uw wijzigingen niet.
- De Data Factory-service is niet geoptimaliseerd voor samenwerking of versiebeheer.

![De Data Factory-service configureren ](media/author-visually/configure-data-factory.png)

Wanneer u de UX gebruikt **Authoring canvas** schrijven rechtstreeks met de Data Factory-service, alleen de **publiceren** modus is beschikbaar. Alle wijzigingen die u aanbrengt, worden gepubliceerd rechtstreeks naar de Data Factory-service.

![Modus publiceren](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Auteur met VSTS Git-integratie
Visual ontwerpen met VSTS Git-integratie ondersteunt broncodebeheer en samenwerking voor de data factory-pijplijnen. U kunt een gegevensfactory koppelen aan een VSTS Git account-opslagplaats voor broncodebeheer, samenwerking en versiebeheer. Een enkel VSTS Git-account kan meerdere opslagplaatsen hebben, maar een VSTS Git-opslagplaats kan worden gekoppeld aan slechts één gegevensfactory. Als u geen een VSTS account of een opslagplaats, voert u de [deze instructies](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) om uw resources te maken.

> [!NOTE]
> U kunt de script-en gegevensbestanden opslaan in een VSTS GIT-opslagplaats. U moet echter de bestanden handmatig uploaden naar Azure Storage. Of het script bestanden opgeslagen in een VSTS GIT-opslagplaats naar Azure Storage niet automatisch wordt geüpload door een Data Factory-pijplijn.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Configureer een VSTS Git-opslagplaats met Azure Data Factory
U kunt een VSTS GIT-opslagplaats met een data factory configureren via twee methoden.

<a name="method1"></a>
#### <a name="configuration-method-1-lets-get-started-page"></a>Van configuratiemethode 1: gaan we aan de slag-pagina
In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Code opslagplaats configureren**:

![Een code-opslagplaats VSTS configureren](media/author-visually/configure-repo.png)

De **opslagplaats instellingen** deelvenster configuratie wordt weergegeven:

![Configureer de instellingen van de opslagplaats code](media/author-visually/repo-settings.png)

Het deelvenster ziet u de volgende VSTS code opslagplaats instellingen:

| Instelling | Beschrijving | Waarde |
|:--- |:--- |:--- |
| **Type van de opslagplaats** | Het type van de opslagplaats VSTS-code.<br/>**Opmerking**: GitHub wordt momenteel niet ondersteund. | Visual Studio Team Services Git |
| **Visual Studio Team Services Account** | De naam van uw VSTS. U kunt vinden op de naam van uw VSTS `https://{account name}.visualstudio.com`. U kunt [aanmelden bij uw account VSTS](https://www.visualstudio.com/team-services/git/) voor toegang tot uw Visual Studio-profiel en uw opslagplaatsen en projecten weergegeven. | \<de accountnaam van uw > |
| **ProjectName** | De naam van uw VSTS-project. U vindt uw VSTS projectnaam `https://{account name}.visualstudio.com/{project name}`. | \<de naam van uw project VSTS > |
| **RepositoryName** | De naam van uw VSTS code-opslagplaats. VSTS projecten bevatten Git-opslagplaatsen voor het beheren van uw broncode wanneer uw project groeit. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats die al in uw project, gebruiken. | \<de naam van uw VSTS code opslagplaats > |
| **Bestaande resources van de Data Factory naar bibliotheek importeren** | Hiermee wordt aangegeven of bestaande data factory resources importeren vanuit de UX **Authoring canvas** in een VSTS Git-opslagplaats. Schakel het importeren van uw data factory-resources in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie exporteert elke resource afzonderlijk (dat wil zeggen, de gekoppelde services en gegevenssets worden geëxporteerd naar afzonderlijke JSONs). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Van configuratiemethode 2: UX canvas ontwerpen
In de Azure Data Factory-UX **Authoring canvas**, zoek uw gegevensfactory. Selecteer de **Data Factory** vervolgkeuzelijst en selecteer vervolgens **Code opslagplaats configureren**.

Een deelvenster configuratie wordt weergegeven. Voor meer informatie over de configuratie-instellingen, Zie de beschrijvingen in <a href="#method1">configuratiemethode 1</a>.

![Configureer de instellingen van de opslagplaats code voor het ontwerpen van UX](media/author-visually/configure-repo-2.png)

### <a name="use-version-control"></a>Versiebeheer gebruiken
Versie besturingssystemen (ook wel bekend als _bronbeheer_), kunnen ontwikkelaars samenwerken aan code en bijhouden van de wijzigingen die zijn aangebracht aan de code base. Verbinding met broncodebeheer is een belangrijk hulpprogramma voor meerdere developer-projecten.

Elke VSTS Git-opslagplaats die is gekoppeld aan een gegevensfactory heeft een hoofdvertakking. Wanneer u toegang tot een VSTS Git-opslagplaats hebt, kunt u de code wijzigen door te kiezen **Sync** of **publiceren**:

![De code wijzigen door synchroniseren of publiceren](media/author-visually/sync-publish.png)

#### <a name="sync-code-changes"></a>Synchronisatie codewijzigingen
Nadat u hebt geselecteerd **Sync**, kunt u wijzigingen in de hoofdvertakking naar uw lokale vertakking pull of push wordt gewijzigd van uw lokale vertakking naar de hoofdvertakking.

![Synchronisatie codewijzigingen](media/author-visually/sync-change.png)

#### <a name="publish-code-changes"></a>Codewijzigingen publiceren
Selecteer **publiceren** handmatig publiceren van de codewijzigingen in de hoofdvertakking naar de Data Factory-service.

> [!IMPORTANT]
> De hoofdvertakking heeft geen betrekking op wat wordt geïmplementeerd in de Data Factory-service. De hoofdvertakking *moet* handmatig naar de Data Factory-service worden uitgegeven.

## <a name="use-the-expression-language"></a>De expressietaal gebruiken
U kunt expressies voor eigenschapswaarden opgeven met behulp van de expressietaal die wordt ondersteund door Azure Data Factory. Zie voor meer informatie over de ondersteunde expressies [expressies en functies in Azure Data Factory](control-flow-expression-language-functions.md).

Expressies voor eigenschapswaarden opgeven met behulp van de UX **canvas ontwerpen**:

![De expressietaal gebruiken](media/author-visually/expression-language.png)

## <a name="specify-parameters"></a>Geef parameters op
U kunt de parameters voor pijplijnen en gegevenssets opgeven in de Azure Data Factory **Parameters** tabblad. U kunt eenvoudig de parameters in de eigenschappen van gebruiken door te selecteren **dynamische inhoud toevoegen**:

![Dynamische inhoud toevoegen](media/author-visually/dynamic-content.png)

U kunt bestaande parameters gebruiken of nieuwe parameters opgeven voor de eigenschapswaarden:

![Geef parameters op voor waarden van eigenschappen](media/author-visually/parameters.png)

## <a name="provide-feedback"></a>Feedback geven
Selecteer **Feedback** opmerkingen hebt over functies of naar Microsoft sturen over problemen met het hulpprogramma:

![Feedback](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het bewaken en beheren van pijplijnen [bewaken en beheren van pijplijnen programmatisch](monitor-programmatically.md).
