---
title: Maken van een SQL Server-beschikbaarheidsgroeplistener in virtuele machines in Azure | Microsoft Docs
description: Stapsgewijze instructies voor het maken van een listener voor een AlwaysOn-beschikbaarheidsgroep voor SQL Server in Azure virtuele machines
services: virtual-machines
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: monicar
ms.assetid: d1f291e9-9af2-41ba-9d29-9541e3adcfcf
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/01/2017
ms.author: mikeray
ms.openlocfilehash: 09fed7e785708d4afe64905de973becc188181d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-load-balancer-for-an-always-on-availability-group-in-azure"></a>Een load balancer voor een AlwaysOn-beschikbaarheidsgroep configureren in Azure
In dit artikel wordt uitgelegd hoe u een load balancer voor een SQL Server Always On-beschikbaarheidsgroep maken in Azure virtuele machines die worden uitgevoerd met Azure Resource Manager. Een beschikbaarheidsgroep is een load balancer vereist bij de SQL Server-exemplaren op Azure virtuele machines zijn. De load balancer slaat het IP-adres voor de beschikbaarheidsgroeplistener. Als een beschikbaarheidsgroep meerdere regio's omvat, moet elke regio een load balancer.

Om deze taak, moet u hebt een SQL Server-beschikbaarheidsgroep geïmplementeerd op Azure virtuele machines die worden uitgevoerd met Resource Manager. Beide virtuele machines van SQL Server moet behoren tot dezelfde beschikbaarheidsset. U kunt de [Microsoft sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) automatisch maken van de beschikbaarheidsgroep in het Resource Manager. Deze sjabloon maakt automatisch een interne load balancer voor u. 

