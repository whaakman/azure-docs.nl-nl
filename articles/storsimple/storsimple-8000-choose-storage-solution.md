---
title: Opties voor de gegevens overdragen naar Azure met behulp van een apparaat | Microsoft Docs
description: Meer informatie over het kiezen van het juiste apparaat mag gegevens overdragen naar Azure
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 04/01/2019
ms.author: alkohli
ms.openlocfilehash: d727ed62f5a869f6eb67400281d2660607756abf
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802691"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>StorSimple met Azure File Sync en gegevens in Edge data transfer opties vergelijken 
 
Dit document bevat een overzicht van opties voor de overdracht van on-premises naar Azure, vergelijking: Data Box-Edge vs. Azure File Sync vs. StorSimple 8000-serie.

- **[Data Box Edge](/azure/databox-online/data-box-edge-overview)**  -gegevens in Edge is een on-premises netwerk-apparaat die gegevens verplaatst naar en van Azure en compute Edge AI-functionaliteit voor het vooraf verwerken van gegevens tijdens het uploaden is. Data Box-Gateway is een virtuele versie van het apparaat met de dezelfde mogelijkheden voor de overdracht van gegevens.
- **[Azure File Sync](/azure/storage/files/storage-sync-files-deployment-guide)**  – Azure File Sync kan worden gebruikt om het centraliseren van bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. Algemene beschikbaarheid van Azure File Sync is aangekondigd eerder in 2018.
- **[StorSimple](/azure/storsimple/storsimple-overview)**  : StorSimple is een hybride-apparaat waarmee bedrijven hun opslaginfrastructuur voor primaire opslag, gegevensbescherming, archivering en herstel na noodgevallen in één oplossing door te integreren in nauw consolideren Azure-opslag. De levenscyclus voor StorSimple vindt [hier](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).

## <a name="comparison-summary"></a>Vergelijking van de samenvatting

|                           |StorSimple 8000   |Azure File Sync   |Data Box Edge           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Overzicht         |Gelaagde opslag voor hybride en archiveren van gegevens|Algemene file server storage met cloudsynchronisatie cloudlagen en meerdere locaties.  |Opslagoplossing voor gegevens vooraf verwerken en verzenden via netwerk naar Azure.        |
|Scenario's        |Bestandsserver, archivering, back-doel |Bestandsserver, archivering, (meerdere locaties)   |Gegevensoverdracht, het vooraf verwerken met ML inferentietaken gegevens in, IoT, archivering    |
|Edge-rekenproces     |Niet beschikbaar |Niet beschikbaar |Ondersteunt het uitvoeren van containers met behulp van Azure IoT Edge    |
|Vormfactor      |Fysiek apparaat   |Agent is geïnstalleerd op Windows Server |Fysiek apparaat   |
|Hardware         |Fysiek apparaat van Microsoft is geleverd als onderdeel van de service | Klantgeleverde |Fysiek apparaat van Microsoft is geleverd als onderdeel van de service  |
|Gegevensindeling      |Aangepaste notatie   |Bestanden         |BLOBs of bestanden    |
|Protocolondersteuning |iSCSI          |SMB, NFS    | SMB- of NFS      |
|Prijzen          |[StorSimple](https://azure.microsoft.com/pricing/details/storsimple/) |[Azure File Sync](https://azure.microsoft.com/pricing/details/storage/files/)  |[Data Box Edge](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Data Box Edge](/azure/databox-online/data-box-edge-overview) en [Azure Data Box-Gateway](/azure/databox-online/data-box-gateway-overview)
- Meer informatie over [Azure File Sync](/azure/storage/files/storage-sync-files-deployment-guide)
