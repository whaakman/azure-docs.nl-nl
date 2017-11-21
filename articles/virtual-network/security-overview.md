---
title: Overzicht van Azure-netwerkbeveiliging| Microsoft Docs
description: Meer informatie over de beveiligingsopties voor het beheer van het netwerkverkeer tussen Azure-resources.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.openlocfilehash: d4a216b612274ff1de499bd4892ff7422c66b4d0
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="network-security"></a>Netwerkbeveiliging

Met behulp van een netwerkbeveiligingsgroep kunt u netwerkverkeer beperken tot resources in een virtueel netwerk. Een netwerkbeveiligingsgroep bevat een lijst met beveiligingsregels die binnenkomend of uitgaand netwerkverkeer toestaan of weigeren op basis van het bron- of doel-IP-adres, de poort en het protocol. 

## <a name="network-security-groups"></a>Netwerkbeveiligingsgroepen

Aan elke netwerkinterface is geen of één netwerkbeveiligingsgroep gekoppeld. Elke netwerkinterface bestaat in een subnet van een [virtueel netwerk](virtual-networks-overview.md). Aan een subnet kan ook geen of één netwerkbeveiligingsgroep zijn gekoppeld. 

Wanneer beveiligingsregels worden toegepast op een subnet, worden ze toegepast op alle resources in het subnet. Naast netwerkinterfaces hebt u mogelijk exemplaren van andere Azure-services zoals HDInsight, virtuele-machineschaalsets en toepassingsserviceomgevingen geïmplementeerd in het subnet.

Als aan een netwerkinterface en aan het subnet waarin de netwerkinterface zich bevindt een netwerkbeveiligingsgroep is gekoppeld, worden de netwerkbeveiligingsgroepen als volgt toegepast:

