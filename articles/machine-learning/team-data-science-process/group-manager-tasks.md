---
title: Team Data Science Process manager groepstaken
description: Een overzicht van de taken voor een groepmanager op een data science-teamproject.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d44b13bb087f171c3f0474cde73167481221274f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53136577"
---
# <a name="tasks-for-a-group-manager-on-a-data-science-team-project"></a>Taken voor een groepmanager op een data science-teamproject

In dit onderwerp bevat de taken die wordt verwacht dat de Manager van een groep zijn voltooid voor historie / haar data science-organisatie. Het doel is het opzetten van gezamenlijke groepsomgeving die standaardiseert op de [Team Data Science Process](overview.md) (TDSP). Voor een overzicht van de rollen personeel en de bijbehorende taken die worden beheerd door een team van gegevenswetenschappers standaardiseren over dit proces, Zie [Team Data Science Process rollen en taken](roles-tasks.md).

De **groepsbeheerder** is de manager van de volledige data science-eenheid in een onderneming. Een data science-eenheid mogelijk meerdere teams, die elk is bezig met meerdere data science-projecten in afzonderlijk zakelijk verticalen. De Manager van een groep kunnen hun taken delegeren aan een vervangend, maar de taken die zijn gekoppeld aan de rol zijn hetzelfde. Er zijn zes belangrijkste taken, zoals wordt weergegeven in het volgende diagram:

![0](./media/group-manager-tasks/tdsp-group-manager.png)


>[AZURE.NOTE] We een overzicht van de stappen die nodig zijn voor het instellen van een TDSP-groep-omgeving met Azure DevOps-Services in de volgende instructies. We geven over het uitvoeren van deze taken met Azure DevOps-Services vanwege de manier waarop we TDSP bij Microsoft implementeert. Als een andere code die als host fungeert platform wordt gebruikt voor uw groep, worden de taken die moeten worden uitgevoerd door de manager van de groep in het algemeen niet wijzigen. Maar de manier om deze taken uit te voeren afwijken.

1. Instellen van **Azure DevOps Services** voor de groep.
2. Maak een **group-project** op Azure DevOps-Services (voor gebruikers van Azure DevOps-Services)
3. Maak de **GroupProjectTemplate** opslagplaats
4. Maak de **GroupUtilities** opslagplaats
5. Seed de **GroupProjectTemplate** en **GroupUtilities** opslagplaatsen voor het Azure DevOps-Services met inhoud van de TDSP-opslagplaatsen.
6. Instellen van de **beveiligingsmechanismen** voor teamleden toegang krijgen tot de GroupProjectTemplate en GroupUtilities-opslagplaatsen.

Elk van de voorgaande stappen wordt in detail beschreven. Maar eerst, we raakt u vertrouwd met de afkortingen en bespreken de vereisten voor het werken met opslagplaatsen.

### <a name="abbreviations-for-repositories-and-directories"></a>Afkortingen voor opslagplaatsen en mappen

In deze zelfstudie wordt de afgekorte naam voor opslagplaatsen en mappen. Deze definities wordt het eenvoudiger om te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notatie wordt gebruikt in de volgende secties:

- **G1**: de project-opslagplaats voor sjablonen die zijn ontwikkeld en beheerd door de TDSP-team van Microsoft.
- **G2**: de opslagplaats van de hulpprogramma's die zijn ontwikkeld en beheerd door de TDSP-team van Microsoft.
- **R1**: de GroupProjectTemplate-opslagplaats in Git instellen van op de server van uw Azure DevOps-groep.
- **R2**: de GroupUtilities-opslagplaats in Git instellen van op de server van uw Azure DevOps-groep.
- **LG1** en **LG2**: de lokale mappen op uw computer dat u G1 en G2, respectievelijk kloont.
- **LR1** en **LR2**: de lokale mappen op uw computer dat u R1 en R2, respectievelijk kloont.

### <a name="pre-requisites-for-cloning-repositories-and-checking-code-in-and-out"></a>Vereisten voor het klonen van opslagplaatsen en code in en uit controleren
 
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een Data Science Virtual Machine (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te gaan. Raadpleeg anders de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op uw computer geïnstalleerd. In het README.md-bestand, schuif omlaag naar de **Download en installeer** sectie en klikt u op de *nieuwste installatieprogramma van*. Deze stap gaat u naar de pagina van de meest recente installatieprogramma. Het .exe-installatieprogramma hier downloaden en uitvoeren. 
- Als u **Linux-DSVM**, het maken van een openbare SSH-sleutel op uw DSVM en toe te voegen aan uw groep Azure DevOps-Services. Zie voor meer informatie over SSH, de **openbare maken van SSH-sleutel** sectie de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix). 


