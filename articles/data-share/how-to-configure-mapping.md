---
title: Een gegevenssettoewijzing configureren
description: Een gegevenssettoewijzing configureren
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 581e1eef5f1d64e68a6501f56ce60218281c605d
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789239"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share-preview"></a>Het configureren van een toewijzing voor gegevensset voor het delen van ontvangen in het voorbeeld van Azure-gegevens delen

In dit artikel wordt uitgelegd hoe het configureren van de toewijzing van een gegevensset voor een Share ontvangen met behulp van Azure Data Share Preview. Moet u dit doen als u een share gegevens uitnodiging geaccepteerd, maar heeft gekozen voor ' accepteren en later configureren '. Anders kan wilt wijzigen van de doel-opslagaccount voor de ontvangen gegevens. 

## <a name="navigate-to-a-received-data-share"></a>Navigeer naar een share ontvangen gegevens

In de service Azure-gegevens delen, gaat u naar uw ontvangen bestandsshare en selecteer de **Details** tabblad. 

![Toewijzing van de gegevensset](./media/dataset-mapping.png "toewijzing voor gegevensset") 

Schakel het selectievakje in naast de gegevensset die u graag wilt toewijzen, en op **en wijs deze toe aan het doel**. Mogelijk moet u eerst ontkoppelen Als u al een doelopslagaccount had geconfigureerd en u wilt wijzigen van de toewijzing aan een ander opslagaccount. 

![Wijs deze toe aan het doel](./media/dataset-map-target.png "wijs deze toe aan het doel") 

## <a name="select-a-new-storage-account"></a>Selecteer een nieuw opslagaccount 

Selecteer een opslagaccount dat u wilt de gegevens die moeten worden neergezet in. Houd er rekening mee dat alle gegevens die al in een eerder storage-accounts toegewezen worden niet automatisch worden verplaatst naar het nieuwe opslagaccount.

![Storage-account als doel](./media/map-target.png "Doelopslag") 

## <a name="next-steps"></a>Volgende stappen

Als u wilt meer informatie over het delen van gegevens, blijven de [delen van uw gegevens](share-your-data.md) zelfstudie.



