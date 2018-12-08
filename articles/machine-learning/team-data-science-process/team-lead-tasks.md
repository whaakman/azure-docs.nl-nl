---
title: Taken voor het team leiden in het Team Data Science Process Team - Azure
description: Een overzicht van de taken die ertoe leiden dat een team in een data science-teamproject is opslagbewerkingen uit te voeren voor hun team van gegevenswetenschappers.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, "(previous author=deguhath, ms.author=deguhath)"
ms.openlocfilehash: 68fd0abf314eb4fde5ff335acc161d82ad78cd0c
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106631"
---
# <a name="tasks-for-the-team-lead-in-the-team-data-science-process-team"></a>Taken voor het team leiden in Team Data Science Process Team

Dit onderwerp bevat de taken die een teamleider opslagbewerkingen uit te voeren voor hun team van gegevenswetenschappers. Het doel is het opzetten van collaboratief team-omgeving die standaardiseert op de [Team Data Science Process](overview.md) (TDSP). TDSP is een flexibele, iteratieve data science-methodologie predictive analytics-oplossingen en intelligente toepassingen efficiënt leveren. Het is ontworpen om samenwerking en team learning te verbeteren. Het proces is een distillatie van de best practices en structuren van zowel Microsoft en uit de industrie, die nodig zijn voor een succesvolle implementatie van data science initiatieven om bedrijven volledig te profiteren van de voordelen van hun analytics programma's. Voor een overzicht van de rollen personeel en de bijbehorende taken die worden beheerd door een team van gegevenswetenschappers standaardiseren over dit proces, Zie [Team Data Science Process rollen en taken](roles-tasks.md).

Een **teamleider** beheert een team in de data science-eenheid van een onderneming. Een team bestaat uit meerdere gegevenswetenschappers. Voor data science-eenheid met slechts een beperkt aantal gegevenswetenschappers, de **groepsbeheerder** en de **teamleider** mogelijk dezelfde persoon of dat ze hun taak aan een vervanging kunnen delegeren. Maar de taken zelf niet te wijzigen. De werkstroom voor de taken worden uitgevoerd door het team leidt tot het instellen van deze omgeving worden weergegeven in de volgende afbeelding:

![1](./media/team-lead-tasks/team-leads-1-creating-teams.png)

>[AZURE.NOTE] De taken in blokken van 1 en 2 van de afbeelding zijn nodig als u Azure DevOps gebruikt als het platform voor het hosten van code en als u wilt een afzonderlijke Azure DevOps-project voor uw eigen team. Zodra deze taken zijn voltooid, kunnen alle opslagplaatsen van uw team onder dit project worden gemaakt. 

Na enkele vereisten taken die zijn opgegeven in de volgende sectie wordt voldaan door de manager van de groep, er worden de vijf belangrijkste taken (sommige optioneel) die u in deze zelfstudie hebt voltooid. Deze taken komen overeen met de belangrijkste genummerde secties van dit onderwerp:

1. Maak een **project** op van de groep Azure DevOps-Services van de groep en twee team opslagplaatsen in het project:
    - **ProjectTemplate opslagplaats** 
    - **TeamUtilities opslagplaats**
2. Het team seed **ProjectTemplate** opslagplaats van de **GroupProjectTemplate** -opslagplaats die is ingesteld door de manager van uw groep. 
3. Team data en analyse-resources maken:
    - Toevoegen van het team-specifieke hulpprogramma's voor de **TeamUtilities** opslagplaats. 
    - (Optioneel) Maak een **Azure bestandsopslag** moet worden gebruikt voor het opslaan van de gegevensassets die nuttig voor het hele team zijn kunnen. 
4. (Optioneel) Koppelen van de Azure file storage om de **Data Science Virtual Machine** (DSVM) van het team te leiden en gegevensassets toevoegen op het.
5. Instellen van de **beveiligingscontrole** door toe te voegen leden van een team en zijn bevoegdheden te configureren.

>[AZURE.NOTE] We een overzicht van de stappen die nodig zijn voor het instellen van een TDSP-team-omgeving met behulp van Azure DevOps in de volgende instructies. We geven over het uitvoeren van deze taken met Azure DevOps vanwege de manier waarop we TDSP bij Microsoft implementeert. Als een andere code die als host fungeert platform wordt gebruikt voor uw groep, worden de taken die moeten worden uitgevoerd door de teamleider in het algemeen niet wijzigen. Maar de manier om deze taken uit te voeren afwijken.