- **Binnenkomend verkeer**: de netwerkbeveiligingsgroep die is gekoppeld aan het subnet waarin de netwerkinterface zich bevindt, wordt eerst geëvalueerd. Verkeer dat is toegestaan door de netwerkbeveiligingsgroep die is gekoppeld aan het subnet, wordt vervolgens geëvalueerd door de netwerkbeveiligingsgroep die aan de netwerkinterface is gekoppeld. Stel, u hebt binnenkomende toegang nodig tot een virtuele machine via poort 80 vanaf internet. Als u een netwerkbeveiligingsgroep koppelt aan zowel de netwerkinterface als het subnet waarin de netwerkinterface zich bevindt, wordt de netwerkbeveiligingsgroep gekoppeld aan het subnet en moet de netwerkinterface poort 80 toestaan. Als u alleen poort 80 toestaat via de netwerkbeveiligingsgroep die is gekoppeld aan het subnet of aan de netwerkinterface waarin het subnet zich bevindt, mislukt de communicatie vanwege de standaardbeveiligingsregels. Zie [Standaardbeveiligingsregels](#default-security-rules) voor meer informatie. Als u alleen een netwerkbeveiligingsgroep toepast op het subnet of op de netwerkinterface, en de netwerkbeveiligingsgroep een regel bevat die binnenkomend netwerkverkeer via poort 80 toestaat, lukt de communicatie wel. 
- **Uitgaand verkeer**: de netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface, wordt eerst geëvalueerd. Verkeer dat is toegestaan door de netwerkbeveiligingsgroep die is gekoppeld aan de netwerkinterface, wordt vervolgens geëvalueerd door de netwerkbeveiligingsgroep die aan het subnet is gekoppeld.

U ben er mogelijk niet altijd van op de hoogte wanneer netwerkbeveiligingsgroepen worden toegepast op zowel een netwerkinterface als een subnet. U kunt eenvoudig de cumulatieve regels bekijken die op een netwerkinterface worden toegepast door de [effectieve beveiligingsregels](virtual-network-manage-nsg-arm-portal.md) voor een netwerkinterface te bekijken. U kunt ook de functie [IP-stroom controleren](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) in Azure Network Watcher gebruiken om te bepalen of de communicatie van of naar een netwerkinterface is toegestaan. Deze functie vertelt u of communicatie is toegestaan en welke netwerkbeveiligingsregel verkeer toestaat of weigert.
 
> [!NOTE]
> Netwerkbeveiligingsgroepen worden gekoppeld aan subnetten of aan virtuele machines en cloudservices die het klassieke implementatiemodel implementeren, in plaats van aan netwerkinterfaces in het Resource Manager-implementatiemodel. Zie [Over Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over Azure-implementatiemodellen.

Dezelfde netwerkbeveiligingsgroep kan worden toegepast op zo veel afzonderlijke netwerkinterfaces en subnetten als u wilt.

## <a name="security-rules"></a>Beveiligingsregels

Een netwerkbeveiligingsgroep bevat nul regels of zoveel regels als u wilt binnen de [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) van uw Azure-abonnement. Elke regel geeft de volgende eigenschappen aan:

|Eigenschap  |Uitleg  |
|---------|---------|
|Naam|Een unieke naam binnen de netwerkbeveiligingsgroep.|
|Prioriteit | Een getal tussen 100 en 4096. Regels worden verwerkt in volgorde van prioriteit, waarbij lagere getallen worden verwerkt vóór hogere getallen omdat lagere getallen een hogere prioriteit hebben. Zodra het verkeer overeenkomt met een regel, wordt de verwerking beëindigd. Daardoor worden regels met een lagere prioriteit (een hoger getal) die dezelfde kenmerken hebben als regels met een hogere prioriteit, niet verwerkt.|
|Bron of doel| Alle IP-adressen of een individueel IP-adres, CIDR-blok (zoals 10.0.0.0/24), servicetag of toepassingsbeveiligingsgroep. Meer informatie over [servicetags](#service-tags) en [toepassingbeveiligingsgroepen](#application-security-groups). Als u een bereik, servicetag of toepassingsbeveiligingsgroep opgeeft, hoeft u minder beveiligingsregels te maken. De mogelijkheid om meerdere afzonderlijke IP-adressen en -bereiken op te geven (u kunt niet meerdere servicetags of toepassingsgroepen opgeven) in een regel, is beschikbaar in de preview-versie en wordt aangeduid met de term 'uitgebreide beveiligingsregels'. Uitgebreide beveiligingsregels kunnen alleen worden gemaakt in netwerkbeveiligingsgroepen die zijn gemaakt via het Resource Manager-implementatiemodel. U kunt niet meerdere IP-adressen en IP-adresbereiken opgeven in netwerkbeveiligingsgroepen die zijn gemaakt via het klassieke implementatiemodel.|
|Protocol     | TCP, UDP of Alle, dat TCP, UDP en ICMP omvat. U kunt niet alleen ICMP opgeven, dus als u ICMP vereist, moet u Alle gebruiken. |
|Richting| Hiermee wordt aangegeven of de regel van toepassing is op binnenkomend of uitgaand verkeer.|
|Poortbereik     |U kunt één poort of een poortbereik opgeven. U kunt bijvoorbeeld 80 of 10000-10005 opgeven. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken. De mogelijkheid om meerdere afzonderlijke poorten en poortbereiken op te geven in een regel, is beschikbaar in de preview-versie en wordt aangeduid met de term 'uitgebreide beveiligingsregels'. Lees voordat u uitgebreide beveiligingsregels gebruikt [Preview-functies](#preview-features) voor belangrijke informatie. Uitgebreide beveiligingsregels kunnen alleen worden gemaakt in netwerkbeveiligingsgroepen die zijn gemaakt via het Resource Manager-implementatiemodel. U kunt niet meerdere poorten of poortbereiken opgeven in dezelfde beveiligingsregel in netwerkbeveiligingsgroepen die zijn gemaakt via het klassieke implementatiemodel.   |
|Actie     | Toestaan of weigeren        |

Beveiligingsregels zijn stateful. Als u bijvoorbeeld een beveiligingsregel voor uitgaand verkeer opgeeft voor elk adres via poort 80, hoeft u geen beveiligingsregel voor binnenkomend verkeer op te geven voor de reacties op het uitgaande verkeer. U hoeft alleen een beveiligingsregel voor binnenkomend verkeer op te geven als de communicatie extern is gestart. Het omgekeerde geldt ook. Als binnenkomend verkeer via een poort is toegestaan, is het niet nodig om een beveiligingsregel voor uitgaand verkeer op te geven om te reageren op verkeer via die poort. Zie [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie over limieten bij het maken van beveiligingsregels.

## <a name="augmented-security-rules"></a>Uitgebreide beveiligingsregels

Uitgebreide regels vereenvoudigen de beveiligingsdefinitie voor virtuele netwerken, zodat u een uitgebreider en complexer netwerkbeveiligingsbeleid kunt definiëren met minder regels. U kunt meerdere poorten, meerdere expliciete IP-adressen, servicetags en toepassingsbeveiligingsgroepen combineren in één gemakkelijk te begrijpen beveiligingsregel. U kunt uitgebreide regels gebruiken in de velden voor bron, doel en poort van een regel. Wanneer u een regel maakt, kunt u meerdere expliciete IP-adressen, CIDR-bereiken en poorten opgeven. Om het onderhoud van de definitie van uw beveiligingsregel te vereenvoudigen, kunt u uitgebreide beveiligingsregels combineren met servicetags of toepassingsbeveiligingsgroepen. 

Uitgebreide beveiligingsregels zijn beschikbaar in de preview-versie. Zie [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) voor meer informatie over limieten bij het maken van uitgebreide beveiligingsregels. Functies in de preview-versie hoeven niet dezelfde mate van beschikbaarheid en betrouwbaarheid te hebben als functies in de algemene versie. De functies zijn alleen beschikbaar in de volgende regio's: VS - oost, VS - west, VS - west 2, VS - west-centraal, Australië - oost, Australië - zuidoost en UK - zuid.

## <a name="default-security-rules"></a>Standaardbeveiligingsregels

Als een netwerkbeveiligingsgroep niet is gekoppeld aan een subnet of netwerkinterface, is al het binnenkomende verkeer naar of al het uitgaande verkeer van het subnet of de netwerkinterface toegestaan. Zodra een netwerkbeveiligingsgroep is gemaakt, worden in Azure de volgende standaardregels binnen de netwerkbeveiligingsgroep gemaakt:

### <a name="inbound"></a>Inkomend

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Prioriteit|Bron|Bronpoorten|Doel|Doelpoorten|Protocol|Toegang|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Alle|Toestaan|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Prioriteit|Bron|Bronpoorten|Doel|Doelpoorten|Protocol|Toegang|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Alle|Toestaan|

#### <a name="denyallinbound"></a>DenyAllInbound

|Prioriteit|Bron|Bronpoorten|Doel|Doelpoorten|Protocol|Toegang|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Alle|Weigeren|

### <a name="outbound"></a>Uitgaand

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Prioriteit|Bron|Bronpoorten| Doel | Doelpoorten | Protocol | Toegang |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Alle | Toestaan |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Prioriteit|Bron|Bronpoorten| Doel | Doelpoorten | Protocol | Toegang |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Alle | Toestaan |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Prioriteit|Bron|Bronpoorten| Doel | Doelpoorten | Protocol | Toegang |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Alle | Weigeren |

In de kolommen **Bron** en **Doel** zijn *VirtualNetwork*, *AzureLoadBalancer* en *Internet* [servicetags](#tags) in plaats van IP-adressen. In de kolom protocol omvat **Alle** TCP, UDP en ICMP. Wanneer u een regel maakt, kunt u TCP, UDP of Alle opgeven, maar u kunt niet alleen ICMP opgeven. Als uw regel ICMP vereist, moet u daarom *Alle* voor protocol selecteren. *0.0.0.0/0* in de kolommen **Bron** en **Doel** vertegenwoordigt alle adressen.
 
U kunt de standaardregels niet verwijderen, maar u kunt ze negeren door regels te maken met een hogere prioriteit.

## <a name="service-tags"></a>Servicetags

 Een servicetag vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kunt niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen zijn opgenomen in een tag. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen. U kunt servicetags gebruiken in plaats van specifieke IP-adressen wanneer u beveiligingsregels maakt. De volgende servicetags zijn beschikbaar voor gebruik in de definitie van beveiligingsregels. De namen verschillen enigszins tussen [Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (*Resource Manager) (**VIRTUAL_NETWORK** voor klassiek): deze tag omvat de adresruimte van het virtuele netwerk (alle CIDR-bereiken die voor het virtuele netwerk zijn gedefinieerd), alle verbonden on-premises adresruimten en [via peering gekoppelde](virtual-network-peering-overview.md) virtuele netwerken of virtuele netwerken die zijn verbonden met een [virtuele netwerkgateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** voor klassiek): met deze tag wordt de load balancer voor de infrastructuur van Azure aangeduid. De tag wordt omgezet in het [IP-adres van een Azure-datacenter](https://www.microsoft.com/download/details.aspx?id=41653) van waaruit statuscontroles van Azure worden uitgevoerd. Als u de load balancer van Azure niet gebruikt, kunt u deze regel onderdrukken.
* **Internet** (Resource Manager) (**INTERNET** voor klassiek): met deze tag wordt de IP-adresruimte aangeduid die zich buiten het virtuele netwerk bevindt en bereikbaar is via internet. Dit adresbereik omvat ook de [openbare IP-adresruimte van Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureTrafficManager** (alleen Resource Manager): deze tag geeft de IP-adresruimte voor de service Azure Traffic Manager aan.
* **Storage** (alleen Resource Manager): deze tag geeft de IP-adresruimte voor de service Azure Storage aan. Als u *Storage* opgeeft als waarde, wordt verkeer naar de opslag toegestaan of geweigerd. Als u toegang tot opslag alleen wilt toestaan in een specifieke [regio](https://azure.microsoft.com/regions), kunt u de regio opgeven. Als u toegang tot Azure Storage bijvoorbeeld alleen wilt toestaan in de regio VS - oost, kunt u *Storage.EastUS* opgeven als servicetag. Aanvullende regionale servicetags zijn: Storage.AustraliaEast, Storage.AustraliaSoutheast, Storage.EastUS, Storage.UKSouth, Storage.WestCentralUS, Storage.WestUS en Storage.WestUS2. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure Storage, maar geen specifiek Azure Storage-account.
* **Sql** (alleen voor Resource Manager): deze tag geeft de adresvoorvoegsels aan van de services Azure SQL Database en Azure SQL Data Warehouse. U kunt alleen specifieke regio's opgeven voor deze servicetag. Als u toegang tot Azure SQL Database bijvoorbeeld alleen wilt toestaan in de regio VS - oost, kunt u *Sql.EastUS* opgeven als servicetag. U kunt Sql niet opgeven voor alle Azure-regio's. U moet regio's afzonderlijk opgeven. Overige regionale servicetags die beschikbaar zijn: Sql.AustraliaEast, Sql.AustraliaSoutheast, Sql.EastUS, Sql.UKSouth, Sql.WestCentralUS, Sql.WestUS en Sql.WestUS2. De tag vertegenwoordigt de service, maar geen specifieke exemplaren van de service. De tag vertegenwoordigt bijvoorbeeld de service Azure SQL Database, maar geen specifieke Azure SQL-database.

> [!WARNING]
> De servicetags AzureTrafficManager, Storage en Sql zijn beschikbaar in de preview-versie. Functies in de preview-versie hoeven niet dezelfde mate van beschikbaarheid en betrouwbaarheid te hebben als functies in de algemene versie. De servicetags zijn alleen beschikbaar in de volgende regio's: VS - oost, VS - west, VS - west 2, VS - west-centraal, Australië - oost, Australië - zuidoost en UK - zuid.

> [!NOTE]
> Als u een servicepunt voor een virtueel netwerk implementeert voor een service zoals Azure Storage of Azure SQL Database, voegt Azure een route naar een virtueel netwerksubnet toe voor de service. De adresvoorvoegsels voor de route zijn dezelfde adresvoorvoegsels, of CIDR-bereiken, als de bijbehorende servicetag.

## <a name="application-security-groups"></a>Toepassingsbeveiligingsgroepen

Met behulp van toepassingsbeveiligingsgroepen kunt u netwerkbeveiliging configureren als een natuurlijk verlengstuk van de structuur van een toepassing, waarbij u virtuele machines kunt groeperen en netwerkbeveiligingsbeleid kunt definiëren op basis van die groepen. Deze functie maakt het mogelijk het beveiligingsbeleid op grote schaal opnieuw te gebruiken zonder handmatig onderhoud van expliciete IP-adressen. Het platform verwerkt de complexiteit van expliciete IP-adressen en meerdere regelsets, zodat u zich kunt richten op uw bedrijfslogica.

U kunt een toepassingsbeveiligingsregel opgeven als bron en doel in een beveiligingsregel. Nadat uw beveiligingsbeleid is gedefinieerd, kunt u virtuele machines maken en de netwerkinterfaces in de virtuele machine toewijzen aan een toepassingsbeveiligingsgroep. Het beleid wordt toegepast op basis van het lidmaatschap van de toepassingsbeveiligingsgroep van elke netwerkinterface binnen een virtuele machine. Het volgende voorbeeld laat zien hoe u een toepassingsbeveiligingsgroep kunt gebruiken voor alle webservers in uw abonnement:

1. Maak een toepassingsbeveiligingsgroep met de naam *Webservers*.
2. Maak een netwerkbeveiligingsgroep met de naam *MijnNBG*.
3. Maak een beveiligingsregel voor binnenkomend verkeer in de netwerkbeveiligingsgroep, waarbij u de servicetag *Internet* opgeeft als bronadres en de toepassingsbeveiligingsgroep *Webservers* als doeladres en poorten 80 en 443 toestaat.
4. Implementeer een virtuele machine met een webservertoepassing. Geef op dat de netwerkinterface in de virtuele machine lid is van de toepassingsbeveiligingsgroep *Webservers*. Poorten 80 en 443 zijn vervolgens toegestaan voor de virtuele machine. De poorten zijn ook toegestaan voor eventuele volgende webservers die u lid maakt van de toepassingsbeveiligingsgroep *Webservers*. 

Als u andere regels maakt en daarbij andere toepassingsbeveiligingsgroepen opgeeft als doel, worden deze regels niet toegepast op de webservers in het vorige voorbeeld. Regels die een toepassingsbeveiligingsgroep opgeven, worden alleen toegepast op netwerkinterfaces die lid van de toepassingsbeveiligingsgroep. Toepassingsbeveiligingsgroepen, gecombineerd met uitgebreide beveiligingsregels en servicetags, maken het mogelijk een minimum aantal netwerkbeveiligingsgroepen te maken om de netwerkbeveiliging in uw abonnement te beheren.
 
Voor meer informatie over limieten bij het maken van toepassingsbeveiligingsgroepen en het opgeven ervan in regels raadpleegt u [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Toepassingsbeveiligingsgroepen zijn beschikbaar in de preview-versie. Voordat u toepassingsbeveiligingsgroepen gebruikt, moet u zich registreren voor het gebruik ervan door de stappen 1-5 in [Een netwerkbeveiligingsgroep met toepassingsbeveiligingsgroepen maken](create-network-security-group-preview.md#powershell) uit te voeren en [Preview-functies](#preview-features) te lezen voor belangrijke informatie. Toepassingsbeveiligingsgroepen hebben de volgende beperkingen:

-   Alle netwerkinterfaces binnen een toepassingsbeveiligingsgroep moeten zich in hetzelfde virtuele netwerk bevinden. U kunt geen netwerkinterfaces van verschillende virtuele netwerken toevoegen aan dezelfde toepassingsbeveiligingsgroep. Het virtuele netwerk waarin de eerste netwerkinterface die is toegewezen aan de toepassingsbeveiligingsgroep zich bevindt, definieert het virtuele netwerk waarin alle volgende toegewezen netwerkinterfaces aanwezig moeten zijn.
- Als u toepassingsbeveiligingsgroepen als bron en doel in een beveiligingsregel opgeeft, moeten de netwerkinterfaces in beide beveiligingsgroepen toepassing zich in hetzelfde virtuele netwerk bevinden. Als ASG1 bijvoorbeeld netwerkinterfaces van VNet1 bevat en ASG2 netwerkinterfaces van VNet2, kunt u ASG1 niet toewijzen als de bron en ASG2 als het doel in een regel. Alle netwerkinterfaces moeten aanwezig zijn in VNet1. 

Functies in de preview-versie hoeven niet dezelfde mate van beschikbaarheid en betrouwbaarheid te hebben als functies in de algemene versie. Voordat u toepassingsbeveiligingsgroepen gebruikt, moet u zich registreren voor het gebruik ervan. De functies zijn alleen beschikbaar in de volgende regio's: VS - west-centraal.


## <a name="azure-platform-considerations"></a>Overwegingen bij het Azure-platform

- **Virtueel IP-adres van het hostknooppunt:** basisinfrastructuurservices zoals DHCP, DNS en statuscontrole worden geleverd via de gevirtualiseerde host-IP-adressen 168.63.129.16 en 169.254.169.254. Deze openbare IP-adressen zijn van Microsoft en zijn de enige gevirtualiseerde IP-adressen die in alle regio's voor dit doel worden gebruikt. De adressen worden toegewezen aan het fysieke IP-adres van de servermachine (hostknooppunt) die fungeert als host voor de virtuele machine. Het hostknooppunt fungeert als de DHCP-relay, de recursieve DNS-omzetter en de bron voor de statuscontrole van de load balancer en de machine. Communicatie met deze IP-adressen is geen aanval. Als u verkeer naar of van deze IP-adressen blokkeert, werkt een virtuele machine mogelijk niet goed.
- **Licentieverlening (Key Management Service):** voor alle Windows installatiekopieën die op virtuele machines worden uitgevoerd, is een licentie vereist. Hiervoor wordt een licentieaanvraag verstuurd naar de Key Management Service-hostservers waarop dergelijke query's worden afgehandeld. De uitgaande aanvraag wordt gedaan via poort. 1688.
- **Virtuele machines in groepen met gelijke taakverdeling**: de bronpoort en het bronadresbereik die worden toegepast, zijn die van de oorspronkelijke computer, niet van de load balancer. De doelpoort en het doeladresbereik zijn die van de doelcomputer, niet van de load balancer.
- **Azure-service-exemplaren**: exemplaren van verschillende Azure-services, zoals HDInsight, toepassingsserviceomgevingen en virtuele-machineschaalsets, worden geïmplementeerd in virtuele netwerksubnetten. Zie [Virtueel netwerk voor Azure-services](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network) voor een volledige lijst met services die u in virtuele netwerken kunt implementeren. Zorg ervoor dat u vertrouwd raakt met de poortvereisten voor elke service voordat u een netwerkbeveiligingsgroep toepast op het subnet waarin de resource is geïmplementeerd. Als u poorten weigert die vereist zijn voor de service, werkt de service niet naar behoren.
- **Verzenden van uitgaande e-mail**: Microsoft raadt u aan om geverifieerde SMTP-relayservices te gebruiken (doorgaans verbonden via TCP-poort 587, maar ook vaak andere) om e-mail vanaf Azure Virtual Machines te verzenden. SMTP-relayservices leggen zich toe op de reputatie van de afzender, om zo de kans dat e-mailproviders van derden berichten weigeren, tot het uiterste terug te dringen. Dergelijke SMTP-relayservices omvatten, maar beperken zich niet tot Exchange Online Protection en SendGrid. Het gebruik van de SMTP-relayservices wordt in Azure op geen enkele wijze beperkt, ongeacht welk type abonnement u hebt. 

  Als u uw Azure-abonnement vóór 15 november 2017 hebt gemaakt, kunt u naast de mogelijkheid om gebruik te maken van SMTP-relayservices ook rechtstreeks e-mail verzenden via TCP-poort 25. Als u uw abonnement na 15 november 2017 hebt gemaakt kunt u mogelijk geen e-mail rechtstreeks via poort 25 verzenden. Hoe uitgaande communicatie via poort 25 verloopt, hangt als volgt samen met het type abonnement dat u hebt:

     - **Enterprise Overeenkomst**: communicatie via poort 25 is toegestaan. U kunt uitgaande e-mail rechtstreeks vanaf virtuele machines naar externe e-mailproviders verzenden, zonder dat daar beperkingen voor gelden op grond van het Azure-platform. 
     - **Betalen naar gebruik:** communicatie via uitgaande poort 25 is voor alle resources geblokkeerd. Als u e-mail vanuit een virtuele machine rechtstreeks naar de externe e-providers wilt verzenden (niet via een geverifieerde SMTP-relay), kunt u een aanvraag indienen om de beperking op te heffen. Het is aan Microsoft om te bepalen of aanvragen worden gecontroleerd en goedgekeurd, en worden alleen toegekend nadat er controles ter voorkoming van fraude zijn uitgevoerd. Als u een aanvraag wilt indienen, opent u een ondersteuningsaanvraag met het probleemtype *Technisch*, *Virtuele netwerkverbinding*, *Kan geen e-mail verzenden (SMTP/poort 25)*. Vermeld in uw ondersteuningsaanvraag informatie zoals waarom uw abonnement e-mail rechtstreeks naar e-mailproviders moet kunnen verzenden in plaats van via een geverifieerde SMTP-relay. Als de aanvraag voor uw abonnement wordt toegekend, kunnen alleen de virtuele machines die na de toekenningsdatum zijn gemaakt poort 25 voor uitgaande communicatie gebruiken.
     - **MSDN, Azure Pass, Azure in Open, Education, BizSpark en gratis proefabonnementen**: uitgaande communicatie via poort 25 voor alle resources geblokkeerd. Er kunnen geen aanvragen worden ingediend voor het opheffen van de beperking, omdat zulke aanvragen niet worden toegekend. Als u e-mail vanaf uw virtuele machine wilt verzenden, moet u daarvoor een SMTP-relayservice gebruiken.

  Als Azure u toestaat e-mail te verzenden via poort 25, biedt Microsoft geen garantie dat e-mailproviders inkomende e-mail van uw virtuele machine accepteren. Als een specifieke provider e-mail van uw virtuele machine weigert, moet u samen met de provider alle eventuele problemen met de aflevering van berichten of met de spamfilter oplossen, of u moet een geverifieerde SMTP-relayservice gebruiken. 


## <a name="next-steps"></a>Volgende stappen

* Voltooi de zelfstudie [Een netwerkbeveiligingsgroep maken](virtual-networks-create-nsg-arm-pportal.md)
* Voltooi de zelfstudie [Een netwerkbeveiligingsgroep met toepassingsbeveiligingsgroepen maken](create-network-security-group-preview.md)
