---
title: Gegevens wetenschap proces taken voor een afzonderlijke Inzender - Azure in een team | Microsoft Docs
description: Een overzicht van de taken voor een afzonderlijke Inzender aan een project gegevens wetenschappelijke team.
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
ms.openlocfilehash: 526260c3f61a203fe2770a0c6d0d23d95916a2b7
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="individual-contributor-tasks"></a>Afzonderlijke Inzender taken

Dit onderwerp die worden beschreven de taken die een afzonderlijke Inzender wordt verwacht voor hun wetenschappelijke Gegevensteam voltooien. Het doel is het opzetten van samenwerking teamomgeving die standaardiseert op de [Team gegevens wetenschap proces](overview.md) (TDSP). Voor een overzicht van de rollen personeel en de bijbehorende taken die worden verwerkt door een wetenschappelijke Gegevensteam standaardiseren op dit proces Zie [Team gegevens wetenschap proces rollen en taken](roles-tasks.md).

De taken van project individuele gebruikers (gegevenswetenschappers) voor het instellen van de omgeving TDSP voor het project zijn als volgt afgebeeld: 

![1](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **GroupUtilities** is de opslagplaats die het onderhoud van de groep voor het delen van nuttige hulpprogramma's in de hele groep. 
- **TeamUtilities** is de opslagplaats die het onderhoud van uw team specifiek voor uw team. 

Zie voor instructies over het uitvoeren van een project gegevens wetenschappelijke onder TDSP [uitvoering van de gegevens wetenschappelijke projecten](project-execution.md). 

>[AZURE.NOTE] Overzicht van de stappen die nodig zijn voor het instellen van een omgeving met TDSP team met behulp van Visual Studio Team Services (VSTS) in de volgende instructies. We opgeven hoe u deze taken met VSTS omdat hoe we TDSP bij Microsoft implementeren. Als een ander platform code die als host fungeert voor de groep wordt gebruikt, worden de taken die moeten worden voltooid door de lead team in het algemeen niet wijzigen. Maar de manier om deze taken voltooien is het verstandig om verschillende.


## <a name="repositories-and-directories"></a>-Opslagplaatsen en mappen

Deze zelfstudie wordt afkortingen voor opslagplaatsen en mappen. Deze namen gemakkelijker te volgen de bewerkingen tussen de opslagplaatsen en mappen. Deze notation (**R** voor Git-opslagplaatsen en **D** voor lokale mappen op uw DSVM) wordt gebruikt in de volgende secties:

- **R2**: de GroupUtilities opslagplaats op Git die uw groepsbeheerder is ingesteld op uw server VSTS groep.
- **R4**: de TeamUtilities opslagplaats op Git die uw team lead heeft ingesteld.
- **R5**: de Project-opslagplaats op Git die door uw project lead is ingesteld.
- **D2**: de lokale map gekloond van R2.
- **D4**: de lokale map gekloond van R4.
- **D5**: de lokale map gekloond van R5.


## <a name="step-0-prerequisites"></a>Stap-0: vereisten

De vereisten wordt voldaan door de taken die zijn toegewezen aan uw groepmanager die worden beschreven in [groepsbeheerder taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md). Om samen te vatten hier, moeten de volgende vereisten worden voldaan voordat u begint met het team lead taken: 
- De manager van uw groep heeft ingesteld de **GroupUtilities** opslagplaats (indien aanwezig). 
- Uw team lead heeft ingesteld de **TeamUtilities** opslagplaats (indien aanwezig).
- Uw project lead is ingesteld in de project-opslagplaats. 
- U hebt toegevoegd aan uw project-opslagplaats door uw lead project met de bevoegdheden van klonen en push terug naar de project-opslagplaats.

De tweede **TeamUtilities** -opslagplaats, vereiste is optioneel, afhankelijk van of uw team een opslagplaats team-specifieke hulpprogramma heeft. Als een van de andere drie vereiste onderdelen is niet voltooid, contact op met uw team potentiële klanten, uw project lead of hun gemachtigden instellen door de instructies voor [Team leiden taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md) of voor [ Lead projecttaken voor een wetenschappelijke Gegevensteam](project-lead-tasks.md).

- GIT moet worden geïnstalleerd op uw computer. Als u gebruikmaakt van een virtuele Machine wetenschappelijke gegevens (DSVM), Git vooraf is geïnstalleerd en u bent klaar om te beginnen. Raadpleeg anders de [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix).  
- Als u een **Windows DSVM**, moet u beschikken over [Git referentie Manager (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) op deze computer geïnstalleerd. In het bestand README.md, schuif omlaag naar de **downloaden en installeren** sectie en klik op de *meest recente versie installer*. Hiermee gaat u naar de laatste pagina van het installatieprogramma. Het installatieprogramma .exe hier downloaden en uitvoeren. 
- Als u **Linux DSVM**, een openbare SSH-sleutel op uw DSVM maken en toe te voegen aan uw groep VSTS-server. Zie voor meer informatie over SSH de **openbare maken SSH-sleutel** sectie het [Platforms en hulpprogramma's bijlage](platforms-and-tools.md#appendix). 
- Als uw team en/of project lead kan sommige Azure bestandsopslag die u wilt koppelen aan uw DSVM heeft gemaakt, moet u de Azure file storage-gegevens ophalen uit deze. 

## <a name="step-1-3-clone-group-team-and-project-repositories-to-local-machine"></a>Stap 1-3: klonen van de groep, team en project-opslagplaatsen naar de lokale computer

Deze sectie vindt u instructies voor het voltooien van de eerste drie taken van individuele gebruikers project: 

- Kloon de **GroupUtilities** te D2 opslagplaats R2
- Kloon de **TeamUtilities** R4 te D4 opslagplaats 
- Kloon de **Project** R5 te D5 opslagplaats.

Maak een map op uw lokale machine ***C:\GitRepos*** (voor Windows) of ***$home/GitRepos*** (forLinux) en wijzig vervolgens in die map. 

Voer een van de volgende opdrachten (geschikt voor uw besturingssysteem) als u wilt klonen uw **GroupUtilities**, **TeamUtilities**, en **Project** opslagplaatsen tot mappen op uw lokale machine: 

**Windows**
    
    git clone <the URL of the GroupUtilities repository>
    git clone <the URL of the TeamUtilities repository>
    git clone <the URL of the Project repository>
    
![2](./media/project-ic-tasks/project-ic-2-clone-three-repo-to-ic.png)

Controleer of u de drie mappen onder uw projectmap.

![3](./media/project-ic-tasks/project-ic-3-three-repo-cloned-to-ic.png)

**Linux**
    
    git clone <the SSH URL of the GroupUtilities repository>
    git clone <the SSH URL of the TeamUtilities repository>
    git clone <the SSH URL of the Project repository>

![4](./media/project-ic-tasks/project-ic-4-clone-three-repo-to_ic-linux.png)

Controleer of u de drie mappen onder uw projectmap.

![5](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="step-4-5-mount-azure-file-storage-to-your-dsvm-optional"></a>Stap 4 en 5: bestandsopslag op Azure koppelen aan uw DSVM (optioneel)

Met mount-Azure file storage naar uw DSVM, Zie de instructies in sectie 4 van de [Team lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de rollen en taken die zijn gedefinieerd door het Team gegevens wetenschap proces:

- [Groep Manager-taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md)
- [Team Lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)
- [Projecttaken Lead voor een wetenschappelijke Gegevensteam](project-lead-tasks.md)
- [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md)

