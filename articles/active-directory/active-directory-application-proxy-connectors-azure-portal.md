---
title: Publicatie van toepassingen op afzonderlijke netwerken en locaties met behulp van groepen van de connector in Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over het maken en beheren van groepen van connectors in Azure AD-toepassingsproxy.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: H1Hack27Feb2017; it-pro
ms.openlocfilehash: 7c409e5419a2bd393cb5c7ec265e44310f7e9027
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publiceren van toepassingen op afzonderlijke netwerken en locaties met groepen van de connector

Klanten gebruikmaken van Azure AD-toepassingsproxy voor meer scenario's en toepassingen. Dus hebben we aangebracht toepassingsproxy zelfs flexibelere doordat meer topologieën. U kunt Application Proxy connector groepen maken, zodat u specifieke connectors voor specifieke toepassingen kunt toewijzen. Deze mogelijkheid biedt u meer controle en manieren om uw Application Proxy-implementatie te optimaliseren. 

Elke Application Proxy connector is toegewezen aan een groep connector. Alle connectors die deel uitmaken van dezelfde groep verbindingslijn fungeren als een eenheid voor hoge beschikbaarheid en taakverdeling. Alle connectors deel uitmaken van de groep van een connector. Als u geen groepen maakt, vervolgens zijn alle verbindingslijnen in een standaard-groep. De beheerder kan nieuwe groepen maken en connectors aan ze toewijzen in Azure portal. 

Alle toepassingen zijn toegewezen aan de groep van een connector. Als u geen groepen maakt, zijn al uw toepassingen toegewezen aan een standaard-groep. Maar als u uw connectors in groepen indelen, kunt u elke toepassing werkt met een specifieke connector groep instellen. In dit geval wordt fungeren alleen de connectors in die groep voor de toepassing op verzoek. Deze functie is handig als uw toepassingen worden gehost op verschillende locaties. Kunt u connector groepen op basis van locatie, zodat toepassingen altijd door de connectors die fysiek dicht bij hen worden behandeld.

>[!TIP] 
>Als u een grote Application Proxy-implementatie hebt, niet alle toepassingen aan de standaardgroep connector toewijzen. Op die manier nieuwe connectors niet live verkeer niet ontvangen totdat u ze aan een groep active-connector toewijzen. Deze configuratie kunt u connectors plaatsen in een niet-actieve modus door te terug verplaatsen naar de standaardgroep, zodat voor het uitvoeren van onderhoud zonder enige impact op uw gebruikers.

## <a name="prerequisites"></a>Vereisten
Als u wilt groeperen uw connectors, die u hebt om te controleren of u [geïnstalleerd meerdere connectors](active-directory-application-proxy-enable.md). Wanneer u een nieuwe connector installeert, wordt automatisch lid wordt van de **standaard** connector groep.

## <a name="create-connector-groups"></a>Connector-groepen maken
Volg deze stappen om net zoveel connector groepen als u wilt maken. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **Azure Active Directory** > **bedrijfstoepassingen** > **toepassingsproxy**.
2. Selecteer **nieuwe connector groep**. De blade nieuwe Connector groep wordt weergegeven.

   ![Selecteer de nieuwe connector-groep](./media/active-directory-application-proxy-connectors-azure-portal/new-group.png)

3. Geef een naam op voor de nieuwe connector-groep en vervolgens het vervolgkeuzemenu gebruiken om te selecteren welke connectors in deze groep behoren.
4. Selecteer **Opslaan**.

## <a name="assign-applications-to-your-connector-groups"></a>Toepassingen voor de connector-groepen toewijzen
Volg deze stappen voor elke toepassing die u hebt gepubliceerd met toepassingsproxy. Wanneer u deze publiceren of kunt u deze stappen om te wijzigen van de toewijzing wanneer u wilt, kunt u een toepassing aan een groep connector toewijzen.   

1. Selecteer in het dashboard management voor uw directory **bedrijfstoepassingen** > **alle toepassingen** > de toepassing die u wilt toewijzen aan een groep connector > **Toepassingsproxy**.
2. Gebruik de **Connector groep** vervolgkeuzemenu selecteren de groep die u wilt dat de toepassing te gebruiken.
3. Selecteer **opslaan** de wijziging toepassen.

