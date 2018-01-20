---
title: Aan de slag met Azure Automation | Microsoft Docs
description: Dit artikel bevat een overzicht van Azure Automation-service. Deze controleert de details van het ontwerp en de implementatie in voorbereiding voor de voorbereiding op het aanbod van Azure Marketplace.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: d6ee5c35ce9866f6106c7b5dbc51599b666c3eb1
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/20/2018
---
# <a name="get-started-with-azure-automation"></a>Aan de slag met Azure Automation

Dit artikel bevat de belangrijkste concepten die betrekking hebben op de implementatie van Azure Automation. Als u niet bekend met automatisering in Azure bent of ervaring met automation werkstroom software, zoals System Center Orchestrator hebben, kunt u informatie over het voorbereiden en vrijgeven Automation. Nadat u dit artikel hebt gelezen, kunt u zult gereed om te beginnen met het ontwikkelen van runbooks ter ondersteuning van uw automatiseringsbehoeften proces. 


## <a name="automation-architecture-overview"></a>Overzicht van Automation-architectuur

![Overzicht van Azure Automation](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Automation is een software als een dienst (SaaS)-toepassing die voorziet in een schaalbare en betrouwbare multitenant-omgeving waarin u kunt runbooks automatiseren processen. U kunt Desired State Configuration (DSC) gebruiken in Azure, met andere cloudservices of in een on-premises omgeving voor het beheren van configuratie die is gewijzigd in Windows en Linux-systemen. Entiteiten in uw Automation-account, met inbegrip van runbooks, assets en Run As-accounts, zijn geïsoleerd van andere Automation-accounts in uw abonnement en van andere abonnementen.  

Runbooks die u in Azure uitvoert worden uitgevoerd op de Automation-sandboxes geladen. Sandboxes worden gehost in Azure-platform als een service (PaaS) virtuele machines. 

Automation-sandboxes bieden isolatie van tenants voor alle aspecten van de runbook-uitvoering, inclusief modules, opslag, geheugen, de netwerkcommunicatie en taak stromen. Deze rol wordt beheerd door de service. U geen toegang tot of het beheren van de functie van uw Azure of Automation-account.         

Voor het automatiseren van de implementatie en beheer van bronnen in uw lokale datacentrum of andere cloudservices nadat u een Automation-account hebt gemaakt, kunt u een of meer virtuele machines uitvoeren aanwijzen de [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) rol. Elke Hybrid Runbook Worker vereist Microsoft Management Agent moet worden geïnstalleerd en een Automation-account. De agent moet zijn verbonden met een Azure-logboekanalyse-werkruimte. U kunt Log Analytics gebruiken voor de installatie van de bootstrap onderhouden van Microsoft Management Agent en bewaken van de functionaliteit van de hybride Runbook Worker. Azure Automation voert de levering van runbooks en de aanwijzingen om te worden uitgevoerd.

U kunt meerdere Hybrid Runbook Workers kunt implementeren. Met hybride Runbook Workers gebruiken voor maximale beschikbaarheid voor uw runbooks en taakverdeling runbooktaken. In sommige gevallen kunt u runbooktaken voor specifieke werkbelastingen of omgevingen reserveren. Microsoft Monitoring Agent op de hybride Runbook Worker initieert communicatie met de Automation-service via TCP-poort 443. Met hybride Runbook Workers hebt geen binnenkomende firewallvereisten.  

U kunt een runbook dat wordt uitgevoerd op een hybride Runbook Worker beheertaken kunnen uitvoeren op basis van andere machines of services in uw omgeving. In dit scenario, kan het runbook ook toegang nodig tot andere poorten. Als de beleidsregels van uw IT-beveiliging niet toestaan van computers in uw netwerk verbinding maken met internet, bekijkt u [OMS Gateway](../log-analytics/log-analytics-oms-gateway.md). De Gateway Operations Management Suite (OMS) fungeert als proxy voor de hybride Runbook Worker. Deze verzamelt taakstatus en ontvangt informatie over de configuratie van uw Automation-account.

Runbooks die worden uitgevoerd op een hybride Runbook Worker worden uitgevoerd in de context van het lokale systeemaccount op de computer. Het is raadzaam een beveiligingscontext wanneer u beheertaken op de lokale Windows-computer uitvoeren. Als u wilt dat het runbook aan taken uitvoeren op basis van bronnen die zich buiten de lokale computer, moet u mogelijk beveiligde referentieassets definiëren in het Automation-account. U kunt toegang krijgen tot beveiligde referentieassets van het runbook en deze gebruiken voor verificatie met de externe bron. U kunt [referentie](automation-credentials.md), [certificaat](automation-certificates.md), en [verbinding](automation-connections.md) activa in uw runbook. Gebruik de activa met cmdlets die u gebruiken kunt om op te geven van de referenties voor deze verificatie.

U kunt toepassen DSC-configuraties die zijn opgeslagen in Azure Automation aan virtuele machines. Andere fysieke en virtuele machines kunnen configuraties van aanvragen van de Automation DSC-pull-server. U hoeft niet te implementeren van een infrastructuur voor de ondersteuning van de Automation DSC-pull-server voor het beheren van configuraties van uw on-premises fysieke of virtuele Windows- en Linux systemen. U hoeft alleen uitgaande toegang tot internet vanaf elk systeem dat u beheren wilt met behulp van Automation DSC. Communicatie vindt plaats via TCP-poort 443 met de OMS-service.   

## <a name="prerequisites"></a>Vereisten

### <a name="automation-dsc"></a>Automation DSC
Automation DSC kunt u deze computers beheren:

* Virtuele machines in Azure (klassiek) met Windows of Linux.
* Virtuele machines in Azure waarop Windows of Linux wordt uitgevoerd.
* Amazon Web Services (AWS) virtuele machines waarop Windows of Linux wordt uitgevoerd.
* Fysieke en virtuele Windows-computers die zich on-premises of in een cloud dan Azure of AWS.
* Fysieke en virtuele Linux-computers die zich on-premises of in een cloud dan Azure of AWS.

Voor Windows-machines, moet de meest recente versie van Windows Management Framework (WMF) 5 worden geïnstalleerd. Voor Linux-machines, de nieuwste versie van de [PowerShell DSC-agent voor Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) moet worden geïnstalleerd. De PowerShell DSC-agent gebruikt WMF 5 om te communiceren met Automation. 

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
Wanneer u een computer worden uitgevoerd van hybride runbooktaken instelt, moet de computer aan de volgende vereisten voldoen:

* WindowsServer 2012 of later.
* Windows PowerShell 4.0 of hoger. Voor een hogere mate van betrouwbaarheid, wordt aangeraden Windows PowerShell 5.0. U kunt [downloaden van de nieuwe versie](https://www.microsoft.com/download/details.aspx?id=50395) van het Microsoft Download Center.
* .NET framework 4.6.2 of hoger.
* Minimaal twee kernen.
* Een minimum van 4 GB RAM-geheugen.

### <a name="permissions-required-to-create-an-automation-account"></a>Vereiste machtigingen voor het maken van een Automation-account
Maken of bijwerken van een Automation-account en voer de taken die worden beschreven in dit artikel, moet u de volgende rechten en machtigingen hebben:   
 
* Voor het maken van een Automation-account, moet uw gebruikersaccount van Azure Active Directory (Azure AD) worden toegevoegd aan een rol met de machtigingen die equivalent zijn aan de rol van eigenaar voor **Microsoft.Automation** resources. Zie voor meer informatie [toegangsbeheer op basis van rollen in Azure Automation](automation-role-based-access-control.md).  
* In de Azure-portal onder **Azure Active Directory** > **beheren** > **App registraties**als **App registraties**  is ingesteld op **Ja**, kunnen gebruikers niet-beheerders in uw Azure AD-tenant [Active Directory-toepassingen registreren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Als **App registraties** is ingesteld op **Nee**, moet de gebruiker die deze actie uitvoert een globale beheerder zijn in Azure AD. 

Als u niet lid zijn van de Active Directory-exemplaar van het abonnement voordat u aan het abonnement globale beheerder/CO-rol worden toegevoegd, zijn u als Gast toegevoegd aan Active Directory. In dit scenario wordt dit bericht wordt weergegeven op de **Automation-Account toevoegen** pagina: "U bent niet gemachtigd om te maken." 

Als een gebruiker is toegevoegd aan de rol globale beheerder/CO-eerst, kunt u uit de Active Directory-exemplaar van het abonnement verwijderen en vervolgens opnieuw toe te voegen aan de volledige gebruikersrol in Active Directory.

Gebruikersrollen controleren:
1. In de Azure portal, gaat u naar de **Azure Active Directory** deelvenster.
2. Selecteer **gebruikers en groepen**.
3. Selecteer **alle gebruikers**. 
4. Nadat u een specifieke gebruiker selecteert, selecteert u **profiel**. De waarde van de **gebruikerstype** kenmerk onder profiel van de gebruiker mag geen **Gast**.

## <a name="authentication-planning"></a>Plannen voor verificatie
U kunt taken op basis van bronnen die zich in Azure, on-premises en in andere cloudservices automatiseren in Azure Automation. Voor een runbook de vereiste acties uit te voeren, moet machtigingen voor veilige toegang tot de resources hebben. De minimale rechten die vereist zijn binnen het abonnement moet hebben.  

### <a name="what-is-an-automation-account"></a>Wat is een Automation-account 
Alle automatiseringstaken die u op resources uitvoert met behulp van de cmdlets in Azure Automation verifiëren naar Azure met behulp van verificatie op basis van referenties op de identiteit van de organisatie van Azure AD.  Een Automation-account is gescheiden van het account waarmee u zich aanmeldt bij de portal voor het configureren en gebruiken van Azure-resources. 

De volgende resources zijn opgenomen in een Automation-account:

* **Certificaten**. Een certificaat dat wordt gebruikt voor verificatie van een runbook of de DSC-configuratie bevat. U kunt ook certificaten toevoegen.
* **Verbindingen**. Verificatie- en configuratie-informatie die nodig is om verbinding maken met een externe service of toepassing vanuit een runbook of de DSC-configuratie bevat.
* **Referenties**. Bevat een **PSCredential** -object met beveiligingsgegevens zoals een gebruikersnaam en wachtwoord. De referenties zijn nodig voor het verifiëren van een runbook of de DSC-configuratie.
* **Integratiemodules**. PowerShell-modules die opgenomen in een Automation-account zijn. Gebruik de PowerShell-modules voor het uitvoeren van cmdlets in runbooks en DSC-configuraties.
* **Planningen**. Schema's starten of stoppen van een runbook op een opgegeven periode, met inbegrip van terugkerende frequenties bevat.
* **Variabelen**. Bevatten waarden die beschikbaar zijn vanuit een runbook of de DSC-configuratie.
* **DSC-configuraties**. PowerShell-scripts die een beschrijving van het configureren van een besturingssysteem functie of de instelling of het installeren van een toepassing op een Windows- of Linux-computer.  
* **Runbooks**. Een set taken waarmee een geautomatiseerd proces in Automation op basis van Windows PowerShell.    

Automation-resources voor elk Automation-account zijn gekoppeld aan één Azure-regio. U kunt echter Automation-accounts gebruiken voor het beheren van alle resources in uw abonnement. U kunt Automation-accounts in verschillende regio's maken als u te maken hebt met beleidsregels die bepalen dat gegevens en resources moeten worden geïsoleerd in een specifieke regio.

Wanneer u een Automation-account in de Azure portal maakt, worden twee verificatie entiteiten worden automatisch gemaakt:

* **Run As-account**. Dit account heeft de volgende taken:
  - Hiermee maakt een service-principal in Azure AD.
  - Een certificaat gemaakt.
  - Wijst de Contributor Role-Based toegangsbeheer (RBAC), die Azure Resource Manager-resources met behulp van runbooks beheert.
* **Klassieke Run As-account**. Dit account uploadt een beheercertificaat. Het certificaat wordt gebruikt voor het klassieke resources beheren met behulp van runbooks.

RBAC is beschikbaar met Resource Manager toe te kennen toegestane acties naar een Azure AD-gebruikersaccount en Run As-account. U kunt ook gebruikmaken van RBAC voor verificatie van deze service-principal. Zie voor meer informatie, en hulp bij het ontwikkelen van een model voor het beheren van machtigingen in Automation, [toegangsbeheer op basis van rollen in Azure Automation-artikel](automation-role-based-access-control.md).  

#### <a name="authentication-methods"></a>Verificatiemethoden
De volgende tabel geeft een overzicht van de verificatiemethoden die u kunt gebruiken voor elke omgeving die ondersteuning biedt voor Azure Automation.

| Methode | Omgeving 
| --- | --- | 
| Uitvoeren als-account van Azure en klassiek Uitvoeren als-account |Azure Resource Manager en klassieke Azure-implementatie |  
| Azure AD-gebruikersaccount |Azure Resource Manager en klassieke Azure-implementatie |  
| Windows-verificatie |Lokale datacenter of andere cloud services-provider met behulp van de functie Hybride Runbook Worker |  
| Amazon Web Services-referenties |Amazon Web Services |  

De volgende artikelen bevatten een overzicht en de implementatie stappen voor het configureren van verificatie voor deze omgevingen. De artikelen worden beschreven met behulp van een bestaande en het gebruik van een nieuw account die u wilt reserveren voor deze omgeving. 
* [Een zelfstandige Azure Automation-account maken](automation-create-standalone-account.md)
* [Runbooks verifiëren met de klassieke Azure-implementatie en het Resource Manager](automation-create-aduser-account.md)
* [Runbooks met Amazon webservices verifiëren](automation-config-aws-account.md)
* [Automation Run As-account bijwerken](automation-create-runas-account.md)

Voor de Azure uitvoeren als en klassieke Run As-accounts, [bijwerken Automation Run As-account](automation-create-runas-account.md) wordt beschreven hoe u uw bestaande Automation-account bijwerken met het Run As-accounts vanuit de portal. Ook wordt beschreven hoe u PowerShell als het Automation-account is niet oorspronkelijk is geconfigureerd met een Run As- of klassieke Run As-account wordt gebruikt. U kunt een Run As-account en een klassieke Run As-account maken met behulp van een certificaat dat uitgegeven door uw enterprise-certificeringsinstantie (CA). Bekijk [Automation Update die Run As-account](automation-create-runas-account.md) voor informatie over het maken van de accounts met behulp van deze configuratie.     
 
## <a name="network-planning"></a>Plan uw netwerk
Voor de hybride Runbook Worker verbinding maken met en registreren bij OMS moet hebben toegang tot het poortnummer en de URL's die in deze sectie worden beschreven. Dit is in aanvulling op de [poorten en URL's die zijn vereist voor Microsoft Monitoring Agent](../log-analytics/log-analytics-windows-agent.md) wilt verbinden met OMS. 

Als u een proxyserver voor communicatie tussen de agent en de OMS-service gebruikt, moet u ervoor dat de juiste resources toegankelijk zijn. Als u een firewall gebruikt toegang tot het internet te beperken, moet u uw firewall om toegang te verlenen.

De volgende URL's en -poort zijn vereist voor de Hybrid Runbook Worker-rol om te communiceren met Automation:

* Poort: Alleen TCP 443 is vereist voor uitgaande toegang tot internet.
* Globale URL: *.azure-automation.net.

Als u een Automation-account dat gedefinieerd voor een bepaald gebied hebt, kunt u de communicatie met dat regionale datacenter kunt beperken. De volgende tabel bevat de DNS-record voor elke regio.

| **Regio** | **DNS-record** |
| --- | --- |
| Zuid-centraal VS |scus-jobruntimedata-prod-su1.azure-automation.net |
| VS - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| West-centraal VS | wcus-jobruntimedata-prod-su1.azure-automation.net |
| West-Europa |we-jobruntimedata-prod-su1.azure-automation.net |
| Noord-Europa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Canada - midden |cc-jobruntimedata-prod-su1.azure-automation.net |
| Zuidoost-Azië |sea-jobruntimedata-prod-su1.azure-automation.net |
| Centraal-India |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japan - oost |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australië - zuidoost |ase-jobruntimedata-prod-su1.azure-automation.net |
| Verenigd Koninkrijk Zuid | uks-jobruntimedata-prod-su1.azure-automation.net |
| VS (overheid) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Voor een lijst met regio IP-adressen in plaats van de regionamen, downloadt u de [Azure Datacenter IP-adres](https://www.microsoft.com/download/details.aspx?id=41653) XML-bestand van het Microsoft Download Center. 

> [!NOTE]
> Het Azure Datacenter IP-adres XML-bestand bevat de IP-adresbereiken die worden gebruikt in de Microsoft Azure-datacenters. COMPUTE, SQL en opslag bereiken zijn opgenomen in het bestand. 
>
>Een bijgewerkt bestand is wekelijks geplaatst. Het bestand weerspiegelt de huidige geïmplementeerde bereiken en toekomstige wijzigingen in de IP-adresbereiken. Nieuwe bereiken die worden weergegeven in het bestand worden niet gebruikt in de datacentra voor ten minste één week. 
>
> Er is een goed idee om te downloaden van het nieuwe XML-bestand per week. Werk vervolgens, uw site correct services identificeren die worden uitgevoerd in Azure. Azure ExpressRoute gebruikers Houd er rekening mee dat dit bestand moet worden gebruikt voor het bijwerken van de Border Gateway Protocol (BGP)-aankondiging van Azure ruimte de eerste week van elke maand. 
> 

## <a name="creating-an-automation-account"></a>Een Automation-account maken

De volgende tabel bevat de methoden voor het maken van een Automation-account in de Azure portal. De tabel wordt elk type implementatie-ervaring, en de verschillen beschreven.  

|Methode | Beschrijving |
|-------|-------------|
| Selecteer **Automation en Control** in de Azure Marketplace | Een Azure Marketplace-aanbieding maakt een Automation-account en de OMS-werkruimte die gekoppeld en in dezelfde resourcegroep en regio zijn. Integratie met OMS bevat ook het voordeel van logboekanalyse controleren en analyseren van runbook-taak status en taak streams gedurende een bepaalde periode. U kunt ook de geavanceerde functies in Log Analytics gebruiken om te escaleren of problemen onderzoeken. De aanbieding implementeert de **bijhouden** en **updatebeheer** oplossingen die zijn standaard ingeschakeld. |
| Selecteer **Automation** in de Marketplace | Deze methode maakt u een Automation-account in een nieuwe of bestaande resourcegroep die niet is gekoppeld aan een OMS-werkruimte. Alle beschikbare oplossingen van zijn niet opgenomen de **Automation en Control** aanbieden. Deze methode is een eenvoudige configuratie die u in Automation introduceert. Dit kunt u informatie over het schrijven van runbooks en DSC-configuraties en informatie over het gebruik van de mogelijkheden van de service. |
| Selecteer **Management** oplossingen | Als u selecteert een **Management** oplossing, inclusief [updatebeheer](../operations-management-suite/oms-solution-update-management.md), [starten/stoppen virtuele machines tijdens daluren](automation-solution-vm-management.md), of [bijhouden](../log-analytics/log-analytics-change-tracking.md), de oplossing voor u wordt gevraagd om een bestaand Automation-account en de OMS-werkruimte. De oplossing biedt u de optie voor het maken van een Automation-account en de OMS-werkruimte, zoals is vereist voor de oplossing worden geïmplementeerd in uw abonnement. |

### <a name="create-an-automation-account-thats-integrated-with-oms"></a>Een Automation-account die geïntegreerd met OMS maken
Bij vrijgeven Automation wordt aangeraden dat u selecteert de **Automation en Control** aanbieding in de Marketplace. Met deze methode maakt u een Automation-account en er wordt van de integratie met een OMS-werkruimte. Wanneer u deze methode gebruikt, hebt u ook de optie voor het installeren van de oplossingen die beschikbaar in de aanbieding zijn.  

[Een zelfstandige Automation-account maken](automation-create-standalone-account.md) wordt u begeleid bij het proces van het maken van een Automation-account en de OMS-werkruimte onboarding de **Automation en Control** aanbieden. U kunt informatie over het maken van Automation-account voor het testen van een zelfstandige of een voorbeeld van de service.  

Een Automation-account en de OMS-werkruimte maken met behulp van de **Automation en Control** Marketplace-aanbieding:

1. Aanmelden bij de Azure-portal met een account dat lid is van de rol van de beheerders abonnement en een CO-beheerder van het abonnement.
2. Selecteer **nieuwe**.<br><br> ![Selecteer Nieuw in de Azure-portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Zoeken naar **Automation**. Selecteer in de lijst met zoekresultaten **Automation en Control**.<br><br> ![Zoek en selecteer Automation & besturingselement in Azure Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   
4. Bekijk de beschrijving voor de aanbieding en selecteer vervolgens **maken**.  
5. Onder **Automation en Control**, selecteer **OMS-werkruimte**. Onder **OMS werkruimten**, selecteer een OMS-werkruimte die gekoppeld aan het Azure-abonnement dat u het Automation-account in. Als u een OMS-werkruimte niet hebt, selecteert u **nieuwe werkruimte maken**. Onder **OMS-werkruimte**: 
  1. Voor **OMS-werkruimte**, voer een naam voor de nieuwe werkruimte.
  2. Voor **abonnement**, selecteer een abonnement aan koppelen. Als de standaardselectie niet het abonnement dat u wilt gebruiken, schakelt u het abonnement uit de vervolgkeuzelijst.
  3. Voor **resourcegroep**, kunt u een resourcegroep maken of Selecteer een bestaande resourcegroep.  
  4. Voor **locatie**, selecteer een regio. Zie voor meer informatie [welke regio's Azure Automation is beschikbaar in](https://azure.microsoft.com/regions/services/). Oplossingen worden aangeboden in twee lagen: laag gratis en per knooppunt (OMS). Gratis laag heeft een limiet op de hoeveelheid gegevens die dagelijks, verzameld bewaarperiode en runbook-taak runtime minuten. De per knooppunt (OMS) laag niet hebben een limiet van de hoeveelheid gegevens die worden verzameld per dag.  
  5. Selecteer **Automation-account**.  Als u een nieuwe OMS-werkruimte maakt, moet u ook een Automation-account die is gekoppeld aan de nieuwe OMS-werkruimte maken. Uw Azure-abonnement, resourcegroep en de regio bevatten. 
    1. Selecteer **een Automation-account maken**.
    2. Onder **Automation-Account**, in de **naam** en voer de naam van het Automation-account.
    Alle andere opties worden automatisch ingevuld op basis van de OMS-werkruimte geselecteerd. U kunt deze opties niet wijzigen. 
    3. Een Uitvoeren als-account van Azure is de standaardmethode voor verificatie voor de aanbieding. Nadat u hebt geselecteerd **OK**, de configuratieopties die worden gevalideerd en het Automation-account is gemaakt. Selecteer om de voortgang, in het menu te volgen **meldingen**. 
    4. Selecteer anders een bestaand Automation Uitvoeren als-account. Het account dat u selecteert kan niet al zijn gekoppeld aan een andere OMS-werkruimte. Als het, wordt er een melding weergegeven. Als het account is al gekoppeld aan een OMS-werkruimte, selecteer een andere Automation Run As-account of een maken.
    5. Nadat u opgeven of de vereiste gegevens selecteren, selecteert u **maken**. De informatie is geverifieerd en de Automation-Account en Run As-accounts worden gemaakt. U keert automatisch terug naar de **OMS-werkruimte** deelvenster.  
6. Nadat u opgeven of selecteren van de vereiste informatie op de **OMS-werkruimte** deelvenster **maken**.  De informatie wordt gecontroleerd en de werkruimte is gemaakt. Selecteer om de voortgang, in het menu te volgen **meldingen**. U keert terug naar de **oplossing toevoegen** deelvenster.  
7. Onder **Automation en Control** instellingen u wilt bevestigen dat voor het installeren van de aanbevolen oplossingen die vooraf zijn geselecteerd. Als u een van de standaardopties wijzigen, kunt u de oplossingen later afzonderlijk installeren.  
8. Selecteer het volgende om door te gaan met de voorbereiding Automation en een OMS-werkruimte **maken**. Alle instellingen worden gevalideerd en probeert dan Azure voor het implementeren van de aanbieding in uw abonnement. Dit proces kan enkele seconden duren. Selecteer om de voortgang bijhouden, in het menu **meldingen**. 

Nadat de aanbieding vrijgegeven is, kunt u de volgende taken uitvoeren:
* Begin met het maken van runbooks.
* Werken met de oplossingen die u hebt ingeschakeld.
* Implementeer een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) rol.
* Beginnen met werken met [logboekanalyse](https://docs.microsoft.com/azure/log-analytics) voor het verzamelen van gegevens die worden gegenereerd door de bronnen in uw cloud of on-premises omgeving.   

## <a name="next-steps"></a>Volgende stappen
* Controleer of dat uw nieuwe Automation-account kan worden geverifieerd op basis van de Azure-resources. Zie voor meer informatie [Test Azure Automation uitvoeren als-accountverificatie](automation-verify-runas-authentication.md).
* Informatie over het aan de slag met het maken van runbooks en verwante overwegingen voordat u begint te ontwerpen. Zie voor meer informatie [Automation-runbooktypen](automation-runbook-types.md).


