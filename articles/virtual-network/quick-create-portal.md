---
title: Een virtueel netwerk maken in Azure - Portal | Microsoft Docs
description: "Snel informatie over het maken van een virtueel netwerk met de Azure-portal. Een virtueel netwerk kan veel soorten Azure-bronnen te privé met elkaar communiceren."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 8b6a4abdb7677417462392feade0c7cfdf99246f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Een virtueel netwerk maken met Azure Portal

In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, moet u twee virtuele machines implementeren in het virtuele netwerk en privé communiceren tussen hen, en met het internet.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Voer, of selecteert, worden de volgende informatie en selecteer vervolgens **maken**:
    - **Name**: *myVirtualNetwork*
    - **Adresruimte**: Accepteer de standaardgrootte. De adresruimte is opgegeven in CIDR-notatie.
    - **Abonnement**: uw abonnement te selecteren.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroup*.
    - **Locatie**: Selecteer * Oost-VS **.
    - **De naam van het subnet**: Accepteer de standaardinstelling.
    - **Subnet, adresbereik**: Accepteer de standaardinstelling.
    - **Service-eindpunten**: Accepteer de standaardinstelling.

    ![Geef algemene informatie over het virtuele netwerk](./media/quick-create-portal/virtual-network.png)

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtueel netwerk kunt verschillende soorten Azure-resources privé communiceren met elkaar en met het internet. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine.

1. Selecteer **+ maken van een resource** gevonden in de linkerbovenhoek hoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.
3. Voer, of selecteert, worden de volgende informatie en selecteer vervolgens **OK**:
    - **Name**: *myVm1*
    - **Gebruikersnaam**: Voer een gebruikersnaam van uw keuze.
    - **Wachtwoord**: Voer een wachtwoord van uw keuze. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
    - **Abonnement**: uw abonnement te selecteren.
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer **myResourceGroup**.
    - **Locatie**: Selecteer *VS-Oost*.

    ![Geef algemene informatie over een virtuele machine](./media/quick-create-portal/virtual-machine-basics.png)
4. Selecteer een grootte voor de virtuele machine en selecteer vervolgens **Selecteer**.
5. Onder **instellingen**, *myVirtualNetwork* moet al zijn geselecteerd voor **virtueel netwerk**, maar als dit niet het geval is, selecteert u **virtueel netwerk** , selecteer vervolgens *myVirtualNetwork*. Laat *standaard* voor geselecteerde **Subnet**, en selecteer vervolgens **OK**.

    ![Een virtueel netwerk selecteren](./media/quick-create-portal/virtual-machine-network-settings.png)
6. Op de **samenvatting** pagina **maken** implementatie van virtuele machine te starten. De virtuele machine duurt een paar minuten om te implementeren. 
7. Stappen 1-6 opnieuw, maar in stap 3, de virtuele machine naam *myVm2*.

## <a name="connect-to-a-virtual-machine"></a>Verbinding maken met een virtuele machine

1. Na *myVm1* is gemaakt, op afstand verbinding mee maken. Aan de bovenkant van de Azure-portal, voer *myVm1*. Wanneer **myVm1** wordt weergegeven in de zoekresultaten, selecteer deze. Selecteer de **Connect** knop.

    ![Overzicht van virtuele machines](./media/quick-create-portal/virtual-machine-overview.png)

2. Na het selecteren van de **Connect** knop, een Remote Desktop Protocol (RDP)-bestand wordt gemaakt en gedownload op uw computer.  
3. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), schakelt u op OK. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** of **doorgaan** om door te gaan met de verbinding.

## <a name="communicate-between-vms"></a>Communicatie tussen virtuele machines

1. Voer vanaf een opdrachtprompt `ping myvm2`. Ping mislukt, omdat het gebruik van ping ICMP- en ICMP is niet toegestaan via de Windows firewall standaard. Om toe te staan *myVm2* te pingen *myVm1* in een latere stap, voert u de volgende opdracht vanaf een opdrachtprompt:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

2. Sluit de verbinding met extern bureaublad naar *myVm1*. 

3. Voer de stappen in [verbinding maken met een virtuele machine](#connect-to-a-virtual-machine), maar verbinding maken met *myVm2*. Voer vanaf een opdrachtprompt `ping myvm1`.

    Kan wel worden wel pingen de *myVm1* virtuele machine van de *myVm2* virtuele machine omdat:

    - u ICMP toegestaan via de Windows firewall op de *myVm1* virtuele machine in de vorige stap.
    - Standaard kan Azure al het netwerkverkeer tussen resources in hetzelfde virtuele netwerk.

## <a name="communicate-to-the-internet"></a>Communicatie met internet

1. Tijdens het nog wordt verbonden met de *myVm2* invoeren van de virtuele machine vanaf een opdrachtprompt `ping bing.com`.

    U ontvangt vier reacties van bing.com. 

    Kan wel worden wel pingen een internet-bron van de *myVm2* virtuele machine, omdat een virtuele machine standaard uitgaand naar het internet kan communiceren.

2. De extern bureaublad-sessiehost af te sluiten.

## <a name="communicate-from-the-internet"></a>Communiceren via internet

1. Ophalen van het openbare IP-adres van de *myVm1* virtuele machine. Stap in de afbeelding die wordt weergegeven onder 1 in [verbinding maken met een virtuele machine](#connect-to-a-virtual-machine), ziet u een openbaar IP-adres. In de afbeelding, het adres is *13.90.241.247*. Een ander wordt adres voor uw virtuele machine. 

2. Pingen vanaf uw computer, het openbare IP-adres van uw *myVm1* virtuele machine. Pingen mislukt, ondanks dat ICMP geopend met behulp van Windows firewall is.

    Ping mislukt, omdat alle verkeer naar virtuele machines van Windows, met uitzondering van extern bureaublad-verbindingen via poort 3389 standaard door Azure, is geweigerd. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel als u een standaard virtueel netwerk met één subnet geïmplementeerd. Als u wilt weten hoe u een aangepaste virtueel netwerk maken met meerdere subnetten, blijven de zelfstudie voor het maken van een aangepaste virtueel netwerk.

> [!div class="nextstepaction"]
> [Een aangepaste virtueel netwerk maken](virtual-networks-create-vnet-arm-pportal.md)