## <a name="repositories-and-directories"></a>Opslagplaatsen en mappen

In dit onderwerp maakt gebruik van verkorte namen voor opslagplaatsen en mappen. Deze namen maken het gemakkelijker zijn te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notatie (**R** voor Git-opslagplaatsen en **D** voor lokale mappen op uw DSVM) wordt gebruikt in de volgende secties:

- **R1**: de **GroupProjectTemplate** -opslagplaats in Git die de manager van de groep die is ingesteld op de server van uw Azure DevOps-groep.
- **R3**: het team **ProjectTemplate** -opslagplaats in Git die u instelt.
- **R4**: de **TeamUtilities** -opslagplaats in Git die u instelt.
- **D1**: de lokale map gekloond vanuit R1 en gekopieerd naar D3.
- **D3**: de lokale map gekloond vanuit R3, aanpassen en worden gekopieerd naar R3.
- **D4**: de lokale map gekloond vanuit R4, aanpassen en naar R4 gekopieerd.

De namen opgegeven voor de opslagplaatsen en mappen in deze zelfstudie is te vinden op de veronderstelling dat het doel is het opzetten van een ander project voor uw eigen team binnen een grotere data science-groep. Maar er zijn andere opties voor u als teamleider openen:

- De hele groep kunt maken van een enkel project. Vervolgens zou alle projecten in alle teams van data science worden onder dit één project. Om dit te doen, kunt u een git-beheerder om te volgen van deze instructies voor het maken van een enkel project aanwijzen. In dit scenario is mogelijk geldig is, bijvoorbeeld voor:
    -  een kleine data science-groep die beschikt niet over meerdere data science-teams 
    -  een grotere data science groep met meerdere data-scienceteams die niettemin wil de samenwerking tussen team met activiteiten, zoals op het niveau sprint plannen te optimaliseren. 
- Teams kunnen ervoor kiezen teamspecifiek projectsjablonen of team-specifieke hulpprogramma's onder het één project voor de hele groep. In dit geval moeten de leads team project sjabloon opslagplaatsen en/of opslagplaatsen voor team-hulpprogramma's onder hetzelfde project maken. Naam van de volgende opslagplaatsen *< TeamName\>ProjectTemplate* en *< TeamName\>hulpprogramma's voor*, bijvoorbeeld *TeamJohnProjectTemplate*en *TeamJohnUtilities*. 

Team leads moeten in elk geval laat de teamleden weten welke sjabloon en hulpprogramma's voor opslagplaatsen vast te stellen wanneer ze zijn instellen en de opslagplaatsen project en hulpprogramma's voor het klonen. Project leads diende de [Project leiden taken voor een team van gegevenswetenschappers](project-lead-tasks.md) te maken project-opslagplaatsen, of onder de verschillende projecten of een enkel project. 


## <a name="0-prerequisites"></a>0. Vereisten

De vereisten wordt voldaan door het uitvoeren van de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md). Om samen te vatten hier, moeten de volgende vereisten voldoen voordat u begint met het team lead taken: 

- Uw **groep Azure DevOps Services** (of groepsaccount op sommige andere code die als host fungeert platform) is ingesteld door de manager van uw groep.
- Uw **GroupProjectTemplate opslagplaats** (R1) is ingesteld op uw groepsaccount door de groepmanager van uw op de code die als host fungeert platform dat u van plan bent te gebruiken.
- U zijn **geautoriseerd** op uw groepsaccount voor het maken van opslagplaatsen voor uw team.
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een Data Science Virtual Machine (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te gaan. Raadpleeg anders de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op uw computer geïnstalleerd. In het README.md-bestand, schuif omlaag naar de **Download en installeer** sectie en klikt u op de *nieuwste installatieprogramma van*. Hiermee gaat u naar de pagina van de meest recente installatieprogramma. Het .exe-installatieprogramma hier downloaden en uitvoeren. 
- Als u **Linux-DSVM**, het maken van een openbare SSH-sleutel op uw DSVM en toe te voegen aan uw groep Azure DevOps-Services. Zie voor meer informatie over SSH, de **openbare maken van SSH-sleutel** sectie de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix). 
    
## <a name="1-create-a-project-and-repositories"></a>1. Maken van een project en -opslagplaatsen