## <a name="1-create-account-on-azure-devops-services"></a>1. Account maken op Azure DevOps-Services

Het Azure DevOps-Services als host fungeert voor de volgende opslagplaatsen:

- **algemene opslagplaatsen groep**: algemene opslagplaatsen die kunnen worden vastgesteld door meerdere teams binnen een groep voor meerdere data science-projecten. Bijvoorbeeld, de *GroupProjectTemplate* en *GroupUtilities* opslagplaatsen.
- **team opslagplaatsen**: opslagplaatsen voor specifieke teams binnen een groep. Deze opslagplaatsen zijn specifiek voor de behoeften van een team, en kunnen worden vastgesteld door meerdere projecten uitgevoerd door dat team, maar geen algemene genoeg om nog nuttig zijn voor meerdere teams binnen een groep van data science. 
- **Project opslagplaatsen**: opslagplaatsen die beschikbaar zijn voor specifieke projecten. Deze opslagplaatsen zijn mogelijk niet algemeen om nog nuttig zijn voor meerdere projecten uitgevoerd door een team en voor meerdere teams in de groep van een data science.


### <a name="setting-up-the-azure-devops-services-sign-into-your-microsoft-account"></a>Instellen van het Azure DevOps Services Meld u aan bij uw Microsoft-account
    
