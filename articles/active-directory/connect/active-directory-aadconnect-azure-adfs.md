---
title: Active Directory Federation Services in Azure | Microsoft Docs
description: In dit document leert u hoe u AD FS implementeert in Azure voor hoge beschikbaarheid.
keywords: AD FS implementeren in azure, azure adfs implementeren, azure adfs, azure ad fs, adfs implementeren, ad fs implementeren, adfs in azure, adfs implementeren in azure, AD FS implementeren in azure, adfs azure, introductie tot AD FS, Azure, AD FS in Azure, iaas, ADFS, adfs verplaatsen naar azure
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddd29a1230286de8999175498ee793f3b3ea24e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Active Directory Federation Services in Azure implementeren
AD FS biedt vereenvoudigde, beveiligde identiteitsfederatie en mogelijkheden voor eenmalige webaanmelding (SSO of Single Sign-on). Federatie met Azure AD of O365 biedt gebruikers de mogelijkheid om zich te verifiëren met on-premises referenties en toegang te krijgen tot alle bronnen in de cloud. Daarom is het echter wel heel belangrijk dat u beschikt over een maximaal beschikbare AD FS-infrastructuur voor toegang tot zowel on-premises resources als resources in de cloud. De implementatie van AD FS in Azure kan helpen met minimale inspanningen de vereiste hoge beschikbaarheid te bewerkstelligen.
De implementatie van AD FS in Azure heeft verschillende voordelen, waaronder de volgende:

* **Hoge beschikbaarheid**: met de kracht van Azure-beschikbaarheidssets kunt u een maximaal beschikbare infrastructuur garanderen.
* **Eenvoudig te schalen**: hebt u betere prestaties nodig? U kunt met slechts enkele muisklikken in Azure migreren naar krachtigere machines
* **Grensoverschrijdende redundantie**: met de geografische redundantie van Azure kunt u er zeker van zijn dat uw infrastructuur maximaal beschikbaar is over de hele wereld
* **Gemakkelijk te beheren**: met de sterk vereenvoudigde beheeropties in Azure Portal is het beheer van uw infrastructuur zeer eenvoudig en probleemloos 

## <a name="design-principles"></a>Ontwerpprincipes
![Implementatie-ontwerp](./media/active-directory-aadconnect-azure-adfs/deployment.png)

In het bovenstaande diagram ziet u de aanbevolen basistopologie om de AD FS-infrastructuur in Azure te implementeren. Hieronder worden de beginselen van de verschillende onderdelen van de topologie toegelicht:

* **DC-/ADFS-servers**: als u minder dan 1000 gebruikers hebt, kunt u gewoon de AD FS-rol op uw domeincontrollers installeren. Als u geen prestatie-invloed op de domeincontrollers wilt of als u meer dan 1000 gebruikers hebt, implementeer AD FS dan op afzonderlijke servers.
* **WAP-server**: u moet webtoepassingsproxyservers implementeren, zodat gebruikers de AD FS ook kunnen bereiken wanneer ze zich niet op het bedrijfsnetwerk bevinden.
* **DMZ**: de webtoepassingsproxyservers worden geplaatst in de DMZ en TCP/443-toegang is alleen toegestaan tussen de DMZ en het interne subnet.
* **Load balancers**: voor hoge beschikbaarheid van AD FS en webtoepassingsproxyservers wordt geadviseerd om een interne load balancer voor AD FS-servers te gebruiken en Azure Load Balancer voor webtoepassingsproxyservers.
* **Beschikbaarheidssets**: voor redundantie voor uw AD FS-implementatie wordt aanbevolen dat u twee of meer virtuele machines in een beschikbaarheidsset voor vergelijkbare werkbelastingen groepeert. Deze configuratie zorgt ervoor dat tijdens een geplande of onvoorziene onderhoudsgebeurtenis ten minste één virtuele machine beschikbaar is
* **Opslagaccounts**: twee opslagaccounts worden aanbevolen. Het gebruik van één opslagaccount kan leiden tot het ontstaan van één potentieel risico waardoor de implementatie niet meer beschikbaar is in het onwaarschijnlijke scenario waarin het opslagaccount uitvalt. Met twee opslagaccounts kan er één opslagaccount worden gekoppeld aan elke storingslijn.
* **Netwerkscheiding**: implementeer webtoepassingsproxyservers in een afzonderlijk DMZ-netwerk. U kunt één virtueel netwerk verdelen in twee subnetten en de webtoepassingsproxyserver(s) in een geïsoleerd subnet implementeren. U kunt gewoon de instellingen voor de netwerkbeveiligingsgroep voor elk subnet configureren en alleen de vereiste communicatie tussen de twee subnetten toestaan. Meer informatie vindt u hieronder per implementatiescenario

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Stappen voor het implementeren van AD FS in Azure
De stappen die in deze sectie worden beschreven, vormen een handleiding voor de implementatie van de hieronder beschreven AD FS-infrastructuur in Azure.

