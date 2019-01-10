---
title: Overzicht van Microsoft Azure StorSimple-gegevensbeheer | Microsoft Docs
description: Biedt een overzicht van de StorSimple Data Manager-service
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/21/2018
ms.author: vidarmsft
ms.openlocfilehash: fe4d332859b3ec0ba03e64f9a830b7710586aa17
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54158814"
---
# <a name="storsimple-data-manager-solution-overview"></a>Oplossingenoverzicht van StorSimple-gegevensbeheer

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple maakt gebruik van cloudopslag als een uitbreiding van de on-premises oplossing en automatisch de gegevens van de lagen alle on-premises opslag en de cloud. Gegevens worden opgeslagen in de cloud in een indeling ontdubbelde en niet-gecomprimeerde voor maximale efficiency en lagere kosten. Als de gegevens worden opgeslagen in de StorSimple-indeling, is het niet gemakkelijk worden gebruikt door andere cloudtoepassingen die u wilt gebruiken.

De StorSimple-gegevensbeheer kunt u naadloos toegang tot en gebruik van de gegevens van StorSimple-indeling in de cloud. Dit gebeurt door het StorSimple-indeling transformeren naar native-blobs en -bestanden, die u met andere services zoals Azure Media Services, Azure HDInsights en Azure Machine Learning gebruiken kunt.

Dit artikel bevat een overzicht van de oplossing StorSimple Data Manager. Hierin wordt ook uitgelegd hoe u deze service kunt u toepassingen schrijft die gebruikmaken van StorSimple-gegevens en andere Azure-services kunt gebruiken in de cloud.

## <a name="how-it-works"></a>Hoe het werkt?

De service StorSimple Data Manager identificeert een StorSimple-gegevens in de cloud uit een on-premises apparaat van de StorSimple 8000-serie. De StorSimple-gegevens in de cloud is ontdubbelde, StorSimple-indeling gecomprimeerd. De Data Manager-service biedt API's voor de StorSimple-indeling-gegevens ophalen en transformeren naar andere indelingen, zoals Azure-blobs en Azure Files. Dit getransformeerde gegevens vervolgens gemakkelijk wordt gebruikt door Azure HDInsight en Azure Media services. De gegevenstransformatie, dus kan deze services worden de getransformeerde gegevens StorSimple uit de StorSimple 8000-serie on-premises apparaat. Deze stroom wordt weergegeven in het volgende diagram.

![Diagram op hoog niveau](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Gebruiksvoorbeelden van Data Manager

U kunt het gegevensbeheer met Azure Functions, Azure Automation en Azure Data Factory gebruiken dat werkstromen die worden uitgevoerd op uw gegevens er naar StorSimple. Het is raadzaam voor het verwerken van uw media-inhoud opslaan op StorSimple met Azure Media Services, of een Machine Learning-algoritme worden uitgevoerd op die gegevens of een Hadoop-cluster openen om de gegevens die u op StorSimple opslaat te analyseren. Met de groot scala aan services die beschikbaar zijn op Azure in combinatie met de gegevens op StorSimple, kunt u de kracht van uw gegevens ontgrendelen.


## <a name="region-availability"></a>Beschikbaarheid voor de regio

De StorSimple-gegevensbeheer is beschikbaar in de volgende 7 regio's:

 - Azië - zuidoost
 - US - oost
 - US - west
 - US - west 2
 - US - west-centraal
 - Europa - noord
 - Europa -west

De StorSimple-gegevensbeheer kan echter worden gebruikt om gegevens in de volgende regio's te transformeren. 

![Regio's beschikbaar voor gegevens](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions-m.png)

Deze set is groter, omdat de resource-implementatie in een van de bovenstaande regio's is geschikt voor waardoor het proces van de transformatie in de volgende regio's. Dus als uw gegevens zich in een van de 19 regio's bevinden, kunt u uw gegevens met behulp van deze service transformeren.


## <a name="choosing-a-region"></a>Een regio kiezen

We raden aan dat:
 - Uw bron-storage-account (het account dat is gekoppeld aan uw StorSimple-apparaat) en doel-opslagaccount (waar u de gegevens in de oorspronkelijke indeling) zich in dezelfde Azure-regio.
 - U kunt brengen van de definitie van de gegevens en taakplanning in de regio die het StorSimple-opslagaccount bevat. Als dit niet mogelijk is, brengen van de Data Manager in de dichtstbijzijnde Azure-regio en vervolgens de definitie van de taak te maken in dezelfde regio als uw StorSimple-opslagaccount. 

    Als uw StorSimple-storage-account zich niet in de 26 regio's die ondersteuning bieden voor de definitie van het maken van taken, raden wij dat u StorSimple Data Manager niet uitvoeren doet omdat er lang latentie en kosten voor potentieel grote uitgaand verkeer.

## <a name="security-considerations"></a>Beveiligingsoverwegingen

De StorSimple-gegevensbeheer moet de versleutelingssleutel voor servicegegevens voor het transformeren van StorSimple-indeling naar de oorspronkelijke indeling. De versleutelingssleutel voor servicegegevens wordt gegenereerd wanneer het eerste apparaat wordt geregistreerd bij de StorSimple-service. Voor meer informatie over deze sleutel, gaat u naar [StorSimple security](storsimple-8000-security.md).

De versleutelingssleutel voor servicegegevens opgegeven als invoer wordt opgeslagen in een sleutelkluis die is gemaakt bij het maken van een Data Manager. De kluis bevinden zich in dezelfde Azure-regio als uw StorSimple-gegevensbeheer. Deze sleutel wordt verwijderd wanneer u uw Data Manager-service verwijderen.

Deze sleutel wordt gebruikt door de compute-resources uit te voeren van de transformatie. Deze compute-resources bevinden zich in dezelfde Azure-regio als de taakdefinitie van de. Deze regio kan of kunnen niet hetzelfde zijn als de regio waar u van uw Data Manager brengt.

Als uw regio Data Manager af van uw taak definitie regio wijkt, is het belangrijk dat u begrijpt welke gegevens/metagegevens bevindt zich in elk van deze regio's. Het volgende diagram illustreert het effect van de verschillende regio's voor de definitie van gegevens en taakplanning.

![Definitie van de service en -taak in verschillende regio 's](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="managing-personal-information"></a>Persoonlijke gegevens beheren

De StorSimple-gegevensbeheer niet verzamelen of geen persoonlijke gegevens worden weergegeven. Bekijk het Microsoft-privacybeleid in het [Vertrouwenscentrum](https://www.microsoft.com/trustcenter) voor meer informatie.

## <a name="known-limitations"></a>Bekende beperkingen

De service heeft momenteel de volgende beperkingen:
- StorSimple-gegevensbeheer werkt op dit moment niet met de volumes die bitlocker is versleuteld zijn. Als u probeert uit te voeren van de service met een versleutelde schijf, wordt u taakfouten ziet.
- Sommige metagegevens van bestanden (met inbegrip van ACL's) blijft niet behouden in de getransformeerde gegevens.
- Deze service werkt alleen met NTFS-volumes.
- Bestand pad lengte moeten minder dan 256 tekens anders die de taak mislukt.

## <a name="next-steps"></a>Volgende stappen

[Gebruik StorSimple Data Manager UI om uw gegevens te transformeren](storsimple-data-manager-ui.md).
