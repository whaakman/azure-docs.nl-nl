---
title: De uitvoering van de gegevens wetenschappelijke projecten - Azure | Microsoft Docs
description: Hoe een wetenschappelijk gegevens een wetenschappelijke gegevensproject kunt uitvoeren in een herleidbare versie beheerd en gezamenlijke manier.
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
ms.date: 11/16/2017
ms.author: bradsev;
ms.openlocfilehash: 1015a9f24ca2c175ff367b1748f05bb3e464457f
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="execution-of-data-science-projects"></a>De uitvoering van de gegevens wetenschappelijke projecten

Dit document wordt beschreven hoe ontwikkelaars een wetenschappelijke gegevensproject in een systematische, versie beheerd en gezamenlijke manier binnen een projectteam uitvoeren kunnen met behulp van de [Team gegevens wetenschap proces](overview.md) (TDSP). De TDSP ligt een framework dat is ontwikkeld door Microsoft, met een gestructureerd opeenvolging van activiteiten uit te voeren van cloud-gebaseerde, predictive analytics-oplossingen efficiënt biedt. Zie voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een gegevens wetenschappelijke team standaardiseren op dit proces [Team gegevens wetenschap proces rollen en taken](roles-tasks.md). 

In dit artikel bevat instructies voor het: 