### <a name="1-deploying-the-network"></a>1. Het netwerk implementeren
Zoals hierboven is beschreven, kunt u twee subnetten in één virtueel netwerk maken, maar ook twee volledig verschillende virtuele netwerken (VNets). Dit artikel gaat over de implementatie van één virtueel netwerk en de verdeling daarvan in twee subnetten. Dit is momenteel een gemakkelijkere benadering, omdat u voor twee afzonderlijke VNets een VNet naar een VNet-gateway nodig hebt voor communicatie.

**1.1 Virtueel netwerk maken**

![Virtueel netwerk maken](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)

Als u in Azure Portal Virtueel netwerk selecteert, kunt u met één klik onmiddellijk het virtuele netwerk en één subnet implementeren. Er wordt ook een INT-subnet gedefinieerd en u kunt daar nu virtuele machines aan toevoegen.
In de volgende stap gaat u nog een subnet aan het netwerk toevoegen, dat wil zeggen het DMZ-subnet. Als u het DMZ-subnet wilt maken:

* Selecteer het zojuist gemaakte netwerk
* Selecteer het subnet in de eigenschappen
* Klik in het deelvenster van het subnet op de knop Toevoegen
* Geef de naam en adresruimte van het subnet op om het subnet te maken

![Subnet](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)

