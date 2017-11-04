---
title: Maken van een test-virtuele machine in Azure-Stack | Microsoft Docs
description: Informatie over het inrichten van een test-virtuele machine in Azure-Stack als een cloud-operator.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: erikje
ms.openlocfilehash: 233cf4df53af6a49e5fe4c5d51e112d8196a7530
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Maken van een virtuele machine in Azure-Stack

*Van toepassing op: Azure stapelen Development Kit*

Als een Azure-Stack-operator, kunt u een virtuele testmachine valideren uw [Azure Stack](azure-stack-poc.md) Developer Kit implementatie.

> [!NOTE]
> Voordat u virtuele machines inrichten kan, moet u [de evaluatie van Windows Server 2016-installatiekopie toevoegen aan de Stack van Azure marketplace](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
1. Op de host Azure Stack Development Kit [aanmelden](azure-stack-connect-azure-stack.md) naar de beheerdersportal (`https://adminportal.local.azurestack.external`), en klik vervolgens op **nieuw** > **Compute**  >  **Windows Server 2016 Datacenter Eval** > **maken**.  
2. In de **basisbeginselen** blade, typ een **naam**, **gebruikersnaam**, en **wachtwoord**. Kies een **abonnement**. Maak een **resourcegroep**, of een bestaande set selecteren en klik vervolgens op **OK**.  
3. In de **een grootte kiezen** blade, klikt u op **A1 standaard**, en klik vervolgens op **Selecteer**.  
4. In de **instellingen** blade, accepteer de standaardinstellingen en klik op **OK**
5. In de blade **Samenvatting** klikt u op **OK** om de virtuele machine te maken.  
6. Klik op een overzicht van de nieuwe virtuele machine **alle resources**, zoekt u naar de virtuele machine en klik op de naam.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Volgende stappen
[Met behulp van de portals beheerder en gebruiker in Azure-Stack](azure-stack-manage-portals.md)
