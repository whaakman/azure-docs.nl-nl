---
title: Team Data Science Process Project leiden taken - Azure | Microsoft Docs
description: Een overzicht van de taken voor een project leiden op een data science-teamproject.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 0b5129c4642a0c4381591f751bc4d5a51990b256
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442894"
---
# <a name="project-lead-tasks"></a>Projecttaken Lead

Deze zelfstudie bevat de taken die een projectleider opslagbewerkingen uit te voeren voor zijn/haar projectteam. Het doel is het opzetten van collaboratief team-omgeving die standaardiseert op de [Team Data Science Process](overview.md) (TDSP). De TDSP is een framework ontwikkeld door Microsoft en die een gestructureerde opeenvolging van activiteiten biedt voor het efficiënt uitvoeren van cloud gebaseerde predictive analytics-oplossingen. Voor een overzicht van de rollen personeel en de bijbehorende taken die worden beheerd door een team van gegevenswetenschappers standaardiseren over dit proces, Zie [Team Data Science Process rollen en taken](roles-tasks.md).

Een **Project leiden** beheert de dagelijkse werkzaamheden van afzonderlijke datawetenschappers in een specifieke data science-project. De werkstroom voor de taken worden uitgevoerd door project leidt tot het instellen van deze omgeving worden weergegeven in de volgende afbeelding:

![1](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Dit onderwerp bevat momenteel taken 1,2 en 6 van deze werkstroom voor project leads.

>[AZURE.NOTE] We een overzicht van de stappen die nodig zijn voor het instellen van een omgeving TDSP-team voor een project met behulp van Azure DevOps in de volgende instructies. We geven over het uitvoeren van deze taken met Azure DevOps vanwege de manier waarop we TDSP bij Microsoft implementeert. Als een ander platform van de code die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden uitgevoerd door de teamleider in het algemeen niet wijzigen. Maar de manier om deze taken uit te voeren afwijken.


## <a name="repositories-and-directories"></a>Opslagplaatsen en mappen

In deze zelfstudie wordt de afgekorte naam voor opslagplaatsen en mappen. Deze namen maken het gemakkelijker zijn te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notatie (R voor Git-opslagplaatsen) en D voor lokale mappen op uw DSVM wordt gebruikt in de volgende secties:

- **R3**: het team **ProjectTemplate** -opslagplaats in Git uw teamleider heeft ingesteld.
- **R5**: de projectopslagplaats van Git u voor uw project instellen.
- **D3**: de lokale map gekloond vanuit R3.
- **D5**: de lokale map gekloond vanuit R5.


## <a name="0-prerequisites"></a>0. Vereisten

De vereisten wordt voldaan door het uitvoeren van de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md) en naar u leads die worden beschreven in team [Team lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md). 

Om samen te vatten hier, moeten de volgende vereisten voldoen voordat u begint met het team lead taken: 

- Uw **groep Azure DevOps Services** (of groepsaccount op sommige andere codehosting platform) is ingesteld door de manager van uw groep.
- Uw **TeamProjectTemplate opslagplaats** (R3) is ingesteld onder uw groepsaccount door uw team lead op het hosten van code-platform u van plan bent te gebruiken.
- U zijn **geautoriseerd** door uw team leiden tot het maken van opslagplaatsen voor uw groepsaccount voor uw team.
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een Data Science Virtual Machine (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te gaan. Raadpleeg anders de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op uw computer geïnstalleerd. In het README.md-bestand, schuif omlaag naar de **Download en installeer** sectie en klikt u op de *nieuwste installatieprogramma van*. Hiermee gaat u naar de pagina van de meest recente installatieprogramma. Het .exe-installatieprogramma hier downloaden en uitvoeren. 
- Als u **Linux-DSVM**, het maken van een openbare SSH-sleutel op uw DSVM en toe te voegen aan uw groep Azure DevOps-Services. Zie voor meer informatie over SSH, de **openbare maken van SSH-sleutel** sectie de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix). 


## <a name="1-create-a-project-repository-r5"></a>1. Maken van een projectopslagplaats (R5)

- Meld u aan bij uw groep Azure DevOps-Services op *https://\<naam Azure DevOps-Services\>. visualstudio.com*. 
- Onder **recente projecten en teams**, klikt u op **Bladeren**. Een venster dat verschijnt, geeft een lijst van alle projecten op het Azure DevOps-Services. 

    ![2](./media/project-lead-tasks/project-leads-2-create-project-repo.png)

