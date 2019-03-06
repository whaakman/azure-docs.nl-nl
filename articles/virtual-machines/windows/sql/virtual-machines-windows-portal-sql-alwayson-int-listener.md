---
title: Een beschikbaarheidsgroeplistener van SQL Server in virtuele machines van Azure maken | Microsoft Docs
description: Stapsgewijze instructies voor het maken van een listener voor een AlwaysOn-beschikbaarheidsgroep voor SQL Server in virtuele machines van Azure
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/16/2017
ms.author: mikeray
ms.openlocfilehash: 43f2694f597d99edaf127a6afd64376cca33dad2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57448149"
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Een load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure
In dit artikel wordt uitgelegd hoe u een load balancer voor een SQL Server Always On-beschikbaarheidsgroep maken in Azure virtuele machines die worden uitgevoerd met Azure Resource Manager. Een beschikbaarheidsgroep is een load balancer vereist bij de SQL Server-exemplaren op Azure virtual machines zijn. De load balancer slaat het IP-adres voor de beschikbaarheidsgroep-listener. Als een beschikbaarheidsgroep meerdere regio's omvat, moet elke regio een load balancer.

Als u wilt deze taak hebt voltooid, moet u hebt een SQL Server-beschikbaarheidsgroep geïmplementeerd op Azure virtual machines die worden uitgevoerd met Resource Manager. Beide virtuele machines met SQL Server moet behoren tot dezelfde beschikbaarheidsset. U kunt de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatisch de beschikbaarheidsgroep maken in Resource Manager. Deze sjabloon maakt automatisch een interne load balancer voor u. 

Als u liever, kunt u [handmatig configureren van een beschikbaarheidsgroep](virtual-machines-windows-portal-sql-availability-group-tutorial.md).

In dit artikel is vereist dat al uw beschikbaarheidsgroepen zijn geconfigureerd.  

Verwante onderwerpen zijn onder andere:

* [AlwaysOn-beschikbaarheidsgroepen configureren in Azure VM (GUI)](virtual-machines-windows-portal-sql-availability-group-tutorial.md)   
* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Door dit artikel hek, maken en configureren van een load balancer in Azure portal. Nadat het proces voltooid is, configureert u het cluster voor het gebruik van het IP-adres van de load balancer voor de beschikbaarheidsgroep-listener.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Maken en configureren van de load balancer in Azure portal
In dit gedeelte van de taak, het volgende doen:

1. In de Azure-portal, de load balancer maken en configureren van het IP-adres.
2. Configureer de back-end-adresgroep.
3. Maak de test. 
4. Stel de load balancer-regels.

> [!NOTE]
> Als de SQL Server-exemplaren in meerdere resourcegroepen en regio's, uitvoeren van elke stap twee keer één keer in elke resourcegroep.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>Stap 1: De load balancer maken en configureren van het IP-adres
Maak eerst de load balancer. 

1. Open de resourcegroep met de SQL Server-machines in Azure portal. 

2. Klik in de resourcegroep op **toevoegen**.

3. Zoeken naar **netwerktaakverdeler** en selecteer vervolgens in de lijst met zoekresultaten **Load Balancer**, die is gepubliceerd via **Microsoft**.

4. Op de **Load Balancer** blade, klikt u op **maken**.

