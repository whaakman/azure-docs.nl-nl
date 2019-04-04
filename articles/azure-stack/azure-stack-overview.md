---
title: Wat is Azure Stack? | Microsoft Docs
description: Meer informatie over hoe u met Azure Stack kunt u voor het uitvoeren van Azure-services in uw datacenter.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/29/2019
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: ''
ms.lastreviewed: 03/29/2019
ms.openlocfilehash: e5e05ad4f2ae7e718e160916144f03bfb74a2a52
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631294"
---
# <a name="azure-stack-overview"></a>Overzicht van Azure Stack

Azure Stack is een uitbreiding van Azure waarmee u een manier voor het uitvoeren van toepassingen in een on-premises omgeving en Azure-services leveren in uw datacenter. Met een consistent platform, kunnen organisaties vol vertrouwen er beslissingen voor de technologie op basis van zakelijke vereisten, in plaats van zakelijke beslissingen op basis van technologie beperkingen.

## <a name="why-use-azure-stack"></a>Waarom Azure Stack gebruiken?

Azure biedt een uitgebreid platform voor ontwikkelaars om moderne toepassingen te bouwen. Sommige toepassingen cloud-gebaseerde geconfronteerd echter obstakels zoals latentie, onregelmatige connectiviteit en voorschriften. Azure en Azure Stack ontgrendelen van nieuwe hybride cloud use cases voor zowel klantgerichte en interne line-of-business-toepassingen:

- **Edge-oplossingen en oplossingen zonder verbinding**. Pak wachttijd- en connectiviteitsvereisten door gegevens lokaal in Azure Stack wordt verwerkt en vervolgens geaggregeerd voor verdere analyse, met algemene toepassingslogica via zowel in Azure. U kunt ook Azure Stack is niet verbonden met het internet zonder verbinding naar Azure implementeren. Beschouwen factory verdiepingen, blader wordt verzonden en Mijn schachten als voorbeelden.

- **Toepassingen die voldoen aan uiteenlopende regelgeving in de cloud**. Ontwikkel en implementeer toepassingen in Azure, met de volledige flexibiliteit on-premises met Azure Stack om te voldoen aan regelgeving of beleidsregels, zonder codewijzigingen nodig implementeren. Toepassing voorbeelden zijn onder meer globale controle, financiële rapportage, valuta trading, onlinegames en kostenrapportage.

- **Cloud-toepassing cloudtoepassingsmodel on-premises**. Azure-services, containers, serverloze, en microservice-architecturen gebruiken om te werken en bestaande toepassingen uitbreiden of nieuwe bouwen. Gebruik consistent DevOps-processen voor Azure in de cloud en Azure Stack on-premises om modernisering van toepassingen voor core, essentiële toepassingen te versnellen.

Azure Stack kunt deze scenario's voor gebruik door te geven:

- Een geïntegreerde delivery-ervaring aan de slag kunt en snel slag met speciaal ontwikkelde Azure Stack-geïntegreerde systemen van vertrouwde hardwarepartners. Na de levering, is Azure Stack eenvoudig worden geïntegreerd in het datacenter met het bewaken via de System Center Operations Manager Management Pack of Nagios-uitbreiding.

- Flexibele identiteitsbeheer met behulp van Azure Active Directory (Azure AD) voor Azure en Azure Stack-hybride omgevingen, en maakt gebruik van Active Directory Federation Services (AD FS) voor de verbinding verbroken implementaties. 

- Een Azure-consistente toepassingsontwikkelomgeving, zodat de productiviteit van ontwikkelaars te maximaliseren en algemene DevOps inschakelen benadert in hybride omgevingen.

- Azure-services-levering vanuit on-premises met hybride cloud rekenkracht. Algemene operationele procedures voor Azure en Azure Stack implementeren en gebruiken van Azure IaaS en PaaS-services met dezelfde administratieve ervaringen en hulpprogramma's als Azure vast. Microsoft biedt continue innovatie van Azure naar Azure Stack, met inbegrip van nieuwe Azure-services, updates voor Azure Marketplace-toepassingen en afbeeldingen voor bestaande services en meer.

