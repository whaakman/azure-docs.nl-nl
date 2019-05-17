---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 56545b02c35d68455497c93582073c73d6c75da5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65815554"
---
Als u een v2-opslagaccount voor algemeen gebruik wilt maken in de Azure Portal, volgt u deze stappen:

1. Selecteer in de Azure-portal de optie **Alle services**. Typ in de lijst met resources **Opslagaccounts**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Opslagaccounts**.
1. Kies in het venster **Opslagaccounts** dat wordt weergegeven de optie **Toevoegen**.
1. Selecteer het abonnement waarin u het opslagaccount wilt maken.
1. Selecteer **Nieuwe maken** onder het veld **Resourcegroep**. Voer een naam in voor de nieuwe resourcegroep, zoals in de volgende afbeelding wordt weergegeven.

    ![Schermafbeelding van het maken van een resourcegroep in de portal](./media/storage-create-account-portal-include/create-resource-group.png)

1. Voer vervolgens een naam in voor het opslagaccount. De naam die u kiest, moet uniek zijn binnen Azure. Verder moet de naam 3 tot 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
1. Selecteer een locatie voor uw opslagaccount of gebruik de standaardlocatie.
1. Laat deze velden ingesteld staan op de standaardwaarden:

   |Veld  |Value  |
   |---------|---------|
   |Implementatiemodel     |Resource Manager         |
   |Prestaties     |Standard         |
   |Soort account     |StorageV2 (general-purpose v2)         |
   |Replicatie     |Geografisch redundante opslag met leestoegang (RA-GRS)         |
   |Toegangslaag     |Warm         |

1. Selecteer **Beoordelen en maken** om uw opslagaccountinstellingen te bekijken en het account te maken.
1. Selecteer **Maken**.

Zie [Azure-opslagaccountoverzicht](https://docs.microsoft.com/azure/storage/common/storage-account-overview) voor meer informatie over typen opslagaccounts en andere opslagaccountinstellingen. Zie [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen. 
