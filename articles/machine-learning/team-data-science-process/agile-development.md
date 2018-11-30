---
title: Flexibele ontwikkeling van wetenschappelijke gegevensprojecten - Azure Machine Learning | Microsoft Docs
description: Hoe kunnen ontwikkelaars een data science-project in een systematische, versie beheerd en gezamenlijke manier binnen een projectteam uitvoeren met behulp van het Team Data Science Process.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: 9fd8714e4c9fdc89036a3b05ba835b140363c0e3
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52443369"
---
# <a name="agile-development-of-data-science-projects"></a>Flexibele ontwikkeling van wetenschappelijke gegevensprojecten

Dit document wordt beschreven hoe ontwikkelaars een data science-project in een systematische, versie beheerd en gezamenlijke manier binnen een projectteam uitvoeren kunnen met behulp van de [Team Data Science Process](overview.md) (TDSP). De TDSP is een framework ontwikkeld door Microsoft en die een gestructureerde opeenvolging van activiteiten biedt voor het efficiënt uitvoeren van cloud gebaseerde predictive analytics-oplossingen. Zie voor een overzicht van de rollen personeel en de bijbehorende taken die worden beheerd door een data science team standaardiseren over dit proces, [Team Data Science Process rollen en taken](roles-tasks.md). 

In dit artikel bevat instructies over het: 