Voer deze stap als u Azure DevOps als de code die als host fungeert platform voor versiebeheer en samenwerking. In deze sectie heeft u drie artefacten in het Azure DevOps-Services van uw groep maken:

- **MyTeam** -project in Azure DevOps
- **MyProjectTemplate** opslagplaats (**R3**) van Git
- **MyTeamUtilities** opslagplaats (**R4**) van Git

### <a name="create-the-myteam-project"></a>Het MyTeam-project maken

- Ga naar de startpagina van Azure DevOps-Services van uw groep op de URL `https://<Azure DevOps Services Name\>.visualstudio.com`. 
- Klik op **nieuw** om een project te maken. 

    ![2](./media/team-lead-tasks/team-leads-2-create-new-team.png)

- Een venster van het project maken, vraagt u de naam van het Project (**MyTeam** in dit voorbeeld). Zorg ervoor dat u selecteert **Agile** als de **processjabloon** en **Git** als de **versiebeheer**. 

    ![3](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)

- Klik op **project maken**. Uw project **MyTeam** wordt gemaakt in minder dan 1 minuut. 

- Nadat het project **MyTeam** is gemaakt, klikt u op **navigeren naar project** knop, om te worden omgeleid naar de startpagina van uw project. 

    ![4](./media/team-lead-tasks/team-leads-4-create-new-team-3.png)

- Als u ziet een **Gefeliciteerd!** pop-upvenster, klikt u op de **code toevoegen** (knop in met een rood kader). Klik anders op **Code** (in geel vak). Dit verwijst u naar de pagina van de Git-opslagplaats van uw project. 

    ![5](./media/team-lead-tasks/team-leads-5-team-project-home.png)

### <a name="create-the-myprojecttemplate-repository-r3-on-git"></a>Maken van de opslagplaats MyProjectTemplate (R3) van Git

- Klik op de pijl-omlaag naast de naam van de opslagplaats op de pagina Git-opslagplaats van uw project **MyTeam**, en selecteer **opslagplaatsen beheren...** .

    ![6](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)

- Op de **versiebeheer** tabblad van het Configuratiescherm van uw project, klikt u op **MyTeam**en selecteer vervolgens **opslagplaats wijzigen...** . 

    ![7](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)

- Voer een nieuwe naam naar de opslagplaats in de **Wijzig de naam van de opslagplaats MyTeam** venster. In dit voorbeeld *MyTeamProjectTemplate*. U kunt er ongeveer als *< Teamnaam van uw\>ProjectTemplate*. Klik op **naam** om door te gaan.

    ![8](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)

### <a name="create-the-myteamutilities-repository-r4-on-git"></a>Maken van de opslagplaats MyTeamUtilities (R4) van Git

- Het maken van een nieuwe opslagplaats *< Teamnaam van uw\>hulpprogramma's voor* onder uw project, klikt u op **nieuwe opslagplaats...**  op de **versiebeheer** tabblad van het Configuratiescherm van uw project.  

    ![9](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)

- In de **maken van een nieuwe opslagplaats** venster dat verschijnt, Geef een naam op voor deze opslagplaats. In dit voorbeeld, noem we deze *MyTeamUtilities*, die is **R4** in onze-notatie. Kies er ongeveer als *< Teamnaam van uw\>hulpprogramma's voor*. Zorg ervoor dat u selecteert **Git** voor **Type**. Klik vervolgens op **maken** om door te gaan.

    ![10](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)

- Bevestig dat u de twee nieuwe Git-opslagplaatsen gemaakt op basis van uw project **MyTeam**. In dit voorbeeld: 

- **MyTeamProjectTemplate** (R3) 
- **MyTeamUtilities** (R4).

    ![11](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)


## <a name="2-seed-your-projecttemplate-and-teamutilities-repositories"></a>2. Uw opslagplaatsen ProjectTemplate en TeamUtilities seed

De seeding procedure gebruikt de mappen op uw lokale DSVM als tussenliggende staging-sites. Als u nodig hebt om aan te passen uw **ProjectTemplate** en **TeamUtilities** opslagplaatsen om te voldoen aan bepaalde specifieke team nodig heeft, u dit doen in de voorlaatste stap van de volgende procedure. Hier volgt een samenvatting van de stappen voor het seeden van de inhoud van de **MyTeamProjectTemplate** en **MyTeamUtilities** opslagplaatsen voor een team van gegevenswetenschappers. De afzonderlijke stappen komen overeen met de subsecties in de seeding procedure:

- Groep kloonopslagplaats naar lokale map: R1 - gekloond voor lokale D1 ->-team
- Uw team opslagplaatsen klonen naar lokale mappen: team R3 & R4 - gekloond voor lokale D3 & D4 ->
- De inhoud van de groep project sjabloon kopiëren naar de map lokale team: D1 - inhoud wordt gekopieerd om D3 ->
- (Optioneel) aanpassing van de lokale D3 & D4
- Inhoud van de lokale directory naar team opslagplaatsen pushen: D3 & D4 - inhoud toevoegen -> team R3 & R4


### <a name="initialize-the-team-repositories"></a>Initialiseren van de opslagplaatsen team

In deze stap maakt initialiseren u uw project-opslagplaats voor sjablonen van de sjabloonopslagplaats groep project:

- **MyTeamProjectTemplate** opslagplaats (**R3**) van uw **GroupProjectTemplate** (**R1**) opslagplaats


### <a name="clone-group-repositories-into-local-directories"></a>Groep opslagplaatsen klonen naar lokale mappen

Om te beginnen met deze procedure:

- Mappen maken op uw lokale computer:
    - Voor **Windows**: **C:\GitRepos\GroupCommon** en **C:\GitRepos\MyTeam**
    - Voor **Linux**: **GitRepos\GroupCommon** en **GitRepos\MyTeam** op de oorspronkelijke directory 
- Ga naar map **GitRepos\GroupCommon**.
- Voer de volgende opdracht, waar nodig, op het besturingssysteem van de lokale computer.

**Windows**

    git clone https://<Your Azure DevOps Services name>.visualstudio.com/GroupCommon/_git/GroupProjectTemplate
    

![12](./media/team-lead-tasks/team-leads-12-create-two-group-repos.png)

**Linux**
    
    git clone ssh://<Your Azure DevOps Services name>@<Your Azure DevOps Services name>.visualstudio.com:22/GroupCommon/_git/GroupProjectTemplate
    
    
![13](./media/team-lead-tasks/team-leads-13-clone_two_group_repos_linux.png)

Klonen van deze opdrachten uw **GroupProjectTemplate** (R1)-opslagplaats voor uw groep Azure DevOps-Services naar een lokale map in **GitRepos\GroupCommon** op uw lokale computer. Na het kopiëren van de directory **GroupProjectTemplate** (D1) wordt gemaakt in de directory **GitRepos\GroupCommon**. Hier, nemen we aan dat de manager van uw groep een project gemaakt **GroupCommon**, en de **GroupProjectTemplate** opslagplaats onder dit project is. 


### <a name="clone-your-team-repositories-into-local-directories"></a>Uw team opslagplaatsen klonen naar lokale mappen

Klonen van deze opdrachten uw **MyTeamProjectTemplate** (R3) en **MyTeamUtilities** (R4) opslagplaatsen onder uw project **MyTeam** voor uw groep Azure DevOps-Services naar de **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4)-mappen in **GitRepos\MyTeam** op uw lokale computer. 

- Ga naar map **GitRepos\MyTeam**
- Voer de volgende opdrachten, waar nodig, op het besturingssysteem van de lokale computer. 

**Windows**

    git clone https://<Your Azure DevOps Services name>.visualstudio.com/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone https://<Your Azure DevOps Services name>.visualstudio.com/<Your Team Name>/_git/MyTeamUtilities

![14](./media/team-lead-tasks/team-leads-14-clone_two_empty_team_repos.png)
        
**Linux**
    
    git clone ssh://<Your Azure DevOps Services name>@<Your Azure DevOps Services name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone ssh://<Your Azure DevOps Services name>@<Your Azure DevOps Services name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamUtilities
    
![15](./media/team-lead-tasks/team-leads-15-clone_two_empty_team_repos_linux.png)

Na het kopiëren van de twee directory's **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4) worden gemaakt in map **GitRepos\MyTeam**. We hier hebben aangenomen dat u de naam van uw project sjabloon en hulpprogramma's voor opslagplaatsen **MyTeamProjectTemplate** en **MyTeamUtilities**. 

### <a name="copy-the-group-project-template-content-to-the-local-project-template-directory"></a>De inhoud van de groep project sjabloon kopiëren naar de lokale projectmap sjabloon

