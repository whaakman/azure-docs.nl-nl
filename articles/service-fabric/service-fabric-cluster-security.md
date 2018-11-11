---
title: Een Azure Service Fabric-cluster beveiligen | Microsoft Docs
description: Meer informatie over security scenario's voor een Azure Service Fabric-cluster en de verschillende technologieën die u kunt gebruiken voor het implementeren ervan.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: ''
ms.assetid: 26b58724-6a43-4f20-b965-2da3f086cf8a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/14/2018
ms.author: aljo
ms.openlocfilehash: aa0d209cf3da65bb3d50a6458ecc33cfcd85eecb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240593"
---
# <a name="service-fabric-cluster-security-scenarios"></a>Service Fabric-clusterbeveiligingsscenario 's
Een Azure Service Fabric-cluster is een resource waarvan u eigenaar bent. Het is uw verantwoordelijkheid voor het beveiligen van uw clusters om te voorkomen dat onbevoegde gebruikers verbinding maken met deze. Een beveiligd cluster is vooral belangrijk wanneer u bij het uitvoeren van productieworkloads op het cluster. Hoewel het mogelijk te maken van een niet-beveiligd cluster als-beheereindpunten met het openbare internet wordt aangegeven dat het cluster, anonieme gebruikers verbinding mee kunnen maken. Niet-beveiligde clusters worden niet ondersteund voor productieworkloads. 

In dit artikel wordt een overzicht van scenario's voor beveiliging voor Azure-clusters en zelfstandige clusters en de verschillende technologieën die u gebruiken kunt voor het implementeren ervan:

* Beveiliging van knooppunt-naar-knooppunt
* Beveiliging van de client-naar-knooppunt
* RBAC (op rollen gebaseerd toegangsbeheer)

## <a name="node-to-node-security"></a>Beveiliging van knooppunt-naar-knooppunt
Knooppunt-naar-knooppunt security helpt veilige communicatie tussen de virtuele machines of computers in een cluster. In dit scenario voor beveiliging zorgt ervoor dat alleen computers die zijn gemachtigd om lid van het cluster te kunnen deelnemen die als host fungeert voor toepassingen en services in het cluster.

![Diagram van knooppunt-naar-knooppunt communicatie][Node-to-Node]