## <a name="use-cases-for-connector-groups"></a>Gebruiksvoorbeelden voor connector-groepen 

Connector-groepen zijn handig voor verschillende scenario's, waaronder:

### <a name="sites-with-multiple-interconnected-datacenters"></a>Sites met meerdere onderling verbonden datacenters

Veel organisaties hebben een aantal onderling verbonden datacenters. In dit geval u zoveel verkeer binnen het datacenter mogelijk behouden omdat cross-datacenter koppelingen dure en traag zijn. U kunt implementeren connectors in elk datacenter te bedienen alleen de toepassingen die zich in het datacenter bevinden. Deze aanpak cross-datacenter koppelingen wordt geminimaliseerd en een volledig transparant ervaring voor uw gebruikers.

### <a name="applications-installed-on-isolated-networks"></a>Toepassingen die zijn geïnstalleerd op de geïsoleerde netwerken

Toepassingen kunnen worden gehost in netwerken die geen deel uitmaken van de belangrijkste bedrijfsnetwerk. Connector-groepen kunt u specifieke connectors installeren op geïsoleerde netwerken ook om toepassingen te isoleren met het netwerk. Dit gebeurt meestal wanneer de leverancier van een derde partij een bepaalde toepassing is voor uw organisatie onderhoudt. 

Connector groepen kunnen u voor het installeren van speciale connectors voor netwerken die alleen specifieke toepassingen publiceren gemakkelijker en veiliger dan uitbesteden Toepassingsbeheer van externe leveranciers.

### <a name="applications-installed-on-iaas"></a>Toepassingen die op IaaS geïnstalleerd 

Toepassingen die zijn geïnstalleerd op IaaS voor toegang tot de cloud, bieden connector groepen een algemene service de toegang tot alle apps beveiligen. Groepen van de connector geen extra afhankelijkheid in uw bedrijfsnetwerk maken of fragment van de appervaring. Connectors kunnen worden geïnstalleerd op elke clouddatacenter en dienen alleen toepassingen die zich op dat netwerk bevinden. U kunt verschillende connectors om te zorgen voor hoge beschikbaarheid installeren.

Neem bijvoorbeeld een organisatie met verschillende virtuele machines die zijn verbonden met hun eigen IaaS gehost virtueel netwerk. Zodat werknemers van het gebruik van deze toepassingen worden deze particuliere netwerken zijn verbonden met het bedrijfsnetwerk via VPN van site-naar-site. Dit biedt een goede ervaring voor werknemers die zich op locatie zijn. Maar deze mogelijk niet ideaal voor externe werknemers, omdat hiervoor extra on-premises infrastructuur voor het routeren van toegang, zoals u in het onderstaande diagram kunt zien:

![AzureAD Iaas-netwerk](./media/application-proxy-publish-apps-separate-networks/application-proxy-iaas-network.png)
  
Met groepen van Azure AD-toepassingsproxy-connector, kunt u een algemene service de toegang tot alle toepassingen beveiligen zonder aanvullende afhankelijkheid in uw bedrijfsnetwerk inschakelen:

![AzureAD Iaas meerdere Cloud leveranciers](./media/application-proxy-publish-apps-separate-networks/application-proxy-multiple-cloud-vendors.png)

### <a name="multi-forest--different-connector-groups-for-each-forest"></a>Meerdere forests – verschillende connector groepen voor elke forest

De meeste klanten die hebben geïmplementeerd toepassingsproxy gebruikt de eenmalige aanmelding (SSO) mogelijk door het uitvoeren van Kerberos-beperkt delegatie (KCD). Om dit te bereiken, moeten de connector-machines worden gekoppeld aan een domein of de gebruikers naar de toepassing kan delegeren. KCD ondersteunt interforest-mogelijkheden. Maar voor de bedrijven die beschikken over verschillende omgevingen voor meerdere forests zonder vertrouwensrelatie tussen deze twee, één connector kan niet worden gebruikt voor alle forests. 