Om te kopiëren van de inhoud van de lokale **GroupProjectTemplate** (D1) map aan de lokale **MyTeamProjectTemplate** (D3), voer een van de volgende shell-scripts: 

#### <a name="from-the-powershell-command-line-for-windows"></a>Vanaf de opdrachtregel voor Windows PowerShell       

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 2

    
![16](./media/team-lead-tasks/team-leads-16-local_copy_team_lead_new.png)

#### <a name="from-the-linux-shell-for-the-linux-dsvm"></a>Vanuit de Linux-shell voor het **Linux-DSVM**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 2
    
![17](./media/team-lead-tasks/team-leads-17-local-copy-team-lead-linux-new.png)

De scripts uitsluiten de inhoud van de map .git. De scripts vraagt u om aan te bieden de **voltooien paden** naar de bronmap D1 en naar de doelmap D3.
        

### <a name="customize-your-project-template-or-team-utilities-optional"></a>Aanpassen van uw project-sjabloon of het team-hulpprogramma's (optioneel)

Pas uw **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4), indien nodig, in deze fase van het installatieproces. 

- Als u aanpassen van de inhoud van D3 wilt om te voldoen aan de specifieke behoeften van uw team, kunt u de sjabloon voor documenten wijzigen of wijzigen van de mapstructuur.

- Als uw team heeft enkele hulpprogramma's die u wilt delen met uw hele team ontwikkeld, kopieer en plak deze hulpprogramma's in directory D4. 


### <a name="push-local-directory-content-to-team-repositories"></a>Inhoud van de lokale directory naar team opslagplaatsen pushen

De inhoud in de (eventueel aangepaste) lokale mappen D3 en D4 toevoegen aan het team opslagplaatsen R3 en R4, voer de volgende git bash-opdrachten van een Windows PowerShell-console of van de Linux-shell. Voer de opdrachten van de **GitRepos\MyTeam\MyTeamProjectTemplate** directory.

    git status
    git add .
    git commit -m"push from DSVM"
    git push
    
![18](./media/team-lead-tasks/team-leads-18-push-to-group-server-2.png)

De bestanden in de opslagplaats MyTeamProjectTemplate van van uw groep Azure DevOps-Services worden bijna ogenblikkelijk gesynchroniseerd wanneer dit script wordt uitgevoerd.

![19](./media/team-lead-tasks/team-leads-19-push-to-group-server-showed-up.png)

Voer nu de dezelfde set vier git-opdrachten uit de **GitRepos\MyTeam\MyTeamUtilities** directory. 

> [AZURE.NOTE]Als dit de eerste keer dat u door te op een Git-opslagplaats voeren, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten uit:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Als u gegevens worden doorgevoerd op meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres wanneer u aan elk van deze. Met dezelfde naam en e-mailadres bewijst handige later op wanneer u Power BI-dashboards voor het bijhouden van uw Git-activiteiten op meerdere opslagplaatsen kunt bouwen.

![20](./media/team-lead-tasks/team-leads-20-git-config-name.png)


## <a name="3-create-team-data-and-analytics-resources-optional"></a>3. Team data en analyse-resources (optioneel) maken

