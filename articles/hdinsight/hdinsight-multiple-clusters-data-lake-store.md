---
title: Gebruik van meerdere HDInsight-clusters met een Azure Data Lake Store-account - Azure | Microsoft Docs
description: "Informatie over het gebruik van meer dan een HDInsight-cluster met één Data Lake Store-account"
keywords: hdinsight-opslag, hdfs, gestructureerde gegevens, ongestructureerde gegevens, data lake store
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 1fe81286c395a529a14ba87edc26390a2bab3f90
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Meerdere HDInsight-clusters met een Azure Data Lake Store-account gebruiken

Beginnen met HDInsight versie 3.5, kunt u HDInsight-clusters maken met Azure Data Lake Store-accounts als het bestandssysteem standaard.
Data Lake Store biedt onbeperkte opslag die het maakt niet alleen voor het hosten van grote hoeveelheden gegevens; ideaal ondersteuning maar ook voor het hosten van meerdere HDInsight-clusters die share één Data Lake Store-Account. Voor instructionson hoe een HDInsight-cluster maken met Data Lake Store omdat de opslaggroep raadpleegt [HDInsight-clusters maken met Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Dit artikel bevat aanbevelingen voor de Data Lake opslaan administrator voor het instellen van een enkel en gedeelde Data Lake opslaan-Account waarmee u over meerdere **active** HDInsight-clusters. Deze aanbevelingen gelden voor het hosten van meerdere beveiligde, evenals een niet-beveiligde Hadoop-clusters op een gedeelde Data Lake store-account.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store bestanden en mappen niveau ACL 's

De rest van dit artikel wordt ervan uitgegaan dat u een goede kennis hebben van bestanden en mappen niveau ACL's in Azure Data Lake Store, die wordt beschreven op [toegangsbeheer in Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Data Lake Store-installatie voor meerdere HDInsight-clusters
Laat het ons nemen een maphiërarchie met twee niveaus om uit te leggen van de aanbevelingen voor het gebruik van bevat meerdere HDInsight-clusters met een Data Lake Store-account. Houd rekening met het hebben van een Data Lake Store-account met de mapstructuur **/clusters/financiën**. Met deze structuur de clusters die zijn vereist voor de organisatie Financiën /clusters/finance gebruiken als de opslaglocatie. In de toekomst, als er een andere organisatie spreken Marketing, wil maken van HDInsight-clusters met dezelfde Data Lake Store-account, kunnen deze clusters/marketing maken. Nu gaan we gebruiken **/clusters/financiën**.

Om deze mapstructuur effectief door HDInsight-clusters worden gebruikt, moet de beheerder van de Data Lake Store relevante machtigingen toewijzen, zoals beschreven in de tabel. De machtigingen weergegeven in de tabel overeen met het Access-ACL's en geen standaard-ACL's. 


|Map  |Machtigingen  |Gebruiker die eigenaar is  |Groep die eigenaar is  | Benoemde gebruiker | Benoemde gebruikersmachtigingen | Benoemde groep | Benoemde groepsmachtigingen |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |Beheerder |Beheerder  |Service-principal |--x  |FINGRP   |r-x         |
|/clusters | rwxr-x--x |Beheerder |Beheerder |Service-principal |--x  |FINGRP |r-x         |
|clusters/Financiën | rwxr-x--t |Beheerder |FINGRP  |Service-principal |rwx  |-  |-     |

In de tabel

- **beheerder** is de maker en de beheerder van het Data Lake Store-account.
- **Service-principal** is de Azure Active Directory (AAD)-service-principal die is gekoppeld aan het account.
- **FINGRP** is een gebruikersgroep gemaakt in AAD die gebruikers van de organisatie Financiën bevat.

Zie voor instructies over het maken van een AAD-toepassing (die u maakt ook een Service-Principal), [een AAD-toepassing maken](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Zie voor instructies over het maken van een gebruikersgroep in AAD [groepen beheren in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

Belangrijke punten in overweging moet nemen.

- De twee niveau mapstructuur (**/clusters financiën/**) moet worden gemaakt en ingericht met de juiste machtigingen door de beheerder van de Data Lake Store **voordat** met behulp van het opslagaccount voor clusters. Deze structuur wordt niet automatisch gemaakt tijdens het maken van clusters.
- Het bovenstaande voorbeeld raadt aan om het instellen van de groep die eigenaar van **/clusters/financiën** als **FINGRP** en waardoor **r-x** toegang tot FINGRP aan de hiërarchie van de hele map starten in de hoofdmap. Dit zorgt ervoor dat de leden van FINGRP de mapstructuur vanaf root kunnen navigeren.
- In het geval wanneer verschillende AAD-Service-Principals kunt clusters onder maken **clusters/financiën**, de tijdelijke-bit (wanneer ingesteld op de **financiën** map) zorgt ervoor dat mappen door één Service-Principal gemaakt kan niet worden verwijderd door de andere.
- Nadat de mapstructuur en de machtigingen die gemaakt zijn, aanmaakproces voor HDInsight-cluster maakt een loaction clusterspecifieke opslag onder **/clustersfinanciën/**. Bijvoorbeeld, de opslag voor een cluster met de naam fincluster01 kan worden **/clusters/finance/fincluster01**. De eigenaar en machtigingen voor de mappen die door HDInsight-cluster gemaakt in de tabel hier weergegeven.

    |Map  |Machtigingen  |Gebruiker die eigenaar is  |Groep die eigenaar is  | Benoemde gebruiker | Benoemde gebruikersmachtigingen | Benoemde groep | Benoemde groepsmachtigingen |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finanace/fincluster01 | rwxr-x---  |Service-Principal |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Aanbevelingen voor taak-en uitvoergegevens

Het is raadzaam dat invoergegevens naar een taak en de uitvoer van een taak worden opgeslagen in een map buiten de **/clusters**. Dit zorgt ervoor dat zelfs als de cluster-specifieke map wordt verwijderd als u wilt vrijmaken sommige opslagruimte, taak- en uitgangen nog steeds beschikbaar voor toekomstig gebruik zijn. Zorg ervoor dat de mappenhiërarchie voor het opslaan van taak- en uitgangen passend niveau van toegang voor de Service-Principal toestaat in dat geval.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>De limiet voor clusters delen van een enkele storage-account

De limiet voor het aantal clusters die één Data Lake Store-account kunt delen, is afhankelijk van de werkbelasting die wordt uitgevoerd op deze clusters. Te veel clusters of zeer zware werklasten op clusters die delen van een opslagaccount, kunnen er de storage-account inkomend en uitgaand naar ophalen beperkt.

## <a name="support-for-default-acls"></a>Ondersteuning voor standaard-ACL 's

Wanneer een Service-Principal maken met de naam van de gebruiker toegang (zoals in de bovenstaande tabel), raden we aan **niet** het toevoegen van met de naam-gebruiker met een standaard-ACL. Inrichten met de naam van de gebruiker toegang met behulp van resultaten van de standaard-ACL's in de toewijzing van 770 machtigingen voor die eigenaar is van de gebruiker die eigenaar is van de groep en de andere. Hoewel deze standaardwaarde van 770 worden machtigingen van die eigenaar is van gebruiker (7) of die eigenaar is van-groep (7), heeft de weg alle machtigingen voor andere (0). Dit resulteert in een bekend probleem met een bepaalde use case die wordt uitgebreid beschreven in de [bekende problemen en tijdelijke oplossingen](#known-issues-and-workarounds) sectie.

## <a name="known-issues-and-workarounds"></a>Bekende problemen en oplossingen

Deze sectie vindt de bekende problemen voor het gebruik van HDInsight met Data Lake Store en de tijdelijke oplossingen.

### <a name="publicly-visible-localized-yarn-resources"></a>Openbaar zichtbaar gelokaliseerde YARN-bronnen

Wanneer een nieuw Azure Data Lake store-account is gemaakt, wordt de hoofdmap automatisch ingericht met ACL Access machtiging bitset naar 770. De basismap die eigenaar is van gebruiker is ingesteld op de gebruiker die het account (de beheerder van de Data Lake Store) hebt gemaakt en de groep die eigenaar is ingesteld op de primaire groep van de gebruiker die het account hebt gemaakt. Geen toegang is opgegeven voor 'anderen'.

Deze instellingen bekend is dat invloed op een specifieke HDInsight use case vastgelegd in [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Voorbeelden van de taak kunnen mislukken met een vergelijkbaar met het volgende foutbericht weergegeven:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Zoals vermeld in de YARN-JIRA gekoppeld eerder tijdens het lokaliseren van openbare bronnen, de localizer wordt gevalideerd of de gewenste bronnen inderdaad openbare door hun machtigingen op het externe bestand-systeem controleren. Alle LocalResource die voldoet niet aan die voorwaarde is voor lokalisatie geweigerd. De controle voor machtigingen, omvat leestoegang tot het bestand voor 'anderen'. Dit scenario werkt niet out-of-the-box-bij het hosten van HDInsight-clusters op Azure Data Lake omdat Azure Data Lake alle de toegang aan 'anderen weigert' op hoofdniveau van de map.

#### <a name="workaround"></a>Tijdelijke oplossing
Set lees-schrijfrechten hebben voor **anderen** via de hiërarchie, bijvoorbeeld op  **/** , **/clusters** en   **/clusters/financiën** zoals weergegeven in de bovenstaande tabel.

## <a name="see-also"></a>Zie ook

* [Een HDInsight-cluster maken met Data Lake Store als opslag](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)


