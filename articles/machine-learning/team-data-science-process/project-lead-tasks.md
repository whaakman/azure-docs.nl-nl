---
title: Gegevens wetenschap proces Project leiden taken - Azure in een team | Microsoft Docs
description: Een overzicht van de taken voor een project leiden op een project gegevens wetenschappelijke team.
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
ms.openlocfilehash: fe7cf1e6167de7d0dfaa5ed75c0b7f5ec23b18cf
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="project-lead-tasks"></a>Projecttaken Lead

Deze zelfstudie overzichten van de taken die een project lead wordt verwacht voor stelt zijn/haar projectteam hebt voltooid. Het doel is het opzetten van samenwerking teamomgeving die standaardiseert op de [Team gegevens wetenschap proces](overview.md) (TDSP). De TDSP ligt een framework dat is ontwikkeld door Microsoft, met een gestructureerd opeenvolging van activiteiten uit te voeren van cloud-gebaseerde, predictive analytics-oplossingen efficiënt biedt. Voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een wetenschappelijke Gegevensteam standaardiseren op dit proces Zie [Team gegevens wetenschap proces rollen en taken](roles-tasks.md).

Een **Project leiden** beheert de dagelijkse activiteiten van afzonderlijke gegevenswetenschappers op een specifieke gegevens wetenschappelijke project. De werkstroom voor de taken worden uitgevoerd door project leidt tot het instellen van deze omgeving zijn afgebeeld in de volgende afbeelding:

![1](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Dit onderwerp worden momenteel taken 1,2 en 6 van deze werkstroom voor potentiële klanten project.

>[AZURE.NOTE] Overzicht van de stappen die nodig zijn voor het instellen van een TDSP teamomgeving voor een project met behulp van Visual Studio Team Services (VSTS) in de volgende instructies. We opgeven hoe u deze taken met VSTS omdat hoe we TDSP bij Microsoft implementeren. Als een ander platform code die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de lead team in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende.


## <a name="repositories-and-directories"></a>-Opslagplaatsen en mappen

Deze zelfstudie wordt afkortingen voor opslagplaatsen en mappen. Deze namen gemakkelijker te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notation (R voor Git-opslagplaatsen) en D voor lokale mappen op uw DSVM wordt gebruikt in de volgende secties:

- **R3**: het team **ProjectTemplate** opslagplaats op Git uw team lead heeft ingesteld.
- **R5**: de project-opslagplaats op Git u voor uw project instellen.
- **D3**: de lokale map gekloond van R3.
- **D5**: de lokale map gekloond van R5.


## <a name="0-prerequisites"></a>0. Vereisten

De vereisten wordt voldaan door de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md) en aan u potentiële klanten die worden beschreven in een team [Team lead taken voor een team van de wetenschap](team-lead-tasks.md). 

Om samen te vatten hier, moeten de volgende vereisten voldoen voordat u begint met het team lead taken: 

