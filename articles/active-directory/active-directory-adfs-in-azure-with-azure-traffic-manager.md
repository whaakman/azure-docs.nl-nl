---
title: "AD FS-implementaties in meerdere regio’s in Azure, met maximale beschikbaarheid dankzij Azure Traffic Manager | Microsoft Docs"
description: In dit document leert u hoe u AD FS implementeert in Azure voor hoge beschikbaarheid.
keywords: Ad fs met Azure traffic manager, adfs met Azure Traffic Manager, geografisch, multi-datacenter, geografische datacenters, multi-geografische datacenters, AD FS implementeren in azure, azure adfs implementeren, azure adfs, azure ad fs, adfs implementeren, ad fs implementeren, adfs in azure, adfs implementeren in azure, AD FS implementeren in azure, adfs azure, introductie tot AD FS, Azure, AD FS in Azure, iaas, ADFS, adfs verplaatsen naar azure
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 077710049894d2690299ce0fcb0ead9911aa4bb6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>AD FS-implementaties in meerdere regio’s in Azure, met maximale beschikbaarheid dankzij Azure Traffic Manager
[AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md) biedt stapsgewijze richtlijnen over het implementeren van een eenvoudige AD FS-infrastructuur voor uw organisatie in Azure. In dit artikel vindt u de volgende stappen voor het maken van een implementatie van AD FS in Azure in meerdere regio’s met [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Met Azure Traffic Manager kunt u geografisch verspreide AD FS-infrastructuur met een hoge beschikbaarheid en uitstekende prestaties maken voor uw organisatie. Hiervoor maakt u gebruikt van de verschillende routeringsmethoden die beschikbaar zijn om aan verschillende eisen te voldoen voor de infrastructuur.

Met een maximaal beschikbare AD FS-infrastructuur in meerdere regio’s kunt u:

* **Storingen door één fout elimineren:** dankzij de mogelijkheden voor failover van Azure Traffic Manager kunt u een maximaal beschikbare AD FS-infrastructuur creëren. Deze blijft zelfs beschikbaar wanneer één van de datacenters ergens ter wereld uitvalt
* **Verbeterde prestaties:** u kunt de voorgestelde implementatie in dit artikel gebruiken om een uitmuntende AD FS-infrastructuur te maken waardoor gebruikers sneller kunnen worden geverifieerd. 

## <a name="design-principles"></a>Ontwerpprincipes
![Algemeen ontwerp](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

De algemene ontwerpprincipes zijn hetzelfde als de principes die worden vermeld in Ontwerpprincipes in het artikel AD FS-implementatie in Azure. In het bovenstaande diagram ziet u een eenvoudige uitbreiding van de basisimplementatie naar een andere geografische regio. Hieronder ziet u elke punten om te overwegen bij het uitbreiden van uw implementatie naar een nieuwe geografische regio

* **Virtueel netwerk:** u moet een nieuw virtueel netwerk maken in de geografische regio waarin u een aanvullende AD FS-infrastructuur wilt implementeren. In het bovenstaande diagram ziet u Geo1 VNET en Geo2 VNET als de twee virtuele netwerken in elke geografische regio.
* **Domeincontrollers en AD FS-servers in nieuw geografisch VNET:** het wordt aanbevolen om domeincontrollers te implementeren in de nieuwe geografische regio, zodat de AD FS-servers in de nieuwe regio geen contact op hoeven te nemen met een domeincontroller in een netwerk dat ver weg ligt om een verificatie te voltooien. Daardoor worden de prestaties beter.
* **Opslagaccounts:** opslagaccounts worden gekoppeld aan een regio. Omdat u machines in een nieuwe geografische regio gaat implementeren, moet u nieuwe opslagaccounts maken om te gebruiken in die regio.  
* **Netwerkbeveiligingsgroepen:** net als opslagaccounts kunnen netwerkbeveiligingsgroepen die in een bepaalde regio zijn gemaakt, niet in een andere geografische regio worden gebruikt. U moet daarom nieuwe netwerkbeveiligingsgroepen maken voor het INT- en DMZ-subnet die lijken op de groepen in de eerste geografische regio.
* **DNS-labels voor openbare IP-adressen:** Azure Traffic Manager kan ALLEEN via DNS-labels verwijzen naar eindpunten. U moet daarom DNS-labels maken voor de openbare IP-adressen van de externe load balancers.
* **Azure Traffic Manager:** Met Microsoft Azure Traffic Manager kunt u de distributie van gebruikersverkeer naar uw service-eindpunten beheren dat wordt uitgevoerd in verschillende datacenters overal ter wereld. Azure Traffic Manager werkt op DNS-niveau. Er wordt gebruikgemaakt van DNS-antwoorden om verkeer van eindgebruikers door te sturen naar wereldwijd gedistribueerde eindpunten. Clients maken vervolgens rechtstreeks verbinding met deze eindpunten. Met de routeringsopties Prestaties, Gewogen en Prioriteit kunt u eenvoudig de routeringsoptie kiezen die het beste aansluit op de behoeften van uw organisatie. 
* **Connectiviteit tussen virtuele netwerken in twee regio’s:** er hoeft geen connectiviteit te zijn tussen de virtuele netwerken zelf. Omdat elk virtueel netwerk toegang heeft tot domeincontrollers en beschikt over een AD FS- en WAP-server, kunnen virtuele netwerken zonder connectiviteit ertussen wél functioneren in verschillende regio’s. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Stappen voor het integreren van Azure Traffic Manager
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>AD FS implementeren in de nieuwe geografische regio
Volg de stappen en richtlijnen in [AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md) voor het implementeren van dezelfde topologie in de nieuwe geografische regio.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>DNS-labels voor openbare IP-adressen van de openbare, internetgerichte load balancers
Zoals eerder vermeld, kan Azure Traffic Manager alleen verwijzen naar DNS-labels als eindpunt. Daarom is het belangrijk om DNS-labels te maken voor de openbare IP-adressen van de externe load balancers. In de schermafbeelding hieronder ziet u hoe u uw DNS-label kunt configureren voor het openbare IP-adres. 

![DNS-label](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Azure Traffic Manager implementeren
Volg de onderstaande stappen om een Traffic Manager-profiel te maken. Zie [Een Azure Traffic Manager-profiel beheren](../traffic-manager/traffic-manager-manage-profiles.md) voor meer informatie.

1. **Een Traffic Manager-profiel maken:** geef uw Traffic Manager-profiel een unieke naam. Deze naam van het profiel maakt deel uit van de DNS-naam en fungeert als voorvoegsel voor het Traffic Manager-domeinnaamlabel. De naam/het voorvoegsel wordt toegevoegd aan .trafficmanager.net om een DNS-label te maken voor uw Traffic Manager. In de onderstaande schermafbeelding ziet u hoe het DNS-voorvoegsel van Traffic Manager wordt ingesteld als mysts. Het resulterende DNS-label wordt dan mysts.trafficmanager.net. 
   
    ![Een Traffic Manager-profiel maken](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Routeringsmethode voor verkeer:** er zijn drie routeringsopties beschikbaar in Traffic Manager:
   
   * Prioriteit 
   * Prestaties
   * Gewogen
     
     **prestaties** is de aanbevolen optie om een zeer responsieve AD FS-infrastructuur te creëren. U kunt echter kiezen voor de routeringsmethode die het beste aansluit op uw implementatiebehoeften. De geselecteerde routeringsoptie is niet van invloed op de AD FS-functionaliteit. Zie [Verkeersrouteringsmethoden voor Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md) voor meer informatie. In de voorbeeld schermafbeelding hierboven ziet u dat de methode **Prestaties** is geselecteerd.
3. **Eindpunten configureren:** op de Traffic Manager-pagina klikt u op eindpunten en selecteert u Toevoegen. De pagina Eindpunt toevoegen wordt dan geopend; die lijkt op de onderstaande schermafbeelding
   
   ![Eindpunten configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Volg de onderstaande richtlijn voor de verschillende soorten invoer:
   
   **Type:** selecteer het Azure-eindpunt omdat er wordt verwezen naar een openbaar Azure-IP-adres.
   
   **Naam:** geef de naam op die u wilt koppelen aan het eindpunt. Dit is niet de DNS-naam en de naam heeft geen gevolgen voor de DNS-records.
   
   **Doelresourcetype:** selecteer Openbaar IP-adres als waarde van deze eigenschap. 
   
   **Doelresource:** hiermee krijgt u de optie om te kiezen uit de verschillende DNS-labels die beschikbaar zijn voor uw abonnement. Kies het DNS-label dat overeenkomt met het eindpunt dat u configureert.
   
   Voeg een eindpunt toe voor elke geografische regio waarvan u wilt dat Azure Traffic Manager er verkeer naar routeert.
   Zie [Eindpunten toevoegen, uitschakelen, inschakelen of verwijderen](../traffic-manager/traffic-manager-endpoints.md) voor meer informatie en gedetailleerde stappen over het toevoegen/configureren van eindpunten in Traffic Manager
4. **Test configureren:** klik op de pagina Traffic Manager op Configuratie. Op de pagina Configuratie moet u de bewakingsinstellingen wijzigen voor tests bij HTTP-poort 80 en de/het bijbehorende pad/AD FS/test
   
    ![Test configureren](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Controleer of de status van de eindpunten ONLINE is nadat de configuratie is voltooid**. Als alle eindpunten een 'verminderde' status hebben, doet Azure Traffic Manager een poging om het verkeer te routeren, ervan uitgaande dag de diagnostische gegevens onjuist zijn en alle eindpunten bereikbaar zijn.
   > 
   > 
5. **DNS-records wijzigen voor Azure Traffic Manager:** uw Federation Service moet een CNAME zijn die verwijst naar de DNS-naam van Azure Traffic Manager. Maak een CNAME in de openbare DNS-records zodat iedereen die de Federation Service probeert te bereiken, Azure Traffic Manager ook daadwerkelijk bereikt.
   
    Als u de Federation Service fs.fabidentity.com bijvoorbeeld wilt doorverwijzen naar Traffic Manager, moet u uw DNS-resourcerecord bijwerken naar het volgende:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>De routering en aanmelding bij AD FS testen
### <a name="routing-test"></a>Routeringstest
Een voorbeeld van een eenvoudige routeringstest is het pingen van de DNS-naam van de Federation Service via één computer in elke geografische regio. Afhankelijk van de gekozen routeringsmethode wordt het eindpunt dat wordt gepingd weergegeven in het pingscherm. Als u bijvoorbeeld de routeringsoptie Prestaties hebt geselecteerd, wordt het eindpunt dat het dichtst bij de regio van de client ligt, bereikt. Hieronder ziet u een schermafbeelding van twee pings van twee clientapparaten in verschillende regio’s: één in Oost-Azië en één in het westen van de Verenigde Staten. 

![Routeringstest](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS-aanmeldingstest
De gemakkelijkste manier om AD FS te testen, is met de pagina IdpInitiatedSignon.aspx. Daartoe moet IdpInitiatedSignOn in de eigenschappen van AD FS worden ingeschakeld. Volg onderstaande stappen om uw AD FS-installatie te controleren

1. Voer onderstaande cmdlet met PowerShell uit op de AD FS-server om deze in te schakelen. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Ga vanaf een externe computer naar https://<yourfederationservicedns>adfs/ls/IdpInitiatedSignon.aspx
3. De volgende AD FS-pagina moet worden weergegeven:
   
    ![AD FS-test - verificatievraag](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    Bij een geslaagde aanmelding wordt een soortgelijk positief bericht weergegeven:
   
    ![AD FS-test - verificatie geslaagd](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>Verwante koppelingen
* [AD FS-implementatie in Azure](active-directory-aadconnect-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [Traffic Manager traffic routing methods](../traffic-manager/traffic-manager-routing-methods.md) (Verkeersrouteringsmethoden van Traffic Manager)

## <a name="next-steps"></a>Volgende stappen
* [Azure Traffic Manager-profielen beheren](../traffic-manager/traffic-manager-manage-profiles.md)
* [Eindpunten toevoegen, uitschakelen, inschakelen of verwijderen](../traffic-manager/traffic-manager-endpoints.md) 

