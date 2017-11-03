---
title: Belangrijke functies en -concepten in Azure-Stack | Microsoft Docs
description: Meer informatie over de belangrijkste functies en -concepten in Azure-Stack.
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: fd16748e1369b8abcab38ce1945f72c681c344b8
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="key-features-and-concepts-in-azure-stack"></a>Belangrijke functies en -concepten in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als u geen ervaring met Microsoft Azure-Stack, zijn deze voorwaarden en beschrijvingen van functies handig zijn.

## <a name="personas"></a>Persona 's
Er zijn twee varianten van gebruikers voor Microsoft Azure-Stack, de operator cloud (provider) en de tenant (consument).

* Een **cloud operator** kunt Stack Azure configureren en beheren van aanbiedingen, plannen, services, quota en prijzen om bronnen voor hun tenants.  Cloudoperators ook capaciteit beheren en reageren op waarschuwingen.  
* Een **tenant** (ook wel een gebruiker genoemd) maakt gebruik van services die de beheerder van de cloud biedt. Tenants kunnen inrichten, bewaken en beheren van services die ze zich hebt geabonneerd, zoals Web-Apps, opslag en virtuele Machines.

## <a name="portal"></a>Portal
De primaire methoden van de interactie met Microsoft Azure-Stack zijn de beheerdersportal, gebruikersportal en PowerShell.

De Azure-Stack-portals zijn elk ondersteund door de afzonderlijke exemplaren van Azure Resource Manager.  Een cloud-operator gebruikt de beheerdersportal voor het beheren van Azure-Stack en voor handelingen zoals tenant aanbiedingen maakt.  De gebruikersportal (ook wel de tenantportal genoemd) biedt een selfservice-ervaring voor het gebruik van cloudbronnen, zoals virtuele machines, opslagaccounts en Web-Apps. Zie voor meer informatie [met behulp van de Azure-Stack-beheerder en gebruiker portals](azure-stack-manage-portals.md).

## <a name="identity"></a>Identiteit 
Azure Stack maakt gebruik van Azure Active Directory (AAD) of Active Directory Federation Services (AD FS) als een id-provider.  

### <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory is een Microsoft cloud-gebaseerde, multitenant-id-provider.  De meeste hybride scenario's gebruiken Azure Active Directory als de identity-store.

### <a name="active-directory-federation-services"></a>Active Directory Federatieservices
U kunt Active Directory Federation Services (AD FS) voor niet-verbonden implementaties van Azure-Stack gebruiken.  Azure Stack, resourceproviders en andere toepassingen het ongeveer dezelfde manier als met AD FS werken als met Azure Active Directory. Azure Stack bevat een eigen exemplaar van AD FS en Active Directory en een Active Directory Graph API. Azure-Stack Development Kit ondersteunt de volgende AD FS-scenario's:

- Meld u aan de implementatie met AD FS.
- Een virtuele machine maken met geheimen in de Sleutelkluis
- Een kluis voor het opslaan van/openen van geheimen maken
- Aangepaste RBAC-rollen in abonnement maken
- Gebruikers toewijzen aan rollen RBAC voor bronnen
- RBAC-rollen gehele systeem via Azure PowerShell maken
- Meld u aan als een gebruiker via Azure PowerShell
- Maken van service principals ze aan te melden bij Azure PowerShell gebruiken


## <a name="regions-services-plans-offers-and-subscriptions"></a>Regio's, services, plannen, aanbiedingen en abonnementen
In Azure-Stack worden services geleverd aan tenants met behulp van de regio's, abonnementen aanbiedingen en plannen. Tenants kunnen zich abonneren op meerdere aanbiedingen. Aanbiedingen kunnen een of meer abonnementen hebben kunnen, en abonnementen een of meer services.

![](media/azure-stack-key-features/image4.png)

Voorbeeld van de hiërarchie van een tenant abonnementen op aanbiedingen, elk met verschillende abonnementen en services.

### <a name="regions"></a>Regio's
Azure Stack-regio's zijn basiselement schaal en het beheer van. Een organisatie kan meerdere regio's met beschikbare bronnen hebben in elke regio. Gebieden kunnen ook andere serviceaanbiedingen beschikbaar hebben. In de Azure-Stack Development Kit maar één regio wordt ondersteund en automatisch de naam *lokale*.