- Uw **groep VSTS server** (of groepsaccount op sommige andere code-hosting platform) is ingesteld door het beheer van uw groep.
- Uw **TeamProjectTemplate opslagplaats** (R3) is ingesteld onder uw groepsaccount door uw team lead op het code-hosting platform u van plan bent te gebruiken.
- U zijn **geautoriseerd** door uw team lead opslagplaatsen op uw groepsaccount voor uw team maken.
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een virtuele Machine wetenschappelijke gegevens (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te beginnen. Raadpleeg anders de [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git referentie Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op deze computer geïnstalleerd. In het bestand README.md, schuif omlaag naar de **downloaden en installeren** sectie en klik op de *meest recente versie installer*. Hiermee gaat u naar de laatste pagina van het installatieprogramma. Het installatieprogramma .exe hier downloaden en uitvoeren. 
- Als u **Linux DSVM**, een openbare SSH-sleutel op uw DSVM maken en toe te voegen aan uw groep VSTS-server. Zie voor meer informatie over SSH de **openbare maken SSH-sleutel** sectie het [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix). 


## <a name="1-create-a-project-repository-r5"></a>1. Maken van een project-opslagplaats (R5)

- Aanmelden bij de server van uw groep VSTS op *https://\<VSTS servernaam\>. visualstudio.com*. 
- Onder **recente projecten & teams**, klikt u op **Bladeren**. Een venster dat een lijst met alle teamprojecten op de server VSTS verschijnt. 

    ![2](./media/project-lead-tasks/project-leads-2-create-project-repo.png)

- Klik op de naam van het projectteam waarin u gaat de opslagplaats van uw project maken. In dit voorbeeld klikt u op **MyTeam**. 
- Klik vervolgens op **navigeren** om te worden omgeleid naar de startpagina van het teamproject **MyTeam**:

    ![3](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)

- Klik op **samenwerken op code** om te worden omgeleid naar de git-startpagina van uw teamproject.  

    ![4](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

- Klik op de pijl-omlaag in de linkerbovenhoek, en selecteer **+ nieuwe opslagplaats**. 
    
    ![5](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)

- In de **maken van een nieuwe opslagplaats** venster Voer een naam in voor uw project git-opslagplaats. Zorg ervoor dat u selecteert **Git** als het type van de opslagplaats. In dit voorbeeld gebruiken we de naam van de *DSProject1*. 

    ![6](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

- Maken van uw ***DSProject1*** project git-opslagplaats, klikt u op **maken**.


## <a name="2-seed-the-dsproject1-project-repository"></a>2. De project-opslagplaats DSProject1 seed

De taak hier seed is de **DSProject1** project opslagplaats (R5) van uw team project sjabloon opslagplaats (R3). De seeding procedure gebruikt de mappen D3 en D5 op uw lokale DSVM als tussenliggende staging sites. In de samenvatting, de seeding pad als volgt: R3 -> D3 -> D5 R5 ->.

Als u nodig hebt voor het aanpassen van uw **DSProject1** project opslagplaats om te voldoen aan enkele specifieke behoeften project, u dit doen in de voorlaatste stap van de volgende procedure. Hier volgt een samenvatting van de stappen die wordt gebruikt bij de seeding van de inhoud van de **DSProject1** project-opslagplaats. De afzonderlijke stappen overeenkomen met de subsecties in de seeding procedure:

- Kloon team project sjabloon opslagplaats naar de lokale map: R3 - gekloond om lokale D3 -> in een team.
- Kloon DSProject1 opslagplaats naar een lokale map: R5 - gekloond om lokale D5 -> in een team.
- Gekloonde team project sjablooninhoud kopiëren naar de lokale kloon van de opslagplaats DSProject1: D3 - inhoud wordt gekopieerd om D5 ->.
- (Optioneel) Lokale D5 aanpassing.
- Lokale DSProject1 inhoud push naar team opslagplaatsen: D5 - inhoud toevoegen -> team R5.


### <a name="clone-your-team-project-template-repository-r3-to-a-directory-d3-on-your-local-machine"></a>Kloon uw team project sjabloon opslagplaats (R3) naar een map (D3) op uw lokale computer.

Maak een map op uw lokale computer:

- *C:\GitRepos\MyTeamCommon* voor Windows 
- *$home/GitRepos/MyTeamCommon* voor Linux

Ga naar die map. Voer de volgende opdracht voor het klonen van uw team project sjabloon opslagplaats op uw lokale computer. 

**Windows**
            
    git clone <the HTTPS URL of the TeamProjectTemplate repository>
    
Als u VSTS als het platform code-hosting normaal gesproken de *HTTPS-URL van uw team project sjabloon opslagplaats* is:

 ***https://\<VSTS servernaam\>.visualstudio.com/\<de naam van uw team project\>/_git/\<de naam van uw team project sjabloon opslagplaats\>***. 

In dit voorbeeld hebben we:

***https://mysamplegroup.visualstudio.com/MyTeam/_git/MyTeamProjectTemplate***. 

![7](./media/project-lead-tasks/project-leads-7-clone-team-project-template.png)
            
**Linux**

    git clone <the SSH URL of the TeamProjectTemplate repository>
        
![8](./media/project-lead-tasks/project-leads-8-clone-team-project-template-linux.png)

Als u VSTS als het platform code-hosting normaal gesproken de *SSH-URL van de opslagplaats team project sjabloon* is:

***SSH: / /\<VSTS servernaam\>@\<VSTS servernaam\>.visualstudio.com:22/\<uw Team projectnaam > /_git/\<uw team project sjabloon opslagplaats naam\>.*** 

In dit voorbeeld hebben we:

***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/MyTeamProjectTemplate***. 

### <a name="clone-dsproject1-repository-r5-to-a-directory-d5-on-your-local-machine"></a>Kloon DSProject1 opslagplaats (R5) naar een map (D5) op uw lokale computer

Map wijzigen naar **GitRepos**, en voer de volgende opdracht voor het klonen van de opslagplaats van uw project naar uw lokale computer. 

**Windows**
            
    git clone <the HTTPS URL of the Project repository>

![9](./media/project-lead-tasks/project-leads-9-clone-project-repository.png)

Als u VSTS als het platform code-hosting normaal gesproken de _HTTPS-URL van de opslagplaats Project_ is ***https://\<VSTS servernaam\>.visualstudio.com/\<uw Team Naam project > /_git/ < naam van uw project-opslagplaats\>***. In dit voorbeeld hebben we ***https://mysamplegroup.visualstudio.com/MyTeam/_git/DSProject1***.

**Linux**

    git clone <the SSH URL of the Project repository>

![10](./media/project-lead-tasks/project-leads-10-clone-project-repository-linux.png)

Als u VSTS als het platform code-hosting normaal gesproken de _SSH-URL van de opslagplaats project_ _ssh is: / / < servernaam VSTS\>@< servernaam VSTS\>.visualstudio.com:22/<Your Team Project Name> / \_git / < naam van uw project-opslagplaats\>. In dit voorbeeld hebben we  ***ssh://mysamplegroup@mysamplegroup.visualstudio.com:22/MyTeam/_git/DSProject1*** .

### <a name="copy-contents-of-d3-to-d5"></a>Kopieer de inhoud van D3 naar D5 

Nu in uw lokale computer, moet u kopiëren van de inhoud van _D3_ naar _D5_, behalve de git-metagegevens in .git directory. De volgende scripts doet de taak. Zorg ervoor dat in de juiste en volledige paden naar de mappen te typen. Bronmap is voor uw team (_D3_); doelmap is voor uw project (_D5_).    

**Windows**
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 -role 3
    
![11](./media/project-lead-tasks/project-leads-11-local-copy-project-lead-new.png)

Nu u kunt zien in _DSProject1_ map alle bestanden (met uitzondering van de .git) worden gekopieerd van _MyTeamProjectTemplate_.

![12](./media/project-lead-tasks/project-leads-12-teamprojectTemplate_copied_to_local.png)

**Linux**
            
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 3
        
![13](./media/project-lead-tasks/project-leads-13-local_copy_project_lead_linux_new.png)

Nu u kunt zien in _DSProject1_ map (met uitzondering van de metagegevens in .git) alle bestanden worden gekopieerd van _MyTeamProjectTemplate_.

![14](./media/project-lead-tasks/project-leads-14-teamprojectTemplate_copied_to_local_linux_new.png)


### <a name="customize-d5-if-you-need-to-optional"></a>D5 aanpassen als u wilt (optioneel)

Als uw project een aantal specifieke mappen of documenten moet, dan degene die u krijgt van uw team projectsjabloon (gekopieerd naar de map D5 in de vorige stap), kunt u de inhoud van D5 nu. 

### <a name="add-contents-of-dsproject1-in-d5-to-r5-on-your-group-vsts-server"></a>Inhoud van DSProject1 in D5 toevoegen aan R5 op uw groep VSTS server

U moet nu voor de push-inhoud  **_DSProject1_**  naar _R5_ -opslagplaats in uw teamproject op uw groep VSTS server. 


- Ga naar map **D5**. 
- Gebruik de volgende git-opdrachten om toe te voegen inhoud **D5** naar **R5**. De opdrachten zijn hetzelfde voor Windows-en Linux. 
    
    GIT status git toevoegen.
    GIT commit -m 'push van win DSVM' git push
    
- Wijzigings- en push doorvoeren. 

>[AZURE.NOTE] Als dit de eerste keer dat u een Git-opslagplaats vastleggen, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
> Als u gegevens worden doorgevoerd naar meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres voor alle hiervan. Met behulp van dezelfde naam en e-mailadres bewijst handige later op als u Power BI-dashboards voor de Git-activiteiten bijhouden op meerdere opslagplaatsen bouwen.

![15](./media/project-lead-tasks/project-leads-15-git-config-name.png)


## <a name="6-create-and-mount-azure-file-storage-as-project-resources-optional"></a>6. Maken en koppelen van Azure file storage als projectresources (optioneel)

Als u maken van Azure file storage om gegevens te delen wilt, zoals het project onbewerkte gegevens of de functies die worden gegenereerd voor uw project, zodat alle leden toegang tot de dezelfde gegevenssets van meerdere DSVMs hebben Volg de instructies in de secties 3 en 4 van [ Taken voor een wetenschappelijke Gegevensteam Lead team](team-lead-tasks.md). 


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de rollen en taken die zijn gedefinieerd door het Team gegevens wetenschap proces:

- [Groep Manager-taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md)
- [Team Lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)
- [Projecttaken Lead voor een wetenschappelijke Gegevensteam](project-lead-tasks.md)
- [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md)