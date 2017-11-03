---
title: Azure Data Lake Store regio-overschrijdende migratie | Microsoft Docs
description: Meer informatie over de migratie van de regio-overschrijdende voor Azure Data Lake Store.
services: data-lake-store
documentationcenter: 
author: swums
manager: amitkul
editor: swums
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/27/2017
ms.author: stewu
ms.openlocfilehash: b04cca6e551a15a31bbebc4932ea05dd39e8e916
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-data-lake-store-across-regions"></a>Data Lake Store migreren tussen regio 's

Wanneer nieuwe regio's Azure Data Lake Store beschikbaar wordt, kunt u kunt kiezen om een eenmalige migratie, om te profiteren van de nieuwe regio doen. Meer informatie over wat u moet overwegen bij het plannen en uitvoeren van de migratie.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie voor meer informatie [maken van uw gratis Azure-account vandaag](https://azure.microsoft.com/pricing/free-trial/).
* **Een Data Lake Store-account in twee verschillende regio's**. Zie voor meer informatie [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Azure Data Factory**. Zie voor meer informatie [Inleiding tot Azure Data Factory](../data-factory/introduction.md).


## <a name="migration-considerations"></a>Overwegingen bij migratie

Bepaal eerst de strategie voor clientmigratie die geschikt is voor uw toepassing die u schrijft, leest of gegevens in Data Lake Store worden verwerkt. Wanneer u een strategie kiest, kunt u beschikbaarheidsvereisten van uw toepassing en de downtime die deze gebeurtenis treedt op tijdens een migratie. De eenvoudigste manier kan bijvoorbeeld zijn het model van de migratie cloud 'lift-en-shift' gebruiken. In deze methode voert onderbreken u om de toepassing in uw bestaande regio terwijl al uw gegevens wordt gekopieerd naar de nieuwe regio. Wanneer het kopieerproces is voltooid, moet u uw toepassing in de nieuwe regio hervatten en verwijder de oude Data Lake Store-account. Er is uitvaltijd tijdens de migratie.

Als u wilt de uitvaltijd beperken, kunt u bijvoorbeeld onmiddellijk starten opnemen van nieuwe gegevens in de nieuwe regio. Wanneer u de minimale gegevens nodig hebt, kunt u uw toepassing uitvoeren in de nieuwe regio. Doorgaan met het kopiëren van oudere gegevens van de bestaande Data Lake Store-account naar de nieuwe Data Lake Store-account in de nieuwe regio op de achtergrond. Met behulp van deze benadering kunt u de schakeloptie voor de nieuwe regio weinig uitvaltijd. Wanneer de oudere gegevens is gekopieerd, moet u de oude Data Lake Store-account verwijderen.

Andere belangrijke informatie om u te overwegen bij het plannen van de migratie zijn:

* **Gegevensvolume**. De hoeveelheid gegevens (in gigabytes, het aantal bestanden en mappen, enzovoort) is van invloed op de tijd en bronnen die u nodig hebt voor de migratie.

* **Data Lake Store-accountnaam**. De nieuwe accountnaam in de nieuwe regio moet wereldwijd uniek zijn. De naam van uw oude Data Lake Store-account in VS-Oost 2 kan bijvoorbeeld contosoeastus2.azuredatalakestore.net zijn. U kunt uw nieuwe Data Lake Store-account in Noord EU contosonortheu.azuredatalakestore.net naam.

* **Hulpprogramma's voor**. Het is raadzaam dat u de [Azure Data Factory-Kopieeractiviteit](../data-factory/connector-azure-data-lake-store.md) Data Lake Store-bestanden te kopiëren. Data Factory ondersteunt de verplaatsing van gegevens met hoge prestaties en betrouwbaarheid. Houd er rekening mee dat de Data Factory alleen de mappenhiërarchie en de inhoud van de bestanden kopieert. U moet eventuele toegangsbeheerlijsten (ACL's) die u in de oude account naar de nieuwe account handmatig toepassen. Zie voor meer informatie, prestatiedoelen voor best mogelijke scenario's, waaronder de [Kopieeractiviteit prestaties en prestatieafstemming handleiding](../data-factory/copy-activity-performance.md). Als u gegevens kopiëren sneller wilt, moet u mogelijk aanvullende Cloud Data Movement-eenheden gebruiken. Sommige andere hulpmiddelen, zoals AdlCopy, bieden geen ondersteuning voor kopiëren van gegevens tussen regio's.  

* **Kosten van bandbreedte**. [Kosten van bandbreedte](https://azure.microsoft.com/en-us/pricing/details/bandwidth/) toepassen omdat de gegevens worden overgebracht buiten een Azure-regio.

* **ACL's voor uw gegevens**. Beveilig uw gegevens in het nieuwe door de ACL's toepassen op bestanden en mappen. Zie voor meer informatie [beveiligen van gegevens die zijn opgeslagen in Azure Data Lake Store](data-lake-store-secure-data.md). Het is raadzaam dat u de migratie gebruiken om te werken en uw ACL's aanpassen. Het is raadzaam om instellingen die vergelijkbaar is met uw huidige instellingen te gebruiken. U kunt de ACL's die worden toegepast op een bestand met behulp van de Azure-portal bekijken [PowerShell-cmdlets](/powershell/module/azurerm.datalakestore/get-azurermdatalakestoreitempermission), of de SDK's.  

* **Locatie van de services analytics**. Voor optimale prestaties moet uw analytics-services, zoals Azure Data Lake Analytics of Azure HDInsight in dezelfde regio bevinden als uw gegevens.  

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
