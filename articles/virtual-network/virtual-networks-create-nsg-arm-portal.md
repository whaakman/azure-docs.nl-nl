---
title: Netwerkbeveiligingsgroepen - Azure-portal beheren | Microsoft Docs
description: Informatie over het beheren van netwerkbeveiligingsgroepen met de Azure portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: faee5ac8-f4c4-4f97-ade5-197a37aad496
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ecb4fb4608628f5a1bd54fac6af19fecfa4508f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-portal"></a>Netwerkbeveiligingsgroepen met de Azure portal beheren

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [nsg's maken in het klassieke implementatiemodel](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Het voorbeeld PowerShell onderstaande opdrachten een eenvoudige omgeving al gemaakt verwacht op basis van de bovenstaande scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, moet u eerst de testomgeving verder door de implementatie [deze sjabloon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), klikt u op **implementeren in Azure**, vervangt u de standaardwaarden voor parameters indien nodig en volg de instructies in de portal. De stappen hieronder gebruik **RG NSG** als de naam van de resourcegroep voor de sjabloon is geïmplementeerd.

## <a name="create-the-nsg-frontend-nsg"></a>Het NSG NSG-FrontEnd maken
Maken van de **NSG-FrontEnd** NSG zoals weergegeven in het bovenstaande scenario Volg de onderstaande stappen.

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Klik op **Bladeren >** > **Netwerkbeveiligingsgroepen**.
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. In de **Netwerkbeveiligingsgroepen** blade, klikt u op **toevoegen**.
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. In de **netwerkbeveiligingsgroep maken** blade maken van een NSG met de naam *NSG-FrontEnd* in de *RG NSG* resourcegroep en klik vervolgens op **maken**.
   
    ![Azure portal - nsg 's](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Regels maken in een bestaande NSG
Voor het maken van regels in een bestaande NSG vanuit de Azure-portal, de volgende stappen uit te voeren.

1. Klik op **Bladeren >** > **Netwerkbeveiligingsgroepen**.
2. Klik in de lijst van nsg's op **NSG-FrontEnd** > **beveiligingsregels voor binnenkomende verbindingen**
   
    ![Azure portal - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. In de lijst met **inkomende beveiligingsregels**, klikt u op **toevoegen**.
   
    ![Azure-portal - regel toevoegen](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. In de **de inkomende beveiligingsregel toevoegen** blade maken van een regel met naam *web regel* met de prioriteit van *200* toegang via *TCP* op poort *80* naar een virtuele machine uit een bron en klik vervolgens op **OK**. U ziet dat de meeste van deze instellingen standaardwaarden al.
   
    ![Azure portal - instellingen van de regel](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Na enkele seconden ziet u de nieuwe regel in de NSG.
   
    ![Azure portal - nieuwe regel](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Herhaal stap 6 voor het maken van een inkomende regel met de naam *rdp-regel* met een prioriteit van *250* toegang via *TCP* op poort *3389* naar een virtuele machine uit een andere bron.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>De NSG aan het FrontEnd-subnet koppelen
1. Klik op **Bladeren >** > **resourcegroepen** > **RG NSG**.
2. In de **RG NSG** blade, klikt u op **...**   >  **TestVNet**.
   
    ![Azure portal - TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. In de **instellingen** blade, klikt u op **subnetten** > **FrontEnd** > **netwerkbeveiligingsgroep** > **NSG-FrontEnd**.
   
    ![Azure portal - subnetinstellingen](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. In de **FrontEnd** blade, klikt u op **opslaan**.
   
    ![Azure portal - subnetinstellingen](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Maken van de NSG NSG-back-end
Maken van de **NSG-back-end** NSG en koppel deze aan de **back-end** subnet, volg de onderstaande stappen.

1. Herhaal de stappen in [maken het NSG NSG-FrontEnd](#Create-the-NSG-FrontEnd-NSG) voor het maken van een NSG met de naam *NSG-back-end*
2. Herhaal de stappen in [regels maken in een bestaande NSG](#Create-rules-in-an-existing-NSG) maken de **inkomende** regels in de onderstaande tabel.
   
   | Regel voor binnenkomende verbindingen | Uitgaande regel |
   | --- | --- |
   | ![Azure portal - regel voor binnenkomende verbindingen](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Azure portal - uitgaande regel](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Herhaal de stappen in [het NSG aan het subnet FrontEnd koppelt](#Associate-the-NSG-to-the-FrontEnd-subnet) koppelen de **NSG-back-end** NSG aan de **back-end** subnet.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [bestaande nsg's beheren](virtual-network-manage-nsg-arm-portal.md)
* [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor nsg's.