5. In de **load balancer maken** dialoogvenster vak, configureer de load balancer als volgt:

   | Instelling | Value |
   | --- | --- |
   | **Naam** |Een naam voor de load balancer. Bijvoorbeeld, **sqlLB**. |
   | **Type** |**Interne**: De meeste implementaties gebruikt een interne load balancer, waardoor toepassingen binnen hetzelfde virtuele netwerk verbinding maken met de beschikbaarheidsgroep.  </br> **Externe**: Zorgt ervoor dat toepassingen verbinding maken met de beschikbaarheidsgroep via een openbare internetverbinding. |
   | **Virtueel netwerk** |Selecteer het virtuele netwerk die de SQL Server-exemplaren in. |
   | **Subnet** |Selecteer het subnet die de SQL Server-exemplaren in. |
   | **IP-adrestoewijzing** |**Statische** |
   | **Privé IP-adres** |Geef een beschikbaar IP-adres van het subnet. Gebruik dit IP-adres wanneer u een listener op het cluster maakt. In een PowerShell-script, verderop in dit artikel gebruikt u dit adres voor de `$ILBIP` variabele. |
   | **Abonnement** |Als u meerdere abonnementen hebt, kan dit veld weergegeven. Selecteer het abonnement dat u wilt koppelen aan deze resource. Het is normaal hetzelfde abonnement als alle resources voor de beschikbaarheidsgroep. |
   | **Resourcegroep** |Selecteer de resourcegroep die de SQL Server-exemplaren in. |
   | **Locatie** |Selecteer de Azure-locatie hebben als de SQL Server-exemplaren. |

6. Klik op **Create**. 

Azure maakt de load balancer. De load balancer behoort tot een bepaald netwerk, subnet, resourcegroep en locatie. Nadat Azure is de taak voltooid, controleert u of de instellingen van de load balancer in Azure. 

### <a name="step-2-configure-the-back-end-pool"></a>Stap 2: De back-endpool configureren
Azure roept de back-end-adresgroep *back-endpool*. In dit geval is de back-end-adrespool de adressen van de twee SQL Server-exemplaren in de beschikbaarheidsgroep. 

1. Klik op de load balancer die u hebt gemaakt in de resourcegroep. 

2. Op **instellingen**, klikt u op **back-endpools**.

3. Op **back-endpools**, klikt u op **toevoegen** te maken van een back-end-adresgroep. 

4. Op **back-endgroep toevoegen**onder **naam**, typ een naam voor de back-endpool.

5. Onder **virtuele machines**, klikt u op **toevoegen van een virtuele machine**. 

6. Onder **virtuele machines kiezen**, klikt u op **Kies een beschikbaarheidsset**, en geef vervolgens de beschikbaarheidsset dat de SQL Server-machines om te horen.

7. Nadat u de beschikbaarheidsset hebt gekozen, klikt u op **kiest u de virtuele machines**, selecteert u de twee virtuele machines die als host fungeren de SQL Server-exemplaren in de beschikbaarheidsgroep en klik vervolgens op **Selecteer**. 

8. Klik op **OK** sluit de blades voor **virtuele machines kiezen**, en **back-endgroep toevoegen**. 

Azure werkt de instellingen voor de back-end-adresgroep. Uw beschikbaarheidsset heeft nu een pool van twee SQL Server-exemplaren.

### <a name="step-3-create-a-probe"></a>Stap 3: Een test maken
De test wordt gedefinieerd hoe Azure controleert of welke van de SQL Server-exemplaren dat momenteel eigenaar is van de beschikbaarheidsgroep-listener. Azure controleert de service op basis van het IP-adres op een poort die u opgeeft bij het maken van de test.

1. Op de load balancer **instellingen** blade, klikt u op **statuscontroles**. 

2. Op de **statuscontroles** blade, klikt u op **toevoegen**.

3. De test configureren op de **test toevoegen** blade. Gebruik de volgende waarden voor de test configureren:

   | Instelling | Value |
   | --- | --- |
   | **Naam** |Een naam voor de test. Bijvoorbeeld, **SQLAlwaysOnEndPointProbe**. |
   | **Protocol** |**TCP** |
   | **Poort** |U kunt een beschikbare poort gebruiken. Bijvoorbeeld, *59999*. |
   | **Interval** |*5* |
   | **Drempelwaarde voor onjuiste status** |*2* |

4.  Klik op **OK**. 