## <a name="azure-stack-architecture"></a>Azure Stack-architectuur
Azure Stack geïntegreerde systemen bestaan in rekken van 4-16-servers die zijn gemaakt door vertrouwde hardwarepartners en geleverd rechtstreeks naar uw datacenter. Een solution provider zal na de levering samen met u de geïntegreerd systeem implementeren en ervoor zorgen dat de Azure Stack-oplossing voldoet aan uw zakelijke vereisten. U moet voorbereiden van uw datacenter door ervoor te zorgen dat alle vereiste voeding en koeling, border-connectiviteit en andere vereiste datacenter-integratie eis is voldaan. 

> Zie voor meer informatie over de Azure Stack datacenter-integratie-ervaring, [datacenter-integratie van Azure Stack](azure-stack-customer-journey.md).

Azure Stack is gebaseerd op de standaardverwerking van de bedrijfstak van de hardware en wordt beheerd met dezelfde hulpmiddelen die u al gebruikt voor het beheren van Azure-abonnementen. Als gevolg hiervan kunt u consistent DevOps-processen toepassen of u met Azure of niet verbonden bent. 

De Azure Stack-architectuur kunt u voor Azure-services aan de rand van externe locaties of onregelmatige connectiviteit, niet verbonden met het internet. U kunt hybride toepassingen die gegevens in Azure Stack lokaal te verwerken en vervolgens samenvoegen in Azure voor aanvullende verwerkingen en analyses maken. Ten slotte, omdat Azure Stack geïnstalleerd on-premises wordt, u kunt voldoen aan specifieke vereisten van regelgeving of beleid met de flexibiliteit van de implementatie van cloud-toepassing op de locatie zonder een code te wijzigen. 

## <a name="deployment-options"></a>Implementatieopties

### <a name="production-or-evaluation-environments"></a>Productie-of evaluatie
Azure Stack wordt aangeboden in twee opties voor implementatie om te voldoen aan uw behoeften, geïntegreerde Azure Stack-systemen voor gebruik in productieomgevingen en de Azure Stack Development Kit (ASDK) voor het evalueren van Azure Stack:

- **Azure Stack-geïntegreerde systemen**. Azure Stack geïntegreerde systemen worden aangeboden via een partnerschap van hardware- en Microsoft-partners, het maken van een oplossing die biedt cloud-eigen tempo innovatie en beheer eenvoudig computing. Omdat Azure Stack wordt geleverd als een geïntegreerd hardware- en softwaresysteem, beschikt u over alle flexibiliteit en controle die u nodig hebt, terwijl u tegelijkertijd vanuit de cloud kunt innoveren. Azure Stack geïntegreerde systemen in grootte variëren van 4-16 knooppunten en gezamenlijk worden ondersteund door de hardware-partner en Microsoft. Geïntegreerde Azure Stack-systemen zijn geknipt voor het maken van nieuwe scenario's en voor het implementeren van nieuwe oplossingen voor uw productieworkloads.

- **Azure Stack Development Kit**. De [Azure Stack Development Kit (ASDK)](./asdk/asdk-what-is.md) is een gratis, één knooppunt implementatie van Azure-Stack die u gebruiken kunt om te evalueren en meer informatie over Azure Stack. U kunt ook de ASDK als een ontwikkelomgeving instellen voor het bouwen van apps met behulp van de API's en hulpprogramma's die consistent is met Azure gebruiken. Echter, de ASDK is niet bedoeld als een productie-omgeving moet worden gebruikt en heeft de volgende beperkingen in vergelijking met de productie-implementatie volledig geïntegreerde systemen:

    - De ASDK kan alleen worden gekoppeld aan een enkele Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS)-id-provider.
    - Omdat Azure Stack-onderdelen zijn geïmplementeerd op een afzonderlijke host-computer, zijn er beperkte fysieke resources beschikbaar voor tenantresources. Deze configuratie is niet bedoeld om te schalen of voor evaluatie van de prestaties.
    - Het aantal netwerkscenario's is beperkt vanwege de vereisten met betrekking tot NIC-implementatie en gebruik van één host.

### <a name="connection-models"></a>Verbinding modellen
U kunt kiezen om een Azure Stack implementeren **verbonden** met internet (en naar Azure) of **verbroken** uit. Deze keuze bepaalt welke opties zijn beschikbaar voor uw identiteitsarchief (Azure AD of AD FS) en de facturering van maandabonnementen (betalen als u op basis van gebruik facturering of op basis van capaciteit facturering).

