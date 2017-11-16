---
title: Team Data wetenschap proces groep Manager taken - Azure | Microsoft Docs
description: Een overzicht van de taken voor een groepmanager op een project gegevens wetenschappelijke team.
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
ms.openlocfilehash: 58cea8b0288469a76dd8c4eb967caa8e87cd3dd7
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="group-manager-tasks"></a>Groep Manager-taken

Dit onderwerp die worden beschreven, de taken die groepsbeheerder naar verwachting worden voltooid voor historie / haar organisatie van de wetenschappelijke gegevens. Het doel is het opzetten van samenwerking groepsomgeving die standaardiseert op de [Team gegevens wetenschap proces](overview.md) (TDSP). Voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een wetenschappelijke Gegevensteam standaardiseren op dit proces Zie [Team gegevens wetenschap proces rollen en taken](roles-tasks.md).

De **groepsbeheerder** de manager van de gehele wetenschappelijke eenheid in een onderneming. Een eenheid van de wetenschappelijke gegevens mogelijk meerdere teams, die elk meerdere gegevens wetenschap-projecten in twee afzonderlijke business verticalen wordt gewerkt. Groepsbeheerder hun taken aan een vervanging kan delegeren, maar de taken die zijn gekoppeld aan de rol zijn hetzelfde. Er zijn zes belangrijkste taken, zoals wordt weergegeven in het volgende diagram:

![0](./media/group-manager-tasks/tdsp-group-manager.png)


>[AZURE.NOTE] Overzicht van de stappen die nodig zijn voor het instellen van een omgeving met TDSP groep VSTS gebruiken in de volgende instructies. We opgeven hoe u deze taken met VSTS omdat hoe we TDSP bij Microsoft implementeren. Als een andere code platform die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de manager van de groep in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende.

1. Instellen van **Visual Studio Team Services (VSTS) server** voor de groep.
2. Maak een **groep teamproject** op Visual Studio Team Services-server (voor VSTS gebruikers)
3. Maak de **GroupProjectTemplate** opslagplaats
4. Maak de **GroupUtilities** opslagplaats
5. Seed de **GroupProjectTemplate** en **GroupUtilities** opslagplaatsen voor de server VSTS met inhoud van de TDSP-opslagplaatsen.
6. Instellen van de **beveiligingsmechanismen** voor teamleden toegang tot de GroupProjectTemplate en GroupUtilities-opslagplaatsen.

Elk van de voorgaande stappen wordt beschreven. Maar eerst we u vertrouwd met de afkortingen en de vereisten voor het werken met opslagplaatsen bespreken.

### <a name="abbreviations-for-repositories-and-directories"></a>Afkortingen voor opslagplaatsen en mappen

Deze zelfstudie wordt afkortingen voor opslagplaatsen en mappen. Deze definities gemakkelijker te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notatie wordt gebruikt in de volgende secties:

- **G1**: de opslaglocatie voor de project-sjablonen die zijn ontwikkeld en beheerd door TDSP team van Microsoft.
- **G2**: de opslagplaats hulpprogramma's die zijn ontwikkeld en beheerd door TDSP team van Microsoft.
- **R1**: de GroupProjectTemplate opslagplaats op Git u instellen op uw server VSTS groep.
- **R2**: de GroupUtilities opslagplaats op Git u instellen op uw server VSTS groep.
- **LG1** en **LG2**: de lokale mappen op uw computer dat u een kloon G1 en G2, respectievelijk.
- **LR1** en **LR2**: de lokale mappen op uw computer dat u een kloon R1 en R2, respectievelijk.

### <a name="pre-requisites-for-cloning-repositories-and-checking-code-in-and-out"></a>Vereisten voor het klonen opslagplaatsen en code in en uit controleren
 
- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een virtuele Machine wetenschappelijke gegevens (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te beginnen. Raadpleeg anders de [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git referentie Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op deze computer geïnstalleerd. In het bestand README.md, schuif omlaag naar de **downloaden en installeren** sectie en klik op de *meest recente versie installer*. Deze stap gaat u naar de laatste pagina van het installatieprogramma. Het installatieprogramma .exe hier downloaden en uitvoeren. 
- Als u **Linux DSVM**, een openbare SSH-sleutel op uw DSVM maken en toe te voegen aan uw groep VSTS-server. Zie voor meer informatie over SSH de **openbare maken SSH-sleutel** sectie het [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix). 


## <a name="1-create-account-on-vsts-server"></a>1. Account maken op de server VSTS

De VSTS-server als host fungeert voor de volgende opslagplaatsen:

- **algemene opslagplaatsen groep**: algemene opslagplaatsen die kunnen worden vastgesteld door meerdere teams binnen een groep voor meerdere gegevens wetenschappelijke projecten. Bijvoorbeeld, de *GroupProjectTemplate* en *GroupUtilities* opslagplaatsen.
- **in een team opslagplaatsen**: opslagplaatsen voor specifieke teams binnen een groep. Deze opslagplaatsen zijn specifiek voor een team nodig en kunnen worden aangenomen door meerdere projecten uitgevoerd door dat team, maar niet algemeen genoeg voor nuttig zijn voor meerdere teams binnen een wetenschappelijke gegevensgroep. 
- **Project opslagplaatsen**: opslagplaatsen beschikbaar voor specifieke projecten. Dergelijke opslagplaatsen zijn mogelijk niet algemeen nuttig meerdere projecten uitgevoerd door een team en meerdere teams in een groep van de wetenschappelijke gegevens.


### <a name="setting-up-the-vsts-server-sign-into-your-microsoft-account"></a>Instellen van het teken van de Server VSTS bij uw Microsoft-account
    
Ga naar [Visual Studio online](https://www.visualstudio.com/), klikt u op **aanmelden** in de rechterbovenhoek en meld u aan bij uw Microsoft-account. 
    
![1](./media/group-manager-tasks/login.PNG)

Als u een Microsoft-account niet hebt, klikt u op **nu aanmelden** voor het maken van een Microsoft-account en vervolgens weer aanmelden met dit account. 

Als uw organisatie een Visual Studio/MSDN-abonnement heeft, klikt u op de groene **aanmelden met uw werk- of schoolaccount** vak en meld u aan met de referenties die zijn gekoppeld aan dit abonnement. 
        
![2](./media/group-manager-tasks/signin.PNG)


        
Nadat u zich aanmeldt, klikt u op **nieuw Account maken** in de rechterbovenhoek zoals weergegeven in de volgende afbeelding:
        
![3](./media/group-manager-tasks/create-account-1.PNG)
        
Vul de gegevens voor de VSTS-server die u maken wilt de **maken van uw account** wizard met de volgende waarden: 

- **Server-URL**: Vervang *mysamplegroup* door uw eigen *servernaam*. De URL van uw server is het verstandig om: *https://\<servername\>. visualstudio.com*. 
- **Beheren met behulp van code:** Selecteer  **_Git_**.
- **Naam van het project:** Enter *GroupCommon*. 
- **Indelen met behulp van work:** Kies *Agile*.
- **Host uw projecten in:** Kies een geografische locatie. In dit voorbeeld kiest *Zuid-centraal VS*. 
        
![4](./media/group-manager-tasks/fill-in-account-information.png)

>[AZURE.NOTE] Als u het volgende pop-upvenster zien nadat u op **nieuw account maken**, en vervolgens moet u op **details wijzigen** om weer te geven van alle velden die zijn gespecificeerd.

![5](./media/group-manager-tasks/create-account-2.png)


Klik op **gaan**. 

## <a name="2-groupcommon-team-project"></a>2. GroupCommon teamproject

De **GroupCommon** pagina (*https://\<servername\>.visualstudio.com/GroupCommon*) wordt geopend nadat uw VSTS-server is gemaakt.
                            
![6](./media/group-manager-tasks/server-created-2.PNG)

## <a name="3-create-the-grouputilities-r2-repository"></a>3. Maken van de opslagplaats GroupUtilities (R2)

Maken van de **GroupUtilities** opslagplaats onder server VSTS (R2):

- Openen de **maken van een nieuwe opslagplaats** wizard, klikt u op **nieuwe opslagplaats** op de **versiebeheer** tabblad van uw teamproject. 

![7](./media/group-manager-tasks/create-grouputilities-repo-1.png) 

- Selecteer *Git* als de **Type**, en voer *GroupUtilities* als de **naam**, en klik vervolgens op **maken**. 

![8](./media/group-manager-tasks/create-grouputilities-repo-2.png)
                
Nu u twee Git-opslagplaatsen ziet **GroupProjectTemplate** en **GroupUtilities** in de linkerkolom van de **versiebeheer** pagina: 

![9](./media/group-manager-tasks/two-repo-under-groupCommon.PNG)


## <a name="4-create-the-groupprojecttemplate-r1-repository"></a>4. De opslagplaats GroupProjectTemplate (R1) maken

De installatie van de opslagplaatsen voor de server van de groep VSTS bestaat uit twee taken:

- Wijzig de naam van de standaard **GroupCommon** opslagplaats***GroupProjectTemplate***.
- Maak de **GroupUtilities** opslagplaats op de server VSTS onder teamproject **GroupCommon**. 

Instructies voor de eerste taak zijn opgenomen in deze sectie na opmerkingen over naamgevingsregels of onze opslagplaatsen en mappen. De instructies voor de tweede taak zijn opgenomen in de volgende sectie voor stap 4.

### <a name="rename-the-default-groupcommon-repository"></a>Wijzig de naam van de standaard GroupCommon opslagplaats

Naam wijzigen van de standaard **GroupCommon** opslagplaats als *GroupProjectTemplate* (genoemd **R1** in deze zelfstudie):
    
- Klik op **samenwerken op code** op de **GroupCommon** pagina team-project. Hiermee gaat u naar de pagina standaard Git-opslagplaats van het teamproject **GroupCommon**. Deze Git-opslagplaats is momenteel leeg. 

![10](./media/group-manager-tasks/rename-groupcommon-repo-3.png)
        
- Klik op **GroupCommon** in de linkerbovenhoek (gemarkeerd met een rood kader in de volgende afbeelding) op de pagina van de Git-opslagplaats van **GroupCommon** en selecteer **opslagplaatsenbeheren**(gemarkeerd met een groene vak in de volgende afbeelding). Deze procedure wordt de **CONFIGURATIESCHERM**. 
- Selecteer de **versiebeheer** tabblad van uw teamproject. 

![11](./media/group-manager-tasks/rename-groupcommon-repo-4.png)

- Klik op de **...**  rechts van de **GroupCommon** opslagplaats op het linkerdeelvenster en selecteer **Rename-opslagplaats**. 

![12](./media/group-manager-tasks/rename-groupcommon-repo-5.png)
        
- In de **Wijzig de naam van de opslagplaats GroupCommon** wizard die POP's, voer *GroupProjectTemplate* in de **naam** vak en klik vervolgens op **wijzigen** . 

![13](./media/group-manager-tasks/rename-groupcommon-repo-6.png)



## <a name="5-seed-the-r1--r2-repositories-on-the-vsts-server"></a>5. De R1 & R2-opslagplaatsen op de server VSTS seed

In deze fase van de procedure die u seed de *GroupProjectTemplate* (R1) en *GroupUtilities* (R2)-opslagplaatsen die u hebt ingesteld in de vorige sectie. Deze opslagplaatsen zijn gemaakt met de ***ProjectTemplate*** (**G1**) en ***hulpprogramma's*** (**G2**) opslagplaatsen die worden beheerd door Microsoft voor het Team gegevens wetenschap proces. Wanneer deze seeding is voltooid:

- uw opslagplaats R1 is vereist, hebben dezelfde set met mappen en documentsjablonen die de G1
- de R2-opslagplaats zal bevatten de set gegevens wetenschappelijke hulpprogramma's die zijn ontwikkeld door Microsoft.

De seeding procedure gebruikt de mappen op uw lokale DSVM als tussenliggende staging sites. Hier volgen de stappen in deze sectie:

- G1 & G2 - gekopieerd -> LG1 & LG2
- R1 & R2 - gekopieerd -> LR1 & LR2
- LG1 & LG2 - bestanden gekopieerd naar -> LR1 & LR2
- (Optioneel) aanpassing van LR1 & LR2
- LR1 & LR2 - inhoud toevoegen -> R1 & R2


### <a name="clone-g1--g2-repositories-to-your-local-dsvm"></a>G1 & G2 opslagplaatsen voor uw lokale DSVM klonen

In deze stap maakt u een Team gegevens wetenschap proces (TDSP) ProjectTemplate opslagplaats (G1) en hulpprogramma's (G2) van de TDSP github-opslagplaatsen voor mappen klonen in uw lokale DSVM als LG1 en LG2:

- Maak een map als de hoofdmap voor het hosten van uw klonen van de opslagplaatsen. 
    -  Maak een map in de Windows-DSVM *C:\GitRepos\TDSPCommon*. 
    -  Maak een map in de DSVM Linux *GitRepos\TDSPCommon* in de basismap. 

- Voer de volgende reeks opdrachten uit de *GitRepos\TDSPCommon* directory.

    `git clone https://github.com/Azure/Azure-TDSP-ProjectTemplate`<br>
    `git clone https://github.com/Azure/Azure-TDSP-Utilities`
        
![14](./media/group-manager-tasks/two-folder-cloned-from-TDSP-windows.PNG)

- Met de namen van onze afgekorte opslagplaats, is dit wat deze scripts hebben bereikt: 
    - G1 - gekloond in LG1 ->
    - G2 - gekloond in LG2 ->
- Nadat het klonen is voltooid, moet u kunnen zien van twee mappen _ProjectTemplate_ en _hulpprogramma's_onder **GitRepos\TDSPCommon** directory. 

### <a name="clone-r1--r2-repositories-to-your-local-dsvm"></a>R1 & R2-opslagplaatsen op uw lokale DSVM klonen

In deze stap maakt u de opslagplaats GroupProjectTemplate (R1) en GroupUtilities opslagplaats (R2) op de lokale mappen (waarnaar wordt verwezen als LR1 en LR2, respectievelijk) onder klonen **GitRepos\GroupCommon** op uw DSVM.

- Als u de URL's van de opslagplaatsen R1 en R2, gaat u naar uw **GroupCommon** introductiepagina op VSTS. Dit heeft meestal de URL *https://\<naam van uw Server VSTS\>.visualstudio.com/GroupCommon*. 
- Klik op **CODE**. 
- Kies de **GroupProjectTemplate** en **GroupUtilities** opslagplaatsen. Kopiëren en opslaan van elk van de URL's (HTTPS voor Windows; SSH voor Linux) van de **kloon-URL** element daarmee ook voor gebruik in de volgende scripts:  

![15](./media/group-manager-tasks/find_https_ssh_2.PNG)

- Wijzigen in de **GitRepos\GroupCommon** map op uw Windows- of Linux DSVM en voer een van de volgende sets opdrachten voor het klonen van R1 en R2 in die map.
        
Hier volgen de Windows- en Linux-scripts:

    # Windows DSVM

    git clone <the HTTPS URL of the GroupProjectTemplate repository>
    git clone <the HTTPS URL of the GroupUtilities repository>

![16](./media/group-manager-tasks/clone-two-empty-group-reo-windows-2.PNG)

    # Linux DSVM

    git clone <the SSH URL of the GroupProjectTemplate repository>
    git clone <the SSH URL of the GroupUtilities repository>

![17](./media/group-manager-tasks/clone-two-empty-group-reo-linux-2.PNG)

>[AZURE.NOTE] Naar verwachting ontvangen waarschuwingsberichten LR1 en LR2 leeg zijn.    

- Met de namen van onze afgekorte opslagplaats, is dit wat deze scripts hebben bereikt: 
    - R1 - gekloond in LR1 ->
    - R2 - gekloond in LR2 ->   


### <a name="seed-your-groupprojecttemplate-lr1-and-grouputilities-lr2"></a>Seed-uw GroupProjectTemplate (LR1) en GroupUtilities (LR2)

Vervolgens in de lokale computer, Kopieer de inhoud van mappen voor ProjectTemplate en hulpprogramma's (met uitzondering van de metagegevens in de mappen .git) onder GitRepos\TDSPCommon naar uw adreslijsten GroupProjectTemplate en GroupUtilities onder **GitRepos\ GroupCommon**. Hier volgen de twee taken in deze stap voltooien:

- Kopieer de bestanden in GitRepos\TDSPCommon\ProjectTemplate (**LG1**) naar GitRepos\GroupCommon\GroupProjectTemplate (**LR1**) 
- Kopieer de bestanden in GitRepos\TDSPCommon\Utilities (**LG2** naar GitRepos\GroupCommon\Utilities (**LR2**). 

Voor deze twee taken, de volgende scripts worden uitgevoerd in PowerShell-console (Windows) of Shell-script console (Linux). U wordt gevraagd voor het invoeren van de volledige paden naar LG1, LR1 LG2 en LR2. De ingevoerde paden worden gevalideerd. Wanneer u een map die niet bestaat, wordt u gevraagd voor het opnieuw invoeren. 

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
        
Nu ziet u dat de bestanden in de twee mappen (met uitzondering van bestanden in de map .git) respectievelijk naar GroupProjectTemplate en GroupUtilities worden gekopieerd.
    
![21](./media/group-manager-tasks/copy-two-folder-to-group-folder-linux.PNG)

- Met de namen van onze afgekorte opslagplaats, is dit wat deze scripts hebben bereikt:
    - LG1 - bestanden gekopieerd naar-LR1 >
    - LG2 - bestanden gekopieerd naar-LR2 >

### <a name="option-to-customize-the-contents-of-lr1--lr2"></a>Optie voor het aanpassen van de inhoud van LR1 & LR2
    
Als u aanpassen van de inhoud van LR1 en LR2 om te voldoen aan de specifieke behoeften van uw groep wilt, is dit de fase van de procedure waar dat geschikt is. U kunt wijzigen van de sjabloon-documenten, wijzigt de mapstructuur en bestaande hulpprogramma's die uw groep heeft ontwikkeld of die handig zijn voor uw hele groep toevoegen. 

### <a name="add-the-contents-in-lr1--lr2-to-r1--r2-on-group-server"></a>De inhoud in de LR1 & LR2 toevoegen aan R1 & R2 op de server voor groep

Nu moet u de inhoud in LR1 en LR2 toevoegen aan opslagplaatsen R1 en R2. Hier volgen de git bash-opdrachten die u in Windows PowerShell- of Linux uitvoeren kunt. 

Voer de volgende opdrachten vanuit de map GitRepos\GroupCommon\GroupProjectTemplate:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

De optie -m kunt u een bericht voor de git-doorvoer instellen.

![22](./media/group-manager-tasks/push-to-group-server-2.PNG)

U kunt zien dat in uw groep VSTS-server in de opslagplaats GroupProjectTemplate de bestanden zijn gesynchroniseerd onmiddellijk.

![23](./media/group-manager-tasks/push-to-group-server-showed-up-2.PNG)

Ten slotte wijzigen in de **GitRepos\GroupCommon\GroupUtilities** directory en voer dezelfde set git bash-opdrachten:

    git status
    git add .
    git commit -m"push from DSVM"
    git push

>[AZURE.NOTE] Als dit de eerste keer dat u een Git-opslagplaats vastleggen, moet u globale parameters configureren *user.name* en *user.email* voordat u de `git commit` opdracht. Voer de volgende twee opdrachten:
        
    git config --global user.name <your name>
    git config --global user.email <your email address>
 
>Als u gegevens worden doorgevoerd naar meerdere Git-opslagplaatsen, gebruikt u dezelfde naam en e-mailadres, wanneer u ze elk doorvoert. Met behulp van dezelfde naam en e-mailadres bewijst handige later op als u Power BI-dashboards voor de Git-activiteiten bijhouden op meerdere opslagplaatsen bouwen.


- Met de namen van onze afgekorte opslagplaats, is dit wat deze scripts hebben bereikt:
    - LR1 - inhoud toevoegen -> R1
    - LR2 - inhoud toevoegen -> R2

## <a name="6-add-group-members-to-the-group-server"></a>6. Leden van de beveiligingsgroep toevoegen aan de groep-server

Startpagina van de groep VSTS server, klik op de **tandwielpictogram pictogram** naast uw gebruikersnaam in de rechterbovenhoek, selecteer de **beveiliging** tabblad. U kunt leden toevoegen aan uw groep met verschillende machtigingen.

![24](./media/group-manager-tasks/add_member_to_group.PNG) 


## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de rollen en taken die zijn gedefinieerd door het Team gegevens wetenschap proces:

- [Groep Manager-taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md)
- [Team Lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)
- [Projecttaken Lead voor een wetenschappelijke Gegevensteam](project-lead-tasks.md)
- [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md)