1. Voer **sprint planning** voor werkitems die zijn betrokken bij een project.<br> Als u niet bekend met sprint planning bent, vindt u meer informatie en algemene informatie [hier](https://en.wikipedia.org/wiki/Sprint_(software_development) "hier"). 
2. **toevoegen van werkitems** naar sprints.
3. **koppelen van de werkitems met behulp van activiteiten coderingen** met git wordt bijgehouden.
4. Voer **code revisie**. 

> [!NOTE]
> De stappen die nodig zijn voor het instellen van een omgeving met TDSP team met behulp van Visual Studio Team Services (VSTS) worden beschreven in de volgende set van instructies. Ze bepalen hoe deze taken met VSTS omdat dat het implementeren van TDSP bij Microsoft.  Als u wilt gebruiken VSTS, artikelen (3) en (4) in de vorige lijst zijn de voordelen van natuurlijk. Als een andere code platform die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de lead team in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende. Bijvoorbeeld: het item in de sectie 6 **koppelen van een werkitem met een vertakking git**, mogelijk niet zo eenvoudig als het zich op VSTS.
>
>

De volgende afbeelding ziet u een typische sprint plannen, coderen en bronbeheer werkstroom betrokken bij de uitvoering van een wetenschappelijke gegevensproject:

![1](./media/project-execution/project-execution-1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologie 

In de TDSP sprint planning framework, zijn er vier veelgebruikte typen **werkitems**: **functie**, **gebruiker verhaal**, **taak**, en **Bug**. Elk teamproject onderhoudt een enkele achterstand voor alle werkitems. Er is geen achterstand op het niveau van de git-opslagplaats onder een teamproject. Hier volgen de definities:

- **Functie**: een functie komt overeen met een project engagement. Andere betrokkenheid met een client worden beschouwd als verschillende functies. Daarnaast is het beste rekening houden met verschillende fasen van een project als verschillende functies met een client. Als u een schema zoals ***ClientName EngagementName*** als naam van uw functies vervolgens u gemakkelijk kunt herkennen de context van het project/engagement uit de namen van zichzelf.
- **Artikel**: artikelen zijn andere werkitems die nodig zijn voor het voltooien van een functie (project) end-to-end. Voorbeelden van artikelen zijn:
    - Ophalen van gegevens 
    - Gegevens verkennen 
    - Genereren van functies
    - Modellen opzetten
    - Operationele modellen 
    - Modellen retraining
- **Taak**: taken zijn toegewezen code of het document werkitems of andere activiteiten die worden uitgevoerd moeten voor het voltooien van een specifieke artikel. Bijvoorbeeld: taken in het artikel *gegevens ophalen van* kan zijn:
    -  Ophalen van referenties van SQL Server 
    -  Het uploaden van gegevens naar SQL datawarehouse. 
- **Fout**: Bugs meestal verwijzen naar oplossingen die nodig zijn voor een bestaande code of het document die worden uitgevoerd als een taak is voltooid. Als de fout wordt veroorzaakt door ontbrekende stadia of taken respectievelijk, kan het escaleren naar een artikel of een taak. 

> [!NOTE]
> Concepten worden van functies, artikelen, taken en fouten van beheer van software-code (SCM) moet worden gebruikt in gegevenswetenschap geleend. Ze kunnen enigszins verschillen van de conventionele SCM-definities.
>
>

Gegevenswetenschappers denken wellicht meer vertrouwd met een flexibele-sjabloon die specifiek wordt uitgelijnd met de fasen van het levenscyclusbeheer TDSP. Met daarom is een Agile afgeleid sprint planning van de sjabloon gemaakt, waarbij Epics, enz., artikelen zijn vervangen door de fasen van het levenscyclusbeheer TDSP of substages. Documentatie over het maken van een flexibele sjabloon vindt [hier](https://msdata.visualstudio.com/AlgorithmsAndDataScience/TDSP/_git/TDSP?path=%2FDocs%2Fteam-data-science-process-agile-template.md&version=GBxibingao&_a=preview).


##  2. <a name='SprintPlanning-2'></a>Sprint plannen 

Sprint planning is handig voor het project prioriteitsaanduiding, en resourceplanning en toewijzing. Veel gegevenswetenschappers zijn die zich bezighoudt met meerdere projecten, die elk maanden kan duren. Projecten gaan vaak op verschillende spaties. Op de server VSTS kunt u eenvoudig maken, beheren, en bijhouden van werkitems in uw teamproject en voeren sprint plannen om ervoor te zorgen dat uw projecten zijn vooruitgang zoals verwacht. 

Ga als volgt [deze koppeling](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) voor de stapsgewijze instructies voor planning in VSTS sprint. 


##  3. <a name='AddFeature-3'></a>Een functie toevoegen  

Nadat de opslagplaats van uw project wordt gemaakt onder een teamproject, gaat u naar het team **overzicht** pagina en klik op **werk beheren**.

![2](./media/project-execution/project-execution-2-sprint-team-overview.png)

Als u wilt opnemen een functie in de achterstand, klikt u op **achterstanden** --> **functies** --> **nieuw**, typt u de functie **titel**(meestal de projectnaam), en klik vervolgens op **toevoegen** .

![3](./media/project-execution/project-execution-3-sprint-team-add-work.png)

Dubbelklik op de functie die u hebt gemaakt. Vul in de beschrijvingen, teamleden voor deze functie toewijzen en parameters voor deze functie planning instellen. 

U kunt deze functie ook koppelen aan de project-opslagplaats. Klik op **koppeling toevoegen** onder de **ontwikkeling** sectie. Nadat u klaar bent met de functie bewerken, klikt u op **opslaan en sluiten** om af te sluiten.


##  4. <a name='AddStoryunderfeature-4'></a>Verhaal onder functie toevoegen 

Onder de functie kunnen artikelen worden toegevoegd om te beschrijven van belangrijke stappen die nodig zijn om het project (functie) te voltooien. Als u wilt een nieuw artikel toevoegen, klikt u op de  **+**  Meld u aan de linkerkant van de functie in de weergave van achterstand.  

![4](./media/project-execution/project-execution-4-sprint-add-story.png)

U kunt de details van het artikel, zoals de status, beschrijving, opmerkingen, planning en prioriteit In het pop-upvenster bewerken.

![5](./media/project-execution/project-execution-5-sprint-edit-story.png)

U kunt dit artikel kunt koppelen aan een bestaande opslagplaats door te klikken op **+ koppeling toevoegen** onder **ontwikkeling**. 

![6](./media/project-execution/project-execution-6-sprint-link-existing-branch.png)


##  5. <a name='AddTaskunderstory-5'></a>Een taak toevoegt aan een artikel 

Taken zijn specifieke gedetailleerde stappen die nodig zijn voor elk artikel. Nadat alle taken van een artikel zijn voltooid, moet het artikel te worden voltooid. 

Een taak wilt toevoegen aan een artikel, klikt u op de  **+**  aanmelding naast het item verhaal, selecteer **taak**, en vul vervolgens de gedetailleerde gegevens van deze taak in het pop-upvenster.

![7](./media/project-execution/project-execution-7-sprint-add-task.png)

Nadat u de functies, artikelen en taken worden gemaakt, kunt u weergeven in de **achterstand** of **mededelingenbord** weergaven voor de status ervan bijhouden.

![8](./media/project-execution/project-execution-8-sprint-backlog-view.png)

![9](./media/project-execution/project-execution-9-link-to-a-new-branch.png)


##  6. <a name='Linkaworkitemwithagitbranch-6'></a>Een werkitem met een vertakking git koppelen 

VSTS biedt een handige manier een werkitem (een artikel of taak) verbinding maken met een vertakking git. Hiermee kunt u uw artikel of de taak directe koppeling naar de code die is gekoppeld. 

Voor een werkitem verbinding met een nieuwe vertakking, dubbelklikt u op een werkitem en klik in het pop-upvenster op **maakt u een nieuwe vertakking** onder **+ koppeling toevoegen**.  

![10](./media/project-execution/project-execution-10-sprint-board-view.png)

Geef de gegevens voor deze nieuwe vertakking, zoals de naam van het filiaal, base git-opslagplaats en de vertakking. De git-opslagplaats gekozen moet de opslagplaats onder de dezelfde teamproject dat het werkitem behoort. De base vertakking is de hoofdvertakking of andere bestaande vertakken.

![11](./media/project-execution/project-execution-11-create-a-branch.png)

Een aanbevolen procedure is het maken van een vertakking git voor elk artikel werkitem. Vervolgens maakt u voor elk Taakwerkitem een vertakking op basis van de vertakking verhaal. De vertakkingen ordenen op deze hiërarchische manier die overeenkomt met de relaties verhaal-taak is handig als er meerdere personen op andere artikelen van hetzelfde project werkt of er meerdere personen op andere taken van hetzelfde artikel werkt. Conflicten kunnen worden geminimaliseerd wanneer elk teamlid werkt op een andere vertakking en wanneer elk lid op verschillende codes of andere artefacten werkt bij het delen van een vertakking. 

De volgende afbeelding ziet u de aanbevolen vertakkende strategie voor TDSP. U moet u mogelijk niet als veel filialen zoals zijn hier, vooral wanneer u alleen hebt een of twee mensen werken op hetzelfde project of slechts één persoon voor alle taken van een artikel werkt. Maar scheiden van de vertakking van de ontwikkeling van de hoofdvertakking is altijd een goede gewoonte. Dit kan helpen voorkomen dat de vertakking release wordt onderbroken door de van ontwikkelingsactiviteiten. Gedetailleerde beschrijving van git vertakking model vindt u in [een geslaagde Git vertakking Model](http://nvie.com/posts/a-successful-git-branching-model/).

![12](./media/project-execution/project-execution-12-git-branches.png)

Als u wilt overschakelen naar de vertakking die u wilt werken, moet u de volgende opdracht uitvoeren in een shell-opdracht (Windows of Linux). 

    git checkout <branch name>

Het wijzigen van de *< Filiaalnaam\>*  naar **master** switches u terug naar de **master** vertakking. Nadat u naar de vertakking werkt overschakelt, kunt u op dat het werkitem werkt, ontwikkelen van de code of documentatie artefacten nodig voor het voltooien van het item kunt starten. 

U kunt ook een werkitem koppelen aan een bestaande vertakking. In de **Detail** pagina van een werkitem, in plaats van te klikken op **maakt u een nieuwe vertakking**, u klikt op **+ koppeling toevoegen**. Selecteer vervolgens de vertakking die u wilt het werkitem koppelen. 

![13](./media/project-execution/project-execution-13-link-to-an-existing-branch.png)

U kunt ook een nieuwe vertakking in git bash opdrachten maken. Als < naam base filiaal\> ontbreekt, de < naam van de nieuwe filiaal\> is gebaseerd op _master_ vertakking. 
    
    git checkout -b <new branch name> <base branch name>


##  7. <a name='WorkonaBranchandCommittheChanges-7'></a>Werken met een vertakking en de wijzigingen worden doorgevoerd 

Nu Stel dat u een wijziging in de *gegevens\_opname* vertakking voor het werkitem, zoals het toevoegen van een R-bestand op de vertakking in uw lokale computer. U kunt het R-bestand dat is toegevoegd aan de vertakking voor dit werkitem doorvoeren op voorwaarde dat u in dat filiaal de Git-shell, met de volgende Git-opdrachten:

    git status
    git add .
    git commit -m"added a R scripts"
    git push origin data_ingestion

![14](./media/project-execution/project-execution-14-sprint-push-to-branch.png)

##  8. <a name='CreateapullrequestonVSTS-8'></a>Maak een pull-aanvraag op VSTS 

Wanneer u klaar na een paar doorvoeracties en pushes bent voor het samenvoegen van de huidige vertakking in de base vertakking, kunt u indienen een **pull-aanvraag** op VSTS-server. 

Ga naar de hoofdpagina van uw teamproject en klik op **CODE**. Selecteer de vertakking worden samengevoegd en de naam van de git-opslagplaats die u wilt samenvoegen van de vertakking in. Klik vervolgens op **Pull-aanvragen**, klikt u op **nieuwe pull-aanvraag** te maken van een beoordeling pull-aanvraag voordat het werk op de vertakking is samengevoegd in de base vertakking.

![15](./media/project-execution/project-execution-15-spring-create-pull-request.png)

Vul in een beschrijving van deze pull-aanvraag, revisoren toevoegen en verzend.

![16](./media/project-execution/project-execution-16-spring-send-pull-request.png)

##  9. <a name='ReviewandMerge-9'></a>Bekijk en samenvoegen 

Als de pull-aanvraag is gemaakt, krijgt uw revisoren een e-mailbericht voor het controleren van de pull-aanvragen. Revisoren moeten controleren of de wijzigingen of niet werkt, en indien mogelijk wijzigingen met de aanvrager testen. Op basis van hun beoordeling, kunnen de revisoren goedkeuren of afwijzen van de pull-aanvraag. 

![17](./media/project-execution/project-execution-17-add_comments.png)

![18](./media/project-execution/project-execution-18-spring-approve-pullrequest.png)

Nadat de controle is voltooid, de werkvertakking wordt samengevoegd aan de basis vertakking door te klikken op de **Complete** knop. U kunt de werkvertakking verwijderen nadat deze is samengevoegd. 

![19](./media/project-execution/project-execution-19-spring-complete-pullrequest.png)

Bevestig op de linkerbovenhoek die de aanvraag is gemarkeerd als **voltooid**. 

![20](./media/project-execution/project-execution-20-spring-merge-pullrequest.png)

Wanneer u teruggaan naar de opslagplaats onder **CODE**, wordt vermeld dat u hebt is overgeschakeld naar de hoofdvertakking.

![21](./media/project-execution/project-execution-21-spring-branch-deleted.png)

U kunt ook de volgende Git-opdrachten gebruiken de werkvertakking verwijderen na het samenvoegen van en uw werkvertakking aan de basis vertakking samenvoegen:

    git checkout master
    git merge data_ingestion
    git branch -d data_ingestion

![22](./media/project-execution/project-execution-22-spring-branch-deleted-commandline.png)


##  10. <a name='DataQualityReportUtility-10'></a>Interactieve Gegevensverkenning, analyse en rapportage (IDEAR)-hulpprogramma

Dit hulpprogramma R op basis van markdown biedt een flexibele en interactieve hulpprogramma om te evalueren en gegevenssets te verkennen. Gebruikers kunnen snel rapporten genereren van de gegevensset met minimale coderen. Gebruikers kunnen klikken op knoppen waarmee u kunt de exploratie resultaten in het hulpprogramma voor interactieve exporteren naar een definitieve rapport die kan worden geleverd aan clients of gebruikt voor het nemen van beslissingen over welke variabelen moeten worden opgenomen in de volgende modellering stap.

Op dit moment werkt het hulpprogramma alleen op gegevensframes in het geheugen. Een .yaml-bestand is nodig om op te geven van de parameters van de gegevensset worden onderzocht. Zie voor meer informatie [IDEAR in hulpprogramma's TDSP gegevens wetenschappelijke](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


##  11. <a name='ModelingUtility-11'></a>Basislijn modelleren en rapportage van hulpprogramma

Dit hulpprogramma voorziet in een aanpasbare, semi geautomatiseerde hulpprogramma model maken met hyper-parameter verstrekkende op uitvoeren en het vergelijken van de nauwkeurigheid van deze modellen. 

Het model maken van het hulpprogramma is een R markdown-bestand dat kan worden uitgevoerd om ingesloten html-uitvoer met een tabel met inhoud voor navigatie door de verschillende secties produceren. Drie algoritmen worden uitgevoerd wanneer de markdown-bestand (knit) wordt uitgevoerd: overgegaan regressie met behulp van de glmnet inpakken, willekeurige forest met behulp van het pakket randomForest en versterking van structuren met het pakket xgboost). Elk van deze algoritmen produceert een getraind model. De nauwkeurigheid van deze modellen wordt vervolgens vergeleken en de functie voor relatieve belang waarnemingspunten worden gerapporteerd. Er zijn momenteel twee hulpprogramma's: een voor een classificatietaak binaire en één voor een taak regressie. De belangrijkste verschillen tussen deze twee parameters voor de manier is en nauwkeurigheid metrische gegevens voor deze taken learning zijn opgegeven. 

Een Yaml-bestand wordt gebruikt om op te geven:

- de gegevensinvoer (een SQL-bron of een R-bestand) 
- welk gedeelte van de gegevens wordt gebruikt voor training en welk gedeelte voor het testen
- welke algoritmen om uit te voeren 
- de keuze van besturingsparameters voor optimalisatie model:
    - kruisvalidatie 
    - uitvoeren van de bootstrap
    - vouwen van kruisvalidatie
- de hyper-parameter wordt ingesteld voor elk algoritme. 

Het nummer van algoritmen, het aantal vouwen voor optimalisatie, hyper-parameters en het aantal hyper-parametersets getrokken via kunnen ook worden gewijzigd in het bestand Yaml worden de modellen snel uitgevoerd. Ze kunnen bijvoorbeeld worden uitgevoerd met een kleiner aantal vouwen dat k/l, een kleiner aantal parametersets. Ze kunnen ook worden uitgevoerd meer uitvoerig met een hoger aantal vouwen dat k/l of een groter aantal parametersets, als die gerechtvaardigd is.

Zie voor meer informatie [geautomatiseerde modelleren hulpprogramma's en rapportage in hulpprogramma's TDSP gegevens wetenschappelijke](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).


##  12. <a name='PowerBI-12'></a>Voortgang van de projecten met Power BI-dashboards

Groepsbeheerders voor wetenschappelijke gegevens, team leads en project leads nodig om de voortgang van hun projecten te volgen, welke werk is uitgevoerd op deze en door wie en blijven op de takenlijsten. Als u VSTS gebruikt, zich u voor het bouwen van Power BI-dashboards om de activiteiten en de werkitems die zijn gekoppeld aan een Git-opslagplaats te houden. Zie voor meer informatie over de Power BI verbinden met Visual Studio Team Services [koppel Power BI aan Team Services](https://www.visualstudio.com/en-us/docs/report/powerbi/connect-vso-pbi-vs). 

Zie voor informatie over het maken van Power BI-dashboards en rapporten voor het bijhouden van uw activiteiten Git-opslagplaats en uw werkitems nadat de gegevens van VSTS is verbonden met Power BI, [maken Power BI-dashboards en rapporten](https://www.visualstudio.com/en-us/docs/report/powerbi/report-on-vso-with-power-bi-vs). 

Hier vindt u twee eenvoudige voorbeeld dashboards die zijn ontworpen voor het bijhouden van Git-activiteiten en werkitems. De git streven activiteiten worden weergegeven door andere gebruikers in het eerste voorbeeld-dashboard op verschillende datums en op andere opslagplaatsen. U kunt eenvoudig segmenteren en geanalyseerd om te filteren op de waarden die u geïnteresseerd bent in.

![23](./media/project-execution/project-execution-23-powerbi-git.png)

In het tweede voorbeeld-dashboard worden de werkitems in verschillende iteraties (artikelen en taken) weergegeven. Ze zijn gegroepeerd op toegewezen en prioriteitsniveaus en gekleurd met status.

![24](./media/project-execution/project-execution-24-powerbi-workitem.png)

 
## <a name="next-steps"></a>Volgende stappen

Volledige end-to-end-scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) artikel. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 

Zie voor voorbeelden van stappen uitvoeren in het Team gegevens wetenschappelijke processen die gebruikmaken van Azure Machine Learning Studio de [met Azure ML](http://aka.ms/datascienceprocess) leertraject.