Ga naar [Visual Studio online](https://www.visualstudio.com/), klikt u op **aanmelden** in de rechterbovenhoek en meld u aan bij uw Microsoft-account. 
    
![1](./media/group-manager-tasks/login.PNG)

Als u een Microsoft-account hebt, klikt u op **Meld u nu** voor het maken van een Microsoft-account en meldt u zich aan met behulp van dit account. 

Als uw organisatie een Visual Studio/MSDN-abonnement heeft, klikt u op de groene **Meld u aan met uw werk- of schoolaccount** vak en meld u aan met de referenties die zijn gekoppeld aan dit abonnement. 
        
![2](./media/group-manager-tasks/signin.PNG)


        
Nadat u zich hebt aangemeld, klikt u op **nieuw Account maken** in de rechterbovenhoek zoals weergegeven in de volgende afbeelding:
        
![3](./media/group-manager-tasks/create-account-1.PNG)
        
Vul de gegevens voor de Azure DevOps-Services die u maken wilt de **maken van uw account** wizard met de volgende waarden: 

- **Server-URL**: Vervang *mysamplegroup* door uw eigen *servernaam*. De URL van uw server is het verstandig om: *https://\<servername\>. visualstudio.com*. 
- **Beheren met behulp van code:** Selecteer  **_Git_**.
- **Naam van het project:** Enter *GroupCommon*. 
- **Met behulp van werk organiseren:** kiezen *Agile*.
- **Host uw projecten in:** een geolocatie te kiezen. In dit voorbeeld, kiezen wij *Zuid-centraal VS*. 
        
![4](./media/group-manager-tasks/fill-in-account-information.png)

>[AZURE.NOTE] Als u het volgende pop-upvenster ziet nadat u op **nieuw account maken**, moet u klikken op **details wijzigen** om weer te geven van alle velden die zijn gespecificeerd.

![5](./media/group-manager-tasks/create-account-2.png)


Klik op **Doorgaan**. 

## <a name="2-groupcommon-project"></a>2. GroupCommon Project

De **GroupCommon** pagina (*https://\<servername\>.visualstudio.com/GroupCommon*) wordt geopend nadat uw DevOps-Services van Azure is gemaakt.
                            
![6](./media/group-manager-tasks/server-created-2.PNG)

## <a name="3-create-the-grouputilities-r2-repository"></a>3. Maken van de opslagplaats GroupUtilities (R2)

Maakt de **GroupUtilities** opslagplaats onder Azure DevOps-Services (R2):

- Om te openen de **maken van een nieuwe opslagplaats** wizard, klikt u op **nieuwe opslagplaats** op de **versiebeheer** tabblad van het project. 

![7](./media/group-manager-tasks/create-grouputilities-repo-1.png) 

- Selecteer *Git* als de **Type**, en voer *GroupUtilities* als de **naam**, en klik vervolgens op **maken**. 

![8](./media/group-manager-tasks/create-grouputilities-repo-2.png)
                
Nu u twee Git-opslagplaatsen ziet **GroupProjectTemplate** en **GroupUtilities** in de linkerkolom van de **versiebeheer** pagina: 

![9](./media/group-manager-tasks/two-repo-under-groupCommon.PNG)


## <a name="4-create-the-groupprojecttemplate-r1-repository"></a>4. Maken van de opslagplaats GroupProjectTemplate (R1)

De installatie van de opslagplaatsen voor de server van het Azure DevOps-groep bestaat uit twee taken:

- Wijzig de naam van de standaard **GroupCommon** opslagplaats***GroupProjectTemplate***.
- Maak de **GroupUtilities** -opslagplaats in de Azure DevOps-Services onder project **GroupCommon**. 

Instructies voor de eerste taak zijn opgenomen in deze sectie na opmerkingen over naamconventies of onze opslagplaatsen en mappen. De instructies voor de tweede taak zijn opgenomen in de volgende sectie voor stap 4.

### <a name="rename-the-default-groupcommon-repository"></a>Wijzig de naam van de standaard GroupCommon-opslagplaats

Naam wijzigen van de standaard **GroupCommon** opslagplaats als *GroupProjectTemplate* (waarnaar wordt verwezen als **R1** in deze zelfstudie):
    
- Klik op **samenwerken op code** op de **GroupCommon** -projectpagina. Hiermee gaat u naar de pagina standaard Git-opslagplaats van het project **GroupCommon**. Deze Git-opslagplaats is momenteel leeg. 

![10](./media/group-manager-tasks/rename-groupcommon-repo-3.png)
        
- Klik op **GroupCommon** in de linkerbovenhoek (gemarkeerd met een rood kader in de volgende afbeelding) op de pagina van de Git-opslagplaats van **GroupCommon** en selecteer **beheren opslagplaatsen**(gemarkeerd met een groene vakje in de volgende afbeelding). Deze procedure wordt de **CONFIGURATIESCHERM**. 
- Selecteer de **versiebeheer** tabblad van het project. 

![11](./media/group-manager-tasks/rename-groupcommon-repo-4.png)

- Klik op de **...**  aan de rechterkant van de **GroupCommon** -opslagplaats in het linkerdeelvenster en selecteer **naam opslagplaats**. 

![12](./media/group-manager-tasks/rename-groupcommon-repo-5.png)
        
- In de **Wijzig de naam van de opslagplaats GroupCommon** wizard die POP's, voer *GroupProjectTemplate* in de **naam van de opslagplaats** vak en klik vervolgens op **wijzigen** . 

![13](./media/group-manager-tasks/rename-groupcommon-repo-6.png)



## <a name="5-seed-the-r1--r2-repositories-on-the-azure-devops-services"></a>5. Seed-de R1 & R2 opslagplaatsen op het Azure DevOps-Services

In deze fase van de procedure worden het seeden de *GroupProjectTemplate* (R1) en *GroupUtilities* (R2)-opslagplaatsen die u hebt ingesteld in de vorige sectie. Deze opslagplaatsen zijn gemaakt met de ***ProjectTemplate*** (**G1**) en ***hulpprogramma's voor*** (**G2**) opslagplaatsen die worden beheerd door Microsoft voor het Team Data Science Process. Wanneer deze seeding is voltooid:

- uw opslagplaats R1 u dezelfde set van mappen en sjablonen die door de G1 worden ondersteund
- de R2-opslagplaats zal bevatten van de set van data science-hulpprogramma's die zijn ontwikkeld door Microsoft.

De seeding procedure gebruikt de mappen op uw lokale DSVM als tussenliggende staging-sites. Hier volgen de stappen in deze sectie wordt gevolgd:

- G1 & G2 - gekloond naar LG1 & LG2 ->
- R1 & R2 - gekloond naar LR1 & LR2 ->
- LG1 & LG2 - bestanden gekopieerd naar LR1 & LR2 ->
- (Optioneel) aanpassing van LR1 & LR2
- LR1 & LR2 - inhoud toevoegen aan R1 & R2 ->


### <a name="clone-g1--g2-repositories-to-your-local-dsvm"></a>G1 & G2 opslagplaatsen op uw lokale DSVM klonen

In deze stap maakt u een de opslagplaats van Team Data Science Process (TDSP) ProjectTemplate (G1) en hulpprogramma's (G2) van de TDSP github-opslagplaatsen op mappen klonen in uw lokale DSVM als LG1 en LG2:

- Maak een map om te fungeren als de hoofdmap voor het hosten van uw klonen van de opslagplaatsen. 
    -  Maak een map in de Windows-DSVM *C:\GitRepos\TDSPCommon*. 
    -  Maak een map in de Linux-DSVM *GitRepos\TDSPCommon* in de basismap. 

- Voer de volgende reeks opdrachten van de *GitRepos\TDSPCommon* directory.

    `git clone https://github.com/Azure/Azure-TDSP-ProjectTemplate`<br>
    `git clone https://github.com/Azure/Azure-TDSP-Utilities`
        
![14](./media/group-manager-tasks/two-folder-cloned-from-TDSP-windows.PNG)

- Met behulp van de namen van onze verkorte opslagplaats, is dit wat deze scripts hebben bereikt: 
    - G1 - gekloond in LG1 ->
    - G2 - gekloond in LG2 ->
- Nadat het klonen is voltooid, zou het mogelijk om te zien van twee directory's, _ProjectTemplate_ en _hulpprogramma's voor_onder **GitRepos\TDSPCommon** directory. 

### <a name="clone-r1--r2-repositories-to-your-local-dsvm"></a>R1 & R2 opslagplaatsen op uw lokale DSVM klonen

In deze stap maakt u kloont de opslagplaats GroupProjectTemplate (R1) en GroupUtilities opslagplaats (R2) op de lokale mappen (waarnaar wordt verwezen als LR1 en LR2, respectievelijk) onder **GitRepos\GroupCommon** op uw DSVM.

- Als u de URL's van de opslagplaatsen R1 en R2, gaat u naar uw **GroupCommon** startpagina op Azure DevOps-Services. Dit heeft meestal de URL *https://\<naam van uw Services voor Azure DevOps\>.visualstudio.com/GroupCommon*. 
- Klik op **CODE**. 
- Kies de **GroupProjectTemplate** en **GroupUtilities** opslagplaatsen. Kopiëren en opslaan van elk van de URL's (HTTPS voor Windows; SSH voor Linux) uit de **-kloon-URL** -element, op zijn beurt voor gebruik in de volgende scripts:  

![15](./media/group-manager-tasks/find_https_ssh_2.PNG)

- Wijzig in het **GitRepos\GroupCommon** map op uw Windows of Linux-DSVM en voer een van de volgende sets opdrachten voor het klonen van R1 en R2 naar die map.
        
Hier volgen de scripts voor Windows en Linux:

    # Windows DSVM

    git clone <the HTTPS URL of the GroupProjectTemplate repository>
    git clone <the HTTPS URL of the GroupUtilities repository>

![16](./media/group-manager-tasks/clone-two-empty-group-reo-windows-2.PNG)

    # Linux DSVM

    git clone <the SSH URL of the GroupProjectTemplate repository>
    git clone <the SSH URL of the GroupUtilities repository>

![17](./media/group-manager-tasks/clone-two-empty-group-reo-linux-2.PNG)

>[AZURE.NOTE] Verwacht te ontvangen waarschuwingsberichten LR1 en LR2 leeg zijn.    

- Met behulp van de namen van onze verkorte opslagplaats, is dit wat deze scripts hebben bereikt: 
    - R1 - gekloond in LR1 ->
    - LR2-R2 - gekloond in >   


### <a name="seed-your-groupprojecttemplate-lr1-and-grouputilities-lr2"></a>Seed-uw GroupProjectTemplate (LR1) en GroupUtilities (LR2)

Vervolgens in uw lokale computer, Kopieer de inhoud van hulpprogramma's en ProjectTemplate mappen (met uitzondering van de metagegevens in de mappen .git) onder GitRepos\TDSPCommon naar uw adreslijsten GroupProjectTemplate en GroupUtilities onder **GitRepos\ GroupCommon**. Dit zijn de twee taken in deze stap voltooien:

- Kopieer de bestanden in GitRepos\TDSPCommon\ProjectTemplate (**LG1**) naar GitRepos\GroupCommon\GroupProjectTemplate (**LR1**) 
- Kopieer de bestanden in GitRepos\TDSPCommon\Utilities (**LG2** naar GitRepos\GroupCommon\Utilities (**LR2**). 

Voer de volgende scripts in PowerShell-console (Windows) of Shell-script (Linux) voor het bereiken van deze twee taken. U wordt gevraagd voor het invoeren van de volledige paden naar LG1, LR1 LG2 en LR2. De paden die u invoert worden gevalideerd. Wanneer u een map die niet bestaat, wordt u gevraagd voor het invoeren van het opnieuw. 

    # Windows DSVM      
    
    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_win.ps1" -outfile "tdsp_local_copy_win.ps1"
    .\tdsp_local_copy_win.ps1 1

![18](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows-2.PNG)

U kunt nu zien dat bestanden in mappen LG1 en LG1 (met uitzondering van bestanden in de map .git) zijn gekopieerd naar LR1 en LR2, respectievelijk.

![19](./media/group-manager-tasks/copy-two-folder-to-group-folder-windows.PNG)

    # Linux DSVM

    wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/tdsp_local_copy_linux.sh"
    bash tdsp_local_copy_linux.sh 1

![20](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux-2.PNG)
        
U ziet nu dat de bestanden in de twee mappen (met uitzondering van bestanden in de map .git) respectievelijk GroupProjectTemplate en GroupUtilities worden gekopieerd.
    
![21](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux.PNG)

- Met behulp van de namen van onze verkorte opslagplaats, is dit wat deze scripts hebben bereikt:
    - LG1 - bestanden gekopieerd naar LR1 ->
    - LG2 - bestanden gekopieerd naar LR2 ->

### <a name="option-to-customize-the-contents-of-lr1--lr2"></a>Optie voor het aanpassen van de inhoud van LR1 & LR2
    
Als u aanpassen van de inhoud van LR1 en LR2 wilt om te voldoen aan de specifieke behoeften van uw groep, is dit de fase van de procedure waar die geschikt is. U kunt de sjabloon voor documenten wijzigen, wijzigen van de mapstructuur en bestaande hulpprogramma's die uw groep heeft ontwikkeld of die handig zijn voor uw hele groep toevoegen. 

### <a name="add-the-contents-in-lr1--lr2-to-r1--r2-on-group-server"></a>De inhoud in de LR1 & LR2 toevoegen aan R1 & R2 op de server voor groep

Nu moet u de inhoud in LR1 en LR2 toevoegen aan opslagplaatsen R1 en R2. Hier volgen de git bash-opdrachten die u in Windows PowerShell- of Linux uitvoeren kunt. 

Voer de volgende opdrachten uit de map GitRepos\GroupCommon\GroupProjectTemplate:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

De m - optie kunt u een bericht voor de git-doorvoer instellen.

![22](./media/group-manager-tasks/push-to-group-server-2.PNG)

U kunt zien dat in van uw groep Azure DevOps-Services, in de opslagplaats GroupProjectTemplate de bestanden worden gesynchroniseerd direct.

![23](./media/group-manager-tasks/push-to-group-server-showed-up-2.PNG)

Ten slotte wijzigen in de **GitRepos\GroupCommon\GroupUtilities** directory en voer de dezelfde set git bash-opdrachten:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

>[AZURE.NOTE] Als dit de eerste keer dat u door te op een Git-opslagplaats voeren, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten uit:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
>Als u gegevens worden doorgevoerd op meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres wanneer u aan elk van deze. Met dezelfde naam en e-mailadres bewijst handige later op wanneer u Power BI-dashboards voor het bijhouden van uw Git-activiteiten op meerdere opslagplaatsen kunt bouwen.


- Met behulp van de namen van onze verkorte opslagplaats, is dit wat deze scripts hebben bereikt:
    - LR1 - inhoud toevoegen aan R1 ->
    - LR2 - inhoud toevoegen aan R2 ->

## <a name="6-add-group-members-to-the-group-server"></a>6. Groepsleden toevoegen aan de groep-server

Startpagina van uw groep Azure DevOps Services, klik op de **tandwielpictogram** naast de naam van de gebruiker in de rechterbovenhoek, selecteert u vervolgens de **Security** tabblad. U kunt leden toevoegen aan uw groep met verschillende machtigingen.

![24](./media/group-manager-tasks/add_member_to_group.PNG) 


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de functies en taken die zijn gedefinieerd door het Team Data Science Process:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md)
- [Afzonderlijke inzenders project voor een team van gegevenswetenschappers](project-ic-tasks.md)