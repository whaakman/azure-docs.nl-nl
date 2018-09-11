---
title: Team Data Science Process taken voor een individuele Inzender - Azure | Microsoft Docs
description: Een overzicht van de taken voor een individuele Inzender van een data science-teamproject.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: f21098381d75a4843e9300beaae687adc6ec107d
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44303758"
---
# <a name="individual-contributor-tasks"></a>Individuele Inzender-taken

In dit onderwerp bevat de taken die een individuele Inzender opslagbewerkingen uit te voeren voor hun team van gegevenswetenschappers. Het doel is het opzetten van collaboratief team-omgeving die standaardiseert op de [Team Data Science Process](overview.md) (TDSP). Voor een overzicht van de rollen personeel en de bijbehorende taken die worden beheerd door een team van gegevenswetenschappers standaardiseren over dit proces, Zie [Team Data Science Process rollen en taken](roles-tasks.md).

De taken van project afzonderlijke inzenders (gegevenswetenschappers) voor het instellen van de TDSP-omgeving voor het project zijn als volgt weergegeven: 

![1](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **GroupUtilities** is de opslagplaats die het onderhoud van de groep voor het delen van nuttige hulpprogramma's voor de hele groep. 
- **TeamUtilities** is de opslagplaats die uw team voor het onderhoud van specifiek voor uw team. 

Zie voor instructies over het uitvoeren van een data science-project onder TDSP [uitvoering van wetenschappelijke Gegevensprojecten](project-execution.md). 

>[AZURE.NOTE] We een overzicht van de stappen die nodig zijn voor het instellen van een TDSP-team-omgeving met behulp van Azure DevOps in de volgende instructies. We geven over het uitvoeren van deze taken met Azure DevOps vanwege de manier waarop we TDSP bij Microsoft implementeert. Als een ander platform van de code die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden uitgevoerd door de teamleider in het algemeen niet wijzigen. Maar de manier om deze taken uit te voeren afwijken.


## <a name="repositories-and-directories"></a>Opslagplaatsen en mappen

In deze zelfstudie wordt de afgekorte naam voor opslagplaatsen en mappen. Deze namen maken het gemakkelijker zijn te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notatie (**R** voor Git-opslagplaatsen en **D** voor lokale mappen op uw DSVM) wordt gebruikt in de volgende secties:

- **R2**: de GroupUtilities-opslagplaats in Git die uw groepsbeheerder heeft ingesteld op de server van uw Azure DevOps-groep.
- **R4**: de TeamUtilities-opslagplaats in Git die uw teamleider heeft ingesteld.
- **R5**: de Project-opslagplaats in Git die is ingesteld door uw projectleider.
- **D2**: de lokale map gekloond van R2.
- **D4**: de lokale map gekloond vanuit R4.
- **D5**: de lokale map gekloond vanuit R5.


## <a name="step-0-prerequisites"></a>Stap-0: vereisten

De vereisten wordt voldaan door het uitvoeren van de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md). Om samen te vatten hier, moeten de volgende vereisten worden voldaan voordat u begint met het team lead taken: 
- De manager van uw groep heeft ingesteld dat de **GroupUtilities** opslagplaats (indien aanwezig). 
- Uw teamleider heeft ingesteld dat de **TeamUtilities** opslagplaats (indien aanwezig).
- Uw projectleider is ingesteld in de projectopslagplaats. 
- U hebt toegevoegd aan de projectopslagplaats van uw door uw projectleider met de machtiging voor het klonen van en teruggeplaatst in de projectopslagplaats.

De tweede **TeamUtilities** opslagplaats, vereiste is optioneel, afhankelijk van of uw team een hulpprogramma voor team-specifieke opslagplaats heeft. Als een van de andere drie vereisten is niet voltooid, contact op met uw team lead, uw projectleider of hun gemachtigden om in te stellen door de instructies voor [teamleider taken voor een team van gegevenswetenschappers](team-lead-tasks.md) of voor [ Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md).

- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een Data Science Virtual Machine (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te gaan. Raadpleeg anders de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git Credential Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op uw computer geïnstalleerd. In het README.md-bestand, schuif omlaag naar de **Download en installeer** sectie en klikt u op de *nieuwste installatieprogramma van*. Hiermee gaat u naar de pagina van de meest recente installatieprogramma. Het .exe-installatieprogramma hier downloaden en uitvoeren. 
- Als u **Linux-DSVM**, het maken van een openbare SSH-sleutel op uw DSVM en toe te voegen aan uw groep Azure DevOps-Services. Zie voor meer informatie over SSH, de **openbare maken van SSH-sleutel** sectie de [platformen en hulpprogramma's voor bijlage](platforms-and-tools.md#appendix). 
- Als uw team en/of projectgroep lead bepaalde Azure-bestandsopslag die u wilt koppelen aan de DSVM gemaakt heeft, krijgt u de Azure informatie over de opslag van deze. 

## <a name="step-1-3-clone-group-team-and-project-repositories-to-local-machine"></a>Stap 1-3: groep, teams en opslagplaatsen project naar de lokale computer klonen

Deze sectie vindt u instructies voor het voltooien van de eerste drie taken van de afzonderlijke inzenders project: 

- Kloon de **GroupUtilities** opslagplaats R2 D2
- Kloon de **TeamUtilities** opslagplaats R4 tot D4 
- Kloon de **Project** opslagplaats R5 D5.

Maak een map op uw lokale machine ***C:\GitRepos*** (voor Windows) of ***$home/GitRepos*** (forLinux), en wijzig vervolgens in die map. 

Een van de volgende opdrachten (als geschikt is voor uw besturingssysteem) worden uitgevoerd als u wilt klonen uw **GroupUtilities**, **TeamUtilities**, en **Project** opslagplaatsen met mappen op uw lokale computer: 

**Windows**
    
    git clone <the URL of the GroupUtilities repository>
    git clone <the URL of the TeamUtilities repository>
    git clone <the URL of the Project repository>
    
![2](./media/project-ic-tasks/project-ic-2-clone-three-repo-to-ic.png)

Controleer of de drie mappen in de projectmap van uw.

![3](./media/project-ic-tasks/project-ic-3-three-repo-cloned-to-ic.png)

**Linux**
    
    git clone <the SSH URL of the GroupUtilities repository>
    git clone <the SSH URL of the TeamUtilities repository>
    git clone <the SSH URL of the Project repository>

![4](./media/project-ic-tasks/project-ic-4-clone-three-repo-to_ic-linux.png)

Controleer of de drie mappen in de projectmap van uw.

![5](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="step-4-5-mount-azure-file-storage-to-your-dsvm-optional"></a>Stap 4 en 5: Azure file storage koppelen aan de DSVM (optioneel)

Met Azure file storage koppelen aan de DSVM, raadpleegt u de instructies in sectie 4 van de [Team lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de functies en taken die zijn gedefinieerd door het Team Data Science Process:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md)
- [Afzonderlijke inzenders project voor een team van gegevenswetenschappers](project-ic-tasks.md)