> [!NOTE]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van zowel SQL Server-exemplaren is. Beide exemplaren vereisen een inkomende regel voor de TCP-poort die u gebruikt. Zie voor meer informatie, [toevoegen of bewerken firewallregel](https://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure wordt de test gemaakt en vervolgens gebruikt om te testen die SQL Server-exemplaar de listener voor de beschikbaarheidsgroep heeft.

### <a name="step-4-set-the-load-balancing-rules"></a>Stap 4: Instellen van de load balancer-regels
De regels voor taakverdeling configureren hoe de load balancer verkeer routeert naar de SQL Server-exemplaren. Voor deze load balancer inschakelen u direct server return, omdat er slechts één van de twee SQL Server-exemplaren is eigenaar van de listener voor de beschikbaarheidsgroepresource tegelijk.

1. Op de load balancer **instellingen** blade, klikt u op **Taakverdelingsregels**. 

2. Op de **Taakverdelingsregels** blade, klikt u op **toevoegen**.

3. Op de **taakverdelingsregels toevoegen** blade configureren van de load balancer-regel. Gebruik de volgende instellingen: 

   | Instelling | Value |
   | --- | --- |
   | **Naam** |Een naam voor de load balancer-regels. Bijvoorbeeld, **SQLAlwaysOnEndPointListener**. |
   | **Protocol** |**TCP** |
   | **Poort** |*1433* |
   | **Back-Endpoort** |*1433*. Deze waarde wordt genegeerd omdat deze regel maakt gebruik van **zwevend IP (direct server return)**. |
   | **Probe** |Gebruik de naam van de test die u hebt gemaakt voor deze load balancer. |
   | **Sessiepersistentie** |**Geen** |
   | **Time-out voor inactiviteit (minuten)** |*4* |
   | **Zwevend IP (direct server return)** |**Ingeschakeld** |

   > [!NOTE]
   > Mogelijk moet u schuiven lager op de blade om alle instellingen weer te geven.
   > 

4. Klik op **OK**. 
5. Azure configureert u de load balancer-regel. De load balancer is nu geconfigureerd om verkeer te routeren naar het SQL Server-exemplaar dat als host fungeert voor de listener voor de beschikbaarheidsgroep. 

Op dit moment is de resourcegroep een load balancer die verbinding met zowel SQL Server-machines maakt. De load balancer bevat ook een IP-adres voor de SQL Server Always On beschikbaarheidsgroep-listener, zodat beide computers op aanvragen van de beschikbaarheidsgroepen reageren kunt.

> [!NOTE]
> Als uw SQL Server-exemplaren in twee afzonderlijke regio's zijn, herhaalt u de stappen in de andere regio. Elke regio vereist een load balancer. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Het cluster voor het gebruik van de IP-adres van de load balancer configureren
De volgende stap is het configureren van de listener op het cluster en de listener online te brengen. Ga als volgt te werk: 

1. De beschikbaarheidsgroep-listener op het failovercluster maken. 

2. De listener online brengen.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>Stap 5: De beschikbaarheidsgroep-listener op het failovercluster maken
In deze stap maakt maken u handmatig de beschikbaarheidsgroep-listener in Failoverclusterbeheer en SQL Server Management Studio.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Controleer de configuratie van de listener

Als de clusterresources en de afhankelijkheden juist zijn geconfigureerd, zou het mogelijk om de listener in SQL Server Management Studio weer te geven. Om in te stellen de listener-poort, het volgende doen:

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

2. Ga naar **AlwaysOn hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **beschikbaarheidsgroep Listeners**.  
    U ziet nu de listener met de naam die u hebt gemaakt in Failoverclusterbeheer. 

3. Met de rechtermuisknop op de listenernaam van de, en klik vervolgens op **eigenschappen**.

4. In de **poort** vak, geef het poortnummer voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), en klik vervolgens op **OK**.

U hebt nu een beschikbaarheidsgroep in Azure virtuele machines die in Resource Manager-modus uitgevoerd. 

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener
Test de verbinding door het volgende te doen:

1. RDP naar een SQL Server-exemplaar dat in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. Deze server worden de andere SQL Server-exemplaar in het cluster.

2. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script maakt een **sqlcmd** verbinding met de primaire replica met de listener met de Windows-verificatie:
   
        sqlcmd -S <listenerName> -E

De verbinding met SQLCMD maakt automatisch verbinding met het SQL Server-exemplaar dat als host fungeert voor de primaire replica. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Een IP-adres voor een aanvullende beschikbaarheidsgroep maken

Elke beschikbaarheidsgroep maakt gebruik van een afzonderlijke listener. Elke listener beschikt over een eigen IP-adres. Gebruik dezelfde load balancer voor het opslaan van het IP-adres voor extra listeners. Nadat u een beschikbaarheidsgroep hebt gemaakt, het IP-adres toevoegen aan de load balancer en configureer vervolgens de listener.

Een IP-adres toevoegen aan een load balancer met de Azure-portal, het volgende doen:

1. In de Azure-portal, opent u de resourcegroep met de load balancer en klik vervolgens op de load balancer. 

2. Onder **instellingen**, klikt u op **Frontend-IP-adresgroep**, en klik vervolgens op **toevoegen**. 

3. Onder **frontend-IP-adres toevoegen**, een naam op voor de front-end toewijzen. 

4. Controleer de **virtueel netwerk** en de **Subnet** zijn hetzelfde als de SQL Server-exemplaren.

5. Stel het IP-adres voor de listener. 
   
   >[!TIP]
   >U kunt het IP-adres in statisch en typt u een adres dat wordt momenteel niet gebruikt in het subnet. U kunt ook het IP-adres ingesteld op dynamisch en opslaan van de nieuwe front-end-IP-adresgroep. Als u dit doet, de Azure-portal een beschikbaar IP-adres automatisch toegewezen aan de groep. U kunt vervolgens opnieuw de front-end-IP-adresgroep en de toewijzing wijzigen in statisch. 

6. Sla het IP-adres voor de listener. 

7. Een statustest toevoegen met behulp van de volgende instellingen:

   |Instelling |Value
   |:-----|:----
   |**Naam** |Een unieke naam voor de test.
   |**Protocol** |TCP
   |**Poort** |Een niet-gebruikte TCP-poort, die beschikbaar zijn op alle virtuele machines moet zijn. Het kan niet worden gebruikt voor andere doeleinden. Er zijn geen twee listeners kunnen dezelfde testpoort gebruiken. 
   |**Interval** |Tijdsduur tussen testpogingen. Gebruik de standaardwaarde (5).
   |**Drempelwaarde voor onjuiste status** |Het aantal opeenvolgende drempelwaarden die er mislukken moeten voordat een virtuele machine als slecht beschouwd.

8. Klik op **OK** om op te slaan van de test. 

9. Maak een regel voor taakverdeling. Klik op **Taakverdelingsregels**, en klik vervolgens op **toevoegen**.

10. De nieuwe load balancer-regel met behulp van de volgende instellingen configureren:

   |Instelling |Value
   |:-----|:----
   |**Naam** |Een unieke naam voor de load balancer-regel. 
   |**Frontend-IP-adres** |Selecteer het IP-adres dat u hebt gemaakt. 
   |**Protocol** |TCP
   |**Poort** |Gebruik de poort die van de SQL Server-exemplaren gebruikmaken. Een standaardexemplaar gebruikt poort 1433, tenzij u deze hebt gewijzigd. 
   |**Back-endpoort** |Gebruik dezelfde waarde als **poort**.
   |**Back-endpool** |De groep met de virtuele machines met de SQL Server-exemplaren. 
   |**Statustest** |Kies de test die u hebt gemaakt.
   |**Sessiepersistentie** |Geen
   |**Time-out voor inactiviteit (minuten)** |Standaard (4)
   |**Zwevend IP (direct server return)** | Ingeschakeld

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>De beschikbaarheidsgroep voor het gebruik van het nieuwe IP-adres configureren

