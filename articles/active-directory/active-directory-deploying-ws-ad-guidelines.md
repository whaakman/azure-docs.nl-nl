---
title: Richtlijnen voor het implementeren van Windows Server Active Directory op virtuele Machines in Azure | Microsoft Docs
description: Als u het implementeren van AD Domain Services en AD Federation Services on-premises, informatie over hoe deze werken op virtuele machines in Azure.
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: 04df4c46-e6b6-4754-960a-57b823d617fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: femila
ms.openlocfilehash: 342d9e2787add3d04f1b744152e135db98848179
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Richtlijnen voor het implementeren van Windows Server Active Directory op virtuele machines in Azure
Dit artikel wordt uitgelegd dat de belangrijke verschillen tussen implementatie Windows Server Active Directory Domain Services (AD DS) en Active Directory Federation Services (AD FS) lokale versus implementeren op Microsoft Azure virtuele machines.

## <a name="scope-and-audience"></a>Bereik en doelgroep
Het artikel is bedoeld voor die al bij de implementatie van Active Directory lokaal heeft ondergaan. Het bevat informatie over de verschillen tussen Active Directory implementeren op Microsoft Azure virtuele machines/Azure virtual networks en traditionele on-premises Active Directory-implementaties. Azure virtuele machines en virtuele netwerken van Azure uitmaken deel van een infrastructuur-as-a-Service (IaaS) bieden voor organisaties gebruikmaken van computerbronnen in de cloud.

