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
ms.openlocfilehash: 3c040f677aa25656148081d533e87cc55f1e22e7
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Een virtueel netwerk maken met Azure Portal

In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, kunt u twee virtuele machines implementeren in het virtuele netwerk om communicatie tussen deze twee particuliere netwerk te testen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Zoals u in de volgende afbeelding, voer *myVirtualNetwork* voor **naam**, *myResourceGroup* voor **resourcegroep**, selecteert u een  **Locatie** en uw **abonnement**, accepteer de standaardinstellingen van de resterende en selecteer vervolgens **maken**. 

    ![Geef algemene informatie over het virtuele netwerk](./media/quick-create-portal/virtual-network.png)

    De **adresruimte** is opgegeven in CIDR-notatie. Het adresruimte 10.0.0.0/16 omvat 10.0.0.0-10.0.255.254. Een virtueel netwerk bevat nul of meer subnetten. Het opgegeven subnet **-adresbereik** bevat het IP-adressen 10.0.0.0-10.0.0.255. Alleen 10.0.0.4-10.0.0.254 zijn echter beschikbaar omdat Azure reserveert de eerste vier adressen (0-3) en het laatste adres in elk subnet. De beschikbare IP-adressen zijn toegewezen aan resources die zijn geïmplementeerd in een virtueel netwerk.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Een virtueel netwerk kunt verschillende soorten Azure-bronnen te privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u persoonlijke communicatie tussen deze in een later stadium kunt valideren.

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Selecteer **+ maken van een resource** gevonden in de linkerbovenhoek hoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.
3. Geef informatie op virtuele machine die wordt weergegeven in de volgende afbeelding. De **gebruikersnaam** en **wachtwoord** u zich aanmelden bij de virtuele machine in een latere stap worden gebruikt. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). Selecteer uw **abonnement**, gebruikt de bestaande *myResourceGroup* resource groep en zorg ervoor dat de **locatie** geselecteerd is de dezelfde locatie die u hebt gemaakt de virtueel netwerk in. Als u klaar selecteren **OK**.

    ![Geef algemene informatie over een virtuele machine](./media/quick-create-portal/virtual-machine-basics.png)
4. Selecteer een grootte voor de virtuele machine en selecteer vervolgens **Selecteer**. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. De grootten die worden weergegeven voor u mogelijk anders dan het volgende voorbeeld: 

    ![Selecteer een grootte voor een virtuele machine](./media/quick-create-portal/virtual-machine-size.png)
5. Onder **instellingen**, *myVirtualNetwork* moet al zijn geselecteerd voor **virtueel netwerk**, maar als dit niet het geval is, selecteert u **virtueel netwerk** , selecteer vervolgens *myVirtualNetwork*. Laat *standaard* voor geselecteerde **Subnet**, en selecteer vervolgens **OK**.

    ![Een virtueel netwerk selecteren](./media/quick-create-portal/virtual-machine-network-settings.png)
