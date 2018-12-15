---
title: Gebruik van meerdere HDInsight-clusters met een Azure Data Lake Store-account - Azure
description: Informatie over het gebruik van meer dan één HDInsight-cluster met één Data Lake Store-account
keywords: hdinsight-opslag, hdfs, gestructureerde gegevens, niet-gestructureerde gegevens, data lake store
services: hdinsight,storage
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 6b835dc300808453240dd3b9fe47957bd5feab9b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435222"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Meerdere HDInsight-clusters met een Azure Data Lake Store-account gebruiken

Beginnen met een HDInsight versie 3.5, kunt u HDInsight-clusters maken met Azure Data Lake Store-accounts als de standaard-bestandssysteem.
Data Lake Store biedt ondersteuning voor onbeperkte opslag die ideaal niet alleen voor het hosten van grote hoeveelheden gegevens. maar ook voor het hosten van meerdere HDInsight-clusters die share één Data Lake Store-Account. Zie voor instructies over het maken van een HDInsight-cluster met Data Lake Store als opslag [Quick Start: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

In dit artikel biedt aanbevelingen om de Data Lake opslaan van de beheerder voor het instellen van een enkel en gedeelde Data Lake Store-Account die kunnen worden gebruikt voor meerdere **active** HDInsight-clusters. Deze aanbevelingen gelden voor het hosten van meerdere beveiligde als niet-beveiligde Apache Hadoop-clusters op een gedeelde Data Lake store-account.


## <a name="data-lake-store-file-and-folder-level-acls"></a>Data Lake Store-bestand en map niveau ACL 's

De rest van dit artikel wordt ervan uitgegaan dat u een goede kennis van bestanden en mappen niveau ACL's in Azure Data Lake Store, die wordt beschreven op [toegangsbeheer in Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Data Lake Store-instellingen voor meerdere HDInsight-clusters
We gaan een maphiërarchie met twee niveaus om uit te leggen van de aanbevelingen voor het gebruik van bevat meerdere HDInsight-clusters met een Data Lake Store-account. U hebt een Data Lake Store-account met de mapstructuur overwegen **/clusters/financiën**. Met deze structuur, de clusters die zijn vereist voor de financiële organisatie /clusters/finance gebruiken als de opslaglocatie. In de toekomst, als er een andere organisatie, bijvoorbeeld Marketing, wil maken van HDInsight-clusters met behulp van hetzelfde Data Lake Store-account, kunnen ze/clusters/marketing maken. Nu dit voorbeeld gebruiken we **/clusters/financiën**.

Om in te schakelen deze mapstructuur kunnen effectief worden gebruikt door HDInsight-clusters, moet de beheerder van de Data Lake Store relevante machtigingen toewijzen zoals beschreven in de tabel. De machtigingen die wordt weergegeven in de tabel overeen met de toegangs-ACL-'s en niet standaard-ACL's. 


|Map  |Machtigingen  |Gebruiker die eigenaar is  |Groep die eigenaar is  | Benoemde gebruiker | Benoemde gebruikersmachtigingen | De benoemde groep | De benoemde groepsmachtigingen |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x: x  |beheerder |beheerder  |Service-principal |--x  |FINGRP   |l-         |
|/clusters | rwxr-x: x |beheerder |beheerder |Service-principal |--x  |FINGRP |l-         |
|/ clusters/Financiën | rwxr-x - t |beheerder |FINGRP  |Service-principal |LSU  |-  |-     |

In de tabel

- **beheerder** is de maker en de beheerder van de Data Lake Store-account.
- **Service-principal** is de Azure Active Directory (AAD)-service-principal die is gekoppeld aan het account.
- **FINGRP** is een groep hebt gemaakt in AAD die gebruikers van de financiële organisatie bevat.

Zie voor instructies over het maken van een AAD-toepassing (die ook wordt een Service-Principal gemaakt), [een AAD-toepassing maken](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application). Zie voor instructies over het maken van een gebruikersgroep in AAD [groepen beheren in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Enkele belangrijke punten om te overwegen.

- De twee niveau mapstructuur (**/clusters/financiën/**) moeten worden gemaakt en kan worden ingericht met de juiste machtigingen door de beheerder van de Data Lake Store **voordat** voor clusters met behulp van het storage-account. Deze structuur wordt niet automatisch gemaakt tijdens het maken van clusters.
- Het bovenstaande voorbeeld wordt aanbevolen voor het instellen van de groep die eigenaar is van **/clusters/financiën** als **FINGRP** en waardoor **l-** toegang tot FINGRP aan de hiërarchie van de hele map starten in de hoofdmap. Dit zorgt ervoor dat de leden van FINGRP de mapstructuur starten vanaf de basis kunnen navigeren.
- In het geval wanneer andere AAD-Service-Principals kunt clusters onder maken **/clusters/financiën**, de vergrendelde bit (wanneer ingesteld op de **financiën** map) zorgt ervoor dat mappen door een Service-Principal gemaakt kan niet worden verwijderd door de andere.
- Nadat de mapstructuur en de machtigingen gemaakt zijn, proces voor het maken van HDInsight-cluster maakt een cluster-specifieke storage loaction onder **/clusters/financiën/**. Bijvoorbeeld, de opslag voor een cluster met de naam fincluster01 mogelijk **/clusters/finance/fincluster01**. De eigenaar en de machtigingen voor de mappen die zijn gemaakt door HDInsight-cluster wordt weergegeven in de tabel hier.

    |Map  |Machtigingen  |Gebruiker die eigenaar is  |Groep die eigenaar is  | Benoemde gebruiker | Benoemde gebruikersmachtigingen | De benoemde groep | De benoemde groepsmachtigingen |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finanace/fincluster01 | rwxr-x---  |Service-Principal |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Aanbevelingen voor taak-en uitvoergegevens

We raden aan dat de invoergegevens voor een taak en de uitvoer van een taak worden opgeslagen in een map buiten **/clusters**. Dit zorgt ervoor dat, zelfs als de cluster-specifieke map wordt verwijderd om sommige opslag vrij te maken, de taak invoer en uitvoer nog steeds beschikbaar voor toekomstig gebruik zijn. Zorg ervoor dat de maphiërarchie voor het opslaan van de taak invoer en uitvoer passend niveau van toegang voor de Service-Principal kunt in dat geval.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>De limiet voor het delen van een enkel opslagaccount clusters

De limiet voor het aantal clusters die één Data Lake Store-account kunt delen, is afhankelijk van de werkbelasting die wordt uitgevoerd op deze clusters. Te veel clusters of zeer zware workloads die in de clusters die delen van een opslagaccount kan leiden tot de storage-account Inkomend/uitgaand verkeer op te maken met beperkingen.

## <a name="support-for-default-acls"></a>Ondersteuning voor standaard-ACL 's

Wanneer een Service-Principal maken met de naam van de gebruiker toegang heeft (zoals in de bovenstaande tabel), raden we aan **niet** toe te voegen de met de naam-gebruiker met een standaard-ACL. Inrichten met de naam van de gebruiker toegang met behulp van resultaten van de standaard-ACL's in de toewijzing van 770 machtigingen voor die eigenaar is van de gebruiker, groep die eigenaar is en anderen. Terwijl deze standaardwaarde van 770 neemt weg machtigingen van die eigenaar is gebruiker (7) of die eigenaar is-groep (7), maar het duurt onmiddellijk alle machtigingen voor anderen (0). Dit resulteert in een bekend probleem met een bepaalde gebruiksscenario die wordt besproken in de [bekende problemen en tijdelijke oplossingen](#known-issues-and-workarounds) sectie.

## <a name="known-issues-and-workarounds"></a>Bekende problemen en oplossingen

Deze sectie vindt u de bekende problemen voor het gebruik van HDInsight met Data Lake Store en hun oplossingen.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Zichtbaar voor iedereen gelokaliseerde Apache Hadoop YARN-bronnen

Wanneer een nieuw Azure Data Lake store-account wordt gemaakt, wordt automatisch de hoofdmap met toegangs-ACL machtiging bitset naar 770 ingericht. De basismap die eigenaar is van gebruiker is ingesteld op de gebruiker die het account (de beheerder van de Data Lake Store) hebt gemaakt en de groep die eigenaar is ingesteld op de primaire groep van de gebruiker die het account hebt gemaakt. Geen toegang wordt geboden voor 'anderen'.

Deze instellingen bekend is dat ze invloed hebben op een specifieke HDInsight-use-casescenario vastgelegd in [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). -Taakinzendingen kunnen mislukken met een foutbericht ongeveer als volgt uit:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Zoals vermeld in de YARN-JIRA eerder gekoppeld tijdens het lokaliseren van openbare-resources, wordt de localizer valideert dat de aangevraagde resources inderdaad openbaar zijn door het controleren van hun machtigingen op het externe bestand-systeem. Alle LocalResource die niet zijn voor die voorwaarde geschikt is is geweigerd voor lokalisatie. De controle voor machtigingen, bevat leestoegang tot het bestand voor 'anderen'. In dit scenario werkt niet out-of-the-box bij het hosten van HDInsight-clusters in Azure Data Lake, omdat Azure Data Lake "anderen" alle toegang weigert op hoofdniveau van de map.

#### <a name="workaround"></a>Tijdelijke oplossing
Set lezen-machtigingen voor uitvoeren voor **anderen** via de hiërarchie, bijvoorbeeld op **/**, **/clusters** en   **/clusters/financiën** zoals wordt weergegeven in de bovenstaande tabel.

## <a name="see-also"></a>Zie ook

* [Snelstartgids: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)


