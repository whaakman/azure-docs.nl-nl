---
title: Azure Data Lake Storage Gen1 regio-overschrijdende-migratie | Microsoft Docs
description: Meer informatie over de migratie van de regio-overschrijdende voor Azure Data Lake Storage Gen1.
services: data-lake-store
documentationcenter: ''
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: 0bf0843314f38c0de28820c82e95b7921297bf40
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233045"
---
# <a name="migrate-azure-data-lake-storage-gen1-across-regions"></a>Azure Data Lake Storage Gen1 migreren tussen regio 's

Als Azure Data Lake Storage Gen1 in nieuwe regio's beschikbaar komt, kunt u ervoor kiezen om te doen eenmalig worden gemigreerd, om te profiteren van de nieuwe regio. Meer informatie over wat u moet overwegen bij het plannen en uitvoeren van de migratie.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie voor meer informatie, [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/).
* **Een Data Lake Storage Gen1-account in twee verschillende regio's**. Zie voor meer informatie, [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md).
* **Azure Data Factory**. Zie voor meer informatie [Inleiding tot Azure Data Factory](../data-factory/introduction.md).


## <a name="migration-considerations"></a>Overwegingen bij migraties

Bepaal eerst de strategie voor clientmigratie die geschikt is voor uw toepassing die wordt geschreven, leest of gegevens in Data Lake Storage Gen1 worden verwerkt. Als u een strategie kiest, houd rekening met de beschikbaarheidsvereisten van uw toepassing en de downtime die optreedt tijdens een migratie. Bijvoorbeeld, mogelijk de eenvoudigste manier om te gebruiken van de 'lift-and-shift'-migratie cloudmodel. In deze benadering onderbreekt u de toepassing in uw bestaande regio terwijl al uw gegevens worden gekopieerd naar de nieuwe regio. Wanneer het kopieerproces is voltooid, moet u doorgaan met uw toepassing in de nieuwe regio en verwijder vervolgens het oude Gen1 van Data Lake Storage-account. Downtime tijdens de migratie is vereist.

U downtime verminderen, kunt u onmiddellijk starten opnemen van nieuwe gegevens in de nieuwe regio. Wanneer u de minimumhoeveelheid gegevens die nodig hebt, kunt u uw toepassing uitvoert in de nieuwe regio. Doorgaan met het kopiëren van oudere gegevens van de bestaande Gen1 van Data Lake Storage-account naar de nieuwe Gen1 van Data Lake Storage-account in de nieuwe regio op de achtergrond. Met behulp van deze benadering kunt u de switch naar de nieuwe regio met weinig downtime. Wanneer de oudere gegevens zijn gekopieerd, verwijdert u de oude Gen1 van Data Lake Storage-account.

Er zijn andere belangrijke informatie om te overwegen bij het plannen van uw migratie:

* **Gegevensvolume**. De hoeveelheid gegevens (in gigabytes, het aantal bestanden en mappen, enzovoort) is van invloed op de tijd en resources die u nodig hebt voor de migratie.

* **Data Lake Storage Gen1 accountnaam**. De nieuwe accountnaam in de nieuwe regio moet globaal uniek zijn. De naam van uw oude Gen1 van Data Lake Storage-account in VS-Oost 2 is bijvoorbeeld mogelijk contosoeastus2.azuredatalakestore.net. U kunt uw nieuwe Gen1 van Data Lake Storage-account in de EU Noord contosonortheu.azuredatalakestore.net naam.

* **Tools**. Het is raadzaam dat u de [Azure Data Factory Copy Activity](../data-factory/connector-azure-data-lake-store.md) Data Lake Storage Gen1 bestanden te kopiëren. Data Factory ondersteunt de verplaatsing van gegevens met hoge prestaties en betrouwbaarheid. Houd er rekening mee dat Data Factory alleen de mappenhiërarchie en de inhoud van deze bestanden kopieert. U moet handmatig toepassen eventuele toegangscontrolelijsten (ACL's) die u in het oude account aan het nieuwe account gebruiken. Zie voor meer informatie, met inbegrip van prestatiedoelen voor het scenario's, de [Kopieeractiviteit prestatie- en afstemmingshandleiding](../data-factory/copy-activity-performance.md). Als u wilt dat gegevens sneller worden gekopieerd, moet u mogelijk aanvullende Cloud Gegevensverplaatsingseenheden gebruiken. Sommige andere hulpmiddelen, zoals AdlCopy, ondersteunen geen kopiëren van gegevens tussen regio's.  

* **Bandbreedte kosten**. [Bandbreedte kosten](https://azure.microsoft.com/pricing/details/bandwidth/) worden toegepast omdat de gegevens worden overgebracht buiten een Azure-regio.

* **ACL's op uw gegevens**. Uw gegevens in de nieuwe regio worden beveiligd door de ACL's toepassen op bestanden en mappen. Zie voor meer informatie, [beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Storage Gen1](data-lake-store-secure-data.md). U wordt aangeraden dat u de migratie gebruiken om te werken en uw ACL's aanpassen. Het is raadzaam om instellingen die vergelijkbaar is met uw huidige instellingen te gebruiken. U kunt de ACL's die worden toegepast op alle bestanden met behulp van de Azure-portal weergeven [PowerShell-cmdlets](/powershell/module/az.datalakestore/get-azdatalakestoreitempermission), of -SDK's.  

* **Locatie van analyseservices**. Voor de beste prestaties, moeten de analytics-services, zoals Azure Data Lake Analytics of Azure HDInsight, zich in dezelfde regio als uw gegevens.  

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Data Lake Storage Gen1](data-lake-store-overview.md)
