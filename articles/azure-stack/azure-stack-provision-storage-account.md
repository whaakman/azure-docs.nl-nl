---
title: Storage-accounts in Azure-Stack | Microsoft Docs
description: Informatie over het maken van een Azure-Stack storage-account.
services: azure-stack
documentationcenter: 
author: vhorne
manager: byronr
editor: 
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/1/2017
ms.author: victorh
ms.openlocfilehash: 41c9ee37c43d4ad41c51ea2ed023d3b47d460dd1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storage-accounts-in-azure-stack"></a>Opslagaccounts in Azure Stack
Opslagaccounts bevatten Blob- en Tabelservices en de unieke naamruimte voor de objecten van uw opslaggegevens. De gegevens in uw account zijn standaard alleen beschikbaar voor u, de eigenaar van het opslagaccount.

1. Op de Stack Implementatiemodel van Azure-computer, moet u zich aanmelden bij `https://adminportal.local.azurestack.external` als [een beheerder](azure-stack-connect-azure-stack.md), en klik vervolgens op **nieuw** > **gegevens en opslag**  >  **Opslagaccount**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. In de **storage-account maken** blade, typ een naam voor uw opslagaccount. Maak een nieuwe **resourcegroep**, of een bestaande set selecteren en klik vervolgens op **maken** voor het maken van het opslagaccount.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Klik op een overzicht van uw nieuwe opslagaccount **alle resources**, zoekt u naar het storage-account en klik op de naam.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Volgende stappen
[Azure Resource Manager-sjablonen gebruiken](user/azure-stack-arm-templates.md)

[Meer informatie over Azure storage-accounts](../storage/common/storage-create-storage-account.md)

[Download de Azure-Stack consistent zijn met Azure Storage validatie gids](http://aka.ms/azurestacktp1doc)