> Zie voor meer informatie, de overwegingen voor [verbonden](azure-stack-connected-deployment.md) en [verbroken](azure-stack-disconnected-deployment.md) implementatiemodellen. 

### <a name="identity-provider"></a>Identiteitsprovider 
Azure Stack maakt gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) als een id-provider tot stand brengen van Azure Stack-identiteiten. 

> [!IMPORTANT]
> Dit is een belangrijke beslissing. Kiest Azure AD of AD FS als id-provider is een eenmalige beslissing die u tijdens de implementatie moet aanbrengen. U kunt geen dit later wijzigen zonder het hele systeem opnieuw te implementeren.

Azure AD is van Microsoft cloud-gebaseerde, multitenant-id-provider. Azure AD de meeste hybride scenario's met internet verbonden implementaties gebruiken als het identiteitsarchief. U kunt echter gebruik van Active Directory Federation Services (AD FS) voor niet-verbonden implementaties van Azure Stack. Azure Stack-resourceproviders en andere toepassingen, werkt ongeveer dezelfde manier met AD FS als ze dit met Azure AD doen. Azure Stack bestaat uit een eigen Active Directory-exemplaar en een Active Directory Graph API. 

> U kunt meer informatie over overwegingen voor Azure Stack-identiteit op [overzicht van de identiteit voor Azure Stack](azure-stack-identity-overview.md).

## <a name="how-is-azure-stack-managed"></a>Hoe wordt Azure Stack beheerd?
Nadat Azure Stack is geïmplementeerd in de implementatie van een geïntegreerde systemen of een ASDK-installatie, wordt de primaire methoden van de interactie met Azure Stack zijn de beheerportal, gebruikersportal en PowerShell. De Azure Stack-portals zijn elk ondersteund door een afzonderlijke exemplaren van Azure Resource Manager. Een **Azure Stack-operators** de beheerportal gebruikt voor het beheren van Azure Stack, en voor het zaken als tenant aanbiedingen maken en onderhouden van de status en de monitor status van het geïntegreerd systeem. De gebruikersportal (ook wel de tenantportal genoemd) biedt een selfservice-ervaring voor gebruik van cloudresources, zoals virtuele machines, opslagaccounts en web-apps. 

> Zie voor meer informatie over het beheren van Azure Stack met behulp van de beheerportal, gebruikt de [beheer portal-quickstart voor Azure Stack](azure-stack-manage-portals.md).

