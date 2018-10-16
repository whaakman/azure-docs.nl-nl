---
title: Belangrijke functies en concepten in Azure Stack | Microsoft Docs
description: Meer informatie over de belangrijkste functies en concepten in Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.openlocfilehash: 16c908dabd313cd9d64ce5be9b7d0299423a7675
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344682"
---
# <a name="key-features-and-concepts-in-azure-stack"></a>Belangrijke functies en concepten in Azure Stack
Als u geen ervaring met Microsoft Azure Stack, kunnen deze voorwaarden en beschrijvingen van functies in kwestie nuttig zijn.

## <a name="personas"></a>Persona's
Er zijn twee soorten gebruikers voor Microsoft Azure Stack, de cloud-operator (provider) en de tenant (consument).

* Een **cloud operator** kunnen Azure Stack configureren en beheren van aanbiedingen, plannen, services, quota en prijzen voor resources voor hun tenants.  Cloudoperators ook capaciteit beheren en reageren op waarschuwingen.  
* Een **tenant** (ook wel een gebruiker genoemd)-services die de beheerder van de cloud biedt verbruikt. Tenants kunnen inrichten, bewaken en beheren van services die ze zich hebt geabonneerd, zoals Web-Apps, opslag en virtuele Machines.

## <a name="portal"></a>Portal
De primaire methoden van de interactie met Microsoft Azure Stack zijn de beheerdersportal, gebruikersportal en PowerShell.

De Azure Stack-portals zijn elk ondersteund door een afzonderlijke exemplaren van Azure Resource Manager.  Een cloud-operator gebruikt de beheerdersportal voor het beheren van Azure Stack, en voor zaken als tenant aanbiedingen maken.  De gebruikersportal (ook wel de tenantportal genoemd) biedt een selfservice-ervaring voor gebruik van cloudresources, zoals virtuele machines, opslagaccounts en Web-Apps. Zie voor meer informatie, [met behulp van de Azure Stack-beheerder en gebruiker portals](azure-stack-manage-portals.md).

## <a name="identity"></a>Identiteit 
Azure Stack maakt gebruik van Azure Active Directory (AAD) of Active Directory Federation Services (AD FS) als een id-provider.  

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory is van Microsoft cloud-gebaseerde, multitenant-id-provider.  De meeste hybride scenario's Azure Active Directory gebruiken als het identiteitsarchief.

### <a name="active-directory-federation-services"></a>Active Directory Federation Services
U kunt kiezen voor het gebruik van Active Directory Federation Services (AD FS) voor niet-verbonden implementaties van Azure Stack.  Azure Stack, resourceproviders en andere toepassingen werken ongeveer dezelfde manier met AD FS als ze dit met Azure Active Directory doen. Azure Stack bestaat uit een eigen instantie van de AD FS en Active Directory en een Active Directory Graph API. Azure Stack Development Kit ondersteunt de volgende AD FS-scenario's:

- Meld u aan de implementatie met behulp van AD FS.
- Een virtuele machine maken met geheimen in Key Vault
- Maak een kluis voor geheimen opslaan/toegang tot
- Aangepaste RBAC-rollen in abonnement maken
- Gebruikers toewijzen aan RBAC-rollen op resources
- RBAC-rollen van het gehele systeem via Azure PowerShell maken
- Meld u aan als een gebruiker via Azure PowerShell
- Maken van de service principals gebruiken om aan te melden bij Azure PowerShell


## <a name="regions-services-plans-offers-and-subscriptions"></a>Regio's, services, plannen, aanbiedingen en abonnementen
Services worden geleverd in Azure Stack, aan tenants met behulp van de regio's, abonnementen, aanbiedingen en abonnementen. Tenants kunnen zich abonneren op meerdere aanbiedingen. Aanbiedingen kunnen een of meer abonnementen hebben en plannen kunnen hebben een of meer services.

![](media/azure-stack-key-features/image4.png)

Voorbeeld van de hiërarchie van een tenant abonnementen, aanbiedingen, elk met verschillende abonnementen en services.

### <a name="regions"></a>Regio's
Azure Stack-regio's zijn een basic-element van de schaal en. Een organisatie kan meerdere regio's met beschikbare bronnen in elke regio hebben. Regio's mogelijk ook andere service-aanbiedingen die verkrijgbaar zijn. In Azure Stack Development Kit, maar één regio wordt ondersteund en wordt automatisch met de naam *lokale*.

