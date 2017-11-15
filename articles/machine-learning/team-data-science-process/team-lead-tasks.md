---
title: Gegevens wetenschap proces Team leiden taken - Azure in een team | Microsoft Docs
description: Een overzicht van de taken voor een team leiden op een project gegevens wetenschappelijke team.
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: bradsev;
ms.openlocfilehash: 58e8bfeb547fbcd7f472fbed50073b1ca3176b72
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="team-lead-tasks"></a>Team Lead taken

Dit onderwerp die worden beschreven de taken die een team lead verwacht om voor hun wetenschappelijke Gegevensteam te voltooien. Het doel is het opzetten van samenwerking teamomgeving die standaardiseert op de [Team gegevens wetenschap proces](overview.md) (TDSP). TDSP is een flexibele, herhalende gegevens wetenschappelijke methodologie predictive analytics-oplossingen en intelligente toepassingen efficiënt te leveren. Het is ontworpen om samenwerking en team learning te verbeteren. Het proces is een distillatie van de aanbevolen procedures en structuren uit zowel Microsoft als de industrie, die nodig zijn voor een geslaagde implementatie van gegevens wetenschappelijke initiatieven ondernemingen te helpen volledig realiseren de voordelen van hun analytics programma's. Voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een wetenschappelijke Gegevensteam standaardiseren op dit proces Zie [Team gegevens wetenschap proces rollen en taken](roles-tasks.md).

Een **Team leiden** beheert een team in de wetenschap gegevenseenheid van een onderneming. Een team bestaat uit meerdere gegevenswetenschappers. Voor wetenschap gegevenseenheid met een klein aantal gegevenswetenschappers, de **groepsbeheerder** en de **Team leiden** mogelijk dezelfde persoon of ze kunnen hun taak delegeren aan een vervanging. Maar de taken zelf niet wijzigen. De werkstroom voor de taken worden uitgevoerd door het team leidt tot het instellen van deze omgeving zijn afgebeeld in de volgende afbeelding:

![1](./media/team-lead-tasks/team-leads-1-creating-teams.png)

>[AZURE.NOTE] De taken in blokken 1 en 2 van de afbeelding zijn nodig als u Visual Studio Team Services (VSTS) gebruikt als de code die als host fungeert platform en u wilt een afzonderlijk team-project voor uw team. Zodra deze taken zijn voltooid, kunnen alle opslagplaatsen van uw team onder dit teamproject worden gemaakt. 

Na enkele vereisten taken die zijn opgegeven in een volgende sectie wordt voldaan door de manager van de groep, er zijn vijf principal taken (enkele optioneel) die u in deze zelfstudie hebt voltooid. Deze taken overeenkomen met de belangrijkste genummerde secties van dit onderwerp:

1. Maak een **teamproject** op van de groep VSTS server van de groep en twee team opslagplaatsen in het project:
    - **ProjectTemplate opslagplaats** 
    - **TeamUtilities opslagplaats**
2. Het team seed **ProjectTemplate** opslagplaats van de **GroupProjectTemplate** opslagplaats die is ingesteld door het beheer van uw groep. 
3. Resources voor gegevens en analyse van het team maken:
    - Toevoegen van het team-specifieke hulpprogramma's voor de **TeamUtilities** opslagplaats. 
    - (Optioneel) Maak een **Azure file storage** moet worden gebruikt voor het opslaan van gegevensassets die nuttig is voor het hele team kunnen zijn. 
4. (Optioneel) Koppel de bestandsopslag van de Azure-naar de **gegevens wetenschappelijke virtuele Machine** (DSVM) van het team te leiden en gegevensassets toevoegen op het.
5. Instellen van de **beveiligingscontrole** door teamleden toe te voegen en hun bevoegdheden configureren.

>[AZURE.NOTE] Overzicht van de stappen die nodig zijn voor het instellen van een omgeving met TDSP team met VSTS in de volgende instructies. We opgeven hoe u deze taken met VSTS omdat hoe we TDSP bij Microsoft implementeren. Als een andere code platform die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de lead team in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende.

## <a name="repositories-and-directories"></a>-Opslagplaatsen en mappen

In dit onderwerp maakt gebruik van afkortingen voor opslagplaatsen en mappen. Deze namen gemakkelijker te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notation (**R** voor Git-opslagplaatsen en **D** voor lokale mappen op uw DSVM) wordt gebruikt in de volgende secties:

