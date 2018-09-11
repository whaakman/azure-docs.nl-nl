---
title: Visueel ontwerpen in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van de visual ontwerpen in Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: shlo
ms.openlocfilehash: 8ad587f7aa7aeb5b7176e63b52f6dea8286055a6
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296888"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visueel ontwerpen in Azure Data Factory
De Azure Data Factory gebruikersinterface gebruikerservaring (UX) kunt u visueel ontwerpen en resources van uw gegevensfactory implementeren zonder code te schrijven. U kunt slepen van activiteiten op een pijplijncanvas, testuitvoeringen uitvoeren, iteratief, fouten en implementeren en uw pijplijnuitvoeringen controleren. Er zijn twee benaderingen voor het gebruik van de UX om uit te voeren visueel ontwerpen:

- Auteur rechtstreeks met de Data Factory-service.
- Ontwerpen met Azure DevOps Git-integratie voor samenwerking, broncodebeheer of versiebeheer.

## <a name="author-directly-with-the-data-factory-service"></a>De auteur van rechtstreeks met de Data Factory-service
Visueel ontwerpen met de Data Factory-service wijkt af van visueel ontwerpen met Azure DevOps op twee manieren:

- De Data Factory-service bevat geen een opslagplaats voor het opslaan van de JSON-entiteiten voor uw wijzigingen.
- De Data Factory-service is niet geoptimaliseerd voor samenwerking of versiebeheer.

![De Data Factory-service configureren ](media/author-visually/configure-data-factory.png)

Bij het gebruik van de UX **Ontwerpcanvas** om rechtstreeks met de Data Factory-service, alleen de **Alles publiceren** modus beschikbaar is. Alle wijzigingen die u worden rechtstreeks naar de Data Factory-service gepubliceerd.

![Modus publiceren](media/author-visually/data-factory-publish.png)

## <a name="author-with-azure-devops-git-integration"></a>Ontwerpen met Azure DevOps Git-integratie
Visueel ontwerpen met Azure DevOps Git-integratie ondersteunt broncodebeheer en samenwerking voor het werk aan uw data factory-pijplijnen. U kunt een data factory koppelen aan een opslagplaats van de organisatie Azure DevOps Git voor broncodebeheer, samenwerking, versiebeheer enzovoort. Een enkele Azure DevOps Git-organisatie kan meerdere opslagplaatsen hebben, maar een Azure DevOps Git-opslagplaats kan worden gekoppeld aan slechts één data factory. Als u een Azure DevOps-organisatie of een opslagplaats geen hebt, volgt u [deze instructies](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) om uw resources te maken.

> [!NOTE]
> U kunt het taakscript en gegevensbestanden opslaan in een Azure DevOps Git-opslagplaats. U moet echter de bestanden handmatig uploaden naar Azure Storage. Een Data Factory-pijplijn uploadt script of gegevens-bestanden die zijn opgeslagen in een Azure DevOps Git-opslagplaats naar Azure Storage niet automatisch.

### <a name="configure-an-azure-devops-git-repository-with-azure-data-factory"></a>Een Azure DevOps Git-opslagplaats met Azure Data Factory configureren
U kunt een Azure DevOps Git-opslagplaats met een data factory configureren via twee methoden.

#### <a name="method1"></a> Van configuratiemethode 1 (Azure DevOps Git-opslagplaats): we pagina aan de slag

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Een Azure DevOps-codeopslagplaats configureren](media/author-visually/configure-repo.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Configureer de instellingen van de opslagplaats code](media/author-visually/repo-settings.png)

Het deelvenster ziet u de volgende Azure DevOps-code instellingen opslagplaats:

| Instelling | Beschrijving | Waarde |
|:--- |:--- |:--- |
| **Type opslagplaats** | Het type van de codeopslagplaats Azure DevOps.<br/>**Houd er rekening mee**: GitHub wordt momenteel niet ondersteund. | Azure Dev Ops Git |
| **Azure Active Directory** | De naam van uw Azure AD-tenant. | <your tenant name> |
| **Azure DevOps-organisatie** | De naam van uw Azure DevOps-organisatie. U vindt de naam van uw Azure DevOps-organisatie op `https://{organization name}.visualstudio.com`. U kunt [aanmelden bij uw organisatie Azure DevOps](https://www.visualstudio.com/team-services/git/) voor toegang tot uw Visual Studio-profiel en -opslagplaatsen en projecten weergegeven. | <your organization name> |
| **ProjectName** | De naam van uw Azure DevOps-project. U vindt de naam van uw Azure DevOps-project op `https://{organization name}.visualstudio.com/{project name}`. | <your Azure DevOps project name> |
| **RepositoryName** | De naam van de opslagplaats in de Azure DevOps-code. Met Azure DevOps projects bevatten Git-opslagplaatsen voor het beheren van uw broncode als uw project groeit. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw project. | <your Azure DevOps code repository name> |
| **Samenwerking vertakking** | De vertakking van uw Azure DevOps-samenwerking die wordt gebruikt voor het publiceren. Dit is standaard `master`. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking. | <your collaboration branch name> |
| **Hoofdmap** | De hoofdmap in de vertakking van uw Azure DevOps samenwerking. | <your root folder name> |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **canvas voor ontwerpen** in een Azure DevOps Git-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |

#### <a name="configuration-method-2--azure-devops-git-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (Azure DevOps Git-opslagplaats): UX canvas voor ontwerpen
In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in <a href="#method1">configuratiemethode 1</a>.

![Configureer de instellingen van de opslagplaats code voor het ontwerpen van UX](media/author-visually/configure-repo-2.png)

## <a name="use-a-different-azure-active-directory-tenant"></a>Gebruik een andere Azure Active Directory-tenant

U kunt een Azure DevOps Git-repo maken in een andere Azure Active Directory-tenant. Om op te geven van een andere Azure AD-tenant die u hebt beheerdersmachtigingen voor de Azure-abonnement dat u gebruikt.

## <a name="switch-to-a-different-git-repo"></a>Schakel over naar een andere Git-opslagplaats

Als u wilt overschakelen naar een andere Git-opslagplaats, zoekt u het pictogram in de rechterbovenhoek van de overzichtspagina van Data Factory, zoals wordt weergegeven in de volgende schermafbeelding. Als u het pictogram niet ziet, moet u de cache van uw lokale browser wissen. Selecteer het pictogram op de koppeling met de huidige opslagplaats verwijderen.

Nadat u de koppeling met de huidige opslagplaats hebt verwijderd, kunt u uw Git-instellingen voor het gebruik van een andere opslagplaats kunt configureren. Vervolgens kunt u bestaande resources van Data Factory importeren naar de nieuwe opslagplaats.

![Verwijder de koppeling met de huidige Git-opslagplaats](media/author-visually/remove-repo.png)

## <a name="use-version-control"></a>Versiebeheer gebruiken
Versiebeheersystemen (ook wel bekend als _broncodebeheer_) kunnen ontwikkelaars samenwerken aan code en wijzigingen bijhouden die worden aangebracht in de code basis. Broncodebeheer is een essentieel hulpprogramma voor ontwikkelaars op meerdere projecten.

Elke Azure DevOps Git-opslagplaats die is gekoppeld aan een data factory is een vertakking samenwerking. (`master` de standaardvertakking samenwerking is). Gebruikers kunnen ook functie vertakkingen maken door te klikken op **+ nieuwe vertakking** en ontwikkeling in de functie vertakkingen.

![Wijzig de code door synchroniseren of publiceren](media/author-visually/sync-publish.png)

Wanneer u klaar met het ontwikkelen van functies in uw vertakking functie bent, kunt u klikken op **pull-aanvraag maken**. Deze actie vindt u op Azure DevOps Git waar u pull-aanvragen kunt verhogen code, revisies, en wijzigingen samen te voegen aan uw vertakking samenwerking. (`master` is de standaardinstelling). U mag alleen publiceren naar de Data Factory-service vanuit uw vertakking samenwerking. 

![Maak een nieuwe pull-aanvraag](media/author-visually/create-pull-request.png)

## <a name="publish-code-changes"></a>Codewijzigingen publiceren
Nadat u wijzigingen in de vertakking samenwerking zijn samengevoegd (`master` is de standaardinstelling), selecteer **publiceren** handmatig uw codewijzigingen publiceren in de master-vertakking naar de Data Factory-service.

![Wijzigingen publiceren naar de Data Factory-service](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> De master-vertakking heeft geen betrekking op wat wordt geïmplementeerd in de Data Factory-service. De master-vertakking *moet* handmatig worden gepubliceerd naar de Data Factory-service.

## <a name="author-with-github-integration"></a>Ontwerpen met GitHub-integratie

Visueel ontwerpen met GitHub-integratie ondersteunt broncodebeheer en samenwerking voor het werk aan uw data factory-pijplijnen. U kunt een data factory koppelen aan een GitHub-account-opslagplaats voor broncodebeheer, samenwerking, versiebeheer. Een enkele GitHub-account kan meerdere opslagplaatsen hebben, maar een GitHub-opslagplaats kan worden gekoppeld aan slechts één data factory. Als u geen een GitHub-account of een opslagplaats hebt, volgt u [deze instructies](https://github.com/join) om uw resources te maken. De GitHub-integratie met Data Factory ondersteunt zowel openbare GitHub, evenals GitHub Enterprise.

U moet beheerdersmachtigingen voor de Azure-abonnement dat u gebruikt voor het configureren van een GitHub-opslagplaats.

Bekijk de volgende video voor een 9 minuten durende inleiding en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Beperkingen

- U kunt het taakscript en gegevensbestanden opslaan in een GitHub-opslagplaats. U moet echter de bestanden handmatig uploaden naar Azure Storage. Een Data Factory-pijplijn uploadt script of gegevens-bestanden die zijn opgeslagen in een GitHub-opslagplaats naar Azure Storage niet automatisch.

- GitHub-onderneming met een versie die ouder zijn dan 2.14.0 werkt niet in de browser Microsoft Edge.

- GitHub-integratie met de gegevens van meerdere factoren visual ontwerpgereedschap werkt alleen in de algemeen beschikbare versie van Data Factory.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Configureren van een openbare GitHub-opslagplaats met Azure Data Factory

U kunt een GitHub-opslagplaats met een data factory configureren via twee methoden.

**Van configuratiemethode 1 (openbare opslagplaats): we pagina aan de slag**

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Data Factory aan de slag-pagina](media/author-visually/github-integration-image1.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Instellingen voor GitHub-opslagplaats](media/author-visually/github-integration-image2.png)

Het deelvenster ziet u de volgende Azure DevOps-code instellingen opslagplaats:

| **Instelling**                                              | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Waarde**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Type opslagplaats**                                      | Het type van de codeopslagplaats Azure DevOps.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **GitHub-account**                                       | De naam van uw GitHub-account. Deze naam kunt u vinden in https://github.com/{account naam} / {naam van de opslagplaats}. Navigeren naar deze pagina, vraagt u om in te voeren van GitHub OAuth-referenties aan uw GitHub-account.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | De naam van de opslagplaats in de GitHub-code. GitHub-accounts bevatten Git-opslagplaatsen voor het beheren van uw broncode. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw account.                                                                                                                                                                                                                              |                    |
| **Samenwerking vertakking**                                 | De vertakking van uw GitHub-samenwerking die wordt gebruikt voor het publiceren. Standaard is het model. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking.                                                                                                                                                                                                                                                               |                    |
| **Hoofdmap**                                          | De hoofdmap in de vertakking van uw GitHub-samenwerking.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **canvas voor ontwerpen** in een GitHub-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |
| **Vertakking voor het importeren van resources in**                       | Hiermee geeft u op in welke vertakking u de data factory-resources (pijplijnen, gegevenssets, gekoppelde services enzovoort) worden geïmporteerd. U kunt resources importeren in een van de volgende opties: een. Samenwerking b. Maak nieuwe c. Bestaande gebruiken                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (openbare opslagplaats): UX canvas voor ontwerpen

In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in *configuratiemethode 1* hierboven.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Een opslagplaats met GitHub Enterprise configureren met Azure Data Factory

U kunt een opslagplaats met GitHub Enterprise configureren met een data factory via twee methoden.

 #### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Van configuratiemethode 1 (Enterprise-opslagplaats): we pagina aan de slag

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Data Factory aan de slag-pagina](media/author-visually/github-integration-image1.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Instellingen voor GitHub-opslagplaats](media/author-visually/github-integration-image3.png)

Het deelvenster ziet u de volgende Azure DevOps-code instellingen opslagplaats:

| **Instelling**                                              | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Waarde**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Type opslagplaats**                                      | Het type van de codeopslagplaats Azure DevOps.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Gebruik GitHub Enterprise**                                | Selectievakje te selecteren van GitHub Enterprise                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **URL voor GitHub Enterprise**                                | De GitHub Enterprise basis-URL. Bijvoorbeeld: https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **GitHub-account**                                       | De naam van uw GitHub-account. Deze naam kunt u vinden in https://github.com/{account naam} / {naam van de opslagplaats}. Navigeren naar deze pagina, vraagt u om in te voeren van GitHub OAuth-referenties aan uw GitHub-account.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | De naam van de opslagplaats in de GitHub-code. GitHub-accounts bevatten Git-opslagplaatsen voor het beheren van uw broncode. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw account.                                                                                                                                                                                                                              |                    |
| **Samenwerking vertakking**                                 | De vertakking van uw GitHub-samenwerking die wordt gebruikt voor het publiceren. Standaard is het model. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking.                                                                                                                                                                                                                                                               |                    |
| **Hoofdmap**                                          | De hoofdmap in de vertakking van uw GitHub-samenwerking.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **canvas voor ontwerpen** in een GitHub-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |
| **Vertakking voor het importeren van resources in**                       | Hiermee geeft u op in welke vertakking u de data factory-resources (pijplijnen, gegevenssets, gekoppelde services enzovoort) worden geïmporteerd. U kunt resources importeren in een van de volgende opties: een. Samenwerking b. Maak nieuwe c. Bestaande gebruiken                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (Enterprise-opslagplaats): UX canvas voor ontwerpen

In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in *configuratiemethode 1* hierboven.

## <a name="use-the-expression-language"></a>Gebruik de expressietaal
Expressies voor waarden van eigenschappen kunt u opgeven met behulp van de expressietaal die wordt ondersteund door Azure Data Factory.

Expressies voor waarden van eigenschappen opgeven door te selecteren **dynamische inhoud toevoegen**:

![Gebruik de expressietaal](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Gebruik functies en parameters

U kunt functies gebruiken of Geef parameters op voor pijplijnen en gegevenssets in de Data Factory **opbouwfunctie voor expressies**:

Zie voor meer informatie over de ondersteunde expressies [expressies en functies in Azure Data Factory](control-flow-expression-language-functions.md).

![Dynamische inhoud toevoegen](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Feedback geven
Selecteer **Feedback** opmerkingen over de functies of Microsoft wil een melding over problemen met het hulpprogramma:

![Feedback](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het controleren en beheren van pijplijnen [bewaken en beheren van pijplijnen programmatisch](monitor-programmatically.md).