### <a name="services"></a>Services
Microsoft Azure-Stack kunnen providers bieden tal van services en toepassingen, zoals virtuele machines, SQL Server-databases, SharePoint, Exchange en meer.

### <a name="plans"></a>Abonnementen
Plannen voor zijn groepen van een of meer services. Als een provider, moet u plannen om aan te bieden aan uw tenants maken. Tenants abonneren zich op hun beurt op uw aanbiedingen om de plannen en bijbehorende services te gebruiken.

Elke service toegevoegd aan een plan kan worden geconfigureerd met de quota-instellingen voor het beheren van de cloudcapaciteit van de. Quota kunnen beperkingen, zoals VM, RAM-geheugen en CPU-limieten en per gebruikerabonnement worden toegepast. Quota's kunnen worden ingedeeld per locatie. Een plan met compute services van een regio kan bijvoorbeeld een quotum van twee virtuele machines, 4 GB RAM-geheugen en 10 CPU-kernen hebben.

Wanneer u een aanbieding maakt, de servicebeheerder kan bevatten een **basisplan**. Deze base plannen zijn standaard opgenomen als een tenant zich op deze aanbieding abonneert. Wanneer een gebruiker zich abonneert (en het abonnement is gemaakt), heeft de gebruiker toegang tot alle resourceproviders die zijn opgegeven in deze base plannen (met de bijbehorende quota's).

De servicebeheerder kan ook betekenen dat **invoegtoepassing plannen** in een aanbieding. Plannen van de invoegtoepassing zijn niet standaard opgenomen in het abonnement. Plannen van de invoegtoepassing zijn aanvullende schema's (quota's) beschikbaar in een aanbieding die eigenaar van een abonnement aan hun abonnementen toevoegen kunt.

### <a name="offers"></a>Aanbiedingen
Aanbiedingen zijn groepen van een of meer plannen dat providers aan tenants aan te schaffen (abonneren op). Bijvoorbeeld, bieden Alpha Plan een kan bevatten die een set van compute services en Plan B die een set van services voor opslag en netwerk.

Een aanbieding wordt geleverd met een set base plannen en servicebeheerders invoegtoepassing plannen die tenants aan hun abonnement toevoegen kunnen kunnen maken.

### <a name="subscriptions"></a>Abonnementen
Een abonnement is hoe tenants uw aanbiedingen kopen. Een abonnement is een combinatie van een tenant met een aanbieding. Een tenant kan abonnementen op meerdere aanbiedingen hebben. Elk abonnement geldt voor slechts één aanbieding. Een tenant abonnementen bepalen welke abonnementen en services voor toegang te krijgen tot.

Abonnementen helpen providers ordenen en toegang tot bronnen en services.

Voor de beheerder is een standaard Provider-abonnement gemaakt tijdens de implementatie. Dit abonnement kan worden gebruikt voor Azure-Stack beheren, implementeren verdere resourceproviders en plannen en aanbiedingen maken voor tenants. Deze mag niet worden gebruikt om uit te voeren van de klant workloads en toepassingen. 

