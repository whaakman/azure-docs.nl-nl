---
title: De Load Balancer op meerdere IP-configuraties in Azure | Microsoft Docs
description: Taakverdeling over de primaire en secundaire IP-configuraties.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: ecb64aa13b3b08f7b054a0665df3dc0cdb3e09bd
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-by-using-the-azure-portal"></a>De Load Balancer op meerdere IP-configuraties met behulp van de Azure-portal

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [CLI](load-balancer-multiple-ip-cli.md)

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

In dit artikel gaan we hoe u Azure Load Balancer gebruiken met meerdere IP-adressen op een secundaire network interfacecontroller (NIC). Het volgende diagram illustreert in ons scenario:

![Load balancer-scenario](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

In ons scenario we maken gebruik van de volgende configuratie:

- Twee virtuele machines (VM's) die Windows worden uitgevoerd.
- Elke virtuele machine heeft een primaire en een secundaire NIC.
- Elke secundaire NIC heeft twee IP-configuraties.
- Elke virtuele machine fungeert als host voor twee websites: contoso.com en fabrikam.com.
- Elke website is gebonden aan een IP-configuratie voor de secundaire NIC.
- Azure Load Balancer wordt gebruikt om twee front-end-IP-adressen, één voor elke website. De front-adressen worden gebruikt voor het distribueren van het verkeer naar de respectieve IP-configuratie voor elke website.
- Hetzelfde poortnummer wordt gebruikt voor zowel de front-end-IP-adressen en de back-end-pool IP-adressen.

## <a name="prerequisites"></a>Vereisten

Ons scenario voorbeeld wordt ervan uitgegaan dat u een resourcegroep met de naam **contosofabrikam** die geconfigureerd is als volgt:

- De resourcegroep omvat een virtueel netwerk met de naam **myVNet**.
- De **myVNet** netwerk omvat twee virtuele machines met de naam **VM1** en **VM2**.
- VM1 en VM2 zich in dezelfde beschikbaarheidsset benoemde **myAvailset**. 
- VM1 en VM2 hebt u een primaire NIC met de naam **VM1NIC1** en **VM2NIC1**respectievelijk. 
- VM1 en VM2 hebt u een secundaire NIC met de naam **VM1NIC2** en **VM2NIC2**respectievelijk.

Zie voor meer informatie over het maken van virtuele machines met meerdere NIC's [een virtuele machine maken met meerdere NIC's met behulp van PowerShell](../virtual-machines/windows/multiple-nics.md).

## <a name="perform-load-balancing-on-multiple-ip-configurations"></a>Taakverdeling op meerdere IP-configuraties uitvoeren

De volgende stappen om te zorgen voor het scenario in dit artikel.

### <a name="step-1-configure-the-secondary-nics"></a>Stap 1: Configureer de secundaire NIC 's

Voor elke virtuele machine in uw virtuele netwerk, voegt u de IP-configuratie voor de Netwerkkaart op secundaire:  

1. Blader naar de Azure-portal: http://portal.azure.com. Aanmelden met uw Azure-account.

2. Selecteer in de linkerbovenhoek van het scherm de **resourcegroep** pictogram. Selecteer vervolgens de resourcegroep waar uw virtuele machines zich bevinden (bijvoorbeeld **contosofabrikam**). De **resourcegroepen** deelvenster ziet u alle resources en NIC's voor de virtuele machines.

3. Voor de secundaire NIC van elke virtuele machine, voegt u de IP-configuratie:

    1. Selecteer de secundaire NIC die u wilt configureren.
    
    2. Selecteer **IP-configuraties**. Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**.

    3. Onder **toevoegen IP-configuraties**, een tweede IP-configuratie toevoegen aan de NIC: 

        1. Voer een naam voor de secundaire IP-configuratie. (Bijvoorbeeld voor VM1 en VM2, de naam van de IP-configuratie **VM1NIC2 ipconfig2** en **VM2NIC2 ipconfig2**respectievelijk.)

        2. Voor de **particuliere IP-adres**, **toewijzing** optie **statische**.

        3. Selecteer **OK**.

Na het tweede IP-configuratie voor de secundaire NIC voltooid is, wordt weergegeven onder de **IP-configuraties** instellingen voor de opgegeven netwerkadapter.

### <a name="step-2-create-the-load-balancer"></a>Stap 2: De load balancer maken

De load balancer voor de configuratie maken:

1. Blader naar de Azure-portal: http://portal.azure.com. Aanmelden met uw Azure-account.

2. Selecteer in de linkerbovenhoek van het scherm **nieuw** > **Networking** > **Load Balancer**. Selecteer vervolgens **maken**.

3. Onder **maken load balancer**, typ een naam voor de load balancer. In dit scenario we maken gebruik van de naam van de **mylb**.

4. Onder **openbaar IP-adres**, maak een nieuwe openbare IP-adres aangeroepen **PublicIP1**.

5. Onder **resourcegroep**, selecteert u de bestaande resourcegroep voor uw virtuele machines (bijvoorbeeld **contosofabrikam**). Selecteer de locatie voor het implementeren van de load balancer op en selecteer vervolgens **OK**.

De load balancer wordt gestart om te implementeren. Implementatie kan enkele minuten voor het voltooien van duren. Nadat de implementatie is voltooid, wordt de load balancer wordt weergegeven als een resource in de resourcegroep.

### <a name="step-3-configure-the-front-end-ip-pool"></a>Stap 3: Configureer de front-end-IP-adresgroep

Configureer de front-end-IP-adresgroep op de load balancer voor elke website (contoso.com en fabrikam.com):

1. Selecteer in de portal **meer services**. Typ in het filtervak **openbaar IP-adres** en selecteer vervolgens **openbare IP-adressen**. Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**.

2. Configureer twee openbare IP-adressen (**PublicIP1** en **PublicIP2**) voor beide websites (contoso.com en fabrikam.com):

    1. Typ een naam voor de front-end-IP-adres.

    2. Voor **resourcegroep**, selecteert u de bestaande resourcegroep voor uw virtuele machines (bijvoorbeeld **contosofabrikam**).

    3. Voor **locatie**, selecteert u dezelfde locatie als de virtuele machines.

    4. Selecteer **OK**.

    Nadat het openbare IP-adressen zijn gemaakt, worden weergegeven onder de **openbare IP-adres** adressen.

3. <a name="step3-3"></a>Selecteer in de portal **meer services**. Typ in het filtervak **netwerktaakverdeler** en selecteer vervolgens **Load Balancer**. 

4. Selecteer de load balancer (**mylb**) dat u wilt toevoegen aan de front-end-IP-adresgroep.

5. Onder **instellingen**, selecteer **Frontend Pools**. Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**.

6. Typ een naam voor uw front-end-IP-adres (bijvoorbeeld **contosofe** of **fabrikamfe**).

7. <a name="step3-7"></a>Selecteer **IP-adres**. Onder **Kies openbaar IP-adres**, selecteert u de IP-adressen voor de front-(**PublicIP1** of **PublicIP2**).

8. De tweede front-end-IP-adres maken door te herhalen <a href="#step3-3">stap 3</a> via <a href="#step3-7">stap 7</a> in deze sectie.

Nadat de front-toepassingen is geconfigureerd, de IP-adressen worden weergegeven onder de load balancer **Frontend-IP-adresgroep** instellingen. 
    
### <a name="step-4-configure-the-back-end-pool"></a>Stap 4: Configureer de back-end-adresgroep

Voor elke website (contoso.com en fabrikam.com), configureert u de back-end-adresgroep op de load balancer:
        
1. Selecteer in de portal **meer services**. Typ in het filtervak **netwerktaakverdeler** en selecteer vervolgens **Load Balancer**.

2. Selecteer de load balancer (**mylb**) dat u wilt de back-end-pool aan toevoegen.

3. Onder **instellingen**, selecteer **back-Endpools**. Typ een naam voor uw back-end-pool (bijvoorbeeld **contosopool** of **fabrikampool**). Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**. 

4. Voor **die is gekoppeld aan**, selecteer **beschikbaarheidsset**.

5. Voor **beschikbaarheidsset**, selecteer **myAvailset**.

6. De doel-IP-netwerkconfiguraties voor beide virtuele machines toevoegen: 

    ![Configureer back-end-adresgroepen voor de load balancer](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
    
    1. Voor **doel-virtuele machine**, selecteert u de virtuele machine die u wilt toevoegen aan de groep back-end (bijvoorbeeld **VM1** of **VM2**).

    2. Voor **netwerk-IP-configuratie**, selecteert u de IP-adresconfiguratie van de secundaire NIC voor de virtuele machine die u hebt geselecteerd in de vorige stap (bijvoorbeeld **VM1NIC2 ipconfig2** of **VM2NIC2 ipconfig2** ).

7. Selecteer **OK**.

Nadat de back-end-adresgroep is geconfigureerd, de adressen worden weergegeven onder de load balancer **back-endpool** instellingen.

### <a name="step-5-configure-the-health-probe"></a>Stap 5: De health test configureren

Configureer een health test voor de load balancer:

1. Selecteer in de portal **meer services**. Typ in het filtervak **netwerktaakverdeler** en selecteer vervolgens **Load Balancer**.

2. Selecteer de load balancer (**mylb**) dat u wilt toevoegen aan de health-test.

3. Onder **instellingen**, selecteer **Health test**. Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**. 

4. Typ een naam voor de health-test (bijvoorbeeld **HTTP**). Selecteer **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>Stap 6: Configureer regels voor taakverdeling

Configureer de load-balancingregels voor elke website (contoso.com en fabrikam.com):
    
1. <a name="step6-1"></a>Onder **instellingen**, selecteer **Health test**. Selecteer in het volgende deelvenster aan de bovenkant **toevoegen**. 

2. Voor **naam**, typ een naam voor de taakverdelingsregel (bijvoorbeeld **HTTPc** voor contoso.com is, of **HTTPf** voor fabrikam.com).

3. Voor **Frontend IP-adres**, selecteert u de front-end-IP-adres dat u eerder hebt gemaakt (bijvoorbeeld **contosofe** of **fabrikamfe**).

4. Voor **poort** en **backendpoort**, hou de standaardwaarde **80**.

5. Voor **zwevend IP (direct server return)**, selecteer **ingeschakeld**.

6. <a name="step6-6"></a>Selecteer **OK**.

7. De tweede regel voor load balancer maken door te herhalen <a href="#step6-1">stap 1</a> via <a href="#step6-6">stap 6</a> in deze sectie.

Nadat de regels zijn geconfigureerd, worden deze weergegeven onder de load balancer **Taakverdelingsregels** instellingen.

### <a name="step-7-configure-dns-records"></a>Stap 7: DNS-records configureren

Configureer uw DNS-bronrecords om te verwijzen naar de respectieve front-end-IP-adressen voor de load balancer als de laatste stap. U kunt uw domeinen in Azure DNS hosten. Zie voor meer informatie over het gebruik van Azure DNS met Load Balancer [met behulp van Azure DNS met andere Azure-services](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het combineren van de load balancer-services in Azure in [met gelijke taakverdeling van services in Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Meer informatie over hoe u verschillende typen logboeken kunt gebruiken om te beheren en oplossen van de load balancer in [analytics aanmelden voor Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md).