- Klik op de naam van het project waarin u wilt maken van de projectopslagplaats van uw. In dit voorbeeld klikt u op **MyTeam**. 
- Klik vervolgens op **Navigate** om te worden omgeleid naar de startpagina van het project **MyTeam**:

    ![3](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)

- Klik op **samenwerken op code** om te worden omgeleid naar de git-startpagina van uw project.  

    ![4](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

- Klik op de pijl-omlaag in de linkerbovenhoek en selecteer **+ nieuwe opslagplaats**. 
    
    ![5](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)

- In de **maken van een nieuwe opslagplaats** venster, Geef een naam op voor uw project git-opslagplaats. Zorg ervoor dat u selecteert **Git** als het type van de opslagplaats. In dit voorbeeld gebruiken we de naam *DSProject1*. 

    ![6](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

- Maakt uw ***DSProject1*** project git-opslagplaats, klikt u op **maken**.


## <a name="2-seed-the-dsproject1-project-repository"></a>2. De projectopslagplaats DSProject1 seed

De taak hier seed is de **DSProject1** projectopslagplaats (R5) uit de opslagplaats voor uw project-sjabloon (R3). De seeding procedure gebruikt de mappen D3 en D5 op uw lokale DSVM als tussenliggende staging-sites. Kortom, het seeden pad is: R3 -> D3 D5 -> R5 ->.

Als u nodig hebt om aan te passen uw **DSProject1** projectopslagplaats om te voldoen aan bepaalde specifieke project nodig heeft, u dit doen in de voorlaatste stap van de volgende procedure. Hier volgt een samenvatting van de stappen voor het seeden van de inhoud van de **DSProject1** projectopslagplaats. De afzonderlijke stappen komen overeen met de subsecties in de seeding procedure:

- Kloon project sjabloonopslagplaats naar lokale map: R3 - gekloond voor lokale D3 ->-team.
- Kloon DSProject1 opslagplaats naar een lokale map: R5 - gekloond voor lokale D5 ->-team.
- Gekloonde project sjablooninhoud kopiëren naar de lokale kloon van de opslagplaats DSProject1: D3 - inhoud wordt gekopieerd om D5 ->.
- (Optioneel) Lokale D5 aanpassing.
- Lokale DSProject1 inhoud pushen naar team opslagplaatsen: D5 - inhoud toevoegen aan team R5 ->.


### <a name="clone-your-project-template-repository-r3-to-a-directory-d3-on-your-local-machine"></a>Uw project sjabloonopslagplaats (R3) naar een map (D3) op uw lokale computer klonen.

Maak een map op uw lokale computer:

- *C:\GitRepos\MyTeamCommon* voor Windows 
- *$home/GitRepos/MyTeamCommon* voor Linux

Ga naar die map. Voer de volgende opdracht uit om uw project sjabloonopslagplaats naar uw lokale machine te klonen. 

**Windows**
            
    git clone <the HTTPS URL of the TeamProjectTemplate repository>
    
Als u Azure DevOps als het platform voor het hosten van code, meestal de *HTTPS-URL van uw project sjabloonopslagplaats* is:

 ***https://\<naam Azure DevOps-Services\>.visualstudio.com/\<naam van uw project\>/_git/\<de naam van uw project sjabloon opslagplaats\>***. 

In dit voorbeeld hebben we de:

***https://mysamplegroup.visualstudio.com/MyTeam/_git/MyTeamProjectTemplate***. 

![7](./media/project-lead-tasks/project-leads-7-clone-team-project-template.png)
            
**Linux**

    git clone <the SSH URL of the TeamProjectTemplate repository>
        
![8](./media/project-lead-tasks/project-leads-8-clone-team-project-template-linux.png)

Als u Azure DevOps als het platform voor het hosten van code, meestal de *SSH URL van de opslagplaats van de sjabloon project* is:

***SSH: / /\<naam Azure DevOps-Services\>@\<naam Azure DevOps-Services\>.visualstudio.com:22/\<uw projectnaam > /_git/\<de projectsjabloon, maken naam van de opslagplaats\>.*** 

In dit voorbeeld hebben we de:

***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/MyTeamProjectTemplate***. 

### <a name="clone-dsproject1-repository-r5-to-a-directory-d5-on-your-local-machine"></a>Kloon DSProject1-opslagplaats (R5) naar een map (D5) op uw lokale computer

Wijzig de map naar **GitRepos**, en voer de volgende opdracht om de projectopslagplaats van uw naar uw lokale computer te klonen. 

**Windows**
            
    git clone <the HTTPS URL of the Project repository>

![9](./media/project-lead-tasks/project-leads-9-clone-project-repository.png)

Als u Azure DevOps als het platform voor het hosten van code, meestal de _HTTPS-URL van de opslagplaats Project_ is ***https://\<naam Azure DevOps-Services\>.visualstudio.com/\<Uw projectnaam > /_git/ < naam van uw project opslagplaats\>***. In dit voorbeeld hebben we ***https://mysamplegroup.visualstudio.com/MyTeam/_git/DSProject1***.

**Linux**

    git clone <the SSH URL of the Project repository>

![10](./media/project-lead-tasks/project-leads-10-clone-project-repository-linux.png)

Als u Azure DevOps als het platform voor het hosten van code, meestal de _SSH URL van de projectopslagplaats_ _ssh is: / / < naam van Azure DevOps-Services\>@< naam van Azure DevOps-Services\>. visualstudio.com:22/<Your Project Name>/\_git / < naam van uw project opslagplaats\>. In dit voorbeeld hebben we ***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/DSProject1***.

### <a name="copy-contents-of-d3-to-d5"></a>Kopieer de inhoud van D3 naar D5 

Nu in uw lokale computer, moet u de inhoud van kopiëren _D3_ naar _D5_, met uitzondering van de git-metagegevens in .git directory. De volgende scripts doet de taak. Zorg ervoor dat op het type in de juiste en volledige paden naar de mappen. Bronmap is het voor uw team (_D3_); doelmap is het voor uw project (_D5_).    

**Windows**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 -role 3
    
![11](./media/project-lead-tasks/project-leads-11-local-copy-project-lead-new.png)

U kunt zien _DSProject1_ map alle bestanden (met uitzondering van de .git) worden gekopieerd van _MyTeamProjectTemplate_.

![12](./media/project-lead-tasks/project-leads-12-teamprojectTemplate_copied_to_local.png)

**Linux**
            
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 3
        
![13](./media/project-lead-tasks/project-leads-13-local_copy_project_lead_linux_new.png)

U kunt zien _DSProject1_ map alle bestanden (met uitzondering van de metagegevens in .git) worden gekopieerd van _MyTeamProjectTemplate_.

![14](./media/project-lead-tasks/project-leads-14-teamprojectTemplate_copied_to_local_linux_new.png)


### <a name="customize-d5-if-you-need-to-optional"></a>D5 aanpassen als u wilt (optioneel)

Als uw project moet enkele specifieke mappen of documenten, dan degene die u krijgt van de projectsjabloon, maken (gekopieerd naar de map D5 in de vorige stap), kunt u de inhoud van een D5 nu. 

### <a name="add-contents-of-dsproject1-in-d5-to-r5-on-your-group-azure-devops-services"></a>Inhoud van DSProject1 in D5 toevoegen aan R5 voor uw groep Azure DevOps-Services

U moet nu push-inhoud **_DSProject1_** naar _R5_ opslagplaats in uw project op van de groep Azure DevOps-Services. 


- Ga naar map **D5**. 
- Gebruik de volgende git-opdrachten om toe te voegen van de inhoud van **D5** naar **R5**. De opdrachten zijn hetzelfde voor zowel Windows als Linux-systemen. 
    
    GIT status git toevoegen.
    GIT commit -m 'push van win DSVM' git-push
    
- De wijziging en push doorvoeren. 

>[AZURE.NOTE] Als dit de eerste keer dat u door te op een Git-opslagplaats voeren, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten uit:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Als u gegevens worden doorgevoerd op meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres op alle. Met dezelfde naam en e-mailadres bewijst handige later op wanneer u Power BI-dashboards voor het bijhouden van uw Git-activiteiten op meerdere opslagplaatsen kunt bouwen.

![15](./media/project-lead-tasks/project-leads-15-git-config-name.png)


## <a name="6-create-and-mount-azure-file-storage-as-project-resources-optional"></a>6. Maken en een koppeling Azure file storage als projectresources (optioneel)

Als u maken van Azure-bestandsopslag wilt voor het delen van gegevens, zoals het project onbewerkte gegevens of de functies die worden gegenereerd voor uw project, zodat alle leden toegang tot de dezelfde gegevenssets in meerdere Datatechnologie hebben, volg de instructies in secties 3 en 4 [ Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md). 


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de functies en taken die zijn gedefinieerd door het Team Data Science Process:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md)
- [Afzonderlijke inzenders project voor een team van gegevenswetenschappers](project-ic-tasks.md)