### <a name="services"></a>Services
Microsoft Azure Stack kunt-providers voor het leveren van een groot aantal services en toepassingen, zoals virtuele machines, SQL Server-databases, SharePoint, Exchange, en meer.

### <a name="plans"></a>Abonnementen
De abonnementen zijn groepen van een of meer services. Als een provider, moet u plannen om aan te bieden aan uw tenants maken. Tenants abonneren zich op hun beurt op uw aanbiedingen om de plannen en bijbehorende services te gebruiken.

Elke service toegevoegd aan een plan kan worden geconfigureerd met quota-instellingen voor het beheren van de cloudcapaciteit van de. Quota beperkingen, zoals virtuele machine, RAM-geheugen en CPU-limieten kunnen bevatten en worden toegepast per gebruikersabonnement. Quota's kunnen worden onderscheiden op locatie. -Een plan met de compute-services van A-regio's kan bijvoorbeeld een quotum van twee virtuele machines, 4 GB RAM-geheugen en 10 CPU-kernen hebben.

Bij het maken van een aanbieding, de servicebeheerder kan bevatten een **basisplan**. Deze basisplannen zijn standaard opgenomen als een tenant zich op deze aanbieding abonneert. Wanneer een gebruiker zich abonneert (en het abonnement is gemaakt), heeft de gebruiker toegang tot alle resourceproviders die zijn opgegeven in deze basisplannen (met de bijbehorende quota).

De service-beheerder kan ook bevatten **aanvullende plannen** in een aanbieding. Aanvullende plannen zijn niet opgenomen in de standaardinstelling in het abonnement. Aanvullende plannen zijn aanvullende plannen (quota) beschikbaar zijn in een aanbieding die eigenaar van een abonnement aan hun abonnementen toevoegen kunt.

### <a name="offers"></a>Aanbiedingen
Aanbiedingen zijn groepen van een of meer plannen die providers aan tenants aan te schaffen bieden (abonneren op). Bijvoorbeeld, bieden Alpha Plan A kan bevatten met een set met compute-services en plannen B met een set met services voor opslag en netwerk.

Een aanbieding wordt geleverd met een set basisplannen en servicebeheerders kunnen aanvullende plannen die tenants aan hun abonnement toevoegen kunnen maken.

### <a name="subscriptions"></a>Abonnementen
Een abonnement is hoe tenants kopen voor uw aanbiedingen. Een abonnement is een combinatie van een tenant met een aanbieding. Een tenant kan abonnementen op meerdere aanbiedingen hebben. Elk abonnement geldt voor slechts één aanbieding. Van een tenant-abonnementen te bepalen welke abonnementen /-services toegang te krijgen tot.

Abonnementen helpen providers organiseren en toegang tot cloud-bronnen en services.

Voor de beheerder een Providerabonnement standaard gemaakt tijdens de implementatie. Dit abonnement kan worden gebruikt om het beheren van Azure Stack, verder resourceproviders te implementeren en plannen en aanbiedingen maken voor tenants. Deze mag niet worden gebruikt om uit te voeren van de klantwerkbelastingen en toepassingen. Vanaf versie 1804, twee aanvullende abonnementen vormen een aanvulling op de Provider standaard-abonnement. een Resourcemetingsdatabase-abonnement en een abonnement op verbruik. Deze toevoegingen vereenvoudigen het beheer van de basisinfrastructuur, extra resourceproviders en werkbelastingen scheiden.  

