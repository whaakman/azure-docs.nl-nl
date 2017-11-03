Het is belangrijk om te realiseren dat er twee manieren zijn voor het configureren van een beschikbaarheidsgroep-listener in Azure. De manieren verschillen in het type van Azure load balancer die u bij het maken van de listener. De volgende tabel worden de verschillen beschreven:

| Type load balancer | Implementatie | Gebruiken wanneer: |
| --- | --- | --- |
| **Externe** |Maakt gebruik van de *openbare virtuele IP-adres* van de cloudservice die als host fungeert voor de virtuele machines (VM's). |U moet toegang tot de listener van buiten het virtuele netwerk, met inbegrip van het Internet. |
| **Interne** |Maakt gebruik van een *interne load balancer* met een persoonlijk adres voor de listener. |U kunt toegang tot de listener alleen via binnen hetzelfde virtuele netwerk. Deze toegang bevat een site-naar-site VPN in hybride scenario's. |

> [!IMPORTANT]
> Voor een listener die gebruikmaakt van de cloudservice openbare VIP (externe load balancer), zo lang als de client,-listener en -databases zich in dezelfde Azure-regio, wordt geen kosten voor uitgaande kosten. Anders worden alle gegevens die zijn geretourneerd door de listener wordt beschouwd als uitgaande en wordt in rekening gebracht volgens de tarieven voor normale gegevensoverdracht. 
> 
> 

Een ILB kan alleen op virtuele netwerken met een regionaal bereik worden geconfigureerd. Bestaande virtuele netwerken die zijn geconfigureerd voor een affiniteitsgroep kunnen een ILB niet gebruiken. Zie voor meer informatie [interne load balancer overzicht](../articles/load-balancer/load-balancer-internal-overview.md).