Voor toepassingen die niet bekend met de implementatie van AD bent, Zie de [Implementatiehandleiding voor AD DS](https://technet.microsoft.com/library/cc753963) of [uw AD FS-implementatie plannen](https://technet.microsoft.com/library/dn151324.aspx) zo nodig.

In dit artikel wordt ervan uitgegaan dat de lezer bekend bent met de volgende concepten:

* Windows Server AD DS-implementatie en beheer
* Implementatie en configuratie van DNS ter ondersteuning van een Windows Server AD DS-infrastructuur
* Windows Server AD FS-implementatie en beheer
* Implementeren, configureren en beheren van relying party-toepassingen (websites en web services) die Windows Server AD FS-tokens kunnen worden gebruikt.
* Concepten van algemene virtuele machine, zoals het configureren van een virtuele machine virtuele schijven en virtuele netwerken

In dit artikel worden de vereisten beschreven voor een hybride implementatiescenario waarin Windows Server AD DS of AD FS gedeeltelijk geïmplementeerde on-premises en gedeeltelijk geïmplementeerd op virtuele machines in Azure. Het document bevat eerst de kritieke verschillen tussen Windows Server AD DS en AD FS uitgevoerd op virtuele machines versus on-premises en belangrijke beslispunten die invloed hebben op het ontwerp en implementatie van Azure. De rest van het artikel wordt uitgelegd richtlijnen voor elk van de beslissingspunten in meer detail en het toepassen van de richtlijnen voor verschillende scenario's.

In dit artikel wordt niet beschreven voor de configuratie van [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), dit is een op REST gebaseerde service die mogelijkheden voor beheer en toegang beheren voor cloud-toepassingen biedt. Azure Active Directory (Azure AD) en Windows Server AD DS zijn, echter ontworpen om samen te werken om op te geven van een beheersysteem voor identiteits- en toegangsbeheer voor vandaag hybride IT-omgevingen en moderne toepassingen. Voor meer informatie over de verschillen en relaties tussen Windows Server AD DS en Azure AD, het volgende overwegen:

1. U kunt Windows Server AD DS in de cloud op Azure virtuele machines uitvoeren als u uw on-premises datacentrum in de cloud uitbreiden via Azure.
2. U kunt Azure AD gebruiken zodat uw gebruikers eenmalige aanmelding tot de Software-as-a-Service (SaaS)-toepassingen. Microsoft Office 365 maakt gebruik van deze technologie bijvoorbeeld en toepassingen die worden uitgevoerd op Azure of andere cloudplatforms kunnen ook worden gebruikt.
3. U kunt Azure AD (de Access Control Service) gebruiken zodat gebruikers zich aanmelden bij het gebruik van identiteiten van Facebook, Google, Microsoft en andere id-providers voor toepassingen die worden gehost in de cloud of on-premises.

Zie voor meer informatie over deze verschillen, [Azure Identity](fundamentals-identity.md).

## <a name="related-resources"></a>Gerelateerde resources
U kunt downloaden en voer de [Azure virtuele Machine Readiness Assessment](https://www.microsoft.com/download/details.aspx?id=40898). De beoordeling wordt automatisch inspecteren van uw on-premises omgeving en genereren van een aangepast rapport op basis van de richtlijnen gevonden in dit onderwerp vindt u de omgeving migreren naar Azure.

We raden u ook eerst de zelfstudies, handleidingen en video's die betrekking hebben op de volgende onderwerpen:

* [Een virtueel netwerk alleen in de Cloud in de Azure-Portal configureren](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
* [Een Site-naar-Site-VPN configureren in de Azure Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](active-directory-new-forest-virtual-machine.md)
* [Een replica Active Directory-domeincontroller installeren in Azure](active-directory-install-replica-active-directory-domain-controller.md)
* [Microsoft Azure IT Pro IaaS: grondbeginselen (01) virtuele Machine](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises connectiviteit](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Inleiding
De fundamentele vereisten voor het implementeren van Windows Server Active Directory op virtuele machines in Azure weinig afwijken van deze is geïmplementeerd in on-premises virtuele machines (en tot op zekere hoogte fysieke machines). Bijvoorbeeld, in het geval van Windows Server AD DS, zo de domeincontrollers (DC's) die u op Azure virtuele machines implementeert replica's in een bestaand zijn on-premises zakelijke domein/forest, en vervolgens de Azure-implementatie kan grotendeels worden behandeld op dezelfde manier zoals u wellicht een andere aanvullende Windows Server Active Directory-site worden beschouwd. Dat wil zeggen, moeten subnetten worden gedefinieerd in Windows Server AD DS, een site die is gemaakt, de subnetten gekoppeld aan die site en verbonden met andere sites met de juiste site-koppelingen. Er zijn echter enkele verschillen die gemeenschappelijk zijn voor alle Azure-implementaties en sommige die variëren afhankelijk van het specifieke implementatiescenario. Twee fundamentele verschillen worden hieronder beschreven:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Virtuele machines in Azure moet verbinding met de on-premises zakelijke netwerk mogelijk.
Verbinding maken met virtuele Azure-machines naar een on-premises zakelijke netwerk vereist virtuele Azure-netwerk, waaronder een site-naar-site of site-naar-punt virtuele particuliere netwerk (VPN)-onderdeel naadloos virtuele Azure-machines verbinding kunnen maken en een on-premises machines. Dit VPN-onderdeel kan ook inschakelen voor lokale computers van domeinleden voor toegang tot een Windows Server Active Directory-domein waarvan domeincontrollers uitsluitend op virtuele machines in Azure worden gehost. Het is belangrijk te weten echter dat als de VPN-verbinding is mislukt, verificatie en andere bewerkingen die afhankelijk van Windows Server Active Directory zijn wordt ook mislukken. Bij gebruikers mogelijk niet kunnen aanmelden met behulp van bestaande referenties, alle peer-to-peer in cache verificatiepogingen client-naar-server waarvoor tickets nog moeten worden uitgegeven of verouderd zijn, anders mislukt.

Zie [virtueel netwerk](http://azure.microsoft.com/documentation/services/virtual-network/) voor een demonstratie van video- en een lijst met zelfstudies met stapsgewijze instructies, inclusief [een Site-naar-Site-VPN configureren in de Azure portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> U kunt ook Windows Server Active Directory implementeren op een Azure-netwerk dat geen verbinding maken met een on-premises netwerk. De richtlijnen in dit onderwerp wordt echter ervan uitgegaan dat een virtuele Azure-netwerk wordt gebruikt, omdat dit IP-adressen van de mogelijkheden die essentieel voor Windows Server zijn biedt.
> 
> 

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statische IP-adressen moeten worden geconfigureerd met Azure PowerShell.
Dynamische adressen worden standaard toegewezen, maar gebruikt u de cmdlet Set-AzureStaticVNetIP in plaats daarvan een statisch IP-adres toewijzen. Die Hiermee stelt u een statisch IP-adres dat via de service herstel- en uitschakelen/opnieuw opstarten van de virtuele machine actief blijft. Zie voor meer informatie [statische interne IP-adres voor virtuele machines](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Termen en definities
Hier volgt een niet-uitputtende lijst met voorwaarden voor verschillende Azure-technologieën die in dit artikel wordt verwezen.

* **Virtuele machines in Azure**: het IaaS-aanbod in Azure waarmee klanten voor het implementeren van virtuele machines met vrijwel elke oudsher lokale serverwerkbelasting.
* **Virtuele Azure-netwerk**: de VPN-service in Azure waarmee klanten maken en beheren van virtuele netwerken in Azure en ze veilig te koppelen aan hun eigen on-premises netwerken infrastructuur met behulp van een virtueel particulier netwerk (VPN).
* **Virtueel IP-adres**: een internetgerichte IP-adres dat niet is gebonden aan een specifieke computer of network interface-kaart. Cloud-services zijn een virtueel IP-adres voor het ontvangen netwerkverkeer dat wordt omgeleid naar een Azure-virtuele machine toegewezen. Een virtueel IP-adres is een eigenschap van een cloudservice die een of meer virtuele machines van Azure kan bevatten. Let ook op dat een virtuele Azure-netwerk een of meer cloud-services kan bevatten. Virtuele IP-adressen bieden mogelijkheden voor native-taakverdeling.
* **Dynamische IP-adres**: dit is het IP-adres is alleen voor intern gebruik. Deze moet worden geconfigureerd als een statisch IP-adres (via de cmdlet Set-AzureStaticVNetIP) voor virtuele machines die de domeincontroller en DNS-serverfuncties hosten.
* **Service herstel**: het proces waarin Azure automatisch een service naar een actieve status opnieuw retourneert nadat wordt gedetecteerd dat de service is mislukt. Herstel-service is een van de aspecten van Azure die ondersteuning biedt voor beschikbaarheid en robuustheid van. Tijdens het onwaarschijnlijk, wordt het resultaat na een service incident herstel voor een domeincontroller die wordt uitgevoerd op een virtuele machine is vergelijkbaar met een niet-geplande opnieuw opstarten, maar heeft enkele neveneffecten:
  
  * De virtuele netwerkadapter in de virtuele machine wordt gewijzigd.
  * De MAC-adres van de virtuele netwerkadapter wordt gewijzigd.
  * De Processor/CPU-ID van de virtuele machine wordt gewijzigd.
  * De IP-adresconfiguratie van de virtuele netwerkadapter wordt niet wijzigen, omdat de virtuele machine is gekoppeld aan een virtueel netwerk en IP-adres van de VM statisch is.
  
  Geen van deze problemen beïnvloeden Windows Server Active Directory omdat er geen afhankelijkheid van de MAC-adres of de Processor/CPU-ID en alle Windows Server Active Directory-implementaties op Azure worden aanbevolen om te worden uitgevoerd op een virtuele Azure-netwerk als hierboven beschreven.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Is het veilig virtualiseren, Windows Server Active Directory-domeincontrollers?
Windows Server Active Directory-DC's op virtuele machines in Azure is geïmplementeerd, is onderworpen aan dezelfde richtlijnen DC's lokaal uitgevoerd op een virtuele machine. Gevirtualiseerde DC's wordt uitgevoerd, is een veilige procedure als richtlijnen gegeven voor een back-up en herstellen van DC's worden gevolgd. Zie voor meer informatie over de beperkingen en richtlijnen voor het uitvoeren van gevirtualiseerde DC's, [Running Domain Controllers in Hyper-V](https://technet.microsoft.com/library/dd363553).

Geef op hypervisors of trivialize technologieën die kunnen problemen veroorzaken voor veel gedistribueerde systemen, met inbegrip van Windows Server Active Directory. Bijvoorbeeld, op een fysieke server, u kunt een schijf klonen of niet-ondersteunde methoden gebruiken voor het terugdraaien van de status van een server, inclusief het gebruik van SAN's, enzovoort, maar dat op een fysieke server doet is veel moeilijker dan het terugzetten van een momentopname van de virtuele machine in een hypervisor. Azure biedt functionaliteit die tot dezelfde ongewenste voorwaarde leiden kan. Bijvoorbeeld, moet u VHD-bestanden van DC's niet kopiëren in plaats van het uitvoeren van regelmatige back-ups, omdat deze terug te zetten in een dergelijke situatie voor het gebruik van de momentopname terugzetten functies kan opleveren.

Dergelijke Rollback introduceren USN-bellen tot permanent uiteenlopende statussen tussen domeincontrollers leiden kunnen. Die de oorzaak van problemen, zoals:

* Achtergebleven objecten
* Inconsistente wachtwoorden
* Inconsistente kenmerkwaarden
* Schema komt niet overeen met als de schema-master wordt teruggedraaid

Zie voor meer informatie over hoe de DC's worden beïnvloed, [USN and USN Rollback](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Vanaf Windows Server 2012 [extra beveiliging zijn ingebouwd in AD DS](https://technet.microsoft.com/library/hh831734.aspx). Met deze beveiligingen te helpen beveiligen van gevirtualiseerde domeincontrollers op basis van de hiervoor genoemde problemen, zolang het onderliggende hypervisor-platform VM-GenerationID ondersteunt. Azure ondersteunt VM-GenerationID, wat betekent dat domeincontrollers met Windows Server 2012 of later op Azure virtuele machines de extra beveiliging.

> [!NOTE]
> U moet afsluiten en opnieuw opstarten van een virtuele machine die de rol domeincontroller in Azure wordt uitgevoerd binnen het gastbesturingssysteem in plaats van de **afsluiten** optie in de Azure Portal. Vandaag de dag afsluiten van een virtuele machine met behulp van de portal zorgt ervoor dat de virtuele machine ongedaan. Een toewijzing ongedaan is gemaakt VM heeft het voordeel van het niet steeds kosten, maar deze ook stelt de generatie-id, die ongewenste voor een domeincontroller is. Wanneer de generatie-id opnieuw wordt ingesteld, de invocationID van de AD DS-database is ook opnieuw instellen, de RID-groep is verwijderd en SYSVOL is gemarkeerd als niet-bindende. Zie voor meer informatie [Inleiding tot virtualisatie van Active Directory Domain Services (AD DS)](https://technet.microsoft.com/library/hh831734.aspx) en [Safely Virtualizing DFSR](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).
> 
> 

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Waarom Windows Server AD DS op Azure Virtual Machines implementeren?
Er zijn veel scenario's voor Windows Server AD DS-implementatie geschikt zijn voor de implementatie als virtuele machines in Azure. Stel dat u hebt een bedrijf in Europa dat moet worden geverifieerd gebruikers in een externe locatie in Azië. Het bedrijf is Windows Server Active Directory-DC's in Azië niet eerder hebt geïmplementeerd vanwege de kosten voor het implementeren van beperkte en ze expertise voor het beheren van de servers na de implementatie. Hierdoor kunnen worden verificatieaanvragen van Azië afgehandeld door de DC's in Europa met suboptimale resultaten. In dit geval kunt u een domeincontroller op een virtuele machine die u hebt opgegeven moet worden uitgevoerd binnen de Azure-datacenter in Azië implementeren. Van die DC koppelen aan een Azure-netwerk dat rechtstreeks is verbonden met de externe locatie, zal de verificatieprestaties verbeteren.

Azure is ook geschikt als vervanging voor anders kostbare disaster recovery (DR) sites. De relatief lage kosten voor het hosten van een klein aantal domeincontrollers en één virtueel netwerk in Azure vertegenwoordigt een aantrekkelijk alternatief.

Ten slotte, wilt u een netwerktoepassing in Azure, zoals SharePoint, die Windows Server Active Directory is vereist, maar heeft geen afhankelijkheid van de on-premises netwerk of de zakelijke Windows Server Active Directory implementeren. In dit geval implementatie van een geïsoleerd forest in Azure om te voldoen aan de SharePoint-server-vereisten is optimaal. Opnieuw wordt implementeren van netwerktoepassingen waarvoor verbinding met de on-premises netwerk en de zakelijke Active Directory ook ondersteund.

> [!NOTE]
> Aangezien een layer 3-verbinding biedt, kunt de VPN-component die connectiviteit tussen een Azure-netwerk en een on-premises netwerk biedt lidservers die on-premises wilt gebruiken voor de DC's die worden uitgevoerd als Azure virtuele machines in Azure virtuele uitgevoerd ook inschakelen netwerk. Maar als de VPN-verbinding niet beschikbaar is, communicatie tussen on-premises computers en domeincontrollers op basis van Azure werkt niet, wat resulteert in verificatie en verschillende andere fouten.  
> 
> 

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Anders dan tussen het implementeren van Windows Server Active Directory-domeincontrollers op Azure Virtual Machines tegenover on-premises
* Voor elke Windows Server Active Directory-implementatiescenario die meer dan één VM bevat, is het nodig zijn voor het gebruik van een virtuele Azure-netwerk consistentie van IP-adres. Houd er rekening mee dat deze handleiding wordt aangenomen dat de DC's worden uitgevoerd op een virtuele Azure-netwerk.
* Net als bij lokale DC's, worden statische IP-adressen aanbevolen. Een statisch IP-adres kan alleen worden geconfigureerd met behulp van Azure PowerShell. Zie [statische interne IP-adres voor virtuele machines](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) voor meer informatie. Als u bewaking van systemen of andere oplossingen die op configuratie van vaste IP-adressen binnen het gastbesturingssysteem controleren hebt, kunt u hetzelfde statische IP-adres toewijzen aan eigenschappen van de netwerkadapter van de virtuele machine. Maar houd er rekening mee dat de netwerkadapter wordt genegeerd als de virtuele machine ondergaat service herstel of in de portal wordt afgesloten en is het adres de toewijzing ongedaan gemaakt. In dat geval wordt het statische IP-adres in de gast moet opnieuw worden ingesteld.
* Implementeren van virtuele machines op een virtueel netwerk niet impliceren (of vereisen) connectiviteit met een on-premises netwerk; het virtuele netwerk kunt alleen deze mogelijkheid. U moet een virtueel netwerk voor persoonlijke communicatie tussen Azure en uw on-premises netwerk maken. U moet een VPN-eindpunt op de on-premises netwerk te implementeren. De VPN-verbinding wordt geopend vanuit Azure naar de on-premises netwerk. Zie voor meer informatie [Virtual Network-overzicht](../virtual-network/virtual-networks-overview.md) en [een Site-naar-Site-VPN configureren in de Azure Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Een optie voor het [maken van een punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) vindt u afzonderlijke op Windows gebaseerde computers rechtstreeks verbinden met een Azure-netwerk.
> 
> 

* Ongeacht of u een virtueel netwerk of niet, Azure kosten voor uitgaande verkeer, maar niet inkomend. Verschillende ontwerpkeuzen van Windows Server Active Directory kunnen invloed hebben op hoeveel uitgaande verkeer wordt gegenereerd door een implementatie. Bijvoorbeeld, beperkt het implementeren van een alleen-lezen domeincontroller (RODC) uitgaande verkeer omdat deze wordt niet gerepliceerd uitgaand. Maar de beslissing voor het implementeren van een alleen-lezen domeincontroller moet worden afgewogen tegen de noodzaak om uit te voeren van schrijfbewerkingen op basis van de domeincontroller en de [compatibiliteit](https://technet.microsoft.com/library/cc755190) toepassingen en services in de site met RODC's hebt. Zie voor meer informatie over kosten voor verkeer [Azure prijzen op overzichtelijke](http://azure.microsoft.com/pricing/).
* Terwijl u volledige hebt via welke server-bronnen voor virtuele machines on-premises, zoals het RAM-geheugen, schijfgrootte en enzovoort, op Azure moet u uit een lijst met vooraf geconfigureerde server grootten beheren. Voor een domeincontroller is een gegevensschijf naast de besturingssysteemschijf nodig om te kunnen opslaan van de Windows Server Active Directory-database.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Kunt u Windows Server AD FS op virtuele machines in Azure implementeren?
Ja, kunt u Windows Server AD FS op Azure virtuele machines implementeren en de [best practices voor AD FS-implementatie](https://technet.microsoft.com/library/dn151324.aspx) lokale gelden voor zowel implementatie van AD FS in Azure. Maar de best practices, zoals taakverdeling en hoge beschikbaarheid technologieën afgezien van wat AD FS biedt zelf nodig hebt. Ze moeten worden geleverd door de onderliggende infrastructuur. Laten we Bekijk deze best practices en hoe ze met behulp van Azure VM's en een Azure-netwerk kunnen worden bereikt.

1. **Servers met beveiliging beveiligingstokenservice (STS) rechtstreeks met het Internet nooit worden blootgesteld.**
   
    Dit is belangrijk omdat de STS verstrekt beveiligingstokens. Als gevolg hiervan moeten STS-servers zoals AD FS-servers worden behandeld met hetzelfde niveau van bescherming als een domeincontroller. Als een STS is geknoeid, hebben kwaadwillende gebruikers de mogelijkheid toegangstokens mogelijk met claims van hun keuze voor relying party-toepassingen en andere servers STS in organisaties vertrouwende uitgeven.
2. **Active Directory-domeincontrollers voor alle gebruikersdomeinen in hetzelfde netwerk bevindt als de AD FS-servers implementeren.**
   
    Active Directory Domain Services AD FS-servers gebruiken om gebruikers te verifiëren. Het verdient aanbeveling domeincontrollers op hetzelfde netwerk bevindt als de AD FS-servers te implementeren. Dit biedt bedrijfscontinuïteit voor het geval de koppeling tussen het Azure-netwerk en uw on-premises netwerk verbroken is en lagere latentie en verbeterde prestaties voor aanmeldingen maakt.
3. **Implementeer meerdere AD FS-knooppunten voor hoge beschikbaarheid en de taakverdeling.**
   
    In de meeste gevallen is het uitvallen van een toepassing waarmee AD FS onaanvaardbaar omdat de toepassingen waarvoor beveiligingstokens zijn vaak bedrijfskritieke. Als gevolg hiervan en omdat AD FS bevindt zich nu in het kritieke pad met het openen van bedrijfskritieke toepassingen, de AD FS-service moet maximaal beschikbaar is via meerdere AD FS-proxy's en AD FS-servers. Als u wilt bereiken distributie van aanvragen, netwerktaakverdelers meestal worden ingezet voor zowel de AD FS-proxy's en de AD FS-servers.
4. **Implementeer een of meer Web Application Proxy knooppunten voor toegang tot internet.**
   
    Wanneer gebruikers toegang krijgen tot toepassingen die zijn beveiligd door de AD FS-service, moet de AD FS-service beschikbaar is via het internet. Dit wordt bereikt door het implementeren van de Web Application Proxy-service. Het is raadzaam voor het implementeren van meer dan één knooppunt voor de doeleinden van hoge beschikbaarheid en taakverdeling.
5. **Toegang van de knooppunten Web Application Proxy beperken tot interne netwerkbronnen.**
   
    U moet implementeren Web Application Proxy knooppunten (of AD FS-Proxy in eerdere versies van Windows Server) zodat externe gebruikers toegang krijgen tot AD FS vanaf internet. De Web Application proxy knooppunten worden rechtstreeks blootgesteld aan Internet. Ze zijn niet vereist om te worden van domein en ze alleen toegang nodig tot de AD FS-servers via TCP-poorten 443 en 80. Het is raadzaam dat de communicatie naar alle andere computers (met name domeincontrollers) geblokkeerd.
   
    Dit is doorgaans bereikte lokale middels een DMZ genoemd. Een modus voor het goedgekeurde IP-adressen van firewalls gebruiken voor het verkeer te beperken van de DMZ tot de on-premises netwerk (dat wil zeggen, alleen verkeer van de opgegeven IP-adressen en via opgegeven poorten is toegestaan en alle andere verkeer wordt geblokkeerd).

Het volgende diagram ziet u een traditioneel een on-premises AD FS-implementatie.

![Diagram van een implementatie van traditionele on-premises Active Directory Federation Services](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Omdat Azure geen native biedt, complete firewall-functie, andere opties moeten echter worden gebruikt voor het verkeer te beperken. De volgende tabel toont elke optie en voordelen en nadelen.

| Optie | Gebruik | Nadeel |
| --- | --- | --- |
| [Azure-netwerk-ACL 's](../virtual-network/virtual-networks-acl.md) |Minder dure en eenvoudigere beginconfiguratie |Extra netwerk ACL-configuratie vereist als de nieuwe VM's worden toegevoegd aan de implementatie |
| [Barracuda NG firewall](https://www.barracuda.com/products/ngfirewall) |Geaccepteerde modus van bewerking en vereist geen ACL-netwerkconfiguratie |Hogere kosten en complexiteit voor de eerste installatie |

De stappen op hoog niveau voor het implementeren van AD FS zijn in dit geval als volgt:

1. Maak een virtueel netwerk met cross-premises-connectiviteit met behulp van een VPN of [ExpressRoute](http://azure.microsoft.com/services/expressroute/).
2. Implementeer domeincontrollers in het virtuele netwerk. Deze stap is optioneel maar aanbevolen.
3. AD FS-servers lid van een domein op het virtuele netwerk implementeert.
4. Maak een [interne load set met gelijke taakverdeling](http://azure.microsoft.com/blog/internal-load-balancing/) die de AD FS-servers bevat en maakt gebruik van een nieuwe persoonlijke IP-adres in het virtuele netwerk (dynamische IP-adres).
   
   1. DNS voor het maken van de FQDN-naam om te verwijzen naar de persoonlijke (dynamische) IP-adres van de interne set met gelijke taakverdeling bijwerken.
5. Maak een cloudservice (of een afzonderlijke virtueel netwerk) voor de Web Application Proxy-knooppunten.
6. Web Application Proxy knooppunten in de cloudservice of het virtuele netwerk implementeren
   
   1. Maak een externe set met gelijke taakverdeling met de Web Application Proxy-knooppunten.
   2. Werk de externe DNS-naam (Fully Qualified Domain Name) om te verwijzen naar de cloud service openbare IP-adres (het virtuele IP-adres).
   3. Configureren van AD FS-proxy's voor het gebruik van de FQDN-naam die overeenkomt met de interne set met gelijke taakverdeling voor de AD FS-servers.
   4. Websites op basis van claims voor het gebruik van de externe FQDN-naam voor de claimprovider bijwerken.
7. Beperk de toegang tussen Web Application Proxy aan een machine in het virtuele netwerk van de AD FS.

Om verkeer te beperken, de set met gelijke taakverdeling voor de Azure interne load balancer moet worden geconfigureerd voor alleen het verkeer naar TCP-poorten 80 en 443 en alle verkeer naar de interne dynamische IP-adres van de set met gelijke taakverdeling is verwijderd.

![Diagram van AD FS-netwerk-ACL's met TCP 443 en 80 wordt toegestaan.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Het verkeer naar de AD FS-servers zou mag alleen door de volgende bronnen:

* De Azure interne load balancer.
* Het IP-adres van een administrator op de on-premises netwerk.

> [!WARNING]
> Het ontwerp moet voorkomen dat Web Application Proxy knooppunten eventuele andere virtuele machines in de virtuele Azure-netwerk of locaties op de on-premises netwerk bereikt. Dat kan worden gedaan door firewallregels configureren in het lokale apparaat voor Express Route-verbindingen of de VPN-apparaat voor site-naar-site VPN-verbindingen.
> 
> 

Een nadeel van deze optie is de noodzaak om de netwerk-ACL's voor meerdere apparaten, waaronder interne load balancer, de AD FS-servers en andere servers die zijn toegevoegd aan het virtuele netwerk te configureren. Als een apparaat wordt toegevoegd aan de implementatie zonder te configureren om verkeer te beperken tot het netwerk-ACL's, kan de volledige implementatie mogelijk risico. Als de IP-adressen van de knooppunten Web Application Proxy ooit wijzigt, het netwerk-ACL's moet opnieuw worden ingesteld (wat betekent dat de proxy's moet worden geconfigureerd voor het gebruik van [statische dynamische IP-adressen](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![AD FS in Azure met de netwerk-ACL's.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Een andere optie is met de [Barracuda NG Firewall](https://www.barracuda.com/products/ngfirewall) toestel aan verkeer voor beheer tussen AD FS-proxyservers en de AD FS-servers. Deze optie voldoet aan best practices voor beveiliging en hoge beschikbaarheid en minder beheer na de initiële installatie vereist, omdat het toestel Barracuda NG Firewall een lijst met geaccepteerde-modus van beheer van de firewall biedt en kan worden geïnstalleerd rechtstreeks op een virtuele Azure-netwerk. Die overbodig op elk gewenst moment die een nieuwe server is toegevoegd aan de implementatie van netwerk-ACL's configureren. Maar deze optie verhoogt de initiële implementatiecomplexiteit en kosten.

In dit geval worden twee virtuele netwerken geïmplementeerd in plaats van een. We bellen u ze VNet1 en VNet2. VNet1 bevat de proxy's en VNet2 bevat de STSs en de netwerkverbinding terug naar het bedrijfsnetwerk. VNet1 daarom is het fysiek (die mogelijk vrijwel) geïsoleerd van VNet2 en op zijn beurt uit het bedrijfsnetwerk. VNet1 vervolgens met VNet2 is verbonden met een speciale tunneling technologie bekend als Transport onafhankelijk netwerk architectuur (TINA). De tunnel TINA is gekoppeld aan elk van de virtuele netwerken die gebruikmaken van een firewall Barracuda NG: één Barracuda op elk van de virtuele netwerken.  Voor hoge beschikbaarheid, wordt aanbevolen dat u twee barracuda's op elke virtuele netwerk implementeert. een actieve, de andere passief. Ze bieden zeer uitgebreide mogelijkheden gebruik waarmee we nabootsen van de werking van een DMZ traditionele on-premises in Azure.

![AD FS in Azure met firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Zie voor meer informatie [AD FS: uitbreiden van een claimbewuste on-premises front-toepassing met het Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Een alternatief voor AD FS-implementatie als het doel alleen van Office 365 SSO is
Er is een ander alternatief voor het implementeren van AD FS helemaal als het doel is om alleen in te schakelen aanmeldingspagina voor Office 365. In dat geval kunt u gewoon DirSync implementeren met wachtwoord sync on-premises en dezelfde eindresultaat bereiken met minimale implementatiecomplexiteit, omdat deze benadering geen AD FS of Azure vereist.

De volgende tabel ziet hoe de processen aanmelden werken met en zonder de AD FS implementeren.

| Office 365 eenmalige aanmelding met AD FS en DirSync | Office 365 dezelfde aanmelding met DirSync + Wachtwoordsynchronisatie |
| --- | --- |
| 1. De gebruiker zich aanmeldt bij een bedrijfsnetwerk en is geverifieerd voor Windows Server Active Directory. |1. De gebruiker zich aanmeldt bij een bedrijfsnetwerk en is geverifieerd voor Windows Server Active Directory. |
| 2. De gebruiker probeert te krijgen tot Office 365 (ik ben @contoso.com). |2. De gebruiker probeert te krijgen tot Office 365 (ik ben @contoso.com). |
| 3. Office 365 wordt de gebruiker omgeleid naar Azure AD. |3. Office 365 wordt de gebruiker omgeleid naar Azure AD. |
| 4. Aangezien Azure AD de gebruiker kan niet worden geverifieerd en er is een vertrouwensrelatie met AD FS on-premises begrijpt, wordt de gebruiker omgeleid naar AD FS. |4. Azure AD kan geen Kerberos-tickets rechtstreeks accepteren en geen vertrouwensrelatie bestaat, dus deze vraagt dat de gebruiker referenties invoert. |
| 5. De gebruiker verzendt een Kerberos-ticket naar de AD FS-STS. |5. De gebruiker voert het lokale wachtwoord en Azure AD wordt deze gevalideerd tegen de gebruikersnaam en wachtwoord dat is gesynchroniseerd met DirSync. |
| 6. AD FS het Kerberos-ticket voor de vereiste token indeling/claims getransformeerd en wordt de gebruiker omgeleid naar Azure AD. |6. Azure AD wordt de gebruiker omgeleid naar de Office 365. |
| 7. De gebruiker wordt geverifieerd naar Azure AD (een andere transformatie optreedt). |7. De gebruiker zich aanmelden bij Office 365 en OWA met de Azure AD-token. |
| 8. Azure AD wordt de gebruiker omgeleid naar de Office 365. | |
| 9. De gebruiker is achtergrond aangemeld op Office 365. | |

In de Office 365 met DirSync met wachtwoordsynchronisatiescenario (geen AD FS), wordt eenmalige aanmelding vervangen door "dezelfde aanmelding' waar 'dezelfde' betekent dat gebruikers moeten hun referenties opnieuw invoeren dezelfde on-premises bij het openen van Office 365. Houd er rekening mee dat deze gegevens kan worden onthouden door de browser van de gebruiker om te beperken van de volgende prompts.

### <a name="additional-food-for-thought"></a>Aanvullende voeding for gedachte
* Als u AD FS-proxy op een virtuele machine van Azure implementeert, wordt de verbinding met de AD FS-servers nodig. Als ze zich on-premises, is het raadzaam dat u gebruikmaken van de site-naar-site VPN-verbindingen geleverd door het virtuele netwerk waarmee de Web Application Proxy-knooppunten te communiceren met hun AD FS-servers.
* Als u een AD FS-server op een virtuele machine van Azure implementeert, verbinding met Windows Server Active Directory-domeincontrollers, Kenmerkarchieven en configuratiedatabases nodig en kan ook een ExpressRoute- of een site-naar-site VPN-verbinding tussen vereist de virtuele Azure-netwerk en de on-premises netwerk.
* Kosten worden toegepast op al het verkeer van virtuele machines in Azure (uitgaande verkeer). Als kosten de aangedreven factor is, is het raadzaam voor het implementeren van de knooppunten Web Application Proxy op Azure, zodat de AD FS-servers on-premises. Als de AD FS-servers zijn geïmplementeerd op virtuele machines in Azure ook, extra kosten in rekening worden gebracht om lokale gebruikers te verifiëren. Uitgaande verkeer maakt een kosten ongeacht of het doorlopen van het ExpressRoute- of de site-naar-site VPN-verbinding.
* Als u Azure systeemeigen taakverdeling van servers mogelijkheden voor hoge beschikbaarheid van de AD FS-servers gebruiken, houd er rekening mee dat de taakverdeling biedt voor de tests die worden gebruikt voor het bepalen van de status van de virtuele machines in de cloudservice. In het geval van virtuele machines in Azure (in plaats van web- of worker-functies), moet een aangepaste test worden gebruikt, omdat de agent dat met de standaard-tests overeenkomt niet aanwezig op de virtuele machines in Azure is. Voor het gemak, kunt u een aangepaste TCP-test: dit is vereist alleen dat een TCP-verbinding (een segment TCP SYN verzonden en beantwoord met een segment TCP SYN ACK) worden tot stand gebracht status van de virtuele machine te bepalen. U kunt de aangepaste test voor het gebruik van een TCP-poort waarop uw virtuele machines actief zijn luistert configureren.

> [!NOTE]
> Virtuele machines die nodig is om dezelfde reeks poorten rechtstreeks met het Internet (zoals poort 80 en 443) weer te geven delen niet de dezelfde cloudservice. Daarom wordt aanbevolen dat u een toegewijde cloud maakt-service voor uw Windows Server AD FS-servers om te voorkomen dat potentiële overlapt tussen Poortvereisten voor een toepassing en Windows Server AD FS.
> 
> 

## <a name="deployment-scenarios"></a>Implementatiescenario's
De volgende sectie bevat een overzicht van de alledaagse implementatiescenario's om de aandacht te vestigen op belangrijke overwegingen die moeten worden meegenomen in aanmerking. Elk scenario bevat koppelingen naar meer informatie over de beslissingen en factoren in overweging moet nemen.

1. [AD DS: Een AD DS-compatibele toepassing implementeren met geen vereiste voor verbinding met zakelijke netwerken](#BKMK_CloudOnly)
   
    Bijvoorbeeld, wordt een internetgerichte SharePoint-service geïmplementeerd op een virtuele machine van Azure. De toepassing heeft geen afhankelijkheden op zakelijke netwerkbronnen. De toepassing Windows Server AD DS is vereist, maar hoeft niet de zakelijke Windows Server AD DS.
2. [AD FS: Een front-claimbewuste on-premises-toepassing met het Internet uitbreiden](#BKMK_CloudOnlyFed)
   
    Bijvoorbeeld, moet een claimbewuste toepassing die is geïmplementeerd op lokale is en die wordt gebruikt door zakelijke gebruikers niet toegankelijk zijn vanaf Internet. De toepassing moet worden geopend rechtstreeks via Internet met een zakelijke partners gebruik van hun eigen bedrijf identiteiten of bestaande zakelijke gebruikers.
3. [AD DS: Implementeert een Windows Server AD DS-compatibele toepassing waarvoor de verbinding met het bedrijfsnetwerk.](#BKMK_HybridExt)
   
    Bijvoorbeeld, is een LDAP-bewuste toepassingen die ondersteuning biedt voor geïntegreerde Windows-verificatie en maakt gebruik van Windows Server AD DS als een opslagplaats voor configuratie en gebruikersprofiel gegevens op een virtuele machine van Azure geïmplementeerd. Is het wenselijk zijn voor de toepassing om gebruikmaken van de bestaande zakelijke Windows Server AD DS en eenmalige aanmelding te bieden. De toepassing is niet claimbewuste.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Een AD DS-compatibele toepassing implementeren met geen vereiste voor verbinding met zakelijke netwerken
![AD DS-implementatie alleen in de cloud](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**afbeelding 1**

#### <a name="description"></a>Beschrijving
SharePoint is geïmplementeerd in Azure een virtuele machine en de toepassing heeft geen afhankelijkheden op zakelijke netwerkbronnen. De toepassing vereist Windows Server AD DS maar *niet* vereisen de zakelijke Windows Server AD DS. Er is geen Kerberos of Federatieve vertrouwensrelaties zijn vereist, omdat gebruikers zelf ingerichte via de toepassing in het Windows Server AD DS-domein dat ook wordt gehost in de cloud op Azure virtuele machines.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe technologiegebieden toepassen op het scenario
* [Netwerktopologie](#BKMK_NetworkTopology): maken van een Azure-netwerk zonder cross-premises-connectiviteit (ook wel bekend als site-naar-site-connectiviteit).
* [DC-implementatieconfiguratie](#BKMK_DeploymentConfig): implementeert een nieuwe domeincontroller in een nieuwe, één domein, Windows Server Active Directory-forest. Dit moet samen met de Windows-DNS-server worden geïmplementeerd.
* [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology): gebruiken de standaard Windows Server Active Directory-site (alle computers zich in een Default-First-Site-Name).
* [De IP-adressen en DNS-](#BKMK_IPAddressDNS):
  
  * Een statisch IP-adres voor de domeincontroller ingesteld met de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
  * Installeren en configureren van Windows Server DNS op de domeincontroller (s) in Azure.
  * Configureer de eigenschappen van virtueel netwerk met de naam en IP-adres van de virtuele machine die als host fungeert voor de domeincontroller en DNS-serverfuncties.
* [Globale catalogus](#BKMK_GC): de eerste domeincontroller in het forest moet een globale-catalogusserver. Aanvullende DC's moeten ook worden geconfigureerd als GC omdat in een forest één domein, de globale catalogus geen extra werk van de domeincontroller vereist.
* [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen aan DC's die worden uitgevoerd als Azure virtuele machines om te kunnen opslaan van de Windows Server Active Directory-database, logboekbestanden en SYSVOL.
* [Back-up en herstel](#BKMK_BUR): bepalen waar u wilt opslaan van de systeemstatusback-ups. Indien nodig, moet u een andere gegevensschijf toevoegen aan de domeincontroller-VM voor het opslaan van back-ups.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: uitbreiden van een claimbewuste on-premises front-toepassing met het Internet
![Federatie met cross-premises connectiviteit](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**afbeelding 2**

#### <a name="description"></a>Beschrijving
Een claimbewuste toepassing die is geïmplementeerd op lokale is en die wordt gebruikt door zakelijke gebruikers moet worden rechtstreeks via Internet toegankelijk. De toepassing fungeert als een web-frontend naar een SQL-database waarin gegevens worden opgeslagen. De SQL-servers die worden gebruikt door de toepassing staan ook in het bedrijfsnetwerk. Twee Windows Server AD FS STSs en een load balancer is geïmplementeerde lokale voor toegang tot de zakelijke gebruikers. De toepassing nu moet bovendien zowel door zakelijke partners gebruik van hun eigen bedrijf identiteiten en bestaande zakelijke gebruikers rechtstreeks via Internet worden geopend.

In een poging om te vereenvoudigen en voldoen aan de behoeften van de implementatie en configuratie van deze nieuwe vereiste, wordt besloten dat twee extra web frontends en twee Windows Server AD FS-proxyservers worden geïnstalleerd op virtuele machines in Azure. Alle vier virtuele machines wordt rechtstreeks worden blootgesteld aan Internet en de verbinding met de on-premises netwerk met behulp van Azure Virtual Network site-naar-site VPN-functionaliteit worden geleverd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe technologiegebieden toepassen op het scenario
* [Netwerktopologie](#BKMK_NetworkTopology): maken van een virtuele Azure-netwerk en [cross-premises connectiviteit configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).
  
  > [!NOTE]
  > Zorg ervoor dat de URL die is gedefinieerd binnen het certificaatsjabloon en de resulterende certificaten kan worden bereikt door de Windows Server AD FS-exemplaren in Azure wordt uitgevoerd voor elk van de Windows Server AD FS-certificaten. Hiervoor moet mogelijk cross-premises-connectiviteit voor het delen van uw PKI-infrastructuur. Voor bijvoorbeeld als de CRL-eindpunt op basis van LDAP en exclusief on-premises gehoste vervolgens cross-premises verbinding moet worden gestart. Als dit niet het wenselijk is, is het mogelijk nodig is om certificaten worden uitgegeven door een CA waarvan CRL toegankelijk via het Internet is te gebruiken.
  > 
  > 
* [Configuratie voor cloud-services](#BKMK_CloudSvcConfig): Controleer of u hebt twee cloudservices in volgorde bieden twee taakverdeling virtuele IP-adressen. Het virtuele IP-adres van de eerste cloudservice wordt omgeleid naar de twee Windows Server AD FS-proxy virtuele machines op poorten 80 en 443. De Windows Server AD FS-proxy virtuele machines wordt geconfigureerd voor het verwijzen naar het IP-adres van de lokale taakverdeling die de Windows Server AD FS-STSs de voorzijde's. Het virtuele IP-adres van de tweede cloudservice wordt omgeleid naar de twee virtuele machines opnieuw uit te voeren de web-frontend op poort 80 en 443. Een aangepaste test om te controleren of dat de load balancer stuurt alleen verkeer functionerende WindowsServer AD FS proxy- en web frontend VM's configureren.
* [Configuratie van de Federation-server](#BKMK_FedSrvConfig): configureren van Windows Server AD FS als federatieserver (STS) voor het genereren van beveiligingstokens voor de Windows Server Active Directory-forest dat is gemaakt in de cloud. Federatieve vertrouwensrelaties van claimprovider provider met de andere partners die u wilt accepteren identiteiten van ingesteld en vertrouwensrelaties met relying party configureren met de andere toepassingen die u wilt genereren van tokens, voor.
  
    Windows Server AD FS proxy-servers zijn geïmplementeerd in een internetgerichte capaciteit om veiligheidsredenen in de meeste gevallen terwijl de oorspronkelijke Windows Server AD FS federation bestanden geïsoleerd van directe verbinding met Internet blijven. Ongeacht het implementatiescenario, moet u uw cloudservice configureren met een virtueel IP-adres waarmee een openbaar blootgestelde IP-adres en poort waarmee u kunt toepassen van taakverdeling tussen de twee exemplaren van Windows Server AD FS-STS of de proxy-exemplaren.
* [Windows Server AD FS-configuratie voor hoge beschikbaarheid](#BKMK_ADFSHighAvail): U wordt aangeraden voor het implementeren van een Windows Server AD FS-farm met ten minste twee servers voor failover en taakverdeling. U kunt overwegen Windows interne Database (WID) voor Windows Server AD FS-configuratiegegevens, en gebruiken de interne load balancing-mogelijkheden van Azure om inkomende aanvragen verdelen over de servers in de farm.

Zie voor meer informatie de [Implementatiehandleiding voor AD DS](https://technet.microsoft.com/library/cc753963).

### <a name="BKMK_HybridExt"></a>3. AD DS: Implementeert een Windows Server AD DS-compatibele toepassing waarvoor de verbinding met het bedrijfsnetwerk.
![Cross-premises AD DS-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**afbeelding 3**

#### <a name="description"></a>Beschrijving
Een LDAP-compatibele toepassing wordt geïmplementeerd op een virtuele machine van Azure. Deze ondersteuning biedt voor geïntegreerde Windows-verificatie en Windows Server AD DS gebruikt als een opslagplaats voor configuratie- en gebruikersbestanden profielgegevens. Het doel is voor de toepassing om gebruikmaken van de bestaande zakelijke Windows Server AD DS en eenmalige aanmelding te bieden. De toepassing is niet claimbewuste. Gebruikers moeten ook rechtstreeks toegang tot de toepassing uit het Internet. Om te optimaliseren voor prestaties en kosten, wordt besloten dat twee extra domeincontrollers die deel uitmaken van het bedrijfsdomein samen met de toepassing in Azure worden geïmplementeerd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe technologiegebieden toepassen op het scenario
* [Netwerktopologie](#BKMK_NetworkTopology): maken van een Azure-netwerk met [cross-premises connectiviteit](../vpn-gateway/vpn-gateway-site-to-site-create.md).
* [Installatiemethode](#BKMK_InstallMethod): replica DC's van het bedrijfsdomein van Windows Server Active Directory implementeren. U kunt voor een replica van een DC, Windows Server AD DS installeren op de virtuele machine en optioneel de functie installeren vanaf medium (IFM) gebruiken om u te verminderen de hoeveelheid gegevens die moeten worden gerepliceerd naar de nieuwe domeincontroller tijdens de installatie. Zie voor een zelfstudie [een replica Active Directory-domeincontroller installeren in Azure](active-directory-install-replica-active-directory-domain-controller.md). Zelfs als u IFM hebt gebruikt, is het mogelijk dat het efficiënter om op te bouwen van de virtuele DC on-premises en de volledige virtuele hardeschijf (VHD) verplaatsen naar de cloud in plaats van Windows Server AD DS tijdens de installatie te repliceren. Voor de veiligheid, wordt aangeraden de VHD uit de on-premises netwerk te verwijderen nadat deze is gekopieerd naar Azure.
* [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology): Maak een nieuwe Azure site in Active Directory: Sites en Services. Maak een Windows Server Active Directory-subnet-object om te vertegenwoordigen de Azure-netwerk en het subnet toevoegen aan de site. Maak een nieuwe sitekoppeling met de nieuwe Azure-site en de site waarin virtuele netwerk van Azure VPN-eindpunt bevindt om te controleren en optimaliseren van Windows Server Active Directory verkeer van en naar Azure.
* [De IP-adressen en DNS-](#BKMK_IPAddressDNS):
  
  * Een statisch IP-adres voor de domeincontroller ingesteld met de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
  * Installeren en configureren van Windows Server DNS op de domeincontroller (s) in Azure.
  * Configureer de eigenschappen van virtueel netwerk met de naam en IP-adres van de virtuele machine die als host fungeert voor de domeincontroller en DNS-serverfuncties.
* [Geografisch verspreide DC's](#BKMK_DistributedDCs): indien nodig aanvullende virtuele netwerken configureren. Als uw Active Directory-site-topologie DC's in de locaties die met andere Azure-regio's vereist, overeenkomen dan u wilt maken van Active Directory-sites dienovereenkomstig.
* [Alleen-lezen domeincontrollers](#BKMK_RODC): U kunt een alleen-lezen domeincontroller in de Azure site zou kunnen implementeren, afhankelijk van uw vereisten voor het uitvoeren van schrijven bewerkingen op de domeincontroller en de compatibiliteit van toepassingen en services in de site met de RODC's. Zie voor meer informatie over de compatibiliteit van toepassingen, de [compatibiliteit gebruikershandleiding voor de toepassing van de domeincontrollers van de alleen-lezen domeincontroller](https://technet.microsoft.com/library/cc755190).
* [Globale catalogus](#BKMK_GC): aangegeven nodig zijn voor het service-aanmeldingsaanvragen in forests met meerdere domeinen. Als u een GC-exemplaar in de Azure site geen implementeren, kunt u kosten voor uitgaand verkeer, worden als verificatieaanvragen query's aangegeven in andere sites veroorzaken. Om te beperken dat verkeer, kunt u inschakelen universeel groepslidmaatschap in cache opslaan voor de Azure site in Active Directory: Sites en Services.
  
    Als u een GC-exemplaar implementeert, sitekoppelingen en koppelingen sitekosten zodanig configureren dat de GC in de Azure site niet aanbevolen als een bron-DC door andere aangegeven die moet wordt worden gerepliceerd van de dezelfde gedeeltelijke domeinpartities.
* [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen aan DC's die worden uitgevoerd op Azure Virtual machines om te kunnen opslaan van de Windows Server Active Directory-database, logboekbestanden en SYSVOL.
* [Back-up en herstel](#BKMK_BUR): bepalen waar u wilt opslaan van de systeemstatusback-ups. Indien nodig, moet u een andere gegevensschijf toevoegen aan de domeincontroller-VM voor het opslaan van back-ups.

## <a name="deployment-decisions-and-factors"></a>Beslissingen voor de implementatie en factoren
Deze tabel bevat een overzicht van de Windows Server Active Directory-technologie-gebieden die in de bovenstaande scenario's en de bijbehorende beslissingen rekening houden met koppelingen naar meer gedetailleerde informatie hieronder worden beïnvloed. Sommige technologiegebieden mogelijk niet van toepassing op elke implementatiescenario en het is mogelijk dat sommige technologiegebieden belangrijker om een implementatiescenario dan andere technologiegebieden.

Bijvoorbeeld, als u een replica van een DC op een virtueel netwerk implementeren en uw forest één domein heeft, kan vervolgens te kiezen voor het implementeren van een globale-catalogusserver in dat geval niet worden essentieel is voor het scenario van implementatie omdat eventuele aanvullende replicatie wordt niet gemaakt vereisten. Aan de andere kant als het forest in meerdere domeinen is, klikt u vervolgens de beslissing voor het implementeren van een globale catalogus op een virtueel netwerk in het gedrang beschikbare bandbreedte, prestaties, verificatie, directory zoekacties, enzovoort.

| Windows Server Active Directory-technologiegebied | Beslissingen | Factoren |
| --- | --- | --- |
| [Netwerktopologie](#BKMK_NetworkTopology) |Een virtueel netwerk maken? |<li>Vereisten voor toegang tot bronnen Corp</li> <li>Authentication</li> <li>Accountbeheer</li> |
| [Configuratie van DC-implementatie](#BKMK_DeploymentConfig) |<li>Een afzonderlijk forest zonder eventuele vertrouwensrelaties implementeren?</li> <li>Een nieuw forest met Federatie implementeren?</li> <li>Een nieuw forest met Windows Server Active Directory-forest vertrouwen of Kerberos implementeren?</li> <li>Corp-forest uitbreiden door het implementeren van een replica van een DC?</li> <li>Corp-forest uitbreiden door het implementeren van een nieuw onderliggend domein of een domeinstructuur?</li> |<li>Beveiliging</li> <li>Naleving</li> <li>Kosten</li> <li>Tolerantie en fouttolerantie</li> <li>Toepassingscompatibiliteit</li> |
| [Windows Server Active Directory-site-topologie](#BKMK_ADSiteTopology) |Hoe configureer u subnetten, sites en site-koppelingen met Azure Virtual Network verkeer optimaliseren en kosten te minimaliseren? |<li>Definities van subnet en een site</li> <li>Site-eigenschappen van koppeling en wijzig melding</li> <li>Compressie van replicatie</li> |
| [IP-adressen en DNS](#BKMK_IPAddressDNS) |Het IP-adressen en naamomzetting configureren? |<li>Gebruik van de cmdlet de Set-AzureStaticVNetIP gebruik een statisch IP-adres toewijzen</li> <li>Windows Server DNS-server installeren en configureren van de eigenschappen van virtueel netwerk met de naam en IP-adres van de virtuele machine die als host fungeert voor de domeincontroller en DNS-server-rollen</li> |
| [Geografisch verspreide DC 's](#BKMK_DistributedDCs) |Hoe kan ik repliceren naar de DC's op afzonderlijke virtuele netwerken? |Als uw Active Directory-site-topologie DC's in de locaties die overeenkomt met andere Azure-regio's vereist, dan u wilt maken van Active Directory-sites dienovereenkomstig. [Virtueel netwerk met virtual network Connectivity configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) om te repliceren tussen domeincontrollers op afzonderlijke virtuele netwerken. |
| [Alleen-lezen domeincontrollers](#BKMK_RODC) |Alleen-lezen of beschrijfbare domeincontrollers gebruiken? |<li>HBI/PII filterkenmerken</li> <li>Filter geheimen</li> <li>Limiet voor uitgaand verkeer</li> |
| [Globale catalogus](#BKMK_GC) |Globale catalogus installeren? |<li>Voor forest met één domein, moet u alle DC's aangegeven</li> <li>Voor de forest met meerdere domeinen zijn GC vereist voor verificatie</li> |
| [Installatiemethode](#BKMK_InstallMethod) |Het installeren van DC in Azure? |Beide: <li>Installatie van AD DS met Windows PowerShell of Dcpromo</li> <li>Verplaats de VHD van een lokale virtuele domeincontroller</li> |
| [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB) |Waar Windows Server AD DS-database, logboekbestanden en SYSVOL opslaan? |Dcpromo.exe standaardwaarden wijzigen. Deze kritieke Active Directory-bestanden *moet* in plaats van de schijven van het besturingssysteem die schrijfcache implementeren op Azure gegevensschijven worden geplaatst. |
| [Back-up en herstel](#BKMK_BUR) |Hoe kan ik beveiligen en herstellen van gegevens? |Systeem voor systeemstatusback-ups maken |
| [Configuratie van de Federation-server](#BKMK_FedSrvConfig) |<li>Een nieuw forest met Federatie in de cloud implementeren?</li> <li>AD FS on-premises implementeren en weergeven van een proxyserver in de cloud?</li> |<li>Beveiliging</li> <li>Naleving</li> <li>Kosten</li> <li>Toegang tot toepassingen door zakelijke partners</li> |
| [Configuratie voor cloud-services](#BKMK_CloudSvcConfig) |De eerste keer dat u een virtuele machine maken wordt impliciet door een cloudservice worden geïmplementeerd. Moet u om meer cloudservices te implementeren? |<li>Een virtuele machine of virtuele machines vereist rechtstreeks blootgesteld aan Internet?</li> <li> De service vereist taakverdeling?</li> |
| [Federatieve server-vereisten voor openbare en particuliere IP-adressering (dynamische IP versus virtuele IP)](#BKMK_FedReqVIPDIP) |<li>Moet het exemplaar van Windows Server AD FS rechtstreeks vanaf het Internet worden bereikt?</li> <li>Vereist de toepassing wordt geïmplementeerd in de cloud zijn eigen internetgerichte IP-adres en poort?</li> |Maak een cloudservice voor elke virtuele IP-adres dat is vereist voor uw implementatie |
| [Windows Server AD FS-configuratie voor hoge beschikbaarheid](#BKMK_ADFSHighAvail) |<li>Het aantal knooppunten in mijn Windows Server AD FS serverfarm?</li> <li>Het aantal knooppunten te implementeren in mijn Windows Server AD FS-farm met proxy?</li> |Tolerantie en fouttolerantie |

### <a name="BKMK_NetworkTopology"></a>Netwerktopologie
Om te voldoen aan de consistentie van IP-adres en DNS-vereisten voor Windows Server AD DS, is het nodig zijn om eerst een [virtuele Azure-netwerk](../virtual-network/virtual-networks-overview.md) en koppelt uw virtuele machines. Tijdens het maken, moet u beslissen of de verbinding met uw bedrijfsnetwerk lokale, die virtuele machines in Azure transparant is verbonden met lokale machines uitbreiden: dit wordt bereikt met traditionele VPN-technologieën en vereist dat een VPN-eindpunt worden blootgesteld aan de rand van het bedrijfsnetwerk. Dat wil zeggen, wordt de VPN-verbinding van Azure gestart met het bedrijfsnetwerk, niet andersom.

Houd er rekening mee dat extra kosten worden toegepast wanneer u een virtueel netwerk naar uw on-premises netwerk buiten de standaard kosten die betrekking hebben op elke virtuele machine uitbreidt. Er zijn in het bijzonder kosten voor CPU-tijd van de Azure Virtual Network gateway en voor het uitgaande verkeer dat is gegenereerd door elke virtuele machine die met lokale virtuele machines via de VPN-verbinding communiceert. Zie voor meer informatie over het netwerkverkeer kosten [Azure prijzen op overzichtelijke](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Configuratie van DC-implementatie
De manier waarop u de domeincontroller configureren, is afhankelijk van de vereisten voor de service die u wilt uitvoeren op Azure. U kunt bijvoorbeeld een nieuw forest is geïsoleerd van uw eigen zakelijke forest, voor het testen van een bewijs van concept, een nieuwe toepassing of enige andere korte termijn project waarvoor directoryservices, maar niet specifiek toegang tot interne bedrijfsbronnen implementeren.

Als een voordeel een geïsoleerd forest die DC wordt niet gerepliceerd met lokale DC's, wat resulteert in minder uitgaand netwerkverkeer gegenereerd door het systeem zelf, rechtstreeks de kosten te verlagen. Zie voor meer informatie over het netwerkverkeer kosten [Azure prijzen op overzichtelijke](http://azure.microsoft.com/pricing/).

Stel een ander voorbeeld: u privacy-vereisten voor een service, maar de service is afhankelijk van de toegang tot uw interne Windows Server Active Directory. Als u zijn toegestaan als host voor gegevens voor de service in de cloud, kunt u een nieuw onderliggend domein kunt implementeren voor de interne forest in Azure. In dit geval kunt u een domeincontroller voor het nieuwe onderliggende domein (zonder de globale catalogus om u te helpen adres privacyproblemen) implementeren. Dit scenario, samen met een replica DC-implementatie vereist een virtueel netwerk voor verbinding met uw lokale DC's.

Als u een nieuw forest maakt, kiest u of u wilt gebruiken [Active Directory vertrouwt](https://technet.microsoft.com/library/cc771397) of [Federation-vertrouwensrelaties](https://technet.microsoft.com/library/dd807036). De vereisten die wordt bepaald door de compatibiliteit, beveiliging, naleving, kosten en tolerantie kan worden verdeeld. Bijvoorbeeld, om te profiteren van [selectieve verificatie](https://technet.microsoft.com/library/cc755844) u kunt kiezen om een nieuw forest in Azure implementeert en maakt u een Windows Server Active Directory-vertrouwensrelatie tussen het lokale forest en het forest van de cloud. Als de toepassing claimbewuste, maar implementeren u federation-vertrouwensrelaties in plaats van Active Directory-forest-vertrouwensrelaties. Een andere factor worden de kosten meer gegevens repliceren door het uitbreiden van uw lokale Windows Server Active Directory naar de cloud of meer uitgaande netwerkverkeer als gevolg van verificatie en querybelasting genereren.

Vereisten voor de beschikbaarheid en fouttolerantie zijn ook van invloed op uw keuze. Bijvoorbeeld, als de koppeling wordt onderbroken, vertrouwen gebruik van een Kerberos-vertrouwensrelatie of een federatieve toepassingen worden alle waarschijnlijk geheel onderverdeeld tenzij u voldoende infrastructuur in Azure hebt geïmplementeerd. Alternatieve implementatieconfiguraties zoals replica DC's (beschrijfbare of RODC's) verhoogt de kans op kunnen tolereren koppeling storingen.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-site-topologie
U moet correct definiëren van sites en sitekoppelingen om verkeer optimaliseren en kosten te minimaliseren. Sites, sitekoppelingen en subnetten invloed hebben op de replicatietopologie tussen domeincontrollers en de verkeersstroom verificatie. Houd rekening met de volgende verkeer kosten en implementeren en DC's volgens de vereisten van het implementatiescenario configureren:

* Er is een nominaal kosten per uur voor de gateway zelf:
  
  * Worden kan gestart en gestopt wens naar
  * Als gestopt, zijn Azure VM's geïsoleerd van het bedrijfsnetwerk.
* Binnenkomend verkeer is gratis
* Uitgaand verkeer wordt in rekening gebracht, volgens [Azure prijzen op overzichtelijke](http://azure.microsoft.com/pricing/). Eigenschappen van de site-koppeling tussen on-premises sites en de cloud-sites kunt u als volgt optimaliseren:
  
  * Als u meerdere virtuele netwerken, de sitekoppelingen en de kosten per hiervoor configureren om te voorkomen dat Windows Server AD DS de Azure site prioriteit boven een gratis de dezelfde serviceniveaus kan bieden. U kunt ook overwegen de brug alle-(BASL)-koppelingsoptie site (die standaard is ingeschakeld) uit te schakelen. Dit zorgt ervoor dat alleen rechtstreeks verbonden sites gerepliceerd met elkaar. DC's in transitief verbonden sites niet langer kunnen repliceren met elkaar zijn, maar moeten worden gerepliceerd via een gemeenschappelijke site of sites. Als de tussenliggende sites niet beschikbaar voor een bepaalde reden replicatie tussen de DC's in transitief verbonden sites niet wordt uitgevoerd zelfs als connectiviteit tussen de sites beschikbaar is. Ten slotte waar secties transitieve replicatie gedrag wenselijk blijven, sitekoppelingsbruggen maken die de juiste sitekoppelingen en sites, zoals het on-premises sites bedrijfsnetwerk bevatten.
  * [Configureren van de kosten van sitekoppelingen](https://technet.microsoft.com/library/cc794882) op de juiste manier om te voorkomen dat onbedoelde verkeer. Bijvoorbeeld, als **probeer dichtstbijzijnde Site** is ingeschakeld, Controleer of de virtuele netwerksites niet het eerstvolgende doordat de kosten van het object van de site-koppeling die de Azure site terug naar het bedrijfsnetwerk verbindt die is gekoppeld.
  * Site-koppeling configureren [intervallen](https://technet.microsoft.com/library/cc794878) en [planningen](https://technet.microsoft.com/library/cc816906) volgens de vereisten van de consistentie en frequentie van object wordt gewijzigd. Replicatieschema uitgelijnd met de tolerantie latentie. DC's repliceren alleen de laatste status van een waarde, zodat kosten verlagen van de replicatie-interval kunt opslaan als er een voldoende object wijzigen.
* Als kosten te minimaliseren, een prioriteit wordt, zorg er dan replicatie is gepland en wijzigingsbericht niet is ingeschakeld. Dit is de standaardconfiguratie bij replicatie tussen sites. Dit is niet van belang als u een alleen-lezen domeincontroller op een virtueel netwerk implementeren wilt, omdat de RODC uitgaande wijzigingen niet gerepliceerd. Maar als u een beschrijfbare domeincontroller implementeert, moet u ervoor zorgen dat de site-koppeling is niet geconfigureerd voor het repliceren van updates met onnodige frequentie. Als u een globale catalogusserver (GC) implementeert, ervoor zorgen dat elke site met een GC domeinpartities van een bron-DC in een site die is verbonden met een site-koppeling of sitekoppelingen die een lagere kosten dan de GC in de Azure site hebben gerepliceerd.
* Het is mogelijk nog steeds verder om netwerkverkeer te beperken die worden gegenereerd door de replicatie tussen sites door het wijzigen van de replicatie-compressiealgoritme. De compressiealgoritme wordt bepaald door de REG_DWORD register vermelding HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator-compressiealgoritme. De standaardwaarde is 3, die correleert met het algoritme Xpress comprimeren. Kunt u de waarde op 2, waardoor het algoritme MSZip is gewijzigd. In de meeste gevallen Hierdoor neemt de compressie, maar gebeurt dit ten koste van de CPU-gebruik. Zie voor meer informatie [werkt hoe Active Directory-replicatie-topologie](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-adressen en DNS
Virtuele machines in Azure zijn "DHCP-lease adressen" Standaard zijn toegewezen. Omdat dynamische adressen voor virtuele Azure-netwerk met een virtuele machine zich blijven gedurende de levensduur van de virtuele machine voordoen, worden de vereisten van Windows Server AD DS wordt voldaan.

Als gevolg hiervan, wanneer u een dynamisch adres in Azure, bent u van kracht met behulp van een statisch IP-adres, omdat het routeerbaar voor de periode van de lease, en de periode van de lease gelijk aan de levensduur van de cloudservice is.

Echter, het adres van de dynamische toewijzing ongedaan wordt gemaakt als de virtuele machine afgesloten wordt. Om te voorkomen dat het IP-adres opgeheven, kunt u [Set AzureStaticVNetIP gebruiken een statische IP-adres toewijzen](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Naamomzetting implementeren om uw eigen (of gebruikmaken van uw bestaande) DNS-serverinfrastructuur; Azure DNS-voldoet niet aan de behoeften van de omzetting van de naam van de geavanceerde van Windows Server AD DS. Bijvoorbeeld deze geen ondersteuning voor dynamische SRV-records, enzovoort. Naamomzetting is een essentiële configuratie-item voor domein-clients en DC's. DC's moet kunnen worden bronrecords registreert en het oplossen van andere DC-bronrecords.
Omwille van de fout tolerantie en de prestaties is het optimaal is voor het installeren van de Windows Server DNS-service op de DC's uitgevoerd op Azure. Vervolgens moet u de eigenschappen van de virtuele Azure-netwerk configureren met de naam en IP-adres van de DNS-server. Wanneer andere virtuele machines op het virtuele netwerk start, wordt de DNS-omzetter clientinstellingen geconfigureerd met DNS-server als onderdeel van de dynamische toewijzing van IP-adres.

> [!NOTE]
> U kunt op lokale computers niet koppelen aan een Active Directory van Windows Server AD DS-domein dat wordt gehost op Azure rechtstreeks via Internet. De Poortvereisten voor Active Directory en de domein-join-bewerking het niet handig rechtstreeks geven blootgesteld de vereiste poorten en in feite een hele DC met het Internet.
> 
> 

Virtuele machines registreren hun DNS-naam automatisch bij het opstarten of wanneer er een wijziging.

Zie voor meer informatie over het volgende voorbeeld en een ander voorbeeld ziet u hoe u de eerste virtuele machine inrichten en AD DS installeren op deze [een nieuw Active Directory-forest installeren in Microsoft Azure](active-directory-new-forest-virtual-machine.md). Zie voor meer informatie over het gebruik van Windows PowerShell [Azure PowerShell installeren](/powershell/azureps-cmdlets-docs) en [Azure Management-Cmdlets](/powershell/module/azurerm.compute/#virtual_machines).

### <a name="BKMK_DistributedDCs"></a>Geografisch verspreide DC 's
Azure biedt voordelen bij het hosten van meerdere domeincontrollers op verschillende virtuele netwerken:

* Fouttolerantie voor meerdere locaties
* Fysieke nabijheid tot filialen (lagere latentie)

Zie voor meer informatie over het configureren van rechtstreekse communicatie tussen virtuele netwerken [virtueel netwerk configureren voor verbinding met het virtuele netwerk](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Alleen-lezen domeincontrollers
U moet kiezen of alleen-lezen of beschrijfbare domeincontrollers implementeren. U kan worden blootgesteld aan RODC's niet implementeren omdat er geen fysieke besturingselement beweegt, maar RODC's zijn ontworpen om te worden geïmplementeerd in de locaties waar hun fysieke beveiliging risico, zoals filialen.

Azure biedt geen gevaar de fysieke beveiliging van een filiaal, maar RODC's mogelijk nog steeds blijken rendabeler omdat de functies die ze bieden geschikt zijn voor deze omgevingen zij voor zeer verschillende redenen. Bijvoorbeeld, RODC's hebben geen uitgaande replicatie en kunnen geen selectief vullen geheimen (wachtwoorden). Op het nadeel het ontbreken van deze geheime gegevens mogelijk uitgaand verkeer op het verzoek om te verifiëren als een gebruiker of computer verifieert. Maar geheimen kunnen selectief worden vooraf ingevuld en worden opgeslagen in de cache.

RODC's bieden een extra voordeel in en rond HBI en PII problemen omdat u de kenmerken die gevoelige gegevens naar de RODC bevatten gefilterde kenmerkset (VA) kunt toevoegen. De door is een aanpasbare set kenmerken die niet zijn gerepliceerd naar de RODC's. Als u niet zijn toegestaan of niet wilt opslaan van persoonlijke gegevens of HBI op Azure, kunt u de door als beveiliging. Zie voor meer informatie [gefilterde kenmerk alleen-lezen domeincontroller [(https://technet.microsoft.com/library/cc753459)] ingesteld.

Zorg ervoor dat toepassingen zijn compatibel met de RODC's die u wilt gebruiken. Veel Windows Server Active Directory-toepassingen met RODC's werken, maar sommige toepassingen kunnen uitvoeren, inefficiënt of mislukken als ze geen toegang tot een beschrijfbare domeincontroller. Zie voor meer informatie [handleiding voor compatibiliteit met alleen-lezen domeincontrollers](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globale catalogus
U moet kiezen of u wilt installeren een globale catalogus (GC). In een forest één domein, moet u alle DC's configureren als GC-servers. Kosten wordt niet verhoogd omdat er geen extra replicatieverkeer zullen zijn.

In een forest zijn aangegeven nodig zijn om uit te breiden Universal groepslidmaatschappen tijdens het verificatieproces. Als u geen een GC-exemplaar implementeert, gegenereerd werkbelastingen op het virtuele netwerk die worden geverifieerd bij een domeincontroller op Azure indirect uitgaande verificatieverkeer om op te vragen van iedere GC lokale tijdens elke authenticatiepoging.

De kosten in verband met iedere GC zijn minder voorspelbare omdat het hosten van elk domein (in onderdeel). Als de werkbelasting fungeert als host voor een service internetgerichte en gebruikers op basis van Windows Server AD DS verifieert, is het mogelijk dat de kosten volledig onvoorspelbaar. Om te beperken GC-query's buiten de cloud site tijdens de verificatie, kunt u [universeel groepslidmaatschap in cache opslaan inschakelen](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installatiemethode
U moet het installeren van de DC's in het virtuele netwerk kiezen:

* Promoveren nieuwe DC's. Zie voor meer informatie [een nieuw Active Directory-forest installeren op een virtuele Azure-netwerk](active-directory-new-forest-virtual-machine.md).
* Verplaats de VHD van een lokale virtuele domeincontroller naar de cloud. In dit geval moet u ervoor zorgen dat de on-premises virtuele DC wordt 'verplaatst,' geen 'gekopieerde' of "gekloonde."

Gebruik alleen Azure virtuele machines voor de DC's (in plaats van Azure 'web' of 'worker'-rol virtuele machines). Ze zijn duurzame en duurzaamheid van de status voor een domeincontroller is vereist. Virtuele machines in Azure zijn ontworpen voor werkbelastingen zoals DC's.

Gebruik geen SYSPREP om te implementeren of klonen van domeincontrollers. De mogelijkheid om te kopiëren van DC's is alleen beschikbaar vanaf Windows Server 2012. De functie voor klonen vereist ondersteuning voor VMGenerationID in de onderliggende hypervisor. Hyper-V in Windows Server 2012 en Azure virtuele netwerken beide VMGenerationID, ondersteunen evenals softwareleveranciers virtualisatie van derden.

### <a name="BKMK_PlaceDB"></a>Plaatsing van de Windows Server AD DS-database en SYSVOL
Selecteer waar u de Windows Server AD DS-database, logboekbestanden en SYSVOL. Ze moeten worden geïmplementeerd op Azure gegevensschijven.

> [!NOTE]
> Azure-schijven die gegevens zijn beperkt tot 1 TB.
> 
> 

Schijfstations voor gegevens niet cache schrijfbewerkingen standaard gebeurt. Schijfstations voor gegevens die zijn gekoppeld aan een virtuele machine gebruik write-through cachebewerkingen. Write-through caching zorgt ervoor dat het schrijven naar wordt doorgevoerd duurzame Azure storage voordat de transactie voltooid vanuit het perspectief van het besturingssysteem van de VM is. Het biedt duurzaamheid ten koste van iets langer schrijfbewerkingen.

Dit is belangrijk voor Windows Server AD DS write-behind schijfcache ongeldig veronderstellingen van de domeincontroller. Windows Server AD DS probeert om uit te schakelen schrijfcache maar het is tot de schijf-i/o-systeem te voldoen aan deze. Fout uitschakelen schrijfcache kan, onder bepaalde omstandigheden, leiden tot USN-terugdraaiacties waardoor achtergebleven objecten en andere problemen.

Als een best practice voor virtuele DC's, het volgende doen:

* De voorkeursinstelling Host Cache ingesteld op de Azure-gegevensschijf voor NONE. Hiermee voorkomt u problemen met schrijfcache voor AD DS-bewerkingen.
* Opslaan van de database, logboekbestanden en SYSVOL op beide dezelfde gegevens of afzonderlijke gegevensschijven. Dit is meestal een andere schijf da de schijf die wordt gebruikt voor het besturingssysteem zelf. De belangrijkste takeaway is dat de Windows Server AD DS-database en SYSVOL, moeten niet worden opgeslagen op een type besturingssysteem van de Azure-schijf. Deze onderdelen worden in de AD DS-installatieproces standaard geïnstalleerd in de map % systemroot %, wat niet wordt aanbevolen voor Azure.

### <a name="BKMK_BUR"></a>Back-up en herstel
Zich bewust zijn van wat is en wordt niet ondersteund voor back-up en herstel van een domeincontroller in het algemeen en meer specifiek, die worden uitgevoerd in een virtuele machine. Zie [back-up en herstellen van de overwegingen voor gevirtualiseerde DC's](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Systeem systeemstatusback-ups met alleen back-upsoftware die specifiek bewust zijn van de back-eisen voor Windows Server AD DS, zoals Windows Server back-up maken.

Kopieer of klonen VHD-bestanden van DC's in plaats van het uitvoeren van regelmatige back-ups niet. Een terugzetbewerking moet ooit zijn vereist, doet, met behulp van de gekloonde of gekopieerde virtuele harde schijven zonder Windows Server 2012 en een ondersteunde hypervisor leidt tot USN-bellen.

### <a name="BKMK_FedSrvConfig"></a>Configuratie van de Federation-server
De configuratie van Windows Server AD FS-federatieservers (STSs) is gedeeltelijk afhankelijk of de toepassingen die u wilt implementeren in Azure nodig hebt voor toegang tot bronnen op uw on-premises netwerk.

Als de toepassingen aan de volgende criteria voldoen, kunt u de toepassingen in isolatie kan implementeren vanuit uw on-premises netwerk.

* Ze accepteren SAML beveiligingstokens
* Ze zijn exposable met het Internet
* Ze geen toegang krijgen tot lokale bronnen

In dit geval wordt de Windows Server AD FS STSs als volgt configureren:

1. Een geïsoleerde forest met één domein configureren in Azure.
2. Bieden federatieve toegang krijgen tot aan het forest met het configureren van een Windows Server AD FS-federatieserverfarm.
3. Windows Server AD FS (federatieserverfarm en federatieve serverproxyfarm) configureren in het lokale forest.
4. Een federatieve vertrouwensrelatie opzetten tussen on-premises en Azure-exemplaren van Windows Server AD FS.

Anderzijds, als de toepassingen toegang tot lokale bronnen vereisen, kan u Windows Server AD FS configureren met de toepassing op Azure als volgt:

1. Connectiviteit tussen on-premises netwerken en Azure configureren.
2. Een Windows Server AD FS-federatieserverfarm configureren in het lokale forest.
3. Een Windows Server AD FS federatieve serverproxyfarm configureren in Azure.

Deze configuratie heeft het voordeel van vermindering van de blootstelling van lokale bronnen, vergelijkbaar met het configureren van Windows Server AD FS met toepassingen in een perimeternetwerk.

Houd er rekening mee dat in beide scenario's, u vertrouwensrelaties relaties met meer id-providers instellen kunt als business-to-business samenwerking nodig is.

### <a name="BKMK_CloudSvcConfig"></a>Configuratie voor cloud-services
Cloudservices zijn nodig als u wilt weergeven van een virtuele machine rechtstreeks met het Internet of om een toepassing internetgerichte Netwerktaakverdeling weer te geven. Dit is mogelijk omdat elke cloudservice één configureerbare virtuele IP-adres biedt.

### <a name="BKMK_FedReqVIPDIP"></a>Federatieve server-vereisten voor openbare en particuliere IP-adressering (dynamische IP versus virtuele IP)
Elke virtuele machine van Azure ontvangt een dynamische IP-adres. Een dynamische IP-adres is een persoonlijk adres toegankelijk zijn alleen binnen Azure. In de meeste gevallen echter moeten voor het configureren van een virtueel IP-adres voor uw Windows Server AD FS-implementaties. Het virtuele IP-adres is nodig om Windows Server AD FS-eindpunten met het Internet weer te geven en door federatieve partners en -clients wordt gebruikt voor verificatie en het doorlopende beheer. Een virtueel IP-adres is een eigenschap van een cloudservice waarin een of meer virtuele machines in Azure. Als de claimbewuste toepassing geïmplementeerd op Azure en Windows Server AD FS internetgerichte zowel share algemene poorten, zal elk een virtueel IP-adres van de eigen is vereist en deze daarom moet u een cloudservice voor de toepassing en een tweede maken voor Windows Server AD FS.

Zie voor definities van de voorwaarden virtueel IP-adres en een dynamisch IP-adres [termen en definities](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-configuratie voor hoge beschikbaarheid
Hoewel het mogelijk om zelfstandige Windows Server AD FS federatieservices te implementeren, wordt het aanbevolen voor het implementeren van een farm met ten minste twee knooppunten voor AD FS-STS- en proxy's voor productieomgevingen.

Zie [aandachtspunten voor AD FS 2.0 implementatie topologie](https://technet.microsoft.com/library/gg982489) in de [AD FS 2.0-ontwerphandleiding](https://technet.microsoft.com/library/dd807036) om te bepalen welke configuratie implementatieopties aanbevolen aan de behoeften van uw specifieke behoeften.

> [!NOTE]
> Alle leden van de Windows Server AD FS-farm configureren in dezelfde cloudservice om op te halen voor de load balancer voor Windows Server AD FS-eindpunten op Azure, en gebruik van de load balancing mogelijkheden van Azure voor HTTP (standaard 80) en de HTTPS-poorten (standaard 443). Zie voor meer informatie [Azure load balancer-test](https://msdn.microsoft.com/library/azure/jj151530).
> Windows Server Network Load Balancing (NLB) wordt niet ondersteund in Azure.
> 
> 