Als een Azure Stack-Operator, kunt u een groot aantal services en toepassingen, zoals leveren [virtuele machines](azure-stack-tutorial-tenant-vm.md), [webtoepassingen](azure-stack-app-service-overview.md), maximaal beschikbare [SQL Server](azure-stack-tutorial-sql.md), en [MySQL-Server](azure-stack-tutorial-mysql.md) databases. U kunt ook [snelstartsjablonen van Azure Stack voor Azure Resource Manager](https://github.com/Azure/AzureStack-QuickStart-Templates) implementatie van SharePoint, Exchange en meer. 

Met de beheerportal kunt u de [configureren van Azure Stack om services te leveren](azure-stack-plan-offer-quota-overview.md) voor tenants die gebruikmaken van plannen, quota, aanbiedingen en abonnementen. Tenantgebruikers kunnen zich abonneren op meerdere aanbiedingen. Aanbiedingen kunnen een of meer abonnementen hebben en plannen kunnen hebben een of meer services. Operators ook capaciteit beheren en reageren op waarschuwingen. 

Wanneer Azure Stack is geconfigureerd, een **Azure Stack-gebruiker** (ook wel een tenant genoemd)-services die de Operator biedt verbruikt. Gebruikers kunnen inrichten, bewaken en beheren van services die ze zich hebt geabonneerd, zoals web-apps, opslag en virtuele machines.

> Voor meer informatie over het beheren van Azure Stack, waaronder wat accounts te gebruiken wanneer typische Operator verantwoordelijkheden, wat uw gebruikers laten weten en hoe u hulp nodig hebt, Raadpleeg [basisprincipes van beheer van Azure Stack](azure-stack-manage-basics.md).

## <a name="resource-providers"></a>Resourceproviders 
Resourceproviders zijn webservices die de basis voor alle Azure Stack-IaaS vormen en PaaS-services. Azure Resource Manager, is afhankelijk van verschillende resourceproviders voor toegang tot services. Elke resourceprovider helpt u bij het configureren en beheren van de bijbehorende resources. Service-beheerders kunnen ook nieuwe aangepaste resourceproviders toevoegen. 

### <a name="foundational-resource-providers"></a>Fundamentele resourceproviders 
Er zijn drie fundamentele IaaS-resourceproviders: COMPUTE, netwerk en opslag:

- **Compute**. De Resourceprovider voor Compute kunnen Azure Stack tenants hun eigen virtuele machines maken. De Resourceprovider voor Compute biedt de mogelijkheid om te maken van virtuele machines, evenals de extensies voor virtuele machines. De virtuele Machine-extensie-service helpt u bij IaaS-mogelijkheden bieden voor Windows en Linux-machines.  Als u bijvoorbeeld kunt u de Resourceprovider voor Compute voor een Linux-machine inrichten en Bash-scripts uitvoeren tijdens de implementatie van het configureren van de virtuele machine.
- **Resourceprovider-netwerk**. De Netwerkresourceprovider biedt een reeks functies voor Software gedefinieerde netwerken (SDN) en netwerk-functie Netwerkvirtualisatie (NFV) voor de privécloud. U kunt de Netwerkresourceprovider gebruiken om resources, zoals software load balancers, openbare IP-adressen, netwerkbeveiligingsgroepen en virtuele netwerken te maken.
- **Storage Resourceprovider**. De Storage Resource Provider biedt vier consistent zijn met Azure storage-services: [blob](https://docs.microsoft.com/azure/storage/common/storage-introduction#blob-storage), [wachtrij](https://docs.microsoft.com/azure/storage/common/storage-introduction#queue-storage), [tabel](https://docs.microsoft.com/azure/storage/common/storage-introduction#table-storage), en accountbeheer van Key Vault biedt beheer en controle van geheimen zoals wachtwoorden en certificaten. De storage resourceprovider biedt ook een opslagservice voor het beheer van cloud serviceprovider beheer van Azure-consistente Storage-services in het kader. Azure Storage biedt de flexibiliteit voor het opslaan en ophalen van grote hoeveelheden ongestructureerde gegevens, zoals documenten en mediabestanden met Azure-Blobs en gestructureerde op NoSQL gebaseerde gegevens met Azure-tabellen. 

### <a name="optional-resource-providers"></a>Optionele resourceproviders
Er zijn drie optionele PaaS resourceproviders die u kunt implementeren en gebruiken met Azure Stack: Resourceproviders voor App Service, SQL Server en MySQL-Server:

- **App Service**. [Azure App Service in Azure Stack](azure-stack-app-service-overview.md) is een platform-as-a-service (PaaS) aanbieding van Microsoft Azure beschikbaar voor Azure Stack. De service kan uw interne of externe klanten te maken van web-API en Azure Functions-toepassingen voor elk platform of apparaat. 
- **SQL Server**. Gebruik de [SQL Server-resourceprovider](azure-stack-sql-resource-provider.md) te bieden van SQL-databases als een service van Azure Stack. Nadat u de resourceprovider installeren en met een of meer exemplaren van SQL Server verbinden, kunnen u en uw gebruikers voor cloud-eigen apps, Websites die gebruikmaken van SQL en andere werkbelastingen die gebruikmaken van SQL database maken.
- **MySQL Server**. Gebruik de [resourceprovider van MySQL-Server](azure-stack-mysql-resource-provider-deploy.md) om MySQL-databases als een Azure Stack-service zichtbaar te maken. De MySQL-resourceprovider wordt uitgevoerd als een service op een Windows Server 2016 Server Core-machine (VM).

## <a name="providing-high-availability"></a>Hoge beschikbaarheid
Voor het bereiken van hoge beschikbaarheid van een systeem van de productie multi-VM in Azure, virtuele machines worden geplaatst een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) die ze verspreidt over meerdere foutdomeinen en updatedomeinen. Een foutdomein in een beschikbaarheidsset is in de kleinere schaal van Azure Stack gedefinieerd als een enkel knooppunt in de schaaleenheid.  

Terwijl de infrastructuur van Azure Stack al tegen uitvallen is, de onderliggende technologie (Failoverclustering) nog steeds leidt tot enige uitvaltijd voor virtuele machines op een betrokken fysieke server als er een hardwarestoring optreedt. Azure Stack biedt ondersteuning voor met een beschikbaarheidsset met een maximum van drie foutdomeinen zodat deze overeenkomt met Azure.

- **Foutdomeinen**. Virtuele machines in een beschikbaarheidsset geplaatst worden fysiek van elkaar geïsoleerd door ze zo gelijkmatig mogelijk te spreiden over meerdere foutdomeinen (Azure Stack-knooppunten). Als er een hardwarestoring optreedt, wordt virtuele machines van de mislukte foutdomein opnieuw opgestart in andere domeinen met fouten, maar, indien mogelijk wordt gehouden in afzonderlijke foutdomeinen van de andere virtuele machines in dezelfde beschikbaarheidsset. Wanneer de hardware weer online komt, worden virtuele machines worden uitgevoerd voor het onderhouden van hoge beschikbaarheid. 
 
- **Updatedomeinen**. Update-domeinen zijn een andere Azure concept, dat zorgt voor hoge beschikbaarheid in beschikbaarheidssets. Een updatedomein is een logische groep onderliggende hardware die onderhoud kan ondergaan op hetzelfde moment. Virtuele machines die zich in hetzelfde updatedomein wordt opnieuw gestart samen tijdens gepland onderhoud. Als tenants virtuele machines in een beschikbaarheidsset maakt, het Azure-platform verdeelt virtuele machines automatisch tussen deze updatedomeinen. In Azure Stack zijn virtuele machines live op andere online hosts in het cluster wordt gemigreerd voordat de hun onderliggende host is bijgewerkt. Omdat er geen tenant-downtime tijdens het bijwerken van een host is, wordt de updatefunctie-domein in Azure Stack alleen bestaat voor de sjabloon voor compatibiliteit met Azure. 

## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
U kunt rollen gebaseerd toegangsbeheer (RBAC) om systeemtoegang te verlenen tot gemachtigde gebruikers, groepen en services door het toewijzen van rollen bij een abonnement, resourcegroep of afzonderlijke resourceniveau gebruiken. Elke rol definieert het toegangsniveau dat een gebruiker, groep of service voor Microsoft Azure Stack-resources heeft.

Azure Stack RBAC heeft drie fundamentele rollen die betrekking hebben op alle resourcetypen: Eigenaar, Inzender en lezer. Eigenaar heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. Inzender kunt maken en beheren van alle typen Azure-resources, maar kan geen toegang om anderen te verlenen. Lezer kan alleen bestaande resources weergeven. De rest van de RBAC-rollen toestaan beheer van specifieke Azure-resources. Bijvoorbeeld, de rol Inzender voor virtuele machines kunt maken en beheren van virtuele machines, maar staat niet toe dat beheer van het virtuele netwerk of het subnet waarmee de virtuele machine verbinding maakt.

> Zie [Manage Role-Based Access Control](azure-stack-manage-permissions.md) voor meer informatie. 

## <a name="reporting-usage-data"></a>Gegevens over gebruik rapporteren
Microsoft Azure Stack verzamelt en verzamelt gebruiksgegevens over alle resourceproviders en verzendt naar Azure voor verwerking door Azure commerce. De gebruiksgegevens worden verzameld in Azure Stack kan worden weergegeven via een REST-API. Er is een Azure-consistente Tenant-API, evenals Provider en gedelegeerd Provider API's om gegevens over gebruik voor alle abonnementen van de tenant. Deze gegevens kunnen worden gebruikt om te integreren met een extern hulpprogramma of de service voor facturering of chargeback. Wanneer gebruik is verwerkt door Azure commerce, kan deze worden weergegeven in de Azure-factureringsportal.

> Meer informatie over [reporting van Azure Stack-gebruiksgegevens naar Azure](azure-stack-usage-reporting.md).

## <a name="next-steps"></a>Volgende stappen

[Azure Stack en de globale Azure vergelijken](compare-azure-azure-stack.md)

[Basisprincipes van beheer](azure-stack-manage-basics.md)

[Snelstartgids: Gebruik de Azure Stack-beheerportal](azure-stack-manage-portals.md)