1. Voer **sprint planning** voor werkitems die betrokken zijn bij een project.<br> Als u niet bekend met sprint plannen bent, vindt u meer informatie en algemene informatie [hier](https://en.wikipedia.org/wiki/Sprint_(software_development) "hier"). 
2. **werkitems toevoegen** naar sprints. 

> [!NOTE]
> De stappen die nodig zijn voor het instellen van een TDSP-team-omgeving met Azure DevOps-Services worden beschreven in de volgende reeks instructies. Ze geven over het uitvoeren van deze taken met Azure DevOps-Services, omdat dat het implementeren van TDSP bij Microsoft.  Als u ervoor kiest het gebruik van Azure DevOps-Services, items (3) en (4) in de vorige lijst zijn voordelen die u op een natuurlijke manier krijgt. Als een andere code die als host fungeert platform wordt gebruikt voor uw groep, worden de taken die moeten worden uitgevoerd door de teamleider in het algemeen niet wijzigen. Maar de manier om deze taken uit te voeren afwijken. Bijvoorbeeld, het item in de sectie 6 **koppelen van een werkitem met een Git-branch**, mogelijk niet zo eenvoudig als het is Azure DevOps-Services.
>
>

De volgende afbeelding toont een typische sprint plannen, coderen en bronbeheer werkstroom betrokken bij het implementeren van een data science-project:

![1](./media/agile-development/1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologie 

In de TDSP sprint planning framework, zijn er vier veelgebruikte typen **werkitems**: **functie**, **gebruiker verhaal**, **taak**, en **Bug**. Elk project heeft één achterstallige voor alle werkitems. Er is geen achterstand op het niveau van de Git-opslagplaats onder een project. Hier vindt u de definities:

- **Functie**: een functie komt overeen met de betrokkenheid van een project. Verschillende engagements met een client worden beschouwd als verschillende functies. Op deze manier is het beste rekening houden met verschillende fasen van een project met een client als andere functies. Als u ervoor een schema zoals kiest ***ClientName EngagementName*** als naam van uw functies vervolgens u gemakkelijk kunt herkennen de context van het project/engagement uit de namen van zichzelf.
- **Verhaal**: verhalen van andere werkitems die nodig zijn om uit te voeren van een functie (project) end-to-end zijn. Voorbeelden van artikelen zijn:
    - Ophalen van gegevens 
    - Gegevens verkennen 
    - Functies genereren
    - Modellen ontwikkelen
    - Tot het operationaliseren van modellen 
    - Opnieuw trainen van modellen
- **Taak**: taken zijn toewijsbare code of het document werkitems of andere activiteiten die worden uitgevoerd moeten voor het voltooien van een bepaald verhaal. Bijvoorbeeld, taken in het artikel *gegevens ophalen* kan zijn:
    -  Ophalen van referenties van SQL Server 
    -  Uploaden van gegevens naar SQL datawarehouse. 
- **Bug**: Bugs meestal verwijzen naar oplossingen die nodig zijn voor een bestaande code of het document die worden uitgevoerd wanneer een taak is voltooid. Als de fout wordt veroorzaakt door ontbrekende respectievelijk de fasen of taken, kan het escaleren naar een artikel of een taak wordt. 

> [!NOTE]
> Concepten worden van functies, verhalen, taken en fouten van beheer van software-code (SCM) moet worden gebruikt in de data science geleend. Ze kunnen enigszins afwijken van de conventionele SCM-definities.
>
>

> [!NOTE]
> Gegevenswetenschappers het gevoel meer ervaring hebt met een flexibele-sjabloon die specifiek worden uitgelijnd met de fasen van de TDSP-levenscyclus. Met die in rekening met is een Agile-afgeleide sprint planningssjabloon gemaakt, waarbij Epics, enz., verhalen zijn vervangen door de fasen in het levenscyclusbeheer TDSP of substages. Zie voor instructies over het maken van een sjabloon voor flexibele [flexibele datatechnologisch proces in Visual Studio Online instellen](agile-development.md#set-up-agile-dsp-6).
>
>

## 2. <a name='SprintPlanning-2'></a>Sprint plannen 

Sprint planning is handig voor prioriteitsaanduiding project, en resourceplanning en -toewijzing. Veel gegevensanalisten zijn energieconsumenten betrokken bij meerdere projecten, die elk maanden in beslag kan nemen. Projecten gaan vaak op verschillende spaties. Op het Azure DevOps-Services, kunt u eenvoudig maken, beheren, en bijhouden van werkitems in uw project en voeren sprint om ervoor te zorgen dat uw projecten zijn vooruit zoals verwacht. 

Ga als volgt [deze koppeling](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) voor de stapsgewijze instructies voor sprint plannen in Azure DevOps-Services. 


## 3. <a name='AddFeature-3'></a>Een functie toevoegen  

Nadat de projectopslagplaats van uw wordt gemaakt onder een project, gaat u naar het team **overzicht** pagina en klik op **work beheren**.

![2](./media/agile-development/2-sprint-team-overview.png)

Als u wilt opnemen een functie in de achterstallige taken, klikt u op **achterstanden** --> **functies** --> **nieuw**, typt u de functie **titel**(meestal naam van uw project), en klik vervolgens op **toevoegen** .

![3](./media/agile-development/3-sprint-team-add-work.png)

Dubbelklik op de functie die u hebt gemaakt. Vul in de beschrijvingen, teamleden voor deze functie toewijzen en stel planning parameters voor deze functie. 

U kunt deze functie ook koppelen aan de projectopslagplaats. Klik op **koppeling toevoegen** onder de **ontwikkeling** sectie. Nadat u klaar bent met het bewerken van de functie, klikt u op **opslaan en sluiten** om af te sluiten.


## 4. <a name='AddStoryunderfeature-4'></a>Verhaal onder de functie toevoegen 

Onder de functie kunnen verhalen worden toegevoegd om te beschrijven van belangrijke stappen die nodig zijn om het project (functie) te voltooien. Als u wilt een nieuw artikel toevoegen, klikt u op de **+** Meld u aan de linkerkant van de functie in de weergave van achterstand.  

![4](./media/agile-development/4-sprint-add-story.png)

U kunt de details van het artikel, zoals de status, beschrijving, opmerkingen, planning en prioriteit In het pop-upvenster bewerken.

![5](./media/agile-development/5-sprint-edit-story.png)

U kunt dit artikel koppelen aan een bestaande opslagplaats door te klikken op **+ koppeling toevoegen** onder **ontwikkeling**. 

![6](./media/agile-development/6-sprint-link-existing-branch.png)


## 5. <a name='AddTaskunderstory-5'></a>Een taak toevoegen aan een verhaal 

Taken worden specifieke gedetailleerde stappen die nodig zijn voor elk artikel. Nadat alle taken van een artikel zijn voltooid, moet het verhaal te worden voltooid. 

Als u wilt een taak aan een verhaal toevoegen, klikt u op de **+** aanmelding naast het verhaal item, selecteer **taak**, en vul vervolgens de gedetailleerde gegevens van deze taak in het pop-upvenster.

![7](./media/agile-development/7-sprint-add-task.png)

Nadat u de functies, verhalen en taken worden gemaakt, kunt u bekijken in de **achterstand** of **bord** weergaven voor het bijhouden van hun status.

![8](./media/agile-development/8-sprint-backlog-view.png)

![9](./media/agile-development/9-link-to-a-new-branch.png)


## 6. <a name='set-up-agile-dsp-6'></a> Instellen van een sjabloon voor het werk van flexibele TDSP in Visual Studio Online

In dit artikel wordt uitgelegd hoe het instellen van een flexibele data science-processjabloon die gebruikmaakt van de TDSP data science lifecycle fasen en werkitems met Visual Studio Online (vso) worden bijgehouden. De stappen hieronder een voorbeeld van het instellen van de data science-specifieke flexibele Walkthrough verwerken sjabloon *AgileDataScienceProcess* en ziet u hoe u data science-werkitems op basis van de sjabloon.

### <a name="agile-data-science-process-template-setup"></a>Installatie van flexibele Data Science Process sjabloon

1. Navigeer naar de startpagina van de server, **configureren** -> **proces**.

    ![10](./media/agile-development/10-settings.png) 

2. Navigeer naar **alle processen** -> **processen**onder **Agile** en klikt u op **maken overgenomen proces**. Vervolgens plaatst u de procesnaam 'AgileDataScienceProcess' en klik op **proces**.

    ![11](./media/agile-development/11-agileds.png)

3. Onder de **AgileDataScienceProcess** -> **typen werkitem** tabblad, uit te schakelen **Epic**, **functie**,  **Gebruiker verhaal**, en **taak** typen door werkitem **configureren -> uitschakelen**

    ![12](./media/agile-development/12-disable.png)

4. Navigeer naar **AgileDataScienceProcess** -> **niveaus achterstand** tabblad. Wijzig de naam 'Epics' naar 'TDSP projecten' door te klikken op de **configureren** -> **bewerken/naam**. Klik in het dialoogvenster dezelfde **+ nieuw type werkitem** in "Data Science-Project" en stel de waarde van **standaard werkitemtype** aan "TDSP-Project" 

    ![13](./media/agile-development/13-rename.png)  

5. Op deze manier achterstand naam 'Functies' wijzigen in 'TDSP fasen' en het volgende toevoegen aan de **nieuwe work item type**:

    - Inzicht in het bedrijf
    - Gegevens ophalen
    - Modelleren
    - Implementatie

6. Wijzig de naam 'Gebruiker verhaal' in 'TDSP Substages"met type werkitem standaard ingesteld op de zojuist gemaakte"TDSP Substage'-type.

7. De 'taken' voor het zojuist gemaakte werkitemtype 'TDSP-taak' instellen 

8. Na deze stappen de niveaus achterstand als volgt uitzien:

    ![14](./media/agile-development/14-template.png)  

 
### <a name="create-data-science-work-items"></a>Data Science-werkitems maken

Nadat de sjabloon voor data science process is gemaakt, kunt u maken en Volg uw data science-werkitems die overeenkomen met de TDSP-levenscyclus.

1. Wanneer u een nieuw project maakt, selecteert u 'Agile\AgileDataScienceProcess' als de **proces van het werkitem**:

    ![15](./media/agile-development/15-newproject.png)

2. Navigeer naar het nieuwe project en klik op **werk** -> **achterstanden**.

3. 'TDSP projecten' zichtbaar maken door te klikken op **configureren van instellingen voor team** en controleer "TDSP projecten"; Sla.

    ![16](./media/agile-development/16-enabledsprojects.png)

4. U kunt nu beginnen met het maken van de data science-specifieke werkitems.

    ![17](./media/agile-development/17-dsworkitems.png)

5. Hier volgt een voorbeeld van hoe de werkitems van data science-project moeten worden weergegeven:

    ![18](./media/agile-development/18-workitems.png)


## <a name="next-steps"></a>Volgende stappen

[Gezamenlijke code met Git](collaborative-coding-with-git.md) wordt beschreven hoe u gezamenlijke code-ontwikkeling voor data science projecten met Git gebruiken als de gedeelde code-webontwikkelingsframework doet en hoe u deze activiteiten op het werk gepland met de flexibele proces coderen koppelen.

Hier vindt u aanvullende koppelingen naar bronnen van flexibele processen.

- Flexibele proces   [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process)
- Flexibele proces werkzaamheden-itemtypen en werkstroom   [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow)


Scenario's die laten zien van alle de stappen in het proces voor het **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en die is gekoppeld met miniaturen beschrijvingen in de [voorbeeld walkthroughs](walkthroughs.md) artikel. Ze laten zien hoe u naar de cloud, on-premises hulpprogramma's en services combineren in een werkstroom of een pijplijn te maken van een intelligente toepassingen. 