## <a name="azure-resource-manager"></a>Azure Resource Manager
Met behulp van Azure Resource Manager, kunt u werken met de infrastructuurresources van uw in een model op basis van een sjabloon, declaratieve.   Het biedt één interface die u gebruiken kunt om te implementeren en beheren van de oplossingsonderdelen van uw. Zie voor volledige informatie over en richtlijnen, de [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Resourcegroepen
Resourcegroepen zijn verzamelingen van resources, services en toepassingen, en elke resource heeft een type, zoals virtuele machines, virtuele netwerken, openbare IP-adressen, opslagaccounts en websites. Elke resource moet zich in een resourcegroep en zodat resourcegroepen logisch indelen van bronnen, zoals door de werkbelasting of locatie.  In Microsoft Azure Stack worden-resources, zoals plannen en aanbiedingen ook beheerd in resourcegroepen.

In tegenstelling tot [Azure](../azure-resource-manager/resource-group-move-resources.md), u kunt geen resources wilt verplaatsen tussen resourcegroepen. Wanneer u de eigenschappen van een resource of resourcegroep in de Azure Stack-beheerportal weergeven, de *verplaatsen* knop lichter gekleurd en niet beschikbaar is. 
 
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
Met Azure Resource Manager kunt u een sjabloon (in JSON-indeling) die de implementatie en configuratie van uw toepassing worden gedefinieerd. Deze sjabloon wordt aangeduid als een Azure Resource Manager-sjabloon en biedt een declaratieve manier om implementatie te definiëren. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus van uw app de app herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

## <a name="resource-providers-rps"></a>Resourceproviders (RPs)
Resourceproviders zijn webservices die de basis voor alle op basis van Azure IaaS vormen en PaaS-services. Azure Resource Manager, is afhankelijk van verschillende RPs voor toegang tot services.

Er zijn vier fundamentele RPs: netwerk-, opslag-, reken- en Key Vault. Elk van deze RPs helpt u bij het configureren en beheren van de bijbehorende resources. Service-beheerders kunnen ook nieuwe aangepaste resourceproviders toevoegen.

### <a name="compute-rp"></a>COMPUTE RP
De Compute Resource Provider (CRP) kunnen Azure Stack tenants hun eigen virtuele machines maken. De CRP biedt de mogelijkheid om te maken van virtuele machines, evenals de extensies voor virtuele machines. De virtuele Machine-extensie-service helpt u bij IaaS-mogelijkheden bieden voor Windows en Linux-machines.  Als u bijvoorbeeld kunt u de CRP een Linux-machine inrichten en Bash-scripts uitvoeren tijdens de implementatie van het configureren van de virtuele machine.

### <a name="network-rp"></a>Netwerk RP
Het netwerk Resource Provider (NRP) biedt een reeks functies voor Software gedefinieerde netwerken (SDN) en netwerk-functie Netwerkvirtualisatie (NFV) voor de privécloud.  U kunt de NRP gebruiken om resources, zoals software load balancers, openbare IP-adressen, netwerk-beveiligingsgroepen, virtuele netwerken te maken.

### <a name="storage-rp"></a>Opslag RP
De RP-opslag biedt vier consistent zijn met Azure storage-services: blob, table, queue en accountbeheer. Het biedt ook een opslagservice voor het beheer van cloud serviceprovider beheer van Azure-consistente Storage-services in het kader. Azure Storage biedt de flexibiliteit voor het opslaan en ophalen van grote hoeveelheden ongestructureerde gegevens, zoals documenten en mediabestanden met Azure-Blobs en gestructureerde op NoSQL gebaseerde gegevens met Azure-tabellen. Zie voor meer informatie over Azure Storage [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Blob Storage
BLOB-opslag opgeslagen gegevens ingesteld. Een blob kan elk type tekst of binaire gegevens zijn, zoals een document, mediabestand of toepassingsinstallatieprogramma. Table storage worden gestructureerde gegevenssets opgeslagen. Table Storage is een gegevensarchief met NoSQL-sleutelkenmerk, waarmee snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens mogelijk is. Queue storage biedt betrouwbare berichten voor werkstroomverwerking en voor communicatie tussen onderdelen van cloudservices.

Elke blob is georganiseerd in een container. Containers bieden ook een handige manier om beveiligingsbeleid toe te wijzen aan groepen objecten. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs, tot de capaciteitslimiet van 500 TB van het opslagaccount bevatten. Blob Storage biedt drie typen blobs: blok-blobs, toevoeg-blobs en pagina-blobs (schijven). Blok-blobs zijn geoptimaliseerd voor streaming en opslag van cloudobjecten en zijn een goede keuze voor het opslaan van documenten, mediabestanden, back-ups enzovoort. Toevoeg-blobs zijn vergelijkbaar met blok-blobs, maar deze zijn geoptimaliseerd voor toevoegbewerkingen. Een toevoeg-blob kan alleen worden bijgewerkt door een nieuw blok aan het eind toe te voegen. Toevoeg-blobs zijn een goede keuze voor scenario's zoals logboekregistratie, waarbij alleen aan het eind van de blob nieuwe gegevens moeten worden geschreven. Pagina-blobs zijn geoptimaliseerd voor de vertegenwoordiging van IaaS-schijven en ondersteuning van willekeurige schrijfbewerkingen en mogelijk tot 1 TB. Een op een Azure Virtual Machine-netwerk aangesloten IaaS-schijf is een VHD die is opgeslagen als pagina-blob.

#### <a name="table-storage"></a>Table Storage
Tabelopslag is van Microsoft NoSQL-sleutel/kenmerkopslag: heeft een ontwerp zonder schema's, waardoor het verschillende van traditionele relationele databases. Omdat gegevens ontbreken schema's worden opgeslagen, is het uw kunt gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. Table Storage is eenvoudig te gebruiken, zodat ontwikkelaars snel toepassingen kunnen maken. Table Storage is een sleutelkenmerkopslag, wat betekent dat elke waarde in een tabel wordt opgeslagen met een getypeerde eigenschapsnaam. De naam van de eigenschap kan worden gebruikt om te filteren en selectiecriteria op te geven. Een verzameling eigenschappen en hun waarden vormen samen een entiteit. Aangezien tabelschema opslag ontbreken, twee entiteiten in dezelfde tabel verschillende verzamelingen eigenschappen kunnen bevatten en de eigenschappen van verschillende typen kunnen worden. U kunt Table Storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. In elke tabel kunt u een willekeurig aantal entiteiten opslaan. Een opslagaccount kan een onbeperkt aantal tabellen bevatten, tot de maximale capaciteit van het opslagaccount.

#### <a name="queue-storage"></a>Queue Storage
Azure Queue Storage biedt uitwisseling van berichten tussen toepassingsonderdelen in de cloud. Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt daarnaast ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

### <a name="keyvault"></a>KeyVault
De KeyVault RP biedt beheer en controle van geheimen zoals wachtwoorden en certificaten. Als u bijvoorbeeld kunt een tenant de RP KeyVault gebruiken voor beheerderswachtwoorden of sleutels tijdens de implementatie van de virtuele machine.

## <a name="high-availability-for-azure-stack"></a>Hoge beschikbaarheid voor Azure Stack
*Is van toepassing op: Azure Stack 1802 of hogere versies*

Virtuele machines worden voor het bereiken van hoge beschikbaarheid van een systeem van de productie multi-VM in Azure, geplaatst in een beschikbaarheidsset die zich ze in verschillende foutdomeinen en updatedomeinen verspreidt. Op deze manier [virtuele machines die worden geïmplementeerd in beschikbaarheidssets](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) zijn fysiek geïsoleerd van elkaar op afzonderlijke server rekken om toe te staan voor fout tolerantie, zoals wordt weergegeven in het volgende diagram:

  ![Hoge beschikbaarheid voor Azure Stack](media/azure-stack-key-features/high-availability.png)

### <a name="availability-sets-in-azure-stack"></a>Beschikbaarheidssets in Azure Stack
Terwijl de infrastructuur van Azure Stack al tegen uitvallen is, de onderliggende technologie (Failoverclustering) nog steeds leidt tot enige uitvaltijd voor virtuele machines op een betrokken fysieke server als er een hardwarestoring optreedt. Azure Stack biedt ondersteuning voor met een beschikbaarheidsset met een maximum van drie foutdomeinen zodat deze overeenkomt met Azure.

- **Foutdomeinen**. Virtuele machines in een beschikbaarheidsset geplaatst worden fysiek van elkaar geïsoleerd door ze zo gelijkmatig mogelijk te spreiden over meerdere foutdomeinen (Azure Stack-knooppunten). Als er een hardwarestoring optreedt, wordt virtuele machines van de mislukte foutdomein opnieuw opgestart in andere domeinen met fouten, maar, indien mogelijk wordt gehouden in afzonderlijke foutdomeinen van de andere virtuele machines in dezelfde beschikbaarheidsset. Wanneer de hardware weer online komt, worden virtuele machines worden uitgevoerd voor het onderhouden van hoge beschikbaarheid. 
 
- **Updatedomeinen**. Update-domeinen zijn een andere Azure concept, dat zorgt voor hoge beschikbaarheid in beschikbaarheidssets. Een updatedomein is een logische groep onderliggende hardware die onderhoud kan ondergaan op hetzelfde moment. Virtuele machines die zich in hetzelfde updatedomein wordt opnieuw gestart samen tijdens gepland onderhoud. Als tenants virtuele machines in een beschikbaarheidsset maakt, het Azure-platform verdeelt virtuele machines automatisch tussen deze updatedomeinen. In Azure Stack zijn virtuele machines live op andere online hosts in het cluster wordt gemigreerd voordat de hun onderliggende host is bijgewerkt. Omdat er geen tenant-downtime tijdens het bijwerken van een host is, wordt de updatefunctie-domein in Azure Stack alleen bestaat voor de sjabloon voor compatibiliteit met Azure. 

### <a name="upgrade-scenarios"></a>Upgradescenario 's 
Virtuele machines in beschikbaarheidssets die zijn gemaakt voordat Azure Stack-versie 1802 krijgt een aantal fout- en updatedomeinen (1 en 1 respectievelijk). Voor het bereiken van hoge beschikbaarheid voor virtuele machines in deze vooraf bestaande beschikbaarheidssets, moet u eerst de bestaande VM's te verwijderen en deze vervolgens opnieuw te implementeren in een nieuwe beschikbaarheidsset met de juiste fout- en domein aantallen zoals beschreven in [wijzigen de beschikbaarheidsset voor een Windows-VM](https://docs.microsoft.com/azure/virtual-machines/windows/change-availability-set). 

Voor virtuele-machineschaalsets, een beschikbaarheidsset maakt intern met een standaardaantal foutdomeinen van het domein en de update (3 en 5 respectievelijk). Een virtuele machine-schaalsets die zijn gemaakt voordat de update 1802 worden geplaatst in een beschikbaarheidsset met de fout- en standaarddomein telt (1 en 1 respectievelijk). Voor het bijwerken van deze virtuele machine schaalsetinstanties voor het bereiken van de nieuwere verspreiding, de virtuele-machineschaalsets uitschalen door het aantal exemplaren die vóór de update 1802 aanwezig waren en verwijder vervolgens de oudere exemplaren van de virtuele-machineschaalsets. 

## <a name="role-based-access-control-rbac"></a>Op rollen gebaseerd toegangsbeheer (RBAC)
U kunt RBAC systeemtoegang verlenen tot gemachtigde gebruikers, groepen en services door het toewijzen van rollen bij een abonnement, resourcegroep of afzonderlijke resourceniveau gebruiken. Elke rol definieert het toegangsniveau dat een gebruiker, groep of service voor Microsoft Azure Stack-resources heeft.

Azure RBAC heeft drie fundamentele rollen die betrekking hebben op alle resourcetypen: eigenaar, Inzender en lezer. Eigenaar heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. Inzender kunt maken en beheren van alle typen Azure-resources, maar kan geen toegang om anderen te verlenen. Lezer kan alleen bestaande Azure-resources weergeven. De rest van de RBAC-rollen in Azure kunt beheer van specifieke Azure-resources. Bijvoorbeeld, de rol Inzender voor virtuele machines kunt maken en beheren van virtuele machines, maar staat niet toe dat beheer van het virtuele netwerk of het subnet waarmee de virtuele machine verbinding maakt.

## <a name="usage-data"></a>Gebruiksgegevens
Microsoft Azure Stack verzamelt en verzamelt gebruiksgegevens over alle resourceproviders en verzendt naar Azure voor verwerking door Azure commerce. De gebruiksgegevens worden verzameld in Azure Stack kan worden weergegeven via een REST-API. Er is een Azure-consistente Tenant-API, evenals Provider en gedelegeerd Provider API's om gegevens over gebruik voor alle abonnementen van de tenant. Deze gegevens kunnen worden gebruikt om te integreren met een extern hulpprogramma of de service voor facturering of chargeback. Wanneer gebruik is verwerkt door Azure commerce, kan deze worden weergegeven in de Azure-factureringsportal.

## <a name="in-development-build-of-azure-stack-development-kit"></a>In ontwikkeling build van Azure Stack Development Kit
In ontwikkeling builds kunnen vroege gebruikers evalueren van de meest recente versie van de Azure Stack Development Kit. Ze incrementele builds op basis van de meest recente primaire versie. Terwijl primaire versies blijft elke paar maanden worden vrijgegeven, wordt de builds in ontwikkeling met tussenpozen tussen de primaire versies brengt.

In ontwikkeling builds biedt de volgende voordelen:
- Opgeloste fouten
- Nieuwe functies
- Andere verbeteringen

## <a name="next-steps"></a>Volgende stappen
[Basisprincipes van beheer](azure-stack-manage-basics.md)

