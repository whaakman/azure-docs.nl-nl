---
title: Overzicht van Microsoft Azure StorSimple Data Manager | Microsoft Docs
description: Biedt een overzicht van de StorSimple Data Manager-service
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: vidarmsft
ms.openlocfilehash: 8b0ff2c100878e568e0a4c67e79864006512bd78
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="storsimple-data-manager-solution-overview"></a>Overzicht van de oplossing StorSimple Data Manager

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple maakt gebruik van cloud-opslag als een uitbreiding van de on-premises oplossing en automatisch lagen gegevens voor lokale opslag en de cloud. Gegevens worden opgeslagen in de cloud in een ontdubbeld en niet-gecomprimeerde indeling voor maximale efficiëntie en lagere kosten. Als de gegevens worden opgeslagen in de indeling van de StorSimple, is het niet gemakkelijk worden gebruikt door andere cloudtoepassingen die u wilt gebruiken.

De Data StorSimple Manager kunt u naadloos toegang tot gegevens en gebruiken de StorSimple-indeling in de cloud. Dit gebeurt door het omzetten van StorSimple-indeling in systeemeigen blobs en bestanden die u met andere services zoals Azure Media Services, Azure HDInsights en Azure Machine Learning gebruiken kunt.

Dit artikel bevat een overzicht van de oplossing StorSimple Data Manager. Ook wordt uitgelegd hoe u deze service om toepassingen die gebruikmaken van StorSimple-gegevens te schrijven en andere Azure-services kunt gebruiken in de cloud.

## <a name="how-it-works"></a>Hoe het werkt?

De StorSimple Data Manager-service identificeert StorSimple-gegevens in de cloud van een StorSimple 8000 series on-premises-apparaat. De StorSimple-gegevens in de cloud is ontdubbelde, gecomprimeerd StorSimple-indeling. De Data Manager-service biedt API's voor het uitpakken van de gegevens van de indeling StorSimple maken en te transformeren naar andere indelingen, zoals Azure blobs en Azure-bestanden. Dit getransformeerd gegevens gemakkelijk vervolgens door Azure HDInsight en Azure Media services wordt gebruikt. De gegevenstransformatie, dus kan deze services worden bewerkt de getransformeerde gegevens StorSimple van StorSimple 8000 series on-premises-apparaat. Deze stroom wordt weergegeven in het volgende diagram.

![Diagram op hoog niveau](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Gebruiksvoorbeelden voor Data Manager

U kunt Data Manager gebruiken met Azure Functions, Azure Automation en Azure Data Factory werkstromen dat wordt uitgevoerd op uw gegevens, zoals komt dit in StorSimple hebben. Het is raadzaam om te verwerken van uw media-inhoud opslaan op StorSimple met Azure Media Services of een Machine Learning-algoritme worden uitgevoerd op die gegevens of online zetten van een Hadoop-cluster om de gegevens die u op het StorSimple opslaat te analyseren. Met de enorme van services die beschikbaar zijn op Azure gecombineerd met de gegevens op StorSimple, kunt u de kracht van uw gegevens te ontgrendelen.


## <a name="region-availability"></a>Beschikbaarheid in regio’s

De StorSimple Data Manager is beschikbaar in de volgende 7 gebieden:

 - Zuidoost-Azië
 - VS - oost
 - VS - west
 - VS - west 2
 - West-centraal VS
 - Noord-Europa
 - West-Europa

De Data StorSimple Manager kan echter worden gebruikt voor het transformeren van gegevens in de volgende regio's. 

![Regio's beschikbaar voor gegevens](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions.png)

Deze verzameling is groter omdat de resource-implementatie in een van de bovenstaande regio's is geschikt voor u het proces transformatie in de onderstaande gebieden. Dus als uw gegevens zich bevinden in een van de 26 regio's, kunt u uw gegevens transformeren met behulp van deze service.


## <a name="choosing-a-region"></a>Een regio kiezen

We raden aan dat:
 - Uw bron storage-account (de knop die zijn gekoppeld aan uw StorSimple-apparaat) en doel-opslagaccount (waar u de gegevens in native-indeling) zich in dezelfde Azure-regio.
 - U een online zetten van de definitie van de Data Manager en de taak in de regio die het StorSimple-opslagaccount bevat. Als dit niet mogelijk is, brengt u de Data Manager in de dichtstbijzijnde Azure-regio en maak vervolgens de taakdefinitie in dezelfde regio bevinden als uw StorSimple-opslagaccount. 

    Als uw StorSimple-storage-account zich niet in de 26 regio's die ondersteuning bieden voor de definitie van het maken van taken, raden wij StorSimple Data Manager niet te uitgevoerd zoals u lang latenties en kosten voor uitgaande mogelijk hoog ziet.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

De Data StorSimple Manager moet de gegevensversleutelingssleutel van service voor het transformeren van StorSimple-indeling naar native-indeling. De gegevensversleutelingssleutel van de service wordt gegenereerd wanneer het eerste apparaat bij de StorSimple-service registreert. Voor meer informatie over deze sleutel, gaat u naar [StorSimple security](storsimple-8000-security.md).

De gegevensversleutelingssleutel van service opgegeven als invoer wordt opgeslagen in een sleutelkluis die wordt gemaakt bij het maken van een Data Manager. De kluis bevindt zich in dezelfde Azure-regio als uw StorSimple Data Manager. Deze sleutel wordt verwijderd wanneer u uw Data Manager-service verwijderen.

Deze sleutel wordt gebruikt door de rekenresources voor het uitvoeren van de transformatie. Deze berekenen resources bevinden zich in dezelfde Azure-regio als de taakdefinitie van de. Deze regio kan of kunnen niet hetzelfde zijn als de regio waar u van uw Data Manager brengt.

Als uw regio Data Manager af van uw taak definitie regio wijkt, is het belangrijk dat u weet welke gegevens/metadata bevindt zich in elk van deze gebieden. Het volgende diagram illustreert het effect van de verschillende regio's voor de definitie van Data Manager en de taak.

![Definitie van de service en de taak in verschillende regio 's](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).