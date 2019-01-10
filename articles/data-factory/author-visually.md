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
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: shlo
ms.openlocfilehash: 323d22363ee52ff6ccf7575b00c6b8b31a0fa156
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188491"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visueel ontwerpen in Azure Data Factory
De Azure Data Factory gebruikersinterface gebruikerservaring (UX) kunt u visueel ontwerpen en resources van uw gegevensfactory implementeren zonder code te schrijven. U kunt slepen van activiteiten op een pijplijncanvas, testuitvoeringen uitvoeren, iteratief, fouten en implementeren en uw pijplijnuitvoeringen controleren. Er zijn twee benaderingen voor het gebruik van de UX om uit te voeren visueel ontwerpen:

- Auteur rechtstreeks met de Data Factory-service.
- Ontwerpen met Azure-opslagplaatsen Git-integratie voor samenwerking, broncodebeheer en versiebeheer.

## <a name="author-directly-with-the-data-factory-service"></a>De auteur van rechtstreeks met de Data Factory-service
Visueel ontwerpen met de Data Factory-service wijkt af van het visuele ontwerp met Git-integratie op twee manieren:

- De Data Factory-service bevat geen een opslagplaats voor het opslaan van de JSON-entiteiten voor uw wijzigingen.
- De Data Factory-service is niet geoptimaliseerd voor samenwerking of versiebeheer.

![De Data Factory-service configureren ](media/author-visually/configure-data-factory.png)

Bij het gebruik van de UX **Ontwerpcanvas** om rechtstreeks met de Data Factory-service, alleen de **Alles publiceren** modus beschikbaar is. Alle wijzigingen die u worden rechtstreeks naar de Data Factory-service gepubliceerd.

![Modus publiceren](media/author-visually/data-factory-publish.png)

## <a name="author-with-azure-repos-git-integration"></a>Ontwerpen met Azure-opslagplaatsen Git-integratie
Visueel ontwerpen met Azure-opslagplaatsen Git-integratie ondersteunt broncodebeheer en samenwerking voor het werk aan uw data factory-pijplijnen. U kunt een data factory koppelen aan een Azure-opslagplaatsen Git-organisatie-opslagplaats voor broncodebeheer, samenwerking, versiebeheer enzovoort. Een enkel Azure-opslagplaatsen Git-organisatie kan meerdere opslagplaatsen hebben, maar een Azure-opslagplaatsen Git-opslagplaats kan worden gekoppeld aan slechts één data factory. Als u een Azure-opslagplaatsen organisatie of een opslagplaats geen hebt, volgt u [deze instructies](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) om uw resources te maken.

> [!NOTE]
> U kunt het taakscript en gegevensbestanden opslaan in een Azure-opslagplaatsen Git-opslagplaats. U moet echter de bestanden handmatig uploaden naar Azure Storage. Een Data Factory-pijplijn uploadt script of gegevens-bestanden die zijn opgeslagen in een Azure-opslagplaatsen Git-opslagplaats naar Azure Storage niet automatisch.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Een Azure-opslagplaatsen Git-opslagplaats met Azure Data Factory configureren
U kunt een Azure-opslagplaatsen Git-opslagplaats met een data factory configureren via twee methoden.

#### <a name="method1"></a> Van configuratiemethode 1 (Azure-opslagplaatsen Git-opslagplaats): Pagina Aan de slag

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Een Azure-opslagplaatsen codeopslagplaats configureren](media/author-visually/configure-repo.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Configureer de instellingen van de opslagplaats code](media/author-visually/repo-settings.png)

Het deelvenster ziet u de volgende code in de Azure-opslagplaatsen instellingen opslagplaats:

| Instelling | Description | Waarde |
|:--- |:--- |:--- |
| **Type opslagplaats** | Het type van de codeopslagplaats Azure-opslagplaatsen.<br/>**Opmerking**: GitHub wordt momenteel niet ondersteund. | Azure-opslagplaatsen Git |
| **Azure Active Directory** | De naam van uw Azure AD-tenant. | <your tenant name> |
| **Organisatie van de Azure-opslagplaatsen** | De naam van uw Azure-opslagplaatsen-organisatie. U vindt de naam van uw Azure-opslagplaatsen organisatie op `https://{organization name}.visualstudio.com`. U kunt [aanmelden bij uw organisatie Azure-opslagplaatsen](https://www.visualstudio.com/team-services/git/) voor toegang tot uw Visual Studio-profiel en -opslagplaatsen en projecten weergegeven. | <your organization name> |
| **ProjectName** | De naam van uw Azure-opslagplaatsen-project. U vindt de naam van uw Azure-opslagplaatsen project op `https://{organization name}.visualstudio.com/{project name}`. | <your Azure Repos project name> |
| **RepositoryName** | De naam van uw Azure-opslagplaatsen code-opslagplaats. Projecten voor Azure opslagplaatsen bevatten de Git-opslagplaatsen voor het beheren van uw broncode als uw project groeit. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw project. | <your Azure Repos code repository name> |
| **Samenwerking vertakking** | Uw Azure-opslagplaatsen samenwerking vertakking die wordt gebruikt voor publicatie. Dit is standaard `master`. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking. | <your collaboration branch name> |
| **Hoofdmap** | De hoofdmap in de vertakking van uw Azure-opslagplaatsen samenwerking. | <your root folder name> |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **canvas voor ontwerpen** in een Azure-opslagplaatsen Git-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |

#### <a name="configuration-method-2-azure-repos-git-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (Azure-opslagplaatsen Git-opslagplaats): UX canvas voor ontwerpen
In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in <a href="#method1">configuratiemethode 1</a>.

![Configureer de instellingen van de opslagplaats code voor het ontwerpen van UX](media/author-visually/configure-repo-2.png)

### <a name="use-a-different-azure-active-directory-tenant"></a>Gebruik een andere Azure Active Directory-tenant

U kunt een Azure-opslagplaatsen Git-repo maken in een andere Azure Active Directory-tenant. Om op te geven van een andere Azure AD-tenant die u hebt beheerdersmachtigingen voor de Azure-abonnement dat u gebruikt.

### <a name="use-your-personal-microsoft-account"></a>Gebruik uw persoonlijke Microsoft-account

Als u wilt gebruiken een persoonlijk Microsoft-account voor Git-integratie, kunt u uw persoonlijke Azure-opslagplaats kunt koppelen aan de Active Directory van uw organisatie.

1. Uw persoonlijke Microsoft-account toevoegen aan Active Directory van uw organisatie als gast. Zie voor meer informatie, [gebruikers toevoegen Azure Active Directory B2B-samenwerking in Azure portal](../active-directory/b2b/add-users-administrator.md).

2. Meld u aan bij Azure portal aan met uw persoonlijke Microsoft-account. Ga vervolgens naar de Active Directory van uw organisatie.

3. Ga naar de sectie Azure DevOps, waar u uw persoonlijke opslagplaats nu zien. Selecteer de opslagplaats en maak verbinding met Active Directory.

Na deze configuratiestappen, uw persoonlijke opslagplaats is beschikbaar bij het instellen van de Git-integratie in de gebruikersinterface van Data Factory.

Zie voor meer informatie over verbinding maken met Azure-opslagplaatsen van uw organisatie Active Directory [verbinding maken met uw organisatie Azure DevOps met Azure Active Directory](/azure/devops/organizations/accounts/connect-organization-to-azure-ad).

### <a name="switch-to-a-different-git-repo"></a>Schakel over naar een andere Git-opslagplaats

Als u wilt overschakelen naar een andere Git-opslagplaats, zoekt u het pictogram in de rechterbovenhoek van de overzichtspagina van Data Factory, zoals wordt weergegeven in de volgende schermafbeelding. Als u het pictogram niet ziet, moet u de cache van uw lokale browser wissen. Selecteer het pictogram op de koppeling met de huidige opslagplaats verwijderen.

Nadat u de koppeling met de huidige opslagplaats hebt verwijderd, kunt u uw Git-instellingen voor het gebruik van een andere opslagplaats kunt configureren. Vervolgens kunt u bestaande resources van Data Factory importeren naar de nieuwe opslagplaats.

![Verwijder de koppeling met de huidige Git-opslagplaats](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Versiebeheer gebruiken
Versiebeheersystemen (ook wel bekend als _broncodebeheer_) kunnen ontwikkelaars samenwerken aan code en wijzigingen bijhouden die worden aangebracht in de code basis. Broncodebeheer is een essentieel hulpprogramma voor ontwikkelaars op meerdere projecten.

Elke Azure-opslagplaatsen Git-opslagplaats die is gekoppeld aan een data factory is een vertakking samenwerking. (`master` de standaardvertakking samenwerking is). Gebruikers kunnen ook functie vertakkingen maken door te klikken op **+ nieuwe vertakking** en ontwikkeling in de functie vertakkingen.

![Wijzig de code door synchroniseren of publiceren](media/author-visually/sync-publish.png)

Wanneer u klaar met het ontwikkelen van functies in uw vertakking functie bent, kunt u klikken op **pull-aanvraag maken**. Deze actie vindt u op Azure-opslagplaatsen Git waar u pull-aanvragen kunt verhogen code, revisies, en wijzigingen samen te voegen aan uw vertakking samenwerking. (`master` is de standaardinstelling). U mag alleen publiceren naar de Data Factory-service vanuit uw vertakking samenwerking. 

![Maak een nieuwe pull-aanvraag](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Publicatie-instellingen configureren

Het configureren van de vertakking publiceren - dat wil zeggen, de vertakking waar de Resource Manager-sjablonen worden opgeslagen - toevoegen een `publish_config.json` bestand naar de hoofdmap van de vertakking samenwerking. Data Factory wordt dit bestand wordt gelezen, zoekt het veld `publishBranch`, en maakt u een nieuwe vertakking (als deze nog niet bestaat) met de opgegeven waarde. Vervolgens worden alle Resource Manager-sjablonen voor de opgegeven locatie opgeslagen. Bijvoorbeeld:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Wanneer u vanuit Git-modus publiceren, kunt u bevestigen dat Data Factory is met behulp van de vertakking publiceren die u verwacht, zoals wordt weergegeven in de volgende schermafbeelding:

![Controleer of de juiste publiceren-vertakking](media/author-visually/configure-publish-branch.png)

Wanneer u een nieuwe vertakking voor publiceren opgeeft, niet de vorige publiceren vertakking door Data Factory worden verwijderd. Als u naar de externe de vertakking van de vorige publiceren wilt, moet u deze handmatig verwijderen.

Data Factory leest alleen de `publish_config.json` wanneer de factory worden geladen. Als u al de fabriek die is geladen in de portal hebt, vernieuw de browser om de wijzigingen van kracht.

### <a name="publish-code-changes"></a>Codewijzigingen publiceren
Nadat u wijzigingen in de vertakking samenwerking zijn samengevoegd (`master` is de standaardinstelling), selecteer **publiceren** handmatig uw codewijzigingen publiceren in de master-vertakking naar de Data Factory-service.

![Wijzigingen publiceren naar de Data Factory-service](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> De master-vertakking heeft geen betrekking op wat wordt geïmplementeerd in de Data Factory-service. De master-vertakking *moet* handmatig worden gepubliceerd naar de Data Factory-service.

### <a name="advantages-of-git-integration"></a>Voordelen van Git-integratie

-   **Besturingselement voor de gegevensbron**. Als uw data factory-workloads cruciaal, wilt u uw factory integreren met Git gebruikmaken van verschillende source control voordelen als volgt uit:
    -   De mogelijkheid om wijzigingen bijhouden/audit.
    -   De mogelijkheid om terug te zetten van wijzigingen die fouten die zijn geïntroduceerd.
-   **Gedeeltelijke slaat**. Wanneer u veel wijzigingen in de fabriek aanbrengt, ontdekt u dat in de normale modus van de LIVE u uw wijzigingen niet opslaan als concept, omdat u niet klaar bent, of u niet dat uw wijzigingen gaan verloren wilt als de computer vastloopt. Met Git-integratie, kunt u doorgaan met het stapsgewijs uw wijzigingen opslaan en publiceren naar de factory, alleen wanneer u klaar bent. GIT fungeert als een tijdelijke locatie voor uw werk, totdat u uw wijzigingen naar uw tevredenheid hebt getest.
-   **Samenwerking en controle**. Als u meerdere teamleden uw deelname aan de dezelfde factory hebt, kunt u kunt uw teamleden die samenwerken met elkaar via het proces voor een code. U kunt ook uw factory instellen dat niet alle bijdragen leveren aan de factory is gemachtigd om te implementeren voor de factory. Teamleden kunnen alleen worden toegestaan aanbrengen via Git, maar alleen bepaalde personen in het team mogen 'Publiceren' de wijzigingen in de fabriek.
-   **Verschillen met**. In de Git-modus, krijgt u om te zien van een mooie diff van de nettolading die draait om te publiceren naar de factory. Deze vergelijking ziet u alle resources/entiteiten die is gewijzigd of toegevoegd/verwijderd sinds de laatste keer dat u gepubliceerd naar uw gegevensfactory. Op basis van deze diff, u kunt doorgaan met publiceren, verder of gaat u terug en wijzigingen controleren en vervolgens het later opnieuw.
-   **CI/CD voor een betere**. Als u Git-modus gebruikt, kunt u de release-pijplijn voor het automatisch wordt geactiveerd zodra er zijn wijzigingen aangebracht in de fabriek dev configureren. Ook krijgt u de eigenschappen in de fabriek die beschikbaar als parameters in de Resource Manager-sjabloon zijn aanpassen. Kan het nuttig zijn om te blijven alleen de vereiste set eigenschappen als parameters en hebt alles wat anders harde code vastgelegd.
-   **Betere prestaties**. Een gemiddelde factory laadt 10 x keer sneller in de Git-modus dan in de normale modus LIVE, omdat de resources worden gedownload via Git.

### <a name="best-practices-for-git-integration"></a>Aanbevolen procedures voor Git-integratie

-   **Machtigingen**. Normaal gesproken wilt u niet dat alle teamleden zijn met machtigingen voor het bijwerken van de factory.
    -   Alle teamleden moeten leesmachtigingen hebben voor de data factory.
    -   Een speciale set gebruikers moet worden toegestaan om te publiceren naar de factory, en voor dat ze moeten deel uitmaken van de rol "Inzender Data Factory" in de fabriek.
    -   Een van de goede procedures die u van broncodebeheer is ook niet toe dat rechtstreeks inchecken in de vertakking samenwerking. Deze vereiste voorkomt u dat fouten wanneer elke check-in niet via een Pull-aanvraag.
-   **Schakelen tussen modi**.
    -    Wanneer u in de Git-modus bent, raden geen we u om over te schakelen en weer in de livemodus, vooral omdat alle wijzigingen die zijn aangebracht in de livemodus, is niet zichtbaar wanneer u gaat u terug naar Git. Probeer de wijzigingen aanbrengen in de Git-modus zelf en ze vervolgens te publiceren via de gebruikersinterface.
    -   Op dezelfde manier gebruik niet een powershell-cmdlets voor Data factory, omdat ze hetzelfde resultaat bereiken door het toepassen van de opgegeven wijzigingen rechtstreeks aan de live factory.
-   **Gebruik wachtwoorden van Azure Key Vault**.
    -   Het is raadzaam AzureKeyVault gebruiken voor het opslaan van alle tekenreeksen voor databaseverbindingen of wachtwoorden met Data Factory, gekoppelde Services.
    -   Slaan we geen dergelijke geheime informatie in Git (om veiligheidsredenen), dus wijzigingen in de gekoppelde Services meteen aan de live factory worden gepubliceerd. Deze direct publiceren soms niet gewenst is, zoals de wijzigingen niet hebt verkregen getest kunnen, die het doel van Git defeats.
    -   Als gevolg hiervan moeten alle deze geheime gegevens worden opgehaald uit gekoppelde Services die gebruikmaken van Azure Key Vault is gebaseerd.
    -   Enkele van de andere voordelen van het gebruik van Key Vault, is dat deze gemakkelijker CICD, door het verstrekken van deze geheime gegevens tijdens de Resource Manager sjabloonimplementatie niet maken.

## <a name="author-with-github-integration"></a>Ontwerpen met GitHub-integratie

Visueel ontwerpen met GitHub-integratie ondersteunt broncodebeheer en samenwerking voor het werk aan uw data factory-pijplijnen. U kunt een data factory koppelen aan een GitHub-account-opslagplaats voor broncodebeheer, samenwerking, versiebeheer. Een enkele GitHub-account kan meerdere opslagplaatsen hebben, maar een GitHub-opslagplaats kan worden gekoppeld aan slechts één data factory. Als u geen een GitHub-account of een opslagplaats hebt, volgt u [deze instructies](https://github.com/join) om uw resources te maken.

De GitHub-integratie met Data Factory ondersteunt zowel openbare GitHub (dat wil zeggen, [ https://github.com ](https://github.com)) en GitHub Enterprise. U kunt zowel openbare als persoonlijke GitHub-opslagplaatsen gebruiken met Data Factory zoals lang u hebt gelezen en schrijftoegang tot de opslagplaats in GitHub.

U moet beheerdersmachtigingen voor de Azure-abonnement dat u gebruikt voor het configureren van een GitHub-opslagplaats.

Bekijk de volgende video voor een 9 minuten durende inleiding en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Beperkingen

- U kunt het taakscript en gegevensbestanden opslaan in een GitHub-opslagplaats. U moet echter de bestanden handmatig uploaden naar Azure Storage. Een Data Factory-pijplijn uploadt script of gegevens-bestanden die zijn opgeslagen in een GitHub-opslagplaats naar Azure Storage niet automatisch.

- GitHub-onderneming met een versie die ouder zijn dan 2.14.0 werkt niet in de browser Microsoft Edge.

- GitHub-integratie met de gegevens van meerdere factoren visual ontwerpgereedschap werkt alleen in de algemeen beschikbare versie van Data Factory.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Configureren van een openbare GitHub-opslagplaats met Azure Data Factory

U kunt een GitHub-opslagplaats met een data factory configureren via twee methoden.

**Van configuratiemethode 1 (openbare opslagplaats): Laten we pagina aan de slag**

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Data Factory aan de slag-pagina](media/author-visually/github-integration-image1.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Instellingen voor GitHub-opslagplaats](media/author-visually/github-integration-image2.png)

Het deelvenster ziet u de volgende code in de Azure-opslagplaatsen instellingen opslagplaats:

| **Instelling**                                              | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Waarde**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Type opslagplaats**                                      | Het type van de codeopslagplaats Azure-opslagplaatsen.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **GitHub-account**                                       | De naam van uw GitHub-account. Deze naam kunt u vinden in https://github.com/{account naam} / {naam van de opslagplaats}. Navigeren naar deze pagina, vraagt u om in te voeren van GitHub OAuth-referenties aan uw GitHub-account.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | De naam van de opslagplaats in de GitHub-code. GitHub-accounts bevatten Git-opslagplaatsen voor het beheren van uw broncode. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw account.                                                                                                                                                                                                                              |                    |
| **Samenwerking vertakking**                                 | De vertakking van uw GitHub-samenwerking die wordt gebruikt voor het publiceren. Standaard is het model. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking.                                                                                                                                                                                                                                                               |                    |
| **Hoofdmap**                                          | De hoofdmap in de vertakking van uw GitHub-samenwerking.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **Ontwerpcanvas** in een GitHub-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |
| **Vertakking voor het importeren van resources in**                       | Hiermee geeft u op in welke vertakking u de data factory-resources (pijplijnen, gegevenssets, gekoppelde services enzovoort) worden geïmporteerd. U kunt resources importeren in een van de volgende opties: een. Samenwerking b. Maak nieuwe c. Bestaande gebruiken                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (openbare opslagplaats): UX canvas voor ontwerpen

In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** Vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in *configuratiemethode 1* hierboven.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Een opslagplaats met GitHub Enterprise configureren met Azure Data Factory

U kunt een opslagplaats met GitHub Enterprise configureren met een data factory via twee methoden.

 #### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Van configuratiemethode 1 (Enterprise-opslagplaats): Pagina Aan de slag

In Azure Data Factory, gaat u naar de **aan de slag** pagina. Selecteer **Codeopslagplaats configureren**:

![Data Factory aan de slag-pagina](media/author-visually/github-integration-image1.png)

De **instellingen opslagplaats** configuratie deelvenster wordt weergegeven:

![Instellingen voor GitHub-opslagplaats](media/author-visually/github-integration-image3.png)

Het deelvenster ziet u de volgende code in de Azure-opslagplaatsen instellingen opslagplaats:

| **Instelling**                                              | **Beschrijving**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Waarde**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Type opslagplaats**                                      | Het type van de codeopslagplaats Azure-opslagplaatsen.                                                                                                                                                                                                                                                                                                                                                                                             | GitHub             |
| **Gebruik GitHub Enterprise**                                | Selectievakje te selecteren van GitHub Enterprise                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **URL voor GitHub Enterprise**                                | De GitHub Enterprise basis-URL. Bijvoorbeeld: https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **GitHub-account**                                       | De naam van uw GitHub-account. Deze naam kunt u vinden in https://github.com/{account naam} / {naam van de opslagplaats}. Navigeren naar deze pagina, vraagt u om in te voeren van GitHub OAuth-referenties aan uw GitHub-account.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | De naam van de opslagplaats in de GitHub-code. GitHub-accounts bevatten Git-opslagplaatsen voor het beheren van uw broncode. U kunt een nieuwe opslagplaats maken of een bestaande opslagplaats, die al in uw account.                                                                                                                                                                                                                              |                    |
| **Samenwerking vertakking**                                 | De vertakking van uw GitHub-samenwerking die wordt gebruikt voor het publiceren. Standaard is het model. Deze instelling wijzigen in het geval u wilt publiceren van bronnen van een andere vertakking.                                                                                                                                                                                                                                                               |                    |
| **Hoofdmap**                                          | De hoofdmap in de vertakking van uw GitHub-samenwerking.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Bestaande resources van Data Factory importeren naar opslagplaats** | Hiermee bepaalt u of bestaande resources van data factory importeren uit de UX **Ontwerpcanvas** in een GitHub-opslagplaats. Schakel het in uw data factory-resources importeren in de bijbehorende Git-opslagplaats in JSON-indeling. Deze actie wordt elke resource afzonderlijk geëxporteerd (dat wil zeggen, de gekoppelde services en gegevenssets zijn geëxporteerd naar afzonderlijke betreffende JSON's). Wanneer dit selectievakje niet is ingeschakeld, wordt de bestaande resources zijn niet geïmporteerd. | Geselecteerde (standaard) |
| **Vertakking voor het importeren van resources in**                       | Hiermee geeft u op in welke vertakking u de data factory-resources (pijplijnen, gegevenssets, gekoppelde services enzovoort) worden geïmporteerd. U kunt resources importeren in een van de volgende opties: een. Samenwerking b. Maak nieuwe c. Bestaande gebruiken                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Configuratiemethode 2 (Enterprise-opslagplaats): UX canvas voor ontwerpen

In de Azure Data Factory-UX **Ontwerpcanvas**, Ga naar uw data factory. Selecteer de **Data Factory** Vervolgkeuzelijst en selecteer vervolgens **Codeopslagplaats configureren**.

Een configuratie-deelvenster wordt weergegeven. Zie voor meer informatie over de configuratie-instellingen, de beschrijvingen in *configuratiemethode 1* hierboven.

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