![Subnet-DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. De netwerkbeveiligingsgroepen maken**

Een netwerkbeveiligingsgroep (NSG) bevat een lijst met ACL-regels (Access Control List, toegangsbeheerlijst) waarmee netwerkverkeer voor uw VM-exemplaren in een virtueel netwerk wordt toegestaan of geweigerd. NSG's kunnen worden gekoppeld aan subnetten of afzonderlijke VM-exemplaren in dat subnet. Als een NSG is gekoppeld aan een subnet, zijn de ACL-regels van toepassing op alle VM-exemplaren in dat subnet.
Voor deze richtlijnen maken we twee NSG's: één voor een intern netwerk en één voor een DMZ. Ze worden respectievelijk NSG_INT en NSG_DMZ genoemd.

![NSG’s maken](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Na het maken van de NSG’s zijn er geen regels voor binnenkomende of uitgaande verbindingen. Zodra de rollen op de desbetreffende servers zijn geïnstalleerd en geactiveerd, kunnen de regels voor binnenkomende en uitgaande verbindingen worden gemaakt volgens het gewenste beveiligingsniveau.

![NSG’s initialiseren](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Na het maken van de NSG's koppelt u NSG_INT aan INT-subnet en NSG_DMZ aan DMZ-subnet. Hieronder ziet u een schermafbeelding van het voorbeeld:

![NPS configureren](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klik op Subnetten om het deelvenster voor subnetten te openen
* Selecteer het subnet dat u aan de NSG wilt koppelen 

Na de configuratie ziet het paneel voor subnetten er als volgt uit:

![Subnetten na NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Verbinding maken met on-premises**

U hebt een verbinding met on-premises nodig om de domeincontroller (DC) in Azure te kunnen implementeren. Azure biedt verschillende connectiviteitsopties om verbinding te maken met de on-premises infrastructuur van uw Azure-infrastructuur.

* Punt-naar-site
* Virtueel netwerk site-naar-site
* ExpressRoute

ExpressRoute heeft de voorkeur. Met ExpressRoute kunt u particuliere verbindingen maken tussen Azure-datacenters en infrastructuur on-premises of in een co-locatie-omgeving. ExpressRoute-verbindingen gaan niet via het openbare internet. Dat maakt ze betrouwbaarder en sneller, en geeft ze lagere latenties en een betere beveiliging dan gewone verbindingen via internet.
Hoewel ExpressRoute wordt aanbevolen, kunt u elke verbindingsmethode kiezen die het meest geschikt is voor uw organisatie. Zie [Technisch overzicht van ExpressRoute](https://aka.ms/Azure/ExpressRoute) voor meer informatie over ExpressRoute en de verschillende connectiviteitsopties met ExpressRoute.

### <a name="2-create-storage-accounts"></a>2. Opslagaccount maken
Als u een hoge beschikbaarheid wilt behouden en afhankelijkheid van één opslagaccount wilt vermijden, kunt u twee opslagaccounts maken. Verdeel de machines in elke beschikbaarheidsset in twee groepen en wijs vervolgens aan elke groep een afzonderlijk opslagaccount toe.

![Opslagaccount maken](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Beschikbaarheidssets maken
Maak voor elke rol (DC/AD FS en WAP) beschikbaarheidssets die minimaal twee machines bevatten. Hiermee kunt u een hogere beschikbaarheid voor elke rol bereiken. Bij het maken van de beschikbaarheidssets is het essentieel dat u de volgende zaken bepaalt:

* **Foutdomeinen**: virtuele machines in hetzelfde foutdomein delen dezelfde voedingsbron en fysieke netwerkswitch. Minimaal twee foutdomeinen worden aanbevolen. De standaardwaarde is 3. U kunt dit voor deze implementatie zo laten staan
* **Updatedomeinen**: machines die tot hetzelfde updatedomein behoren, worden tijdens een update gelijktijdig opnieuw opgestart. U moet minimaal twee updatedomeinen hebben. De standaardwaarde is 5. U kunt dit voor deze implementatie zo laten staan

![Beschikbaarheidssets](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Maak de volgende beschikbaarheidssets

| Beschikbaarheidsset | Rol | Foutdomeinen | Updatedomeinen |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Virtuele machines implementeren
In de volgende stap gaat u de virtuele machines implementeren waarop de verschillende rollen in uw infrastructuur worden uitgevoerd. De aanbevolen configuratie is minimaal twee machines per beschikbaarheidsset. Maak voor de basisimplementatie vier virtuele machines.

| Machine | Rol | Subnet | Beschikbaarheidsset | Storage-account | IP-adres |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Statisch |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Statisch |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statisch |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statisch |

Zoals u misschien al hebt gezien, is er geen NSG opgegeven. Dit komt omdat u in Azure NSG op het subnetniveau kunt gebruiken. Daarna kunt u het netwerkverkeer op de machine beheren met behulp van de afzonderlijke NSG die is gekoppeld aan het subnet of het NIC-object. Zie [Wat is een netwerkbeveiligingsgroep (NSG)?](https://aka.ms/Azure/NSG) voor meer informatie.
Als u de DNS beheert, wordt een statisch IP-adres aanbevolen. U kunt Azure DNS gebruiken en dan in de DNS-records voor uw domein naar de nieuwe machines verwijzen met hun Azure FQDN's.
Wanneer de implementatie is voltooid, moet het deelvenster van de virtuele machine er ongeveer als volgt uitzien:

![Virtuele machines geïmplementeerd](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. De domeincontroller/AD FS-servers configureren
 Om inkomende aanvragen te kunnen verifiëren, moet AD FS contact opnemen met de domeincontroller. Om de kostbare reis van Azure naar on-premises DC voor verificatie te besparen, wordt u geadviseerd om een replica van de domeincontroller in Azure te implementeren. Voor hoge beschikbaarheid doet u er verstandig aan een beschikbaarheidsset van ten minste twee domeincontrollers te maken.

| Domeincontroller | Rol | Storage-account |
|:---:|:---:|:---:|
| contosodc1 |Replica |contososac1 |
| contosodc2 |Replica |contososac2 |

* Wijzig het niveau van de twee servers in replicadomeincontrollers bij DNS
* Configureer de AD FS-servers door met serverbeheer de AD FS-rol te installeren.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Interne load balancer (ILB) implementeren
**6.1. De ILB maken**

U implementeert een ILB door in Azure Portal Load Balancers te selecteren en op Toevoegen (+) te klikken.

> [!NOTE]
> Als **Load Balancers** niet in het menu wordt weergegeven, klikt u in de linkerbenedenhoek van de portal op **Bladeren** en bladert u tot u **Load Balancers** ziet.  Klik vervolgens op de gele ster om deze optie toe te voegen aan het menu. Selecteer nu het nieuwe pictogram van de load balancer om het deelvenster te openen en de load balancer te configureren.
> 
> 

![Bladeren naar load balancer](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Naam**: geef de load balancer een passende naam
* **Schema**: omdat deze load balancer vóór de AD FS-servers wordt geplaatst en alleen voor interne netwerkverbindingen is bedoeld, selecteert u Intern
* **Virtueel netwerk**: kies het virtuele netwerk waarin u de AD FS gaat implementeren
* **Subnet**: kies hier het interne subnet
* **IP-adrestoewijzing**: statisch

![Interne load balancer](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)

Wanneer u op Maken klikt en de ILB is geïmplementeerd, wordt deze weergegeven in de lijst met load balancers:

![Load balancers na ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)

In de volgende stap configureert u de back-endpool en de back-endtest.

**6.2. ILB-back-endpool configureren**

Selecteer de zojuist gemaakte ILB in het deelvenster Load Balancers. Het instellingenvenster wordt geopend. 

1. Back-endpools selecteren in het instellingenvenster
2. Klik in het deelvenster Back-endpool toevoegen op Virtuele machine toevoegen
3. In het volgende deelvenster kunt u een beschikbaarheidsset kiezen
4. Kies de beschikbaarheidsset AD FS

![ILB-back-endpool configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)

**6.3. Test configureren**

Selecteer Tests in het instellingenvenster ILB-instellingen.

1. Klik op Toevoegen
2. Geef details op voor test a. **Naam**: naam van test b. **Protocol**: TCP c. **Poort**: 443 (HTTPS) d. **Interval**: 5 (standaardwaarde). Dit is het interval waarmee de machines in de back-endpool e door ILB worden getest. **Drempelwaarde voor onjuiste status**: 2 (standaardwaarde). Dit is het aantal opeenvolgende mislukte tests waarna ILB een virtuele machine in de back-endpool beschouwt als niet-reagerend en geen verkeer meer naar die machine verzendt.

![Test ILB configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)

**6.4. Taakverdelingsregels maken**

Voor een doeltreffende verdeling van het verkeer moet de ILB worden geconfigureerd met taakverdelingsregels. Als u een regel voor taakverdeling wilt maken: 

1. Selecteer Taakverdelingsregel in het instellingendeelvenster van de ILB
2. Klik in het deelvenster Taakverdelingsregel op Toevoegen
3. In het deelvenster Taakverdelingsregel toevoegen a. **Naam**: geef een naam op voor de regel b. **Protocol**: selecteer TCP c. **Poort**: 443 d. **Back-endpoort**: 443 e. **Back-endpool**: selecteer de pool die u eerder voor het AD FS-cluster hebt gemaakt f. **Test**: selecteer de test die u eerder voor de AD FS-servers hebt gemaakt

![ILB-taakverdelingsregels configureren](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. DNS bijwerken met ILB**

Ga naar de DNS-server en maak een CNAME voor de ILB. Dit moet de CNAME zijn voor de federatieservice met het IP-adres dat verwijst naar het IP-adres van de ILB. Als het IP-adres van de ILB bijvoorbeeld 10.3.0.8 is en de geïnstalleerde federatieservice fs.contoso.com is, maakt u een CNAME voor fs.contoso.com die naar 10.3.0.8 verwijst.
Op die manier komt alle communicatie met betrekking tot fs.contoso.com terecht bij de ILB en wordt deze correct gerouteerd.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. De webtoepassingsproxyserver configureren
**7.1. De webtoepassingsproxyservers configureren om de AD FS-servers te bereiken**

U moet ervoor zorgen dat de webtoepassingsproxyservers de AD FS-servers achter de ILB kunnen bereiken. Daarom maakt u in de map %systemroot%\system32\drivers\etc\hosts een record voor de ILB. De DN-naam (Distinguished Name) moet de naam van de federatieservice zijn, bijvoorbeeld fs.contoso.com. De IP-vermelding moet het IP-adres van de ILB zijn (in het voorbeeld is dit 10.3.0.8).

**7.2. De webtoepassingsproxyrol installeren**

Nadat u ervoor hebt gezorgd dat de webtoepassingsproxyservers de AD FS-servers achter de ILB kunnen bereiken, kunt u de webtoepassingsproxyservers installeren. Webtoepassingsproxyservers hoeven niet te worden toegevoegd aan het domein. Installeer de webtoepassingsproxyrollen op de twee webtoepassingsproxyservers door de rol Externe toegang te selecteren. Serverbeheer leidt u door de WAP-installatie.
Zie [De webtoepassingsproxyserver installeren en configureren](https://technet.microsoft.com/library/dn383662.aspx) voor meer informatie over het implementeren van WAP.

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  De internetgerichte (openbare) load balancer implementeren
**8.1.  Internetgerichte (openbare) load balancer maken**

Selecteer Load Balancers in Azure Portal en klik op Toevoegen. Voer in het deelvenster Load Balancer maken de volgende informatie in

1. **Naam**: naam voor de load balancer
2. **Schema**: Openbaar. Met deze optie geeft u aan dat de load balancer een openbaar adres nodig heeft.
3. **IP-adres**: maak een nieuw IP-adres (dynamisch)

![Internetgerichte load balancer](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Na implementatie wordt de load balancer weergegeven in de lijst met load balancers.

![Lijst met load balancers](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)

**8.2. Een DNS-label toewijzen aan het openbare IP-adres**

Klik op de vermelding van de zojuist gemaakte load balancer in het deelvenster Load Balancers om het deelvenster voor configuratie te openen. Volg onderstaande stappen om het DNS-label voor het openbare IP-adres te configureren:

1. Klik op het openbare IP-adres. Hiermee opent u het deelvenster voor het openbare IP-adres en de bijbehorende instellingen
2. Klik op Configuratie
3. Geef een DNS-label op. Dit wordt het openbare DNS-label waartoe u vanaf elke locatie toegang hebt, bijvoorbeeld contosofs.westus.cloudapp.azure.com. U kunt in de externe DNS een vermelding voor de federatieservice toevoegen (zoals fs.contoso.com). Deze wordt omgezet in het DNS-label van de externe load balancer (contosofs.westus.cloudapp.azure.com).

![Internetgerichte load balancer configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Internetgerichte load balancer (DNS) configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Back-endpool voor internetgerichte (openbare) load balancer configureren** 

Volg dezelfde stappen als voor het maken van de interne load balancer om de back-endpool voor de internetgerichte (openbare) load balancer als beschikbaarheidsset voor de WAP-servers te configureren. Bijvoorbeeld: contosowapset.

![Back-endpool voor internetgerichte (openbare) load balancer configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)

**8.4. Test configureren**

Volg dezelfde stappen als voor het configureren van de interne load balancer om de test voor de back-endpool van de WAP-servers te configureren.

![Test voor internetgerichte load balancer configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)

**8.5. Taakverdelingsregel(s) maken**

Volg dezelfde stappen als in ILB om de taakverdelingsregel voor TCP 443 te configureren.

![Taakverdelingsregels van internetgerichte load balancer configureren](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Het netwerk beveiligen
**9.1. Het interne subnet beveiligen**

Over het algemeen hebt u de volgende regels nodig om het interne subnet doeltreffend te beveiligen (in de hieronder vermelde volgorde)

| Regel | Beschrijving | Stroom |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Staat HTTPS-communicatie vanuit DMZ toe |Inkomend |
| DenyInternetOutbound |Geen toegang tot internet |Uitgaand |

![INT-toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[opmerking]: <> (![INT-toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [opmerking]: <> (![INT-toegangsregels (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))

**9.2. Het DMZ-subnet beveiligen**

| Regel | Beschrijving | Stroom |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Staat HTTPS van internet naar de DMZ toe |Inkomend |
| DenyInternetOutbound |Alles behalve HTTPS naar internet wordt geblokkeerd |Uitgaand |

![EXT-toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[opmerking]: <> (![EXT-toegangsregels (inkomend)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [opmerking]: <> (![EXT-toegangsregels (uitgaand)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

> [!NOTE]
> Als verificatie met certificaat van clientgebruiker (clientTLS-verificatie met X509-gebruikerscertificaten) is vereist, vereist AD FS vervolgens dat TCP-poort 49443 voor inkomende toegang wordt ingeschakeld.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. De aanmelding bij AD FS testen
De gemakkelijkste manier om AD FS te testen, is met de pagina IdpInitiatedSignon.aspx. Daartoe moet IdpInitiatedSignOn in de eigenschappen van AD FS worden ingeschakeld. Volg onderstaande stappen om uw AD FS-installatie te controleren

1. Voer onderstaande cmdlet met PowerShell uit op de AD FS-server om deze in te schakelen.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. Ga vanaf een externe computer naar https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3. De volgende AD FS-pagina moet worden weergegeven:

![Aanmeldingspagina testen](./media/active-directory-aadconnect-azure-adfs/test1.png)

Bij een geslaagde aanmelding wordt een soortgelijk positief bericht weergegeven:

![Test geslaagd](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Sjabloon voor de implementatie van AD FS in Azure
De sjabloon implementeert een installatie voor 6 machines, 2 elk voor domeincontrollers, AD FS en WAP.

[Implementatiesjabloon voor AD FS in Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

U kunt een bestaand virtueel netwerk gebruiken of een nieuw VNET maken tijdens het implementeren van deze sjabloon. De verschillende parameters die beschikbaar zijn voor het aanpassen van de implementatie worden hieronder vermeld met de beschrijving van het gebruik van de parameter in het implementatieproces. 

| Parameter | Beschrijving |
|:--- |:--- |
| Locatie |De regio voor het implementeren van de resources, bijvoorbeeld VS - oost. |
| StorageAccountType |Het type opslagaccount dat wordt gemaakt |
| VirtualNetworkUsage |Geeft aan of een nieuw virtueel netwerk wordt gemaakt of een bestaand wordt gebruikt |
| VirtualNetworkName |De naam van het virtuele netwerk dat wordt gemaakt, verplicht voor gebruik van zowel een bestaand als nieuw virtueel netwerk |
| VirtualNetworkResourceGroupName |De naam van de resourcegroep waarin het bestaande virtuele netwerk zich bevindt Wanneer u een bestaand virtueel netwerk gebruikt, wordt dit een verplichte parameter zodat de implementatie de ID van het bestaande virtuele netwerk kan vinden |
| VirtualNetworkAddressRange |Het adresbereik van de nieuwe VNET, verplicht als u een nieuw virtueel netwerk maakt |
| InternalSubnetName |De naam van het interne subnet, verplicht voor beide soorten opties voor virtueel netwerkgebruik (nieuw of bestaand) |
| InternalSubnetAddressRange |Het adresbereik van het interne subnet, dat de domeincontrollers en AD FS-servers bevat, verplicht als u een nieuw virtueel netwerk maakt |
| DMZSubnetAddressRange |Het adresbereik van het DMZ-subnet, dat de Windows-proxytoepassingsservers bevat, verplicht als u een nieuw virtueel netwerk maakt |
| DMZSubnetName |De naam van het interne subnet, verplicht voor beide soorten opties voor virtueel netwerkgebruik (nieuw of bestaand) |
| ADDC01NICIPAddress |Het interne IP-adres van de eerste domeincontroller. Dit IP-adres wordt statisch toegewezen aan de domeincontroller en moet een geldig IP-adres binnen het interne subnet zijn |
| ADDC02NICIPAddress |Het interne IP-adres van de tweede domeincontroller. Dit IP-adres wordt statisch toegewezen aan de domeincontroller en moet een geldig IP-adres binnen het interne subnet zijn |
| ADFS01NICIPAddress |Het interne IP-adres van de eerste ADFS-server. Dit IP-adres wordt statisch toegewezen aan de ADFS-server en moet een geldig IP-adres binnen het interne subnet zijn |
| ADFS02NICIPAddress |Het interne IP-adres van de tweede ADFS-server. Dit IP-adres wordt statisch toegewezen aan de ADFS-server en moet een geldig IP-adres binnen het interne subnet zijn |
| WAP01NICIPAddress |Het interne IP-adres van de eerste WAP-server. Dit IP-adres wordt statisch toegewezen aan de WAP-server en moet een geldig IP-adres binnen het DMZ-subnet zijn |
| WAP02NICIPAddress |Het interne IP-adres van de tweede WAP-server. Dit IP-adres wordt statisch toegewezen aan de WAP-server en moet een geldig IP-adres binnen het DMZ-subnet zijn |
| ADFSLoadBalancerPrivateIPAddress |Het interne IP-adres van de ADFS load balancer. Dit IP-adres wordt statisch toegewezen aan de load balancer en moet een geldig IP-adres binnen het interne subnet zijn |
| ADDCVMNamePrefix |Naamvoorvoegsel van virtuele machine voor domeincontrollers |
| ADFSVMNamePrefix |Naamvoorvoegsel van virtuele machine voor ADFS-servers |
| WAPVMNamePrefix |Naamvoorvoegsel van virtuele machine voor WAP-servers |
| ADDCVMSize |De VM-grootte van de domeincontrollers |
| ADFSVMSize |De VM-grootte van de AD FS-servers |
| WAPVMSize |De VM-grootte van de WAP-servers |
| AdminUserName |De naam van de lokale beheerder van de virtuele machines |
| AdminPassword |Het wachtwoord van de lokale beheerdersaccount van de virtuele machines |

## <a name="additional-resources"></a>Aanvullende bronnen
* [Beschikbaarheidssets](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Interne load balancer](https://aka.ms/Azure/ILB/Internal)
* [Internetgerichte load balancer](https://aka.ms/Azure/ILB/Internet)
* [Opslagaccounts](https://aka.ms/Azure/Storage)
* [Virtuele netwerken van Azure](https://aka.ms/Azure/VNet)
* [Koppelingen voor AD FS en webtoepassingsproxy](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Volgende stappen
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
* [De AD FS configureren en beheren met Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [AD FS-implementaties in meerdere regio’s in Azure, met maximale beschikbaarheid dankzij Azure Traffic Manager](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)