Clusters die worden uitgevoerd op Azure en zelfstandige clusters die worden uitgevoerd op Windows, beide kunnen beide gebruiken [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows security](https://msdn.microsoft.com/library/ff649396.aspx) voor Windows Server-computers.

### <a name="node-to-node-certificate-security"></a>Knooppunt-naar-knooppunt Certificaatbeveiliging
Service Fabric maakt gebruik van x.509-certificaten die u als onderdeel van de configuratie van het type knooppunt opgeven wanneer u een cluster maakt. Aan het einde van dit artikel ziet u een kort overzicht van wat deze certificaten zijn en hoe u kunt verkrijgen of maken.

Certificaatbeveiliging instellen wanneer u het cluster in Azure portal maakt met behulp van een Azure Resource Manager-sjabloon of met behulp van een zelfstandige JSON-sjabloon. Service Fabric SDK standaardgedrag is om te implementeren en installeer het certificaat met de verst in de toekomst verlopen certificaat; de klassieke gedrag het definiëren van de primaire en secundaire certificaten om toe te staan handmatig gestarte rollovers toegestaan en wordt niet aanbevolen voor gebruik via de nieuwe functionaliteit. De primaire certificaten die worden gebruik heeft verst in de datum in de toekomst verloopt, moet verschillen van de beheerder client en de alleen-lezen-clientcertificaten gebruikt die u instelt voor [client-naar-knooppunt security](#client-to-node-security).

Zie voor meer informatie over het instellen van de Certificaatbeveiliging in een cluster voor Azure, [een cluster instellen met behulp van een Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md).

Zie voor meer informatie over het instellen van de Certificaatbeveiliging in een cluster voor een zelfstandige Windows Server-cluster, [een zelfstandige cluster beveiligen op Windows met behulp van X.509-certificaten](service-fabric-windows-cluster-x509-security.md).

### <a name="node-to-node-windows-security"></a>Knooppunt-naar-knooppunt Windows beveiligingsupdate
Zie voor meer informatie over het instellen van Windows-beveiliging voor een zelfstandige Windows Server-cluster, [een zelfstandige cluster beveiligen op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md).

## <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
Beveiliging van de client-naar-knooppunt clients te verifiëren en helpt veilige communicatie tussen een client en de afzonderlijke knooppunten in het cluster. Dit type beveiliging zorgt ervoor dat alleen gemachtigde gebruikers toegang krijgen tot het cluster en de toepassingen die zijn geïmplementeerd op het cluster. Clients kunnen worden onderscheiden door hun Windows-beveiligingsreferenties of hun beveiligingsreferenties certificaat.

![Diagram van de communicatie van client-naar-knooppunt][Client-to-Node]

Clusters die worden uitgevoerd op Azure en zelfstandige clusters die worden uitgevoerd op Windows, beide kunnen beide gebruiken [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows security](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Client-naar-knooppunt Certificaatbeveiliging
Client-naar-knooppunt Certificaatbeveiliging instellen wanneer u het cluster in Azure portal maakt met behulp van Resource Manager-sjabloon of met behulp van een zelfstandige JSON-sjabloon. Voor het maken van het certificaat, moet u een certificaat voor de Beheerclient of een clientcertificaat voor gebruikers opgeven. Als een best practice, de client en de gebruiker-clientcertificaten van beheerder, u moet verschillen van de primaire en secundaire certificaten die u opgeeft voor [beveiliging van knooppunt-naar-knooppunt](#node-to-node-security). Standaard worden de clustercertificaten voor de beveiliging van knooppunt-naar-knooppunt toegevoegd aan de lijst met toegestane client admin certificaten.

Clients die verbinding met het cluster maken met behulp van het certificaat van de beheerder hebben volledige toegang tot de mogelijkheden voor beheer. Clients die verbinding met het cluster maken met behulp van het clientcertificaat voor alleen-lezen-gebruiker hebben alleen leestoegang tot de mogelijkheden voor beheer. Deze certificaten worden gebruikt voor het RBAC die verderop in dit artikel wordt beschreven.

Zie voor meer informatie over het instellen van de Certificaatbeveiliging in een cluster voor Azure, [een cluster instellen met behulp van een Azure Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md).

Zie voor meer informatie over het instellen van de Certificaatbeveiliging in een cluster voor een zelfstandige Windows Server-cluster, [een zelfstandige cluster beveiligen op Windows met behulp van X.509-certificaten](service-fabric-windows-cluster-x509-security.md).

### <a name="client-to-node-azure-active-directory-security-on-azure"></a>Client-naar-knooppunt Azure Active Directory-beveiliging op Azure
Azure AD kan organisaties (bekend als tenants) voor het beheren van toegang tot toepassingen. Toepassingen worden onderverdeeld in die een webgebaseerde UI voor aanmelden en die met een systeemeigen client-ervaring. Als u nog geen een tenant hebt gemaakt, starten door te lezen [hoe u een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, maakt u twee Azure AD-toepassingen voor het beheren van toegang tot het cluster, een webtoepassing en een systeemeigen toepassing.

Voor clusters die worden uitgevoerd op Azure kunt kunt u ook beveiligen de toegang tot eindpunten voor beheer met behulp van Azure Active Directory (Azure AD). Voor meer informatie over het maken van de vereiste Azure AD-artefacten en hoe u deze invullen bij het maken van het cluster, Zie [instellen van Azure AD om clients te verifiëren](service-fabric-cluster-creation-setup-aad.md).

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging
Azure-clusters en voor de beveiliging van knooppunt-naar-knooppunt, wordt u aangeraden dat u Azure AD-beveiligingsgroep gebruiken om clients en -certificaten te verifiëren.

Voor zelfstandige clusters van WindowsServer, hebt u Windows Server 2012 R2 en Windows Active Directory, raden wij aan dat u Windows-beveiliging met beheerde serviceaccounts voor groepen. Gebruik anders Windows-beveiliging met Windows-accounts.

## <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
U kunt Toegangsbeheer gebruiken om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers. Zo kunt u het cluster beter te beveiligen. Twee access control-typen worden ondersteund voor clients die verbinding met een cluster maken: beheerdersrol en gebruikersrol.

Gebruikers aan wie de rol beheerder toegewezen hebben volledige toegang tot de mogelijkheden voor Computerbeheer, met inbegrip van lezen en schrijven van mogelijkheden. Gebruikers die de gebruikersrol zijn toegewezen standaard alleen leestoegang hebben tot beheermogelijkheden (bijvoorbeeld querymogelijkheden). Ook kunnen ze toepassingen en services oplossen.

De beheerder en gebruiker client rollen ingesteld bij het maken van het cluster. Rollen toewijzen door afzonderlijke identiteiten (bijvoorbeeld met behulp van certificaten of Azure AD) voor elk Roltype. Zie voor meer informatie over instellingen voor toegangsbeheer standaard en het wijzigen van de standaardinstellingen, [Role-Based Access Control voor Service Fabric-clients](service-fabric-cluster-security-roles.md).

## <a name="x509-certificates-and-service-fabric"></a>X.509-certificaten en Service Fabric
Digitale x.509-certificaten worden meestal gebruikt voor verificatie van clients en servers. Ze worden ook gebruikt voor het versleutelen en digitaal ondertekenen van berichten. Service Fabric maakt gebruik van X.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven. Zie voor meer informatie over digitale x.509-certificaten, [werken met certificaten](https://msdn.microsoft.com/library/ms731899.aspx). U gebruikt [Key Vault](../key-vault/key-vault-get-started.md) voor het beheren van certificaten voor Service Fabric-clusters in Azure.

Enkele belangrijke aandachtspunten voor:

* Gebruik voor het maken van certificaten voor clusters die productieworkloads worden uitgevoerd, een correct geconfigureerde certificaatservice van Windows Server of in een goedgekeurde [certificeringsinstantie (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
* Gebruik nooit een tijdelijke of certificaten die u maakt met behulp van hulpprogramma's zoals MakeCert.exe in een productie-omgeving te testen.
* U kunt een zelfondertekend certificaat, maar alleen in een testcluster. Gebruik niet een zelfondertekend certificaat in de productieomgeving.
* Bij het genereren van de certificaatvingerafdruk van het, moet u een vingerafdruk van het SHA1 genereren. SHA1 is wat wordt gebruikt bij het configureren van de vingerafdrukken van Client en het Cluster.

### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Deze certificaten (één primair knooppunt en eventueel een secundaire) zijn vereist voor het beveiligen van een cluster en voorkomt ongeoorloofde toegang tot deze. Deze certificaten bieden cluster en de server-verificatie.

Cluster-verificatie wordt communicatie van knooppunt-naar-knooppunt voor cluster federation geverifieerd. Alleen de knooppunten die hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster. Verificatie van de server verifieert de eindpunten voor clusterbeheer aan een management-client, zodat de management-client weet dat deze communiceert met de echte cluster en niet een 'man in the middle'. Dit certificaat biedt ook een SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS. Wanneer een client of het knooppunt verifieert een knooppunt, een van de initiële controles is de waarde van de algemene naam in de **onderwerp** veld. Deze algemene naam of een van de certificaten alternatieve namen voor onderwerpen (SAN's) moet aanwezig zijn in de lijst met algemene namen zijn.

Het certificaat moet voldoen aan de volgende vereisten:

* Het certificaat moet een persoonlijke sleutel bevatten. Deze certificaten hebben doorgaans extensies .pfx of .pem  
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange (PFX)-bestand.
* De **onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster**. Deze overeenkomst is vereist om te voorzien van een SSL voor de HTTPS-eindpunt voor beheer van het cluster en de Service Fabric Explorer. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de *. cloudapp.azure.com domein. U hebt voor uw cluster een aangepaste domeinnaam nodig. Wanneer u een certificaat van een CA aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Aantal andere zaken te overwegen:

* De **onderwerp** veld meerdere waarden kan hebben. Elke waarde wordt voorafgegaan door een initialisatie om aan te geven van het type. De initialisatie is meestal **CN** (voor *algemene naam*), bijvoorbeeld **CN = www.contoso.com**. 
* De **onderwerp** veld mag leeg zijn. 
* Als de optionele **alternatieve naam voor onderwerp** veld wordt ingevuld, moet de algemene naam van het certificaat en één vermelding per SAN hebben. Deze worden ingevoerd als **DNS-naam** waarden. Zie voor informatie over het genereren van certificaten met SAN's, [een alternatieve naam voor onderwerp toevoegen aan een certificaat voor secure LDAP](https://support.microsoft.com/kb/931351).
* De waarde van de **beoogde doeleinden** veld van het certificaat moet een geschikte waarde, zoals bevatten **serververificatie** of **clientverificatie**.

### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster voor de toepassing uit veiligheidsoverwegingen. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van waarden van de configuratie van toepassing.
* Versleuteling van gegevens tussen knooppunten tijdens de replicatie.

Het concept van het maken van beveiligde clusters is hetzelfde, ongeacht of ze Linux zijn of Windows-clusters.

### <a name="client-authentication-certificates-optional"></a>Certificaten voor clientverificatie (optioneel)
Een willekeurig aantal extra certificaten kan worden opgegeven voor de beheerder of gebruiker clientbewerkingen. De client kan dit certificaat gebruiken als wederzijdse verificatie vereist is. Clientcertificaten worden doorgaans niet uitgegeven door een Certificeringsinstantie van derden. In plaats daarvan bevat het persoonlijke archief van de locatie van de huidige gebruiker meestal clientcertificaten daar geplaatst door een basis-CA. Het certificaat moet een **beoogde doeleinden** waarde van **clientverificatie**.  

Het clustercertificaat heeft standaard beheerdersbevoegdheden voor de client. Deze aanvullende clientcertificaten mag niet worden geïnstalleerd in het cluster, maar zijn opgegeven als wordt toegestaan in de configuratie van het cluster.  De clientcertificaten moeten echter worden geïnstalleerd op de clientcomputers verbinding maken met het cluster en bewerkingen uitvoeren.

> [!NOTE]
> Alle bewerkingen voor de Service Fabric-cluster vereist servercertificaten. Clientcertificaten kunnen niet worden gebruikt voor beheer.

## <a name="next-steps"></a>Volgende stappen
* [Een cluster maken in Azure met behulp van Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md) 
* [Een cluster maken met behulp van Azure portal](service-fabric-cluster-creation-via-portal.md)

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png

[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