- **R1**: de **GroupProjectTemplate** opslagplaats op Git die uw groepsbeheerder ingesteld op de server van de groep VSTS.
- **R3**: het team **ProjectTemplate** opslagplaats op Git die u instelt.
- **R4**: de **TeamUtilities** opslagplaats op Git die u instelt.
- **D1**: de lokale map gekloond van R1 en gekopieerd naar D3.
- **D3**: de lokale map gekloond van R3, aanpassen en naar R3 gekopieerd.
- **D4**: de lokale map gekloond van R4, aanpassen en naar R4 gekopieerd.

De namen opgegeven voor de opslagplaatsen en mappen in deze zelfstudie zijn opgegeven op de veronderstelling dat het doel is tot stand brengen van een afzonderlijk team-project voor uw team in een grotere gegevens wetenschappelijke groep. Maar er zijn andere opties voor u als team lead openen:

- De hele groep kunt maken van een teamproject één. Vervolgens zijn alle projecten van alle gegevens wetenschappelijke teams onder dit teamproject één. Om dit te bereiken, kunt u een beheerder git Volg deze instructies voor het maken van een enkele teamproject aanwijzen. Dit scenario mogelijk geldig is, bijvoorbeeld voor:
    -  een kleine hoeveelheden gegevens wetenschap-groep die geen meerdere gegevens wetenschappelijke teams 
    -  een grotere gegevens wetenschappelijke groep met meerdere gegevens wetenschappelijke teams die niettemin wil tussen teamsamenwerking met activiteiten zoals groepeerniveau sprint planning optimaliseren. 
- Teams kunnen kiezen om team-specifieke projectsjablonen of team-specifieke hulpprogramma's onder het één teamproject voor de hele groep. In dit geval moeten de leads team team project sjabloon opslagplaatsen en/of team hulpprogramma's voor opslagplaatsen onder het dezelfde teamproject maken. Naam van deze opslagplaatsen *< TeamName\>ProjectTemplate* en *< TeamName\>hulpprogramma's*, bijvoorbeeld *TeamJohnProjectTemplate*en *TeamJohnUtilities*. 

Team leads moeten in elk geval laat de teamleden weten welke sjabloon en hulpprogramma's voor opslagplaatsen vast te stellen wanneer ze zijn instellen en de opslagplaatsen project en hulpprogramma's voor het klonen. Project leads moeten volgen de [Project leiden taken voor een wetenschappelijke Gegevensteam](project-lead-tasks.md) maken project-opslagplaatsen of onder de afzonderlijke teamprojecten of een enkel teamproject. 


## <a name="0-prerequisites"></a>0. Vereisten

De vereisten wordt voldaan door de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md). Om samen te vatten hier, moeten de volgende vereisten voldoen voordat u begint met het team lead taken: 

