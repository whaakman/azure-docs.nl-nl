---
title: Maak een beveiligingsgroep voor netwerk - Azure-portal | Microsoft Docs
description: Informatie over het maken en implementeren van netwerkbeveiligingsgroepen met de Azure portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a66de0b0239fef12168733eca7af85c8b08f82
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793640"
---
# <a name="create-a-network-security-group-using-the-azure-portal"></a>Een netwerkbeveiligingsgroep met de Azure portal maken

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [nsg's maken in het klassieke implementatiemodel](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]


## <a name="create-the-nsg-frontend-nsg"></a>Het NSG NSG-FrontEnd maken
Maken van de **NSG-FrontEnd** NSG zoals weergegeven in het scenario, de volgende stappen uitvoeren:

1. Navigeer in een browser naar https://portal.azure.com en meld u, indien nodig, aan met uw Azure-account.
2. Selecteer **+ maken van een resource >** > **Netwerkbeveiligingsgroepen**.
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. Onder **Netwerkbeveiligingsgroepen**, selecteer **toevoegen**.
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. Onder **netwerkbeveiligingsgroep maken**, maakt u een NSG met de naam *NSG-FrontEnd* in de *RG NSG* resource groep en selecteert u vervolgens **maken** .
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Regels maken in een bestaande NSG
Voor het maken van regels in een bestaande NSG vanuit de Azure-portal, moet u de volgende stappen uitvoeren:

1. Selecteer **alle Services**, zoekt u naar **Netwerkbeveiligingsgroepen**. Wanneer **Netwerkbeveiligingsgroepen** worden weergegeven, selecteert u deze.
2. Selecteer in de lijst met nsg's, **NSG-FrontEnd** > **beveiligingsregels voor binnenkomende verbindingen**
   
    ![Azure portal - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. In de lijst met **inkomende beveiligingsregels**, selecteer **toevoegen**.
   
    ![Azure-portal - regel toevoegen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. Onder **de inkomende beveiligingsregel toevoegen**, maakt u een regel met de naam *web regel* met de prioriteit van *200* toegang via *TCP* op poort *80* bron naar een virtuele machine uit en selecteer vervolgens **OK**. U ziet dat de meeste van deze instellingen standaardwaarden al.
   
    ![Azure portal - instellingen van de regel](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Na enkele seconden ziet u de nieuwe regel in de NSG.
   
    ![Azure portal - nieuwe regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Herhaal stap 6 voor het maken van een inkomende regel met de naam *rdp-regel* met een prioriteit van *250* toegang via *TCP* op poort *3389* naar een virtuele machine uit een andere bron.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>De NSG aan het FrontEnd-subnet koppelen

1. Selecteer **alle services >**, voer **resourcegroepen**, selecteer **resourcegroepen** wanneer deze wordt weergegeven, schakelt u **RG NSG**.
2. Onder **RG NSG**, selecteer **...**   >  **TestVNet**.
   
    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. Onder **instellingen**, selecteer **subnetten** > **FrontEnd** > **netwerkbeveiligingsgroep**  >  **NSG-FrontEnd**.
   
    ![Azure portal - subnetinstellingen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. In de **FrontEnd** blade Selecteer **opslaan**.
   
    ![Azure portal - subnetinstellingen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Maken van de NSG NSG-back-end
Maken van de **NSG-back-end** NSG en koppel deze aan de **back-end** subnet, de volgende stappen uit:

1. Maken van een NSG met de naam *NSG-back-end*, Herhaal de stappen in [maken het NSG NSG-FrontEnd](#Create-the-NSG-FrontEnd-NSG).
2. Maken van de **inkomende** regels in de tabel die volgt, Herhaal de stappen in [regels maken in een bestaande NSG](#Create-rules-in-an-existing-NSG).
   
   | Regel voor binnenkomende verbindingen | Uitgaande regel |
   | --- | --- |
   | ![Azure portal - regel voor binnenkomende verbindingen](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - uitgaande regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Om te koppelen de **NSG-back-end** NSG aan de **back-end** subnet, Herhaal de stappen in [het NSG aan het subnet FrontEnd koppelt](#Associate-the-NSG-to-the-FrontEnd-subnet).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [bestaande nsg's beheren](manage-network-security-group.md)
* [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor nsg's.