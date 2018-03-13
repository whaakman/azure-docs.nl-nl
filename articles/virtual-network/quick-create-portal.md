---
title: Maak een virtueel netwerk van Azure - Portal | Microsoft Docs
description: "Snel informatie over het maken van een virtueel netwerk met de Azure-portal. Een virtueel netwerk kunt Azure-resources, zoals virtuele machines, privé communiceren met elkaar en met het internet."
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
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c8f2cbe6b7377772e019a4ff90f91355ba0815ae
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Een virtueel netwerk maken met Azure Portal

Azure-resources, zoals virtuele machines (VM), kunt een virtueel netwerk privé communiceren met elkaar en met het internet. In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, kunt u twee virtuele machines in het virtuele netwerk implementeert. U vervolgens verbinding maken met één virtuele machine vanaf het internet en privé communicatie tussen de twee virtuele machines.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Voer, of selecteert, worden de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep| Selecteer **nieuw** en voer *myResourceGroup*.|
    |Locatie| Selecteer **VS-Oost**.|

    ![Geef algemene informatie over het virtuele netwerk](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee virtuele machines in het virtuele netwerk:

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

1. Selecteer **+ maken van een resource** gevonden in de linkerbovenhoek hoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.
3. Voer, of selecteert, worden de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVm1|
    |Gebruikersnaam| Voer een gebruikersnaam van uw keuze.|
    |Wachtwoord| Voer een wachtwoord van uw keuze. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep| Selecteer **gebruik bestaande** en selecteer **myResourceGroup**.|
    |Locatie| Selecteer **VS-Oost**|

    ![Basisprincipes van de virtuele machine](./media/quick-create-portal/virtual-machine-basics.png)

4. Selecteer een grootte voor de virtuele machine en selecteer vervolgens **Selecteer**.
5. Onder **instellingen**, accepteer alle standaardwaarden en selecteer vervolgens **OK**.

    ![Instellingen voor virtuele machines](./media/quick-create-portal/virtual-machine-settings.png)

6. Onder **maken** van de **samenvatting**, selecteer **maken** implementatie van de virtuele machine te starten. De virtuele machine duurt een paar minuten om te implementeren. 

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken

Stappen 1-6 opnieuw, maar in stap 3, de virtuele machine naam *myVm2*.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine vanaf het internet

1. Na *myVm1* is gemaakt, er verbinding mee maken. Aan de bovenkant van de Azure-portal, voer *myVm1*. Wanneer **myVm1** wordt weergegeven in de zoekresultaten, selecteer deze. Selecteer de **Connect** knop.

    ![Verbinding maken met een virtuele machine](./media/quick-create-portal/connect-to-virtual-machine.png)

2. Na het selecteren van de **Connect** knop, een Remote Desktop Protocol (RDP)-bestand wordt gemaakt en gedownload op uw computer.  
3. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine. U wilt selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **doorgaan**, om door te gaan met de verbinding.

## <a name="communicate-privately-between-vms"></a>Privé communiceren tussen VM 's

1. Voer vanuit PowerShell, `ping myvm2`. Ping mislukt, omdat ping gebruikmaakt van de internet control message protocol (ICMP) en ICMP is niet toegestaan via de Windows firewall standaard.
2. Om toe te staan *myVm2* te pingen *myVm1* in een latere stap, voert u de volgende opdracht vanuit PowerShell waarmee ICMP inkomende via de Windows firewall:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. Sluit de verbinding met extern bureaublad naar *myVm1*. 

4. Voer de stappen in [verbinding maken met een virtuele machine met het internet](#connect-to-a-vm-from-the-internet) opnieuw, maar verbinding maken met *myVm2*. Voer vanaf een opdrachtprompt `ping myvm1`.

    U ontvangt antwoorden van *myVm1*, omdat u ICMP toegestaan via de Windows firewall op de *myVm1* VM in de vorige stap.

5. Sluit de verbinding met extern bureaublad naar *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u gemaakt een standaard virtueel netwerk en twee virtuele machines. U verbonden met één virtuele machine vanaf het Internet en privé uitgewisseld tussen de virtuele machine en een andere virtuele machine. Zie voor meer informatie over virtuele-netwerkinstellingen, [beheren van een virtueel netwerk](manage-virtual-network.md).

Standaard Azure onbeperkte persoonlijke communicatie tussen virtuele machines, maar kunt alleen binnenkomende verbindingen met extern bureaublad voor Windows-VM's van het Internet. Voor informatie over het toestaan of beperken van verschillende soorten netwerkcommunicatie naar en van virtuele machines, Ga naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer](virtual-networks-create-nsg-arm-pportal.md)