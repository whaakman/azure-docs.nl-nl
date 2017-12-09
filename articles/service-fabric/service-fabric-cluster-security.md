---
title: Een Azure Service Fabric-cluster beveiligen | Microsoft Docs
description: "Meer informatie over security scenario's voor een Azure Service Fabric-cluster en de verschillende technologieën die u kunt gebruiken voor het implementeren ervan."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/07/2017
ms.author: chackdan
ms.openlocfilehash: f0fdbd7fc4ec48037371ffa296cf668897e45b70
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="service-fabric-cluster-security-scenarios"></a>Scenario's voor beveiliging van service Fabric-cluster
Een Azure Service Fabric-cluster is een resource waarvan u eigenaar. Het is uw verantwoordelijkheid voor het beveiligen van clusters om te voorkomen dat onbevoegde gebruikers verbinding maken met deze. Een beveiligde cluster is vooral belangrijk wanneer u productieworkloads op het cluster worden uitgevoerd. Hoewel het mogelijk te maken van een niet-beveiligde cluster als het cluster eindpunten voor beheer met het openbare internet beschrijft, anonieme gebruikers verbinding mee kunnen maken. Niet-beveiligde clusters worden niet ondersteund voor productieworkloads. 

Dit artikel is een overzicht van scenario's voor beveiliging voor Azure-clusters en zelfstandige clusters en de verschillende technologieën die u gebruiken kunt voor het implementeren ervan:

* Knooppunt naar beveiliging
* Beveiliging van de client-naar-knooppunt
* RBAC (op rollen gebaseerd toegangsbeheer)

## <a name="node-to-node-security"></a>Knooppunt naar beveiliging
Knooppunt naar beveiliging kunt u veilige communicatie tussen de virtuele machines of computers in een cluster. Dit scenario beveiliging zorgt ervoor dat alleen de computers die zijn gemachtigd om lid van het cluster te kunnen deelnemen die als host fungeert voor toepassingen en services in het cluster.

![Diagram van knooppunt naar communicatie][Node-to-Node]

Clusters met op Azure en zelfstandige cluster waarop Windows beide gebruikt een [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx) voor Windows Server-computers.

### <a name="node-to-node-certificate-security"></a>Beveiliging van een knooppunt naar certificate
Service Fabric maakt gebruik van X.509-servercertificaten die u als onderdeel van de configuratie van het type knooppunt opgeeft wanneer u een cluster maakt. Aan het einde van dit artikel ziet u een kort overzicht van wat deze certificaten zijn en hoe u kunt verkrijgen of ze maken.

Certificaatbeveiliging instellen wanneer u het cluster in de Azure-portal met behulp van een Azure Resource Manager-sjabloon of met behulp van een zelfstandige JSON-sjabloon maakt. U kunt een primaire certificaat en een optionele secundaire certificaat, dat wordt gebruikt voor het certificaat rollovers instellen. De primaire en secundaire certificaten die u moet verschillen van de Beheerclient en alleen-lezen-clientcertificaten die u instelt voor [clientknooppunt beveiliging](#client-to-node-security).

Zie voor meer informatie over het instellen van Certificaatbeveiliging in een cluster voor Azure, [een cluster met behulp van een Azure Resource Manager-sjabloon instellen](service-fabric-cluster-creation-via-arm.md).

Zie voor meer informatie over het instellen van het Certificaatbeveiliging in een cluster voor een zelfstandige Windows Server-cluster, [beveiligen van een zelfstandige cluster in Windows met behulp van X.509-certificaten](service-fabric-windows-cluster-x509-security.md).

### <a name="node-to-node-windows-security"></a>Knooppunt naar Windows-beveiliging
Zie voor meer informatie over het instellen van Windows-beveiliging voor een zelfstandige Windows Server-cluster, [beveiligen van een zelfstandige cluster in Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md).

## <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
Beveiliging van de client-naar-node clients geverifieerd en helpt veilige communicatie tussen een client en de afzonderlijke knooppunten in het cluster. Dit type beveiliging zorgt ervoor dat alleen geautoriseerde gebruikers toegang tot hebben het cluster en de toepassingen die zijn geïmplementeerd op het cluster. Clients via hun Windows-referenties voor beveiliging of hun beveiligingsreferenties certificaat uniek geïdentificeerd.

![Diagram van communicatie van client-naar-knooppunt][Client-to-Node]

Clusters met op Azure en zelfstandige cluster waarop Windows beide gebruikt een [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>De beveiliging van client-naar-node certificate
Certificaatbeveiliging van de client tot knooppunt instellen wanneer u het cluster in de Azure-portal met behulp van een Resource Manager-sjabloon of met behulp van een zelfstandige JSON-sjabloon maakt. Geef een clientcertificaat voor de beheerder of een clientcertificaat voor gebruikers voor het maken van het certificaat. Als een best practice, de client en de gebruiker beheerclientcertificaten u opgeeft moet verschillen van de primaire en secundaire certificaten die u opgeeft voor [knooppunt naar beveiliging](#node-to-node-security). Standaard worden de clustercertificaten voor knooppunt naar beveiliging toegevoegd aan de lijst met toegestane client admin certificaten.

Clients die verbinding met het cluster met behulp van de admin-certificaat hebben volledige toegang tot de beheerfuncties. Clients die verbinding met het cluster met behulp van het clientcertificaat voor de gebruiker alleen-lezen hebben alleen leestoegang tot de beheerfuncties. Deze certificaten worden gebruikt voor het RBAC die verderop in dit artikel wordt beschreven.

Zie voor meer informatie over het instellen van Certificaatbeveiliging in een cluster voor Azure, [een cluster met behulp van een Azure Resource Manager-sjabloon instellen](service-fabric-cluster-creation-via-arm.md).

Zie voor meer informatie over het instellen van het Certificaatbeveiliging in een cluster voor een zelfstandige Windows Server-cluster, [beveiligen van een zelfstandige cluster in Windows met behulp van X.509-certificaten](service-fabric-windows-cluster-x509-security.md).

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Client-naar-node Azure Active Directory-beveiligingsgroep op Azure
Voor clusters die zijn uitgevoerd op Azure, kunt ook u beveiligen toegang tot eindpunten voor beheer met behulp van Azure Active Directory (Azure AD). Voor meer informatie over het maken van de vereiste Azure AD-artefacten, hoe deze te vullen wanneer u het cluster en verbinding maken met de clusters daarna maakt, Zie [een cluster met behulp van een Azure Resource Manager-sjabloon instellen](service-fabric-cluster-creation-via-arm.md).

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging
Azure-clusters, voor een knooppunt naar beveiliging wordt u aangeraden dat u Azure AD-beveiligingsgroep gebruiken om clients en -certificaten te verifiëren.

Voor zelfstandige WindowsServer-clusters, als u Windows Server 2012 R2 en Windows Active Directory, raden we aan dat u Windows-beveiliging met beheerde serviceaccounts voor groepen. Gebruik anders Windows-beveiliging met Windows-accounts.

## <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt Toegangsbeheer gebruiken om te beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers. Zo kunt u het cluster beter kunt beveiligen. Twee access control-typen worden ondersteund voor clients die verbinding met een cluster maken: beheerdersrol en gebruikersrol.

Gebruikers die zijn toegewezen aan de rol beheerder hebben volledige toegang tot beheermogelijkheden, waaronder lezen en schrijven van mogelijkheden. Gebruikers die de gebruikersrol zijn toegewezen standaard alleen leestoegang hebben tot beheermogelijkheden (bijvoorbeeld querymogelijkheden). Ook kunnen ze toepassingen en services oplossen.

De beheerder en gebruiker client rollen ingesteld bij het maken van het cluster. Rollen toewijzen door afzonderlijke identiteiten (bijvoorbeeld met behulp van certificaten of Azure AD) voor elk Roltype. Zie voor meer informatie over instellingen voor toegangsbeheer standaard en het wijzigen van de standaardinstellingen, [toegangsbeheer op basis van rollen voor Service Fabric-clients](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-certificaten en Service Fabric
Digitale x.509-certificaten worden meestal gebruikt voor verificatie van clients en servers. Ze worden ook gebruikt voor het versleutelen en digitaal ondertekenen van berichten. Zie voor meer informatie over digitale x.509-certificaten, [werken met certificaten](http://msdn.microsoft.com/library/ms731899.aspx).

Een aantal belangrijke zaken te overwegen:

* Gebruik voor het maken van certificaten voor clusters die worden uitgevoerd productieworkloads een juist geconfigureerde service voor Windows Server-certificaat of een document uit een goedgekeurde [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Gebruik nooit een tijdelijke en testen van certificaten die u maakt met behulp van hulpprogramma's zoals MakeCert.exe in een productieomgeving.
* U kunt een zelfondertekend certificaat gebruiken maar alleen in een testcluster. Gebruik een zelfondertekend certificaat niet in de productieomgeving.

### <a name="server-x509-certificates"></a>X.509-certificaten van server
Servercertificaten zijn de belangrijkste taak van een server (knooppunt) om clients te verifiëren of verificatie van een server (knooppunt) naar een server (knooppunt). Wanneer een client of het knooppunt verifieert een knooppunt, een van de eerste controles is de waarde van de algemene naam in de **onderwerp** veld. Deze algemene naam of een van de certificaten alternatieve onderwerpnamen () moet aanwezig zijn in de lijst met algemene namen.

Zie voor informatie over het genereren van certificaten met SAN's, [een alternatieve naam voor onderwerp toevoegen aan een beveiligde LDAP-certificaat](http://support.microsoft.com/kb/931351).

De **onderwerp** veld meerdere waarden kan hebben. Elke waarde wordt voorafgegaan door een initialisatie om aan te geven het waardetype. De initialisatie is meestal **CN** (voor *algemene naam*), bijvoorbeeld **CN = www.contoso.com**. De **onderwerp** veld kan niet leeg zijn. Als de optionele **alternatieve naam voor onderwerp** veld wordt ingevuld, moet de algemene naam van het certificaat en één vermelding per SAN hebben. Deze worden ingevoerd als **DNS-naam** waarden.

De waarde van de **beoogde doeleinden** veld van het certificaat moet een geschikte waarde, zoals omvatten **serververificatie** of **clientverificatie**.

### <a name="client-x509-certificates"></a>Client X.509-certificaten
Clientcertificaten worden doorgaans niet uitgegeven door een CA van derden. In plaats daarvan het persoonlijke archief van de locatie van de huidige gebruiker geplaatst door een basiscertificeringsinstantie met clientcertificaten doorgaans bevat een **beoogde doeleinden** waarde van **clientverificatie**. De client kan dit certificaat gebruiken als wederzijdse verificatie vereist is.

> [!NOTE]
> Alle beheerbewerkingen op een Service Fabric-cluster vereist servercertificaat. Clientcertificaten kunnen niet worden gebruikt voor beheer.

## <a name="next-steps"></a>Volgende stappen
* [Maken van een cluster in Azure met behulp van een Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) 
* [Maken van een cluster met behulp van de Azure-portal](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