Gegevens en analyses resources te delen met uw hele team heeft voordelen voor prestaties en kosten: teamleden kunnen hun projecten uitvoeren op de gedeelde bronnen, opslaan op budgetten en efficiënter samen te werken. In deze sectie krijgt u instructies voor het maken van Azure file storage. In de volgende sectie krijgt u instructies over hoe u Azure file storage koppelen met uw lokale machine. Zie voor meer informatie over het delen van andere bronnen, zoals Azure Data Science Virtual Machines, Azure HDInsight Spark-Clusters, [platformen en hulpprogramma's](platforms-and-tools.md). In dit onderwerp vindt u richtlijnen vanuit het oogpunt van data science voor resources die geschikt voor uw behoeften zijn te selecteren, en koppelingen naar de product-pagina's en andere relevante en nuttige zelfstudies die we hebben gepubliceerd.

>[AZURE.NOTE] Om te voorkomen dat gegevens overdragen van meerdere datacenters, die mogelijk traag en kostbaar, zorg ervoor dat de resourcegroep, opslagaccount en de Azure-VM (bijvoorbeeld DSVM) in de dezelfde Azure-Datacenter. 

Voer de volgende scripts voor het maken van Azure file storage voor uw team. Azure file storage voor uw team kan worden gebruikt voor het opslaan van de gegevensassets die nuttig voor uw hele team zijn. U wordt gevraagd door de scripts voor uw Azure-account en abonnement gegevens, hebt u dus deze referenties kunt invoeren. 

### <a name="create-azure-file-storage-with-powershell-from-windows"></a>Azure-bestandsopslag te maken met PowerShell in Windows

Voer dit script uit vanuit de PowerShell opdrachtregel:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
    .\CreateFileShare.ps1

![21](./media/team-lead-tasks/team-leads-21-create-fileshare-win.png)   

Meld u aan bij uw Microsoft Azure-account wanneer u hierom wordt gevraagd:

![22](./media/team-lead-tasks/team-leads-22-file-create-s1.png)

Selecteer het Azure-abonnement dat u wilt gebruiken:

![23](./media/team-lead-tasks/team-leads-23-file-create-s2.png)

Selecteer welk opslagaccount wilt gebruiken of maak een nieuw abonnement onder het geselecteerde abonnement:

![24](./media/team-lead-tasks/team-leads-24-file-create-s3.png)

Voer de naam van de Azure file storage om te maken. Alleen lager geval letters, cijfers en - worden geaccepteerd:

![25](./media/team-lead-tasks/team-leads-25-file-create-s4.png)

De gegevens van de Azure file storage in een tekstbestand opslaan om te kunnen koppelen, en het delen van deze opslag nadat deze is gemaakt, en noteer het pad naar de locatie. In het bijzonder, moet u dit bestand te koppelen van uw Azure file storage om uw Azure virtual machines in de volgende sectie. 

Het is raadzaam om te controleren in dit bestand in uw opslagplaats ProjectTemplate. Het is raadzaam om te plaatsen in de map **Docs\DataDictionaries**. Daarom is deze gegevensasset toegankelijk door alle projecten in uw team. 

![26](./media/team-lead-tasks/team-leads-26-file-create-s5.png)


### <a name="create-azure-file-storage-with-a-linux-script"></a>Azure-bestandsopslag met een Linux-script maken

Voer dit script uit vanuit de Linux-shell:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
    bash CreateFileShare.sh

Meld u aan bij uw Microsoft Azure-account de instructies in dit scherm te volgen:

![27](./media/team-lead-tasks/team-leads-27-file-create-linux-s1.png)

Selecteer het Azure-abonnement dat u wilt gebruiken:

![28](./media/team-lead-tasks/team-leads-28-file-create-linux-s2.png)

Selecteer welk opslagaccount wilt gebruiken of maak een nieuw abonnement onder het geselecteerde abonnement:

![29](./media/team-lead-tasks/team-leads-29-file-create-linux-s3.png)

Voer de naam van de Azure-bestandsopslag te maken, alleen kleine letters, cijfers en - worden geaccepteerd:

![30](./media/team-lead-tasks/team-leads-30-file-create-linux-s4.png)

De gegevens van de Azure file storage in een tekstbestand opslaan om te kunnen verkrijgen van toegang tot deze opslag nadat deze is gemaakt, en noteer het pad naar de locatie. In het bijzonder, moet u dit bestand te koppelen van uw Azure file storage om uw Azure virtual machines in de volgende sectie.

Het is raadzaam om te controleren in dit bestand in uw opslagplaats ProjectTemplate. Het is raadzaam om te plaatsen in de map **Docs\DataDictionaries**. Daarom is deze gegevensasset toegankelijk door alle projecten in uw team. 

![31](./media/team-lead-tasks/team-leads-31-file-create-linux-s5.png)


## <a name="4-mount-azure-file-storage-optional"></a>4. Azure file storage koppelen (optioneel)

Nadat Azure file storage is gemaakt, kan deze worden gekoppeld aan uw lokale machine met behulp van een van de volgende PowerShell- of Linux-scripts.

### <a name="mount-azure-file-storage-with-powershell-from-windows"></a>Azure file storage koppelen met PowerShell in Windows

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
    .\AttachFileShare.ps1
    
U wordt gevraagd om u te melden bij de eerste, als u niet bent aangemeld. 

Klik op **Enter** of **y** om door te gaan wanneer u wordt gevraagd als u een Azure storage bestandsgegevens hebt-bestand en voer vervolgens de ***volledig pad en naam** van het bestand dat u maakt in vorige stap. De informatie om te koppelen van een Azure-bestandsopslag wordt gelezen rechtstreeks via bestands- en u bent klaar om te gaan met de volgende stap.

![32](./media/team-lead-tasks/team-leads-32-attach-s1.png)

> [AZURE.NOTE] Als u een bestand met de Azure file storage-gegevens hebt, vindt u de stappen voor het invoeren van de gegevens van toetsenbord aan het einde van deze sectie.

U wordt vervolgens gevraagd de naam van het station moeten worden toegevoegd aan uw virtuele machine op te geven. Een lijst met namen van bestaande schijf wordt afgedrukt op het scherm. U moet een stationsnaam opgeeft die nog niet in de lijst bestaat.

![33](./media/team-lead-tasks/team-leads-33-attach-s2.png)

Bevestig dat een nieuw F-station is op uw computer is gekoppeld.

![34](./media/team-lead-tasks/team-leads-34-attach-s3.png)

**Hoe u de Azure file storage-gegevens handmatig invoeren:** als u uw Azure file storage-gegevens niet in een tekstbestand hebt, kunt u de instructies volgen op het volgende scherm te typen in de vereiste abonnement, de storage-account en Azure informatie over de opslag van bestand:

![35](./media/team-lead-tasks/team-leads-35-attach-s4.png)

Typ uw Azure-abonnement, selecteer het opslagaccount waarin de Azure file storage wordt gemaakt, en typt u de naam van het Azure storage:

![36](./media/team-lead-tasks/team-leads-36-attach-s5.png)

### <a name="mount-azure-file-storage-with-a-linux-script"></a>Azure file storage koppelen met een Linux-script

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
    bash AttachFileShare.sh

![37](./media/team-lead-tasks/team-leads-37-attach-s1-linux.png)

U wordt gevraagd om u te melden bij de eerste, als u niet bent aangemeld. 

Klik op **Enter** of **y** om door te gaan wanneer u wordt gevraagd als u een Azure storage bestandsgegevens hebt-bestand en voer vervolgens de ***volledig pad en naam** van het bestand dat u maakt in vorige stap. De informatie om te koppelen van een Azure-bestandsopslag wordt gelezen rechtstreeks via bestands- en u bent klaar om te gaan met de volgende stap.

![38](./media/team-lead-tasks/team-leads-38-attach-s2-linux.png)

U wordt vervolgens gevraagd de naam van het station moeten worden toegevoegd aan uw virtuele machine op te geven. Een lijst met namen van bestaande schijf wordt afgedrukt op het scherm. U moet een stationsnaam opgeeft die nog niet in de lijst bestaat.

![39](./media/team-lead-tasks/team-leads-39-attach-s3-linux.png)

Bevestig dat een nieuw F-station is op uw computer is gekoppeld.

![40](./media/team-lead-tasks/team-leads-40-attach-s4-linux.png)

**Hoe u de Azure file storage-gegevens handmatig invoeren:** als u uw Azure file storage-gegevens niet in een tekstbestand hebt, kunt u de instructies volgen op het volgende scherm te typen in de vereiste abonnement, de storage-account en Azure informatie over de opslag van bestand:

- Invoer **n**.
- Selecteer de index van de naam van het abonnement waarin de Azure file storage is gemaakt in de vorige stap:

    ![41](./media/team-lead-tasks/team-leads-41-attach-s5-linux.png)

- Selecteer het opslagaccount in uw abonnement en het type in de naam van het Azure storage:

    ![42](./media/team-lead-tasks/team-leads-42-attach-s6-linux.png)

- Voer de naam van het station moeten worden toegevoegd aan uw computer, wat verschillen van alle bestaande moet:

    ![43](./media/team-lead-tasks/team-leads-43-attach-s7-linux.png)


## <a name="5-set-up-security-control-policy"></a>5. Beheer van beveiligingsbeleid instellen 

Startpagina van uw groep Azure DevOps Services, klik op de **tandwielpictogram** naast de naam van de gebruiker in de rechterbovenhoek, selecteert u vervolgens de **Security** tabblad. U kunt leden toevoegen aan uw team met verschillende machtigingen.

![44](./media/team-lead-tasks/team-leads-44-add-team-members.png)

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de functies en taken die zijn gedefinieerd door het Team Data Science Process:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md)
- [Afzonderlijke inzenders project voor een team van gegevenswetenschappers](project-ic-tasks.md)