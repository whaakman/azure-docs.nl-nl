---
title: Flexibel ontwikkelen van projecten met gegevens wetenschap - Azure Machine Learning | Microsoft Docs
description: Hoe kunnen ontwikkelaars een wetenschappelijke gegevensproject in een systematische, versie beheerd en gezamenlijke manier binnen een projectteam uitvoeren met behulp van het Team gegevens wetenschap proces.
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
ms.date: 11/28/2017
ms.author: bradsev;
ms.openlocfilehash: 686f751b241d49d116948711c683f4b504d5d5f9
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="agile-development-of-data-science-projects"></a>Flexibel ontwikkelen van gegevens wetenschappelijke projecten

Dit document wordt beschreven hoe ontwikkelaars een wetenschappelijke gegevensproject in een systematische, versie beheerd en gezamenlijke manier binnen een projectteam uitvoeren kunnen met behulp van de [Team gegevens wetenschap proces](overview.md) (TDSP). De TDSP ligt een framework dat is ontwikkeld door Microsoft, met een gestructureerd opeenvolging van activiteiten uit te voeren van cloud-gebaseerde, predictive analytics-oplossingen efficiënt biedt. Zie voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een gegevens wetenschappelijke team standaardiseren op dit proces [Team gegevens wetenschap proces rollen en taken](roles-tasks.md). 

In dit artikel bevat instructies voor het: 

1. Voer **sprint planning** voor werkitems die zijn betrokken bij een project.<br> Als u niet bekend met sprint planning bent, vindt u meer informatie en algemene informatie [hier](https://en.wikipedia.org/wiki/Sprint_(software_development) "hier"). 
2. **toevoegen van werkitems** naar sprints. 

> [!NOTE]
> De stappen die nodig zijn voor het instellen van een omgeving met TDSP team met behulp van Visual Studio Team Services (VSTS) worden beschreven in de volgende set van instructies. Ze bepalen hoe deze taken met VSTS omdat dat het implementeren van TDSP bij Microsoft.  Als u wilt gebruiken VSTS, artikelen (3) en (4) in de vorige lijst zijn de voordelen van natuurlijk. Als een andere code platform die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de lead team in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende. Bijvoorbeeld: het item in de sectie 6 **koppelen van een werkitem met een vertakking Git**, mogelijk niet zo eenvoudig als het zich op VSTS.
>
>

De volgende afbeelding ziet u een typische sprint plannen, coderen en bronbeheer werkstroom betrokken bij de uitvoering van een wetenschappelijke gegevensproject:

![1](./media/agile-development/1-project-execute.png)


##  1. <a name='Terminology-1'></a>Terminologie 

In de TDSP sprint planning framework, zijn er vier veelgebruikte typen **werkitems**: **functie**, **gebruiker verhaal**, **taak**, en **Bug**. Elk teamproject onderhoudt een enkele achterstand voor alle werkitems. Er is geen achterstand op het niveau van de Git-opslagplaats onder een teamproject. Hier volgen de definities:

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

> [!NOTE]
> Gegevenswetenschappers denken wellicht meer vertrouwd met een flexibele-sjabloon die specifiek wordt uitgelijnd met de fasen van het levenscyclusbeheer TDSP. Met daarom is een Agile afgeleid sprint planning van de sjabloon gemaakt, waarbij Epics, enz., artikelen zijn vervangen door de fasen van het levenscyclusbeheer TDSP of substages. Zie voor instructies over het maken van een flexibele sjabloon [flexibele gegevens wetenschap proces in Visual Studio Online instellen](agile-development.md#set-up-agile-dsp-6).
>
>

## 2. <a name='SprintPlanning-2'></a>Sprint plannen 

Sprint planning is handig voor het project prioriteitsaanduiding, en resourceplanning en toewijzing. Veel gegevenswetenschappers zijn die zich bezighoudt met meerdere projecten, die elk maanden kan duren. Projecten gaan vaak op verschillende spaties. Op de server VSTS kunt u eenvoudig maken, beheren, en bijhouden van werkitems in uw teamproject en voeren sprint plannen om ervoor te zorgen dat uw projecten zijn vooruitgang zoals verwacht. 

Ga als volgt [deze koppeling](https://www.visualstudio.com/en-us/docs/work/scrum/sprint-planning) voor de stapsgewijze instructies voor planning in VSTS sprint. 


## 3. <a name='AddFeature-3'></a>Een functie toevoegen  

Nadat de opslagplaats van uw project wordt gemaakt onder een teamproject, gaat u naar het team **overzicht** pagina en klik op **werk beheren**.

![2](./media/agile-development/2-sprint-team-overview.png)

Als u wilt opnemen een functie in de achterstand, klikt u op **achterstanden** --> **functies** --> **nieuw**, typt u de functie **titel**(meestal de projectnaam), en klik vervolgens op **toevoegen** .

![3](./media/agile-development/3-sprint-team-add-work.png)

Dubbelklik op de functie die u hebt gemaakt. Vul in de beschrijvingen, teamleden voor deze functie toewijzen en parameters voor deze functie planning instellen. 

U kunt deze functie ook koppelen aan de project-opslagplaats. Klik op **koppeling toevoegen** onder de **ontwikkeling** sectie. Nadat u klaar bent met de functie bewerken, klikt u op **opslaan en sluiten** om af te sluiten.


## 4. <a name='AddStoryunderfeature-4'></a>Verhaal onder functie toevoegen 

Onder de functie kunnen artikelen worden toegevoegd om te beschrijven van belangrijke stappen die nodig zijn om het project (functie) te voltooien. Als u wilt een nieuw artikel toevoegen, klikt u op de  **+**  Meld u aan de linkerkant van de functie in de weergave van achterstand.  

![4](./media/agile-development/4-sprint-add-story.png)

U kunt de details van het artikel, zoals de status, beschrijving, opmerkingen, planning en prioriteit In het pop-upvenster bewerken.

![5](./media/agile-development/5-sprint-edit-story.png)

U kunt dit artikel kunt koppelen aan een bestaande opslagplaats door te klikken op **+ koppeling toevoegen** onder **ontwikkeling**. 

![6](./media/agile-development/6-sprint-link-existing-branch.png)


## 5. <a name='AddTaskunderstory-5'></a>Een taak toevoegt aan een artikel 

Taken zijn specifieke gedetailleerde stappen die nodig zijn voor elk artikel. Nadat alle taken van een artikel zijn voltooid, moet het artikel te worden voltooid. 

Een taak wilt toevoegen aan een artikel, klikt u op de  **+**  aanmelding naast het item verhaal, selecteer **taak**, en vul vervolgens de gedetailleerde gegevens van deze taak in het pop-upvenster.

![7](./media/agile-development/7-sprint-add-task.png)

Nadat u de functies, artikelen en taken worden gemaakt, kunt u weergeven in de **achterstand** of **mededelingenbord** weergaven voor de status ervan bijhouden.

![8](./media/agile-development/8-sprint-backlog-view.png)

![9](./media/agile-development/9-link-to-a-new-branch.png)


## 6. <a name='set-up-agile-dsp-6'></a>Een sjabloon flexibele TDSP werken in Visual Studio Online instellen

Dit artikel wordt uitgelegd hoe u een flexibele gegevens wetenschappelijke processjabloon die gebruikmaakt van de fasen van de levenscyclus van het TDSP gegevens wetenschappelijke en houdt werkitems met Visual Studio Online (vso) instelt. De stappen hieronder een voorbeeld van het instellen van de gegevens wetenschap-specifieke flexibele doorloop process Monitoring template *AgileDataScienceProcess* en laten zien hoe gegevens wetenschappelijke work items op basis van de sjabloon te maken.

### <a name="agile-data-science-process-template-setup"></a>Flexibele gegevens wetenschap proces sjabloon Setup

1. Navigeer naar de startpagina van de server, **configureren** -> **proces**.

    ![10](./media/agile-development/10-settings.png) 

2. Navigeer naar **alle processen** -> **processen**onder **Agile** en klik op **maken overgenomen proces**. Vervolgens moet u de procesnaam 'AgileDataScienceProcess' en klik op **aanmaakproces**.

    ![11](./media/agile-development/11-agileds.png)

3. Onder de **AgileDataScienceProcess** -> **typen werkitem** tabblad, uit te schakelen **Epic**, **functie**,  **Gebruiker verhaal**, en **taak** typen door werkitem **configureren -> uitschakelen**

    ![12](./media/agile-development/12-disable.png)

4. Navigeer naar **AgileDataScienceProcess** -> **niveaus achterstand** tabblad. Wijzig de naam 'Epics' tot 'TDSP projecten' door te klikken op de **configureren** -> **bewerken/Rename**. Klik in het dialoogvenster dezelfde **+ nieuw type werkitem** in 'Data wetenschappelijke Project' en stel de waarde van **type werkitem standaard** aan 'Project TDSP' 

    ![13](./media/agile-development/13-rename.png)  

5. Op dezelfde manier achterstand naam 'Functies' op 'TDSP stadia' wijzigen en het volgende toevoegen aan de **nieuwe work item type**:

    - Inzicht in het bedrijf
    - Overname van gegevens
    - Modelleren
    - Implementatie

6. Wijzig de naam 'Gebruiker verhaal' in 'TDSP Substages' met type werkitem standaard ingesteld op de zojuist gemaakte 'TDSP Substage'-type.

7. Instellen van de 'taken' voor het werkitemtype 'TDSP taak' van een nieuw gemaakt 

8. Na deze stappen er de achterstand niveaus als volgt uit:

    ![14](./media/agile-development/14-template.png)  

 
### <a name="create-data-science-work-items"></a>Gegevens wetenschappelijke werkitems maken

Nadat de gegevens wetenschap processjabloon is gemaakt, kunt u maken en uw gegevens wetenschappelijke werkitems die overeenkomen met de levenscyclus van de TDSP volgen.

1. Wanneer u een nieuw teamproject maakt, selecteert u 'Agile\AgileDataScienceProcess' als de **proces werkitem**:

    ![15](./media/agile-development/15-newproject.png)

2. Navigeer naar de zojuist gemaakte teamproject en klik op **werk** -> **achterstanden**.

3. 'TDSP projecten' zichtbaar maken door te klikken op **configureren van instellingen voor team** en controleer "TDSP projecten"; Sla.

    ![16](./media/agile-development/16-enabledsprojects.png)

4. U kunt nu gaan de gegevens wetenschap-specifieke taken of werkitems maken.

    ![17](./media/agile-development/17-dsworkitems.png)

5. Hier volgt een voorbeeld van hoe de werkitems van gegevens wetenschappelijke project moeten worden weergegeven:

    ![18](./media/agile-development/18-workitems.png)


## <a name="next-steps"></a>Volgende stappen

[Samenwerking coderen met Git](collaborative-coding-with-git.md) wordt beschreven hoe gezamenlijke code ontwikkeling voor gegevens wetenschappelijke projecten met Git als ontwikkelframework voor de gedeelde code en hoe deze codering van het werk met de flexibele verwerken geplande activiteiten wordt gekoppeld.

Hier vindt u aanvullende koppelingen naar bronnen op flexibele processen.

- Flexibele proces [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process)
- Werkstroom en flexibele proces werk itemtypen [https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow](https://www.visualstudio.com/en-us/docs/work/guidance/agile-process-workflow)


Scenario's die de stappen in het proces voor het demonstreren **specifieke scenario's** worden ook gegeven. Ze worden weergegeven en gekoppeld aan de miniatuur beschrijvingen in de [voorbeeld-scenario's](walkthroughs.md) artikel. Ze te laten zien hoe cloud, 's voor on-premises en -services combineren in een werkstroom of pijplijn een intelligente toepassing maken. 