Herhaal de stappen die u hebt gevolgd, wanneer u de eerste beschikbaarheidsgroep hebt gemaakt voor het voltooien van het cluster te configureren. Dat wil zeggen, configureer de [cluster naar het nieuwe IP-adres gebruiken](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Wanneer u een IP-adres hebt toegevoegd voor de listener, moet u de aanvullende beschikbaarheidsgroep configureren door het volgende te doen: 

1. Controleer of de testpoort voor het nieuwe IP-adres in beide virtuele machines met SQL Server open. 

2. [In de Cluster-Manager toevoegen de client access point](#addcap).

3. [Configureer de IP-resource voor de beschikbaarheidsgroep](#congroup).

   >[!IMPORTANT]
   >Wanneer u het IP-adres maakt, gebruikt u het IP-adres dat u hebt toegevoegd aan de load balancer.  

4. [Controleer de SQL Server-beschikbaarheidsgroepresource afhankelijk van de client access point](#dependencyGroup).

5. [Controleer de afhankelijk van het IP-adres van client access point resource](#listname).
 
6. [De clusterparameters instellen in PowerShell](#setparam).

Nadat u de beschikbaarheidsgroep voor het gebruik van het nieuwe IP-adres hebt geconfigureerd, configureert u de verbinding met de listener. 

## <a name="add-load-balancing-rule-for-distributed-availability-group"></a>Regel voor de gedistribueerde beschikbaarheidsgroep voor taakverdeling toevoegen

Als een beschikbaarheidsgroep maakt deel uit van een gedistribueerde beschikbaarheidsgroep, moet de load balancer een extra regel. Deze regel wordt de poort die wordt gebruikt door de gedistribueerde beschikbaarheidsgroep-listener opgeslagen.

>[!IMPORTANT]
>Deze stap is alleen van toepassing als de beschikbaarheidsgroep maakt deel uit van een [gedistribueerde beschikbaarheidsgroep](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups). 

1. Maak een inkomende regel op de gedistribueerde beschikbaarheidsgroep-listener TCP-poort op elke server die deel uitmaakt van de gedistribueerde beschikbaarheidsgroep. In veel voorbeelden gebruikt de documentatie bij 5022. 

1. Klik in de Azure-portal op de load balancer en klikt u op **Taakverdelingsregels**, en klik vervolgens op **+ toevoegen**. 

1. Maak de load balancer-regel met de volgende instellingen:

   |Instelling |Value
   |:-----|:----
   |**Naam** |Een unieke naam voor de load balancer-regel voor de gedistribueerde beschikbaarheidsgroep. 
   |**Frontend-IP-adres** |Gebruik de dezelfde frontend-IP-adres als de beschikbaarheidsgroep.
   |**Protocol** |TCP
   |**Poort** |5022: de poort voor de [gedistribueerde beschikbaarheidsgroep-listener-eindpunt](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-distributed-availability-groups).</br> Een beschikbare poort kan zijn.  
   |**Back-endpoort** | 5022: gebruik dezelfde als waarde **poort**.
   |**Back-endpool** |De groep met de virtuele machines met de SQL Server-exemplaren. 
   |**Statustest** |Kies de test die u hebt gemaakt.
   |**Sessiepersistentie** |Geen
   |**Time-out voor inactiviteit (minuten)** |Standaard (4)
   |**Zwevend IP (direct server return)** | Ingeschakeld

Herhaal deze stappen voor de load balancer op de andere beschikbaarheidsgroepen die deel uitmaken van de gedistribueerde beschikbaarheidsgroepen.

Als u beperken van toegang met een Azure Network Security Group, zorg ervoor dat de regels voor toestaan de back-end SQL Server VM-IP-adressen bevatten, en de load balancer zwevend IP-adressen voor de AG-listener en core IP-adres van het cluster, indien van toepassing.

## <a name="next-steps"></a>Volgende stappen

- [Een SQL Server Always On-beschikbaarheidsgroep configureren op Azure virtual machines in verschillende regio 's](virtual-machines-windows-portal-sql-availability-group-dr.md)