Als u liever, kunt u [handmatig configureren van een beschikbaarheidsgroep](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In dit artikel is vereist dat uw beschikbaarheidsgroepen al zijn geconfigureerd.  

Verwante onderwerpen zijn:

* [Altijd op beschikbaarheidsgroepen configureren in Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
* [Een VNet-naar-VNet-verbinding configureren met behulp van Azure Resource Manager en PowerShell](../../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

Door de stappen in dit artikel, moet u maken en configureren van een load balancer in de Azure portal. Nadat het proces voltooid is, configureert u het cluster voor het gebruik van het IP-adres van de load balancer voor de beschikbaarheidsgroeplistener.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Maken en configureren van de load balancer in de Azure-portal
In dit gedeelte van de taak, het volgende doen:

1. In de Azure portal maken van de load balancer en het IP-adres configureren.
2. Configureer de back-end-adresgroep.
3. Maken van de test. 
4. De load-balancingregels ingesteld.

> [!NOTE]
> Als de SQL Server-exemplaren in meerdere resourcegroepen en regio's zijn, elke stap uitvoeren tweemaal eenmaal binnen elke resourcegroep.
> 
> 

### <a name="step-1-create-the-load-balancer-and-configure-the-ip-address"></a>Stap 1: De load balancer maken en configureren van het IP-adres
Maak eerst de load balancer. 

1. Open de resourcegroep met de virtuele machines van SQL Server in de Azure-portal. 

2. Klik in de resourcegroep op **toevoegen**.

3. Zoeken naar **netwerktaakverdeler** en selecteer vervolgens in de zoekresultaten **Load Balancer**, die is gepubliceerd via **Microsoft**.

4. Op de **Load Balancer** blade, klikt u op **maken**.

5. In de **maken load balancer** dialoogvenster de load balancer als volgt configureren:

   | Instelling | Waarde |
   | --- | --- |
   | **Naam** |Een naam voor de load balancer. Bijvoorbeeld: **sqlLB**. |
   | **Type** |**Interne**: de meeste implementaties gebruikt een interne load balancer, waarmee toepassingen binnen hetzelfde virtuele netwerk verbinding maken met de beschikbaarheidsgroep.  </br> **Externe**: zorgt ervoor dat toepassingen verbinding maken met de beschikbaarheidsgroep via een openbare internetverbinding. |
   | **Virtueel netwerk** |Selecteer het virtuele netwerk die de SQL Server-intances in. |
   | **Subnet** |Selecteer het subnet die de SQL Server-exemplaren in. |
   | **IP-adrestoewijzing** |**Statische** |
   | **Privé IP-adres** |Geef een beschikbaar IP-adres van het subnet. Dit IP-adres gebruiken bij het maken van een listener op het cluster. In een PowerShell-script wordt verderop in dit artikel gebruikt dit adres voor de `$ILBIP` variabele. |
   | **Abonnement** |Als u meerdere abonnementen hebt, wordt dit veld mogelijk weergegeven. Selecteer het abonnement dat u wilt koppelen aan deze resource. Dit is doorgaans hetzelfde abonnement als alle resources voor de beschikbaarheidsgroep. |
   | **Resourcegroep** |Selecteer de resourcegroep die de SQL Server-exemplaren in. |
   | **Locatie** |Selecteer de Azure-locatie die de SQL Server-exemplaren in. |

6. Klik op **Create**. 

Azure maakt de load balancer. De load balancer hoort bij een bepaalde netwerk, subnet, resourcegroep en locatie. Nadat de taken in Azure is voltooid, controleert u of de instellingen van de load balancer in Azure. 

### <a name="step-2-configure-the-back-end-pool"></a>Stap 2: Configureer de back-end-adresgroep
Azure roept de back-end-adresgroep *back-endpool*. In dit geval is de groep back-end de adressen van de twee SQL Server-exemplaren in de beschikbaarheidsgroep. 

1. Klik op de load balancer die u hebt gemaakt in de resourcegroep. 

2. Op **instellingen**, klikt u op **back-endpools**.

3. Op **back-endpools**, klikt u op **toevoegen** voor het maken van een back-end-adresgroep. 

4. Op **toevoegen van de back-endpool**onder **naam**, typ een naam voor de back-end-pool.

5. Onder **virtuele machines**, klikt u op **toevoegen van een virtuele machine**. 

6. Onder **virtuele machines kiezen**, klikt u op **een beschikbaarheidsset kiezen**, en geef vervolgens de beschikbaarheidsset dat de virtuele machines van SQL Server bij hoort.

7. Nadat u de beschikbaarheidsset hebt gekozen, klikt u op **de virtuele machines kiezen**, selecteert u de twee virtuele machines die als host fungeren de SQL Server-exemplaren in de beschikbaarheidsgroep en klik vervolgens op **Selecteer**. 

8. Klik op **OK** te sluiten van de blades voor **virtuele machines kiezen**, en **toevoegen van de back-endpool**. 

De instellingen voor de back-end-adresgroep voor Azure-updates. De beschikbaarheidsset heeft nu een pool van twee exemplaren van SQL Server.

### <a name="step-3-create-a-probe"></a>Stap 3: Een test maken
De test wordt gedefinieerd hoe Azure verifieert dat de SQL Server-exemplaren momenteel eigenaar is van de beschikbaarheidsgroeplistener. Azure-tests de service op basis van het IP-adres op een poort die u bij het maken van de test definiëren.

1. Op de load balancer **instellingen** blade, klikt u op **statuscontroles**. 

2. Op de **statuscontroles** blade, klikt u op **toevoegen**.

3. De test configureren op de **toevoegen test** blade. De volgende waarden gebruiken voor het configureren van de test:

   | Instelling | Waarde |
   | --- | --- |
   | **Naam** |Een naam voor de test. Bijvoorbeeld: **SQLAlwaysOnEndPointProbe**. |
   | **Protocol** |**TCP** |
   | **Poort** |U kunt een beschikbare poort gebruiken. Bijvoorbeeld: *59999*. |
   | **Interval** |*5* |
   | **Drempelwaarde voor onjuiste status** |*2* |

4.  Klik op **OK**. 

> [!NOTE]
> Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide exemplaren van SQL Server is. Beide exemplaren vereisen een inkomende regel voor de TCP-poort die u gebruikt. Zie voor meer informatie [toevoegen of bewerken firewallregel](http://technet.microsoft.com/library/cc753558.aspx). 
> 
> 

Azure maakt van de test en wordt vervolgens gebruikt voor het testen van welke SQL Server-exemplaar de listener voor de beschikbaarheidsgroep heeft.

### <a name="step-4-set-the-load-balancing-rules"></a>Stap 4: Stel de regels voor taakverdeling
De regels voor taakverdeling configureren hoe de load balancer verkeer gerouteerd naar de SQL Server-exemplaren. Voor deze load balancer inschakelen u direct server return omdat slechts één van de twee exemplaren van SQL Server eigenaar is van een listener op de beschikbaarheidsgroepresource tegelijk.

1. Op de load balancer **instellingen** blade, klikt u op **Taakverdelingsregels**. 

2. Op de **Taakverdelingsregels** blade, klikt u op **toevoegen**.

3. Op de **taakverdelingsregels toevoegen** blade de taakverdelingsregel configureren. Gebruik de volgende instellingen: 

   | Instelling | Waarde |
   | --- | --- |
   | **Naam** |Een naam voor de load-balancingregels. Bijvoorbeeld: **SQLAlwaysOnEndPointListener**. |
   | **Protocol** |**TCP** |
   | **Poort** |*1433* |
   | **Back-Endpoort** |*1433*. Deze waarde wordt genegeerd omdat deze regel maakt gebruik van **zwevend IP (direct server return)**. |
   | **Test** |Gebruik de naam van de test op die u hebt gemaakt voor deze load balancer. |
   | **Sessiepersistentie** |**Geen** |
   | **Time-out voor inactiviteit (minuten)** |*4* |
   | **Zwevend IP (direct server return)** |**Ingeschakeld** |

   > [!NOTE]
   > U moet wellicht Blader in de blade om de instellingen te bekijken.
   > 

4. Klik op **OK**. 
5. Azure configureert de taakverdelingsregel. De load balancer is nu geconfigureerd om verkeer te leiden naar het SQL Server-exemplaar dat als host fungeert voor de listener voor de beschikbaarheidsgroep. 

Op dit moment is de resourcegroep een load balancer die verbinding met beide SQL Server-machines maakt. De load balancer bevat ook een IP-adres voor de SQL Server Always On beschikbaarheidsgroep-listener, zodat beide computers op aanvragen voor de beschikbaarheidsgroepen reageren kan.

> [!NOTE]
> Als uw SQL Server-exemplaren in twee afzonderlijke regio's, Herhaal de stappen in andere regio. Elke regio vereist een load balancer. 
> 
> 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Het cluster voor het gebruik van het IP-adres van de load balancer configureren
De volgende stap is het configureren van de listener op het cluster en de listener online te brengen. Ga als volgt te werk: 

1. Maak de beschikbaarheidsgroep-listener op het failovercluster. 

2. De listener online brengen.

### <a name="step-5-create-the-availability-group-listener-on-the-failover-cluster"></a>Stap 5: De beschikbaarheidsgroeplistener op het failovercluster maken
In deze stap moet u handmatig de beschikbaarheidsgroeplistener in Failoverclusterbeheer en SQL Server Management Studio maken.

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

### <a name="verify-the-configuration-of-the-listener"></a>Controleer de configuratie van de listener

Als de cluster-resources en de afhankelijkheden zijn correct geconfigureerd, moet u mogelijk zijn de listener weergeven in SQL Server Management Studio. Om in te stellen de listener-poort, het volgende doen:

1. Start SQL Server Management Studio en maak verbinding met de primaire replica.

2. Ga naar **AlwaysOn hoge beschikbaarheid** > **beschikbaarheidsgroepen** > **beschikbaarheidsgroep-Listeners**.  
    U ziet nu de naam van de listener die u hebt gemaakt in Failoverclusterbeheer. 

3. Met de rechtermuisknop op de naam van de listener en klik vervolgens op **eigenschappen**.

4. In de **poort** vak het poortnummer opgeven voor de beschikbaarheidsgroep-listener met behulp van de $EndpointPort u eerder hebt gebruikt (1433 is de standaardinstelling), en klik vervolgens op **OK**.

U hebt nu een beschikbaarheidsgroep in Azure virtuele machines die worden uitgevoerd in de modus Resource Manager. 

## <a name="test-the-connection-to-the-listener"></a>Test de verbinding met de listener
De verbinding testen als volgt:

1. RDP naar een exemplaar van SQL Server die zich in hetzelfde virtuele netwerk, maar is geen eigenaar van de replica. Deze server worden andere SQL Server-exemplaar in het cluster.

2. Gebruik **sqlcmd** hulpprogramma om de verbinding te testen. Bijvoorbeeld, het volgende script stelt een **sqlcmd** verbinding met de primaire replica via de listener met de Windows-verificatie:
   
        sqlcmd -S <listenerName> -E

De SQLCMD-verbinding wordt automatisch verbinding met de SQL Server-exemplaar dat als host fungeert voor de primaire replica. 

## <a name="create-an-ip-address-for-an-additional-availability-group"></a>Een IP-adres voor een extra beschikbaarheidsgroep maken

Elke beschikbaarheidsgroep een listener afzonderlijke gebruikt. Elke listener beschikt over een eigen IP-adres. De dezelfde load balancer gebruiken om het IP-adres voor extra listeners. Nadat u een beschikbaarheidsgroep hebt gemaakt, het IP-adres toevoegen aan de load balancer en configureer vervolgens de listener.

Als een IP-adres toevoegen aan een load balancer is met de Azure-portal, het volgende doen:

1. Open de resourcegroep met de load balancer in de Azure portal en klik op de load balancer. 

2. Onder **instellingen**, klikt u op **Frontend-IP adresgroep**, en klik vervolgens op **toevoegen**. 

3. Onder **frontend-IP-adres toevoegen**, wijs een naam voor de front-endserver. 

4. Controleer de **virtueel netwerk** en de **Subnet** zijn hetzelfde als de SQL Server-exemplaren.

5. Stel het IP-adres voor de listener. 
   
   >[!TIP]
   >U kunt het IP-adres naar statisch en typ een adres dat wordt momenteel niet gebruikt in het subnet. U kunt ook het IP-adres ingesteld op dynamisch en opslaan van de nieuwe front-end-IP-adresgroep. Als u doet dit, de Azure portal een beschikbaar IP-adres automatisch toegewezen aan de groep. U kunt de front-end-IP-adresgroep Open en wijzig de toewijzing in statisch. 

6. Sla het IP-adres voor de listener. 

7. Een health test toevoegen met behulp van de volgende instellingen:

   |Instelling |Waarde
   |:-----|:----
   |**Naam** |Een unieke naam voor de test.
   |**Protocol** |TCP
   |**Poort** |Een niet-gebruikte TCP-poort, die beschikbaar zijn op alle virtuele machines. Het kan niet worden gebruikt voor andere doeleinden. Er zijn geen twee listeners kunnen dezelfde testpoort gebruiken. 
   |**Interval** |De tijdsduur tussen testpogingen. Gebruik de standaardwaarde (5).
   |**Drempelwaarde voor onjuiste status** |Het aantal opeenvolgende drempelwaarden die moet worden gemist voordat een virtuele machine is in orde beschouwd.

8. Klik op **OK** om op te slaan van de test. 

9. Maak een regel voor taakverdeling. Klik op **Taakverdelingsregels**, en klik vervolgens op **toevoegen**.

10. Een regel voor het nieuwe taakverdeling met behulp van de volgende instellingen configureren:

   |Instelling |Waarde
   |:-----|:----
   |**Naam** |Een unieke naam voor de regel voor taakverdeling. 
   |**Frontend IP-adres** |Selecteer het IP-adres dat u hebt gemaakt. 
   |**Protocol** |TCP
   |**Poort** |De poort die van de exemplaren van SQL Server gebruikmaken gebruiken. Een standaardexemplaar gebruikt poort 1433, tenzij u deze hebt gewijzigd. 
   |**Back-endpoort** |Gebruik dezelfde waarde als **poort**.
   |**Back-endpool** |De groep met de virtuele machines met de SQL Server-exemplaren. 
   |**De statuscontrole** |Kies de test die u hebt gemaakt.
   |**Sessiepersistentie** |Geen
   |**Time-out voor inactiviteit (minuten)** |Standaard (4)
   |**Zwevend IP (direct server return)** | Ingeschakeld

### <a name="configure-the-availability-group-to-use-the-new-ip-address"></a>De beschikbaarheidsgroep configureren voor het nieuwe IP-adres

Herhaal de stappen die u hebt gevolgd wanneer u de eerste beschikbaarheidsgroep gemaakt voor het voltooien van het cluster configureren. Dat wil zeggen, configureert de [cluster naar het nieuwe IP-adres](#configure-the-cluster-to-use-the-load-balancer-ip-address). 

Wanneer u een IP-adres hebt toegevoegd voor de listener, moet u de aanvullende beschikbaarheidsgroep configureren als volgt: 

1. Controleer of dat de testpoort voor het nieuwe IP-adres geopend op beide virtuele machines van SQL Server is. 

2. [In de Cluster-Manager toevoegen, de client access point](#addcap).

3. [Configureer de IP-resource voor de beschikbaarheidsgroep](#congroup).

   >[!IMPORTANT]
   >Wanneer u het IP-adres maakt, gebruikt u de IP-adres dat u hebt toegevoegd aan de load balancer.  

4. [Maak de SQL Server-beschikbaarheidsgroepresource afhankelijk van de client access point](#dependencyGroup).

5. [Controleer de afhankelijk van het IP-adres van client access point resource](#listname).
 
6. [Stel de clusterparameters in PowerShell](#setparam).

Nadat u de beschikbaarheidsgroep configureren voor het nieuwe IP-adres, configureert u de verbinding met de listener. 

## <a name="next-steps"></a>Volgende stappen

- [Een SQL Server Always On-beschikbaarheidsgroep configureren op virtuele Azure-machines in verschillende regio 's](virtual-machines-windows-portal-sql-availability-group-dr.md)