## <a name="azure-resource-manager"></a>Azure Resource Manager
Met behulp van Azure Resource Manager kunt u werken met de infrastructuurresources van uw in een model op basis van een sjabloon, declaratieve.   Het biedt één interface waarmee u kunt implementeren en beheren van de oplossingsonderdelen van uw. Zie voor meer informatie en richtlijnen de [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Resourcegroepen
Resourcegroepen zijn verzamelingen van bronnen, services en toepassingen, en elke resource heeft een type, zoals virtuele machines, virtuele netwerken, openbare IP-adressen, storage-accounts en websites. Elke resource moet zich in een resourcegroep en zodat resourcegroepen logisch helpen indelen van bronnen, zoals door de werkbelasting of locatie.  In Microsoft Azure-Stack, worden resources, zoals plannen en biedt ook beheerd in resourcegroepen.
 
### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen
Met Azure Resource Manager kunt u een sjabloon (in JSON-indeling) waarin de implementatie en configuratie van uw toepassing. Deze sjabloon wordt aangeduid als een Azure Resource Manager-sjabloon en biedt een declaratieve manier om implementatie te definiëren. Door het gebruik van een sjabloon kunt u gedurende de levenscyclus van uw app de app herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

## <a name="resource-providers-rps"></a>Resourceproviders (RPs)
Resourceproviders zijn webservices die de basis van alle op basis van Azure IaaS vormen en PaaS-services. Azure Resource Manager, is afhankelijk van andere RPs voor toegang tot services.

Er zijn vier fundamentele RPs: netwerk, opslag, rekencapaciteit en KeyVault. Elk van deze RPs helpt u bij het configureren en beheren van de respectieve bronnen. Servicebeheerders kunnen ook nieuwe aangepaste resourceproviders toevoegen.

### <a name="compute-rp"></a>COMPUTE RP
Compute Resource Provider (CRP) kunt Azure-Stack tenants hun eigen virtuele machines maken. De CRP omvat de mogelijkheid voor het maken van virtuele machines, evenals de uitbreidingen van de virtuele Machine. De virtuele Machine-extensie-service helpt bij het leveren van IaaS-mogelijkheden voor Windows en Linux virtuele machines.  Als u bijvoorbeeld kunt u de CRP een Linux-machine inrichten en Bash-scripts uitvoeren tijdens de implementatie van de virtuele machine te configureren.

### <a name="network-rp"></a>Netwerk RP
Het netwerk Resource Provider (NRP) biedt een serie van Software gedefinieerde netwerkvoorzieningen (SDN) en netwerk functie virtualisatie (NFV) functies voor de privécloud.  U kunt de NRP gebruiken om resources, zoals software load balancers, openbare IP-adressen, netwerkbeveiligingsgroepen, virtuele netwerken te maken.

### <a name="storage-rp"></a>Opslag RP
Het RP-opslag biedt vier consistent zijn met Azure storage-services: blob, table, wachtrij en accountbeheer. Biedt ook een opslagservice voor het beheer van cloud vergemakkelijkt serviceprovider beheer van consistent zijn met Azure Storage-services. Azure Storage biedt de flexibiliteit voor het opslaan en ophalen van grote hoeveelheden ongestructureerde gegevens, zoals documenten en mediabestanden met Azure Blobs en gestructureerde NoSQL op basis van gegevens met Azure-tabellen. Zie voor meer informatie over Azure Storage [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Blob Storage
BLOB-opslag slaat een gegevensset. Een blob kan elk type tekst of binaire gegevens zijn, zoals een document, mediabestand of toepassingsinstallatieprogramma. Table storage worden gestructureerde gegevenssets opgeslagen. Table Storage is een gegevensarchief met NoSQL-sleutelkenmerk, waarmee snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens mogelijk is. Queue storage biedt betrouwbare berichten voor de verwerking van de werkstroom en voor communicatie tussen onderdelen van cloud-services.

Elke blob is georganiseerd in een container. Containers bieden ook een handige manier om beveiligingsbeleid toe te wijzen aan groepen objecten. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs bevatten, tot de capaciteitslimiet van 500 TB van het opslagaccount is bereikt. Blob Storage biedt drie typen blobs: blok-blobs, toevoeg-blobs en pagina-blobs (schijven). Blok-blobs zijn geoptimaliseerd voor streaming en opslag van cloudobjecten en zijn een goede keuze voor het opslaan van documenten, mediabestanden, back-ups enzovoort. Toevoeg-blobs zijn vergelijkbaar met blok-blobs, maar deze zijn geoptimaliseerd voor toevoegbewerkingen. Een toevoeg-blob kan alleen worden bijgewerkt door een nieuw blok aan het eind toe te voegen. Toevoeg-blobs zijn een goede keuze voor scenario's zoals logboekregistratie, waarbij alleen aan het eind van de blob nieuwe gegevens moeten worden geschreven. Pagina-blobs zijn geoptimaliseerd voor de vertegenwoordiging van IaaS-schijven en ondersteuning van willekeurige schrijfbewerkingen en mag maximaal 1 TB. Een op een Azure Virtual Machine-netwerk aangesloten IaaS-schijf is een VHD die is opgeslagen als pagina-blob.

#### <a name="table-storage"></a>Table Storage
Table storage is van Microsoft NoSQL-sleutel/kenmerkopslag – heeft een ontwerp zonder schema's, waardoor het anders dan traditionele relationele databases. Omdat gegevens ontbreken schema's worden opgeslagen, is uw kunt gegevens eenvoudig aanpassen naarmate de behoeften van uw toepassing veranderen. Table Storage is eenvoudig te gebruiken, zodat ontwikkelaars snel toepassingen kunnen maken. Table Storage is een sleutelkenmerkopslag, wat betekent dat elke waarde in een tabel wordt opgeslagen met een getypeerde eigenschapsnaam. De naam van de eigenschap kan worden gebruikt om te filteren en selectiecriteria op te geven. Een verzameling eigenschappen en hun waarden vormen samen een entiteit. Aangezien tabelschema opslag ontbreken, twee entiteiten in dezelfde tabel verschillende verzamelingen eigenschappen kunnen bevatten en deze eigenschappen van verschillende typen kunnen zijn. U kunt Table Storage gebruiken voor het opslaan van flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, apparaatgegevens en alle overige typen metagegevens die uw service nodig heeft. In elke tabel kunt u een willekeurig aantal entiteiten opslaan. Een opslagaccount kan een onbeperkt aantal tabellen bevatten, tot de maximale capaciteit van het opslagaccount.

#### <a name="queue-storage"></a>Queue Storage
Azure Queue Storage biedt uitwisseling van berichten tussen toepassingsonderdelen in de cloud. Bij het ontwerpen van schaalbare toepassingen worden toepassingsonderdelen vaak ontkoppeld, zodat ze onafhankelijk van elkaar kunnen worden geschaald. Queue Storage biedt asynchrone uitwisseling van berichten voor communicatie tussen toepassingsonderdelen, of deze nu worden uitgevoerd in de cloud, op een desktopcomputer, op een on-premises server of op een mobiel apparaat. Queue Storage biedt daarnaast ondersteuning voor het beheren van asynchrone taken en het samenstellen van proceswerkstromen.

### <a name="keyvault"></a>KeyVault
De KeyVault RP biedt beheer en controle van geheimen zoals wachtwoorden en certificaten. Als u bijvoorbeeld kunt een tenant de KeyVault RP beheerderswachtwoorden of sleutels opgeven tijdens de VM-implementatie.

## <a name="role-based-access-control-rbac"></a>Op rollen gebaseerde toegangsbeheer (RBAC)
U kunt gebruikmaken van RBAC om systeemtoegang te verlenen tot gemachtigde gebruikers, groepen en services door het toewijzen van rollen aan een abonnement, resourcegroep of afzonderlijke resource niveau. Elke rol definieert het toegangsniveau dat een gebruiker, groep of -service op de Stack van Microsoft Azure-resources heeft.

Azure RBAC heeft drie basic rollen die voor alle resourcetypen gelden: eigenaar, bijdrager en Reader. Eigenaar heeft volledige toegang tot alle bronnen, waaronder het recht op toegang aan anderen delegeren. Inzender kunt maken en beheren van alle soorten Azure-resources, maar kan geen toegang verlenen tot anderen. Lezer kan alleen bestaande Azure-resources bekijken. De rest van de RBAC-rollen in Azure toestaan van beheer van specifieke Azure-resources. Bijvoorbeeld de rol Inzender van de virtuele Machine kunt maken en beheren van virtuele machines maar staat niet toe dat beheer van het virtuele netwerk of het subnet dat de virtuele machine verbinding maakt.

## <a name="usage-data"></a>Gebruiksgegevens
Microsoft Azure-Stack verzamelt gebruiksgegevens over alle resourceproviders samenvoegt en verzendt naar Azure voor verwerking door Azure commerce. De verzamelde op Azure-Stack gebruiksgegevens kunnen worden weergegeven via een REST-API. Er is een Azure-consistent Tenant-API, evenals Provider overgedragen Provider API's en ophalen van gebruiksgegevens over alle abonnementen van de tenant. Deze gegevens kunnen worden gebruikt om te integreren met een extern hulpprogramma of de service voor facturerings- of terugstorting. Zodra informatie over het gebruik van Azure handel is verwerkt, kan deze worden weergegeven in de Azure portal facturering.

## <a name="in-development-build-of-azure-stack-development-kit"></a>In ontwikkeling build van Azure Stack Development Kit
In ontwikkeling builds kunnen vroege gebruikers evalueren van de meest recente versie van de Azure-Stack Development Kit. Ze zijn incrementele builds op basis van de meest recente grote release. Terwijl primaire versies om enkele maanden uitgebracht blijven, worden de builds in ontwikkeling tijdelijk vrijgeven tussen belangrijke releases.

In ontwikkeling builds wordt de volgende voordelen bieden:
- Oplossingen voor problemen
- Nieuwe functies
- Andere verbeteringen

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor Azure Stack-implementatie](azure-stack-deploy.md)

