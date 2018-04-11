---
title: Nsg's met de Azure portal beheren | Microsoft Docs
description: Informatie over het beheren van bestaande nsg's met de Azure portal.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-nsgs-using-the-portal"></a>Nsg's met behulp van de portal beheren

> [!div class="op_single_selector"]
> * [Portal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure-CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Informatie ophalen
U kunt uw bestaande nsg's weergeven, regels voor een bestaande NSG ophalen en weten welke resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande nsg's weergeven

Als u wilt weergeven van alle bestaande nsg's in een abonnement, moet u de volgende stappen uitvoeren:

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.

2. Klik op **Bladeren >** > **Netwerkbeveiligingsgroepen**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Controleer de lijst met nsg's in de **Netwerkbeveiligingsgroepen** blade.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Nsg's weergeven in een resourcegroep

De lijst weergeven met nsg's in de **RG NSG** resource groep, de volgende stappen uit:

1. Klik op **resourcegroepen >** > **RG NSG** > **...** .

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. In de lijst met resources zoeken naar objecten worden weergegeven als het NSG-pictogram, zoals wordt weergegeven in de **Resources** blade hieronder.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Lijst met alle regels voor een NSG

Om weer te geven van de regels van een NSG met de naam **NSG-FrontEnd**, de volgende stappen uit:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.

2. In de **instellingen** tabblad **inkomende beveiligingsregels**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. De **inkomende beveiligingsregels** blade wordt weergegeven zoals hieronder wordt weergegeven.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. In de **instellingen** tabblad **uitgaande beveiligingsregels** om te zien van de uitgaande regels.

    > [!NOTE]
    > Als standaardregels wilt weergeven, klikt u op de **regels standaard** bovenin de blade waarin de regels worden weergegeven.
    >

### <a name="view-nsgs-associations"></a>Nsg's koppelingen weergeven

Om weer te geven welke bronnen de **NSG-FrontEnd** NSG koppelen aan, de volgende stappen uit:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.

2. In de **instellingen** tabblad **subnetten** om weer te geven welke subnetten zijn gekoppeld aan het NSG.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. In de **instellingen** tabblad **netwerkinterfaces** om weer te geven wat NIC's zijn gekoppeld aan het NSG.

## <a name="manage-rules"></a>Regels beheren
U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en regels te verwijderen.

### <a name="add-a-rule"></a>Een regel toevoegen
Een regel voor toestaan toevoegen **inkomende** verkeer op poort **443** vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG, de volgende stappen uit:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.
2. In de **instellingen** tabblad **inkomende beveiligingsregels**.
3. In de **inkomende beveiligingsregels** blade, klikt u op **toevoegen**. Klik in de **de inkomende beveiligingsregel toevoegen** blade, vult u de waarden zoals hieronder wordt weergegeven en klik vervolgens op **OK**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Na enkele seconden, ziet u de nieuwe regel in de **inkomende beveiligingsregels** blade.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Een regel wijzigen
Wijzigen van de regel die eerder is gemaakt waarmee binnenkomend verkeer van de **Internet** alleen de volgende stappen uitvoeren:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.
2. In de **instellingen** en klik op de regel die eerder is gemaakt.
3. In de **toestaan https** blade wijzigen de **bron** eigenschap zoals hieronder wordt weergegeven en klik vervolgens op **opslaan**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Een regel verwijderen

Als u wilt verwijderen van de regel die eerder is gemaakt, moet u de volgende stappen uitvoeren:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.
2. In de **instellingen** en klik op de regel die eerder is gemaakt.
3. In de **toestaan https** blade, klikt u op **verwijderen**, en klik vervolgens op **Ja**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Koppelingen beheren
U kunt een NSG aan subnetten en NIC's kunt koppelen. U kunt ook een NSG van elke bron die deze gekoppeld ontkoppelen.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG aan een NIC koppelt
Om te koppelen de **NSG-FrontEnd** NSG aan de **TestNICWeb1** NIC, de volgende stappen uit:

1. Van de **Netwerkbeveiligingsgroepen** blade of de **Resources** blade hierboven, klikt u op **NSG-FrontEnd**.
2. In de **instellingen** tabblad **netwerkinterfaces** > **koppelen** > **TestNICWeb1**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Een NSG van een NIC ontkoppelen

Ontkoppelen de **NSG-FrontEnd** NSG van de **TestNICWeb1** NIC, de volgende stappen uit:

1. Klik in de Azure-portal op **resourcegroepen >** > **RG NSG** > **...**   >  **TestNICWeb1**.

2. In de **TestNICWeb1** blade, klikt u op **beveiliging wijzigen...**   >  **Geen**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> U kunt deze blade ook gebruiken om te koppelen van de NIC van een bestaande NSG.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Een NSG van een subnet ontkoppelen

Ontkoppelen de **NSG-FrontEnd** NSG van de **FrontEnd** het subnet van de volgende stappen uit:

1. Klik in de Azure-portal op **resourcegroepen >** > **RG NSG** > **...**   >  **TestVNet**.

2. In de **instellingen** blade, klikt u op **subnetten** > **FrontEnd** > **netwerkbeveiligingsgroep**  >  **Geen**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. In de **FrontEnd** blade, klikt u op **opslaan**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelt

Om te koppelen de **NSG-FrontEnd** NSG aan de **FronEnd** subnet opnieuw, de volgende stappen uitvoeren:

1. Klik in de Azure-portal op **resourcegroepen >** > **RG NSG** > **...**   >  **TestVNet**.
2. In de **instellingen** blade, klikt u op **subnetten** > **FrontEnd** > **netwerkbeveiligingsgroep** > **NSG-FrontEnd**.
3. In de **FrontEnd** blade, klikt u op **opslaan**.

> [!NOTE]
> U kunt ook een NSG koppelen aan een subnet van thh NSG van **instellingen** blade.
>

## <a name="delete-an-nsg"></a>Een NSG verwijderen
U kunt een NSG alleen verwijderen als deze niet aan een resource zijn gekoppeld. Voor het verwijderen van een NSG voert de volgende stappen uit:.

1. Klik in de Azure-portal op **resourcegroepen >** > **RG NSG** > **...**   >  **NSG-FrontEnd**.
2. In de **instellingen** blade, klikt u op **netwerkinterfaces**.
3. Als er NIC's die worden vermeld, klikt u op de NIC en volgt u stap 2 in [ontkoppelen van een NSG van een NIC](#Dissociate-an-NSG-from-a-NIC).
4. Herhaal stap 3 voor elke NIC.
5. In de **instellingen** blade, klikt u op **subnetten**.
6. Als er subnetten die worden vermeld, klikt u op het subnet en volg de stappen 2 en 3 in [een NSG van een subnet ontkoppelen](#Dissociate-an-NSG-from-a-subnet).
7. Schuift van links naar de **NSG-FrontEnd** blade, klikt u vervolgens op **verwijderen** > **Ja**.

    ![Azure portal - nsg 's](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Volgende stappen
* [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor nsg's.