In dit geval kunnen specifieke connectors worden geïmplementeerd per forest en instellen voor toepassingen die zijn gepubliceerd om alleen de gebruikers van dat specifieke forest. Elke groep connector vertegenwoordigt een ander forest. Terwijl de tenant en het merendeel van de gebruikerservaring is unified voor alle forests, kunnen gebruikers worden toegewezen aan hun forest-toepassingen met behulp van Azure AD-groepen.
 
### <a name="disaster-recovery-sites"></a>Noodherstelsites

Er zijn twee verschillende methoden die u met een site disaster recovery (DR uitvoeren kunt), afhankelijk van hoe uw sites worden geïmplementeerd:

* Als uw site DR is ingebouwd in de actieve-actieve modus waar het is net als de primaire site en heeft de dezelfde netwerken en de AD-instellingen, kunt u de connectors op de site DR in dezelfde groep connector als de primaire site. Dit kan Azure AD failovers voor u gedetecteerd.
* Als uw site DR gescheiden van de primaire site is, u een andere connector-groep in de DR-site maken kunt en 1) back-uptoepassingen laat of 2) handmatig de bestaande toepassing aan de groep van de connector DR omleiden naar behoefte.
 
### <a name="serve-multiple-companies-from-a-single-tenant"></a>Meerdere bedrijven leveren van een enkele tenant

Er zijn veel verschillende manieren voor het implementeren van een model waarin een één-provider worden geïmplementeerd en onderhoudt Azure AD-gerelateerde services voor meerdere bedrijven. Connector-groepen zodat de beheerder scheiden connectors en verschillende groepen van toepassingen. Een manier die geschikt voor kleine bedrijven, is een enkel Azure AD-tenant, terwijl de andere bedrijven hun eigen domeinnaam en netwerken hebben te beschikken. Dit geldt ook voor fusies en scenario's en situaties waarbij een enkel IT-afdeling verschillende bedrijven vanwege regelgeving of zakelijke redenen fungeert. 

## <a name="sample-configurations"></a>Voorbeelden van configuraties

Enkele voorbeelden die u kunt implementeren, zijn de volgende connector-groepen.
 
### <a name="default-configuration--no-use-for-connector-groups"></a>Standaard-configuratie geen nut voor connector-groepen

Als u geen connector groepen gebruikt, wordt de configuratie eruit als volgt:

![AzureAD geen Connector-groepen](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-1.png)
 
Deze configuratie is geschikt voor kleine implementaties en tests. Werkt ook goed als uw organisatie een platte netwerktopologie heeft.
 
### <a name="default-configuration-and-an-isolated-network"></a>Standaardconfiguratie en een geïsoleerd netwerk

Deze configuratie is een van de standaard één, waarin een specifieke app die wordt uitgevoerd in een geïsoleerd netwerk zoals IaaS virtuele netwerk is: 

![AzureAD geen Connector-groepen](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-2.png)
 
### <a name="recommended-configuration--several-specific-groups-and-a-default-group-for-idle"></a>Aanbevolen configuratie – enkele specifieke groepen en een standaardgroep voor inactiviteit

De aanbevolen configuratie voor organisaties met grotere, complexe is om de connector standaardgroep als een groep die geen nut heeft voor alle toepassingen en wordt gebruikt voor niet-actief of geïnstalleerde connectors. Alle toepassingen worden geleverd met aangepaste connector groepen. Hierdoor kan de complexiteit van de scenario's die hierboven worden beschreven.

In het onderstaande voorbeeld van heeft het bedrijf twee datacentra, A en B, met twee connectors die dienen van elke site. Elke site heeft verschillende toepassingen die worden uitgevoerd op deze. 

![AzureAD geen Connector-groepen](./media/application-proxy-publish-apps-separate-networks/application-proxy-sample-config-3.png)
 
## <a name="next-steps"></a>Volgende stappen

* [Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)
* [Eenmalige aanmelding inschakelen](application-proxy-sso-overview.md)