6. Op de **samenvatting** pagina **maken** implementatie van virtuele machine te starten. 
7. De virtuele machine duurt een paar minuten maken. Na het maken, de virtuele machine is vastgemaakt aan de Azure-portaldashboard en de samenvatting van de virtuele machine automatisch wordt geopend. Selecteer **Networking**.

    ![Netwerkgegevens voor virtuele machine](./media/quick-create-portal/virtual-machine-networking.png)

    U ziet dat de **particuliere IP** adres *10.0.0.4*. In stap 5, onder **instellingen**, u hebt geselecteerd de *myVirtualNetwork* virtueel netwerk en het subnet met de naam geaccepteerd *standaard* voor **Subnet**. Wanneer u [gemaakt van het virtuele netwerk](#create-a-virtual-network), geaccepteerd van de standaardwaarde van 10.0.0.0/24 voor het subnet **-adresbereik**. Het eerste beschikbare adres voor het subnet dat u Azure DHCP-server toegewezen aan de virtuele machine. Omdat Azure de eerste vier adressen (0-3) van elk subnet gereserveerd, is het 10.0.0.4 het eerste beschikbare IP-adres beschikbaar is voor het subnet.

    De **openbare IP-adres** -adres is toegewezen, is anders dan het adres dat is toegewezen aan de virtuele machine. Een openbaar, Internet routeerbare IP-adres in Azure met elke virtuele machine standaard wordt toegewezen. Het openbare IP-adres is toegewezen aan de virtuele machine vanuit een [groep adressen toegewezen aan elke Azure-regio](https://www.microsoft.com/download/details.aspx?id=41653). Terwijl Azure welk openbare IP-adres is toegewezen aan een virtuele machine weet, is het besturingssysteem in een virtuele machine geen bewust te maken van een openbaar IP-adres toegewezen.
8. Stappen 1-7 opnieuw, maar in stap 3, de virtuele machine naam *myVm2*. 
9. Nadat de virtuele machine is gemaakt, selecteert u **Networking**, zoals stap 7. U ziet de **particuliere IP** adres *10.0.0.5*. Omdat Azure het eerste bruikbare adres van de eerder toegewezen *10.0.0.4* in het subnet de *myVm1* virtuele machine toegewezen *10.0.0.5* naar de  *myVm2* virtuele machine, omdat het adres van de volgende beschikbaar in het subnet.

### <a name="connect-to-a-virtual-machine"></a>Verbinding maken met een virtuele machine

1. Extern verbinding maken met de *myVm1* virtuele machine. Aan de bovenkant van de Azure-portal, voer *myVm1*. Wanneer **myVm1** wordt weergegeven in de zoekresultaten, selecteer deze. Selecteer de **Connect** knop.

    ![Overzicht van virtuele machines](./media/quick-create-portal/virtual-machine-overview.png)
2. Na het selecteren van de **Connect** knop, een Remote Desktop Protocol (RDP)-bestand wordt gemaakt en gedownload op uw computer.  
3. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine en selecteer vervolgens **OK**. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** of **doorgaan** om door te gaan met de verbinding.

### <a name="validate-communication"></a>Communicatie valideren

Probeert te pingen van een Windows-virtuele machine mislukt, omdat ping niet is toegestaan via de Windows firewall standaard. Om toe te staan ping naar *myVm1*, voer de volgende opdracht uit vanaf de opdrachtprompt:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Valideren van de communicatie met *myVm2*, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* virtuele machine. Geef de referenties die u hebt gebruikt toen u de virtuele machine gemaakt en voltooi vervolgens de verbinding:

```
mstsc /v:myVm2
```

Verbinding met extern bureaublad is mislukt omdat beide virtuele machines privé IP-adressen toegewezen vanuit de *standaard* subnet en omdat de extern bureaublad openen via de Windows-firewall standaard is. U bent geen verbinding maken met *myVm2* door hostnaam omdat Azure automatisch DNS-naamomzetting voor alle hosts binnen een virtueel netwerk biedt. Pingen vanaf een opdrachtprompt *myVm1*, van *myVm2*.

```
ping myvm1
```

Ping is voltooid, omdat deze toegestaan via de Windows firewall op de *myVm1* virtuele machine in de vorige stap. Om te bevestigen uitgaande communicatie met Internet, voer de volgende opdracht:

```
ping bing.com
```

U ontvangt vier reacties van bing.com. Standaard kan een virtuele machine in een virtueel netwerk uitgaand naar het Internet communiceren. 

De extern bureaublad-sessiehost af te sluiten.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel als u een standaard virtueel netwerk met één subnet geïmplementeerd. Als u wilt weten hoe u een aangepaste virtueel netwerk maken met meerdere subnetten, blijven de zelfstudie voor het maken van een aangepaste virtueel netwerk.

> [!div class="nextstepaction"]
> [Een aangepaste virtueel netwerk maken](virtual-networks-create-vnet-arm-pportal.md)