- Uw **groep VSTS server** (of groepsaccount op andere code hosting-platform) is ingesteld door het beheer van uw groep.
- Uw **GroupProjectTemplate opslagplaats** (R1) is ingesteld op uw groepsaccount door uw groepmanager over de code die als host fungeert platform dat u wilt gebruiken.
- U zijn **geautoriseerd** voor je groepsaccount opslagplaatsen voor uw team maken.
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een virtuele Machine wetenschappelijke gegevens (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te beginnen. Raadpleeg anders de [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git referentie Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op deze computer geïnstalleerd. In het bestand README.md, schuif omlaag naar de **downloaden en installeren** sectie en klik op de *meest recente versie installer*. Hiermee gaat u naar de laatste pagina van het installatieprogramma. Het installatieprogramma .exe hier downloaden en uitvoeren. 
- Als u **Linux DSVM**, een openbare SSH-sleutel op uw DSVM maken en toe te voegen aan uw groep VSTS-server. Zie voor meer informatie over SSH de **openbare maken SSH-sleutel** sectie het [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix). 
    
## <a name="1-create-a-team-project-and-repositories"></a>1. Maak een teamproject en opslagplaatsen

Deze stap uitvoeren als u VSTS als uw code hosting-platform voor versiebeheer en samenwerking. Deze sectie heeft u drie artefacten maken in de server VSTS van uw groep:

- **MyTeam** -project in VSTS
- **MyProjectTemplate** opslagplaats (**R3**) op Git
- **MyTeamUtilities** opslagplaats (**R4**) op Git

### <a name="create-the-myteam-project"></a>Het MyTeam-project maken

- Ga naar de startpagina van uw groep VSTS server op URL `https://<VSTS Server Name\>.visualstudio.com`. 
- Klik op **nieuw** voor het maken van een teamproject. 

    ![2](./media/team-lead-tasks/team-leads-2-create-new-team.png)

- Een venster maken team project vraagt u voor het invoeren van de projectnaam (**MyTeam** in dit voorbeeld). Zorg ervoor dat u selecteert **Agile** als de **processjabloon** en **Git** als de **versiebeheer**. 

    ![3](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)

- Klik op **project maken**. Uw teamproject **MyTeam** wordt gemaakt in minder dan 1 minuut. 

- Na het teamproject **MyTeam** is gemaakt, klikt u op **navigeren naar project** knop om te worden omgeleid naar de startpagina van uw teamproject. 

    ![4](./media/team-lead-tasks/team-leads-4-create-new-team-3.png)

- Als u ziet een **Gefeliciteerd!** pop-upvenster, klikt u op de **code toevoegen** (knop in rood kader). Klik anders op **Code** (in geel vak). Dit wordt u verwezen naar de pagina van de Git-opslagplaats van uw teamproject. 

    ![5](./media/team-lead-tasks/team-leads-5-team-project-home.png)

### <a name="create-the-myprojecttemplate-repository-r3-on-git"></a>De opslagplaats MyProjectTemplate (R3) op Git maken

- Klik op de pijl-omlaag naast de naam van de opslagplaats op de pagina Git-opslagplaats van uw teamproject **MyTeam**, en selecteer **opslagplaatsen beheren...** .

    ![6](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)

- Op de **versiebeheer** tabblad van het Configuratiescherm van uw teamproject, klikt u op **MyTeam**, selecteer daarna **Rename-opslagplaats...** . 

    ![7](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)

- Voer een nieuwe naam voor de opslagplaats in de **Wijzig de naam van de opslagplaats MyTeam** venster. In dit voorbeeld *MyTeamProjectTemplate*. U kunt ongeveer *< Teamnaam van uw\>ProjectTemplate*. Klik op **naam** om door te gaan.

    ![8](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)

### <a name="create-the-myteamutilities-repository-r4-on-git"></a>De opslagplaats MyTeamUtilities (R4) op Git maken

- Maken van een nieuwe opslagplaats *< Teamnaam van uw\>hulpprogramma's* onder uw teamproject, klikt u op **nieuwe opslagplaats...**  op de **versiebeheer** tabblad van uw teamproject het Configuratiescherm.  

    ![9](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)

- In de **maken van een nieuwe opslagplaats** venster dat wordt weergegeven, Geef een naam op voor deze opslagplaats. In dit voorbeeld wij namen aan als *MyTeamUtilities*, namelijk **R4** in onze-notatie. Kies ongeveer *< Teamnaam van uw\>hulpprogramma's*. Zorg ervoor dat u selecteert **Git** voor **Type**. Klik vervolgens op **maken** om door te gaan.

    ![10](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)

- Controleer of u de twee nieuwe Git-opslagplaatsen zijn gemaakt voor uw teamproject **MyTeam**. In dit voorbeeld: 

- **MyTeamProjectTemplate** (R3) 
- **MyTeamUtilities** (R4).

    ![11](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)


## <a name="2-seed-your-team-projecttemplate-and-teamutilities-repositories"></a>2. Uw team ProjectTemplate en TeamUtilities opslagplaatsen seed

De seeding procedure gebruikt de mappen op uw lokale DSVM als tussenliggende staging sites. Als u nodig hebt voor het aanpassen van uw **ProjectTemplate** en **TeamUtilities** opslagplaatsen om te voldoen aan enkele specifieke team nodig heeft, u dit doen in de voorlaatste stap van de volgende procedure. Hier volgt een samenvatting van de stappen die wordt gebruikt bij de seeding van de inhoud van de **MyTeamProjectTemplate** en **MyTeamUtilities** opslagplaatsen voor een wetenschappelijke Gegevensteam. De afzonderlijke stappen overeenkomen met de subsecties in de seeding procedure:

- Kloon groep opslagplaats naar de lokale map: R1 - gekloond om lokale D1 -> in een team
- Klonen van uw team-opslagplaatsen naar lokale mappen: R3 & R4 - voor -> lokale D3 & D4 gekloond in een team
- De inhoud van de groep project sjabloon kopiëren naar de map lokale team: D1 - inhoud wordt gekopieerd om D3 ->
- (Optioneel) aanpassing van de lokale D3 & D4
- Push-inhoud van de lokale directory naar team opslagplaatsen: D3 & D4 - inhoud toevoegen -> R3 & R4 in een team


### <a name="initialize-the-team-repositories"></a>Initialiseren van de opslagplaatsen team

In deze stap maakt initialiseren u uw opslagplaats team project sjabloon uit de opslagplaats voor groep project sjabloon:

- **MyTeamProjectTemplate** opslagplaats (**R3**) van uw **GroupProjectTemplate** (**R1**) opslagplaats


### <a name="clone-group-repositories-into-local-directories"></a>Kloon groep opslagplaatsen naar lokale mappen

Om te beginnen met deze procedure:

- Mappen maken op uw lokale machine:
    - Voor **Windows**: **C:\GitRepos\GroupCommon** en **C:\GitRepos\MyTeam**
    - Voor **Linux**: **GitRepos\GroupCommon** en **GitRepos\MyTeam** op uw basismap 
- Ga naar map **GitRepos\GroupCommon**.
- Voer de volgende opdracht, afhankelijk van het besturingssysteem van de lokale computer.

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/GroupCommon/_git/GroupProjectTemplate
    

![12](./media/team-lead-tasks/team-leads-12-create-two-group-repos.png)

**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/GroupCommon/_git/GroupProjectTemplate
    
    
![13](./media/team-lead-tasks/team-leads-13-clone_two_group_repos_linux.png)

Deze opdrachten klonen uw **GroupProjectTemplate** (R1)-bibliotheek op de server van uw groep VSTS naar een lokale map in **GitRepos\GroupCommon** op uw lokale machine. Na het kopiëren van de directory **GroupProjectTemplate** (D1) wordt gemaakt in directory **GitRepos\GroupCommon**. Hier, gaan we ervan uit dat de manager van uw groep een teamproject gemaakt **GroupCommon**, en de **GroupProjectTemplate** -opslagplaats is onder dit teamproject. 


### <a name="clone-your-team-repositories-into-local-directories"></a>Klonen van uw team-opslagplaatsen naar lokale mappen

Deze opdrachten klonen uw **MyTeamProjectTemplate** (R3) en **MyTeamUtilities** (R4)-opslagplaatsen onder uw teamproject **MyTeam** op uw server groep VSTS naar de  **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4) mappen in **GitRepos\MyTeam** op uw lokale machine. 

- Ga naar map **GitRepos\MyTeam**
- Voer de volgende opdrachten, afhankelijk van het besturingssysteem van de lokale computer. 

**Windows**

    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone https://<Your VSTS Server name>.visualstudio.com/<Your Team Name>/_git/MyTeamUtilities

![14](./media/team-lead-tasks/team-leads-14-clone_two_empty_team_repos.png)
        
**Linux**
    
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamProjectTemplate
    git clone ssh://<Your VSTS Server name>@<Your VSTS Server name>.visualstudio.com:22/<Your Team Name>/_git/MyTeamUtilities
    
![15](./media/team-lead-tasks/team-leads-15-clone_two_empty_team_repos_linux.png)

Na het kopiëren van de twee directory's **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4) worden gemaakt in de directory **GitRepos\MyTeam**. We hebben een veronderstelde hier waarvan u de naam uw teamproject sjabloon en hulpprogramma's voor opslagplaatsen **MyTeamProjectTemplate** en **MyTeamUtilities**. 

### <a name="copy-the-group-project-template-content-to-the-local-team-project-template-directory"></a>De inhoud van de groep project sjabloon kopiëren naar de lokale team sjabloon projectmap

De inhoud van de lokale kopieert **GroupProjectTemplate** (D1) map aan de lokale **MyTeamProjectTemplate** (D3), voer een van de volgende shellscripts: 

####<a name="from-the-powershell-command-line-for-windows"></a>Vanaf de opdrachtregel voor Windows PowerShell        

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 2

    
![16](./media/team-lead-tasks/team-leads-16-local_copy_team_lead_new.png)

####<a name="from-the-linux-shell-for-the-linux-dsvm"></a>Vanuit de Linux-shell voor de **Linux DSVM**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 2
    
![17](./media/team-lead-tasks/team-leads-17-local-copy-team-lead-linux-new.png)

De scripts uitsluiten de inhoud van de map .git. De scripts wordt u gevraagd te bieden de **voltooien paden** naar de bronmap D1 en naar de doelmap D3.
        

### <a name="customize-your-team-project-template-or-team-utilities-optional"></a>Aanpassen van uw team projectsjabloon of team hulpprogramma's (optioneel)

Aanpassen van uw **MyTeamProjectTemplate** (D3) en **MyTeamUtilities** (D4), indien nodig, in deze fase van het installatieproces. 

- Als u aanpassen van de inhoud van D3 om te voldoen aan de specifieke behoeften van uw team wilt, kunt u de sjabloon documenten wijzigen of wijzigen van de mapstructuur.

- Als uw team heeft een aantal hulpprogramma's die u wilt delen met uw hele team ontwikkeld, kopieert en plakt u deze hulpprogramma's in de directory D4. 


### <a name="push-local-directory-content-to-team-repositories"></a>Push-inhoud van de lokale directory naar team opslagplaatsen

Als u wilt de inhoud in de (eventueel aangepaste) lokale mappen D3 en D4 toevoegen aan de opslagplaatsen team R3 en R4, voer de volgende git bash opdrachten van een Windows PowerShell-console of van de Linux-shell. Voer de opdrachten van de **GitRepos\MyTeam\MyTeamProjectTemplate** directory.

    git status
    git add .
    git commit -m"push from DSVM"
    git push
    
![18](./media/team-lead-tasks/team-leads-18-push-to-group-server-2.png)

De bestanden in de opslagplaats MyTeamProjectTemplate van uw groep VSTS server worden bijna onmiddellijk gesynchroniseerd wanneer dit script wordt uitgevoerd.

![19](./media/team-lead-tasks/team-leads-19-push-to-group-server-showed-up.png)

Voer nu dezelfde set vier git-opdrachten uit de **GitRepos\MyTeam\MyTeamUtilities** directory. 

> [AZURE.NOTE]Als dit de eerste keer dat u een Git-opslagplaats vastleggen, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Als u gegevens worden doorgevoerd naar meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres, wanneer u ze elk doorvoert. Met behulp van dezelfde naam en e-mailadres bewijst handige later op als u Power BI-dashboards voor de Git-activiteiten bijhouden op meerdere opslagplaatsen bouwen.

![20](./media/team-lead-tasks/team-leads-20-git-config-name.png)


## <a name="3-create-team-data-and-analytics-resources-optional"></a>3. Maken van gegevens en analyse teamresources (optioneel)

Gegevens en analyse-resources te delen met het hele team heeft voordelen van prestaties en kosten: teamleden kunnen hun projecten niet uitvoeren op de gedeelde bronnen, opslaan op budgetten en efficiënter samenwerken. In deze sectie bieden we instructies over het maken van Azure file storage. In de volgende sectie bieden we instructie over het met koppelpunten Azure file storage met uw lokale machine. Zie voor meer informatie over het delen van andere resources, zoals gegevens wetenschappelijke virtuele Machines van Azure, Azure HDInsight Spark-Clusters [Platforms en hulpprogramma's](platforms-and-tools.md). In dit onderwerp vindt u richtlijnen van oogpunt wetenschappelijke gegevens over het selecteren van de resources die geschikt voor uw behoeften zijn en koppelingen naar de productpagina's en andere relevante en nuttige zelfstudies die we hebben gepubliceerd.

>[AZURE.NOTE] Om te voorkomen dat gegevens overdragen over meerdere datacenters, dit is mogelijk traag en kostbaar, zorg ervoor dat de resourcegroep, opslagaccount en de virtuele machine van Azure (bijv, DSVM) in de dezelfde Azure-Datacenter. 

Voer de volgende scripts voor het maken van Azure-bestandsopslag voor uw team. Azure-bestandsopslag voor uw team kan worden gebruikt voor het opslaan van gegevensassets die handig voor het hele team zijn. De scripts wordt u gevraagd om uw Azure-account en abonnement, hebt u zodat deze referenties invoeren. 

### <a name="create-azure-file-storage-with-powershell-from-windows"></a>Bestandsopslag op Azure maken met PowerShell van Windows

Voer dit script van de PowerShell opdrachtregel:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
    .\CreateFileShare.ps1

![21](./media/team-lead-tasks/team-leads-21-create-fileshare-win.png)   

Aanmelden bij uw Microsoft Azure-account als u wordt gevraagd:

![22](./media/team-lead-tasks/team-leads-22-file-create-s1.png)

Selecteer het Azure-abonnement dat u wilt gebruiken:

![23](./media/team-lead-tasks/team-leads-23-file-create-s2.png)

Selecteer welke storage-account te gebruiken of een nieuwe maken onder het geselecteerde abonnement:

![24](./media/team-lead-tasks/team-leads-24-file-create-s3.png)

Voer de naam van de Azure file storage maken. Alleen lager case tekens, cijfers en - worden geaccepteerd:

![25](./media/team-lead-tasks/team-leads-25-file-create-s4.png)

Opslaan van de Azure file storage-gegevens in een tekstbestand te vergemakkelijken koppelen en deze opslag kunt delen nadat deze is gemaakt, en noteer het pad naar de locatie. In het bijzonder, moet u dit bestand te koppelen van uw Azure-bestandsopslag voor uw virtuele Azure-machines in de volgende sectie. 

Het is raadzaam om te controleren in dit tekstbestand in de opslagplaats van uw team ProjectTemplate. Het is raadzaam om te plaatsen in de map **Docs\DataDictionaries**. Daarom kan dit activum gegevens zijn toegankelijk voor alle projecten in uw team. 

![26](./media/team-lead-tasks/team-leads-26-file-create-s5.png)


### <a name="create-azure-file-storage-with-a-linux-script"></a>Bestandsopslag op Azure maken met een Linux-script

Dit script uitvoeren vanuit de shell Linux:

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
    bash CreateFileShare.sh

Aanmelden bij uw Microsoft Azure-account de instructies in dit scherm te volgen:

![27](./media/team-lead-tasks/team-leads-27-file-create-linux-s1.png)

Selecteer de Azure-abonnement dat u wilt gebruiken:

![28](./media/team-lead-tasks/team-leads-28-file-create-linux-s2.png)

Selecteer welke storage-account te gebruiken of een nieuwe maken onder het geselecteerde abonnement:

![29](./media/team-lead-tasks/team-leads-29-file-create-linux-s3.png)

Voer de naam van de Azure-bestandsopslag voor het maken, alleen kleine letters, cijfers en - worden geaccepteerd:

![30](./media/team-lead-tasks/team-leads-30-file-create-linux-s4.png)

Opslaan van de Azure file storage-gegevens in een tekstbestand te vergemakkelijken toegang tot deze opslag nadat deze is gemaakt, en noteer het pad naar de locatie. In het bijzonder, moet u dit bestand te koppelen van uw Azure-bestandsopslag voor uw virtuele Azure-machines in de volgende sectie.

Het is raadzaam om te controleren in dit tekstbestand in de opslagplaats van uw team ProjectTemplate. Het is raadzaam om te plaatsen in de map **Docs\DataDictionaries**. Daarom kan dit activum gegevens zijn toegankelijk voor alle projecten in uw team. 

![31](./media/team-lead-tasks/team-leads-31-file-create-linux-s5.png)


## <a name="4-mount-azure-file-storage-optional"></a>4. Koppelpunt Azure-bestandsopslag (optioneel)

Nadat u Azure file storage is gemaakt, worden gekoppeld aan uw lokale machine, gebruikt u een van de volgende PowerShell- of Linux-scripts.

### <a name="mount-azure-file-storage-with-powershell-from-windows"></a>Koppelpunt Azure-bestandsopslag met PowerShell van Windows

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
    .\AttachFileShare.ps1
    
U wordt gevraagd om te melden bij de eerste, als u niet hebben aangemeld. 

Klik op **Enter** of **y** om door te gaan wanneer u wordt gevraagd als u een Azure file storage-gegevens hebt-bestand en voer vervolgens de ***voltooien pad en de naam** van het bestand dat u maakt in vorige stap. De informatie voor het koppelen van een Azure file storage wordt gelezen rechtstreeks via bestands- en u kunt beginnen met de volgende stap.

![32](./media/team-lead-tasks/team-leads-32-attach-s1.png)

> [AZURE.NOTE] Als u een bestand met de Azure file storage-gegevens niet hebt, worden de stappen voor het invoeren van de gegevens van toetsenbord opgegeven aan het einde van deze sectie.

Vervolgens wordt u gevraagd de naam van het station moet worden toegevoegd aan de virtuele machine op te geven. Een lijst met namen van bestaande schijf wordt weergegeven op het scherm. U moet een stationsnaam opgeeft die niet in de lijst bestaat nog.

![33](./media/team-lead-tasks/team-leads-33-attach-s2.png)

Bevestig dat een nieuw F-station met succes is gekoppeld aan op de computer.

![34](./media/team-lead-tasks/team-leads-34-attach-s3.png)

**Hoe de Azure file storage-gegevens handmatig invoeren:** als u uw Azure file storage-gegevens niet op een tekstbestand hebt, kunt u de instructies in het volgende scherm te typen in de vereiste abonnement, storage-account en Azure File storage-gegevens:

![35](./media/team-lead-tasks/team-leads-35-attach-s4.png)

Type in uw Azure-abonnementsnaam, selecteer de storage-account waarin de Azure file storage is gemaakt, en typt u de naam van het Azure-opslag:

![36](./media/team-lead-tasks/team-leads-36-attach-s5.png)

### <a name="mount-azure-file-storage-with-a-linux-script"></a>Koppelpunt Azure-bestandsopslag met een Linux-script

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
    bash AttachFileShare.sh

![37](./media/team-lead-tasks/team-leads-37-attach-s1-linux.png)

U wordt gevraagd om te melden bij de eerste, als u niet hebben aangemeld. 

Klik op **Enter** of **y** om door te gaan wanneer u wordt gevraagd als u een Azure file storage-gegevens hebt-bestand en voer vervolgens de ***voltooien pad en de naam** van het bestand dat u maakt in vorige stap. De informatie voor het koppelen van een Azure file storage wordt gelezen rechtstreeks via bestands- en u kunt beginnen met de volgende stap.

![38](./media/team-lead-tasks/team-leads-38-attach-s2-linux.png)

Vervolgens wordt u gevraagd de naam van het station moet worden toegevoegd aan de virtuele machine op te geven. Een lijst met namen van bestaande schijf wordt weergegeven op het scherm. U moet een stationsnaam opgeeft die niet in de lijst bestaat nog.

![39](./media/team-lead-tasks/team-leads-39-attach-s3-linux.png)

Bevestig dat een nieuw F-station met succes is gekoppeld aan op de computer.

![40](./media/team-lead-tasks/team-leads-40-attach-s4-linux.png)

**Hoe de Azure file storage-gegevens handmatig invoeren:** als u uw Azure file storage-gegevens niet op een tekstbestand hebt, kunt u de instructies in het volgende scherm te typen in de vereiste abonnement, storage-account en Azure File storage-gegevens:

- Invoer  **n** .
- Selecteer de index van de naam van het abonnement waarin de Azure file storage in de vorige stap is gemaakt:

    ![41](./media/team-lead-tasks/team-leads-41-attach-s5-linux.png)

- Selecteer het opslagaccount onder uw abonnement en een type in de Azure storage bestandsnaam:

    ![42](./media/team-lead-tasks/team-leads-42-attach-s6-linux.png)

- Voer de naam van het station moet worden toegevoegd aan uw computer verschillen van alle bestaande moet:

    ![43](./media/team-lead-tasks/team-leads-43-attach-s7-linux.png)


## <a name="5-set-up-security-control-policy"></a>5. Beheer van beveiligingsbeleid instellen 

Startpagina van de groep VSTS server, klik op de **tandwielpictogram pictogram** naast uw gebruikersnaam in de rechterbovenhoek, selecteer de **beveiliging** tabblad. U kunt leden toevoegen aan uw team met verschillende machtigingen.

![44](./media/team-lead-tasks/team-leads-44-add-team-members.png)

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de rollen en taken die zijn gedefinieerd door het Team gegevens wetenschap proces:

- [Groep Manager-taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md)
- [Team Lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)
- [Projecttaken Lead voor een wetenschappelijke Gegevensteam](project-lead-tasks.md)
- [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md)