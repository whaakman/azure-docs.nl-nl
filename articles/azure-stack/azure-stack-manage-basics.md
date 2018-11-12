---
title: Basisbeginselen voor het beheer van Azure Stack | Microsoft Docs
description: Meer informatie over wat u moet weten voor het beheer van Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeffgilb
ms.openlocfilehash: 5d3073c3ed499ecdb20243e6db2b217aec3e1448
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51254516"
---
# <a name="azure-stack-administration-basics"></a>Basisbeginselen voor het beheer van Azure Stack
Er zijn verschillende dingen die u weten moet als u geen ervaring met Azure Stack-beheer. Deze handleiding biedt een overzicht van uw rol als Azure Stack-operators, en wat u moet uw gebruikers ze snel worden productief laten weten.

## <a name="understand-the-builds"></a>Inzicht in de builds

### <a name="integrated-systems"></a>Geïntegreerde systemen

Als u een geïntegreerde Azure Stack-systeem, worden de bijgewerkte versies van Azure Stack gedistribueerd via updatepakketten. U kunt deze pakketten importeren en ze toepassen met behulp van de tegel Updates in de beheerdersportal.
 
### <a name="development-kit"></a>Development kit

Als u de Azure Stack Development Kit, raadpleegt u de [wat is Azure Stack?](.\asdk\asdk-what-is.md) artikel om ervoor te zorgen dat u leert wat het doel van de development kit en de beperkingen. Als een "sandbox" waar u kunt Azure Stack, evalueren en ontwikkelt en test uw apps in een niet-productieomgeving moet u de development kit. (Zie voor informatie over de implementatie de [Azure Stack Development Kit implementatie](.\asdk\asdk-install.md) artikel.)

Zoals Azure innoveren we snel. We brengen regelmatig nieuwe builds. Als u de development kit uitvoert en u verplaatsen naar de nieuwste build wilt, moet u [Azure Stack opnieuw implementeren](.\asdk\asdk-redeploy.md). U kunt geen updatepakketten toepassen. Dit proces duurt, maar het voordeel is dat u de nieuwste functies kunt uitproberen. De documentatie development kit op onze website weerspiegelt de nieuwste release-build.

## <a name="learn-about-available-services"></a>Meer informatie over beschikbare services

U moet een bewustzijn van welke services u beschikbaar voor uw gebruikers maken kunt. Azure Stack biedt ondersteuning voor een subset van de Azure-services. De lijst met ondersteunde services blijven zich ontwikkelen.

**Fundamentele services**

Azure Stack omvat standaard de volgende "fundamentele services" wanneer u Azure Stack implementeren:

- Compute
- Storage
- Netwerken
- Key Vault

Met deze fundamentele services, kunt u de infrastructuur-as-a-Service (IaaS) bieden aan uw gebruikers met een minimale configuratie.

**Aanvullende services**

Op dit moment ondersteunen we de volgende extra services voor Platform-as-a-Service (PaaS):

- App Service
- Azure Functions
- SQL- en MySQL-databases

Deze services, wordt er aanvullende configuratie vereisen voordat u deze beschikbaar voor uw gebruikers maken kunt. Zie de "zelfstudies' en de secties 'procedures guides\Offer services' van onze Azure Stack-operator-documentatie voor meer informatie.

**Service-roadmap**

Azure Stack, blijft het toevoegen van ondersteuning voor Azure-services. Zie voor het verwachte schema, de [Azure Stack: een uitbreiding van Azure](https://go.microsoft.com/fwlink/?LinkId=842846&clcid=0x409) technisch document. U kunt ook controleren de [Azure Stack-blogberichten](https://azure.microsoft.com/blog/tag/azure-stack-technical-preview) voor nieuwe meldingen.

## <a name="what-account-should-i-use"></a>Welk account moet ik gebruiken?
Er zijn enkele aandachtspunten voor gebruikersaccounts die u houden moet rekening bij het beheren van Azure Stack. Met name in implementaties met behulp van Windows Server Active Directory Federation Services (AD FS) als de id-provider in plaats van Azure Active Directory (Azure AD). De volgende aandachtspunten voor gebruikersaccounts van toepassing op zowel geïntegreerde Azure Stack-systemen en ASDK implementaties:


|Account|Azure AD|AD FS|
|-----|-----|-----|
|Lokale Administrator (. \Administrator)|ASDK host beheerder|ASDK host beheerder|
|AzureStack\AzureStackAdmin|ASDK host beheerder<br><br>Kan worden gebruikt voor aanmelding bij de beheerportal van Azure Stack<br><br>Toegang tot het weergeven en beheren van Service Fabric-ringen|ASDK host beheerder<br><br>Geen toegang tot de Azure Stack-beheerportal<br><br>Toegang tot het weergeven en beheren van Service Fabric-ringen<br><br>Niet langer eigenaar van de standaard Provider abonnement (DPS)|
|AzureStack\CloudAdmin|Toegang tot en toegestane opdrachten binnen het eindpunt van de bevoegdheden uitvoeren|Toegang tot en toegestane opdrachten binnen het eindpunt van de bevoegdheden uitvoeren<br><br>Kan niet aanmelden bij de host ASDK<br><br>Eigenaar van de Provider standaardabonnement (DPS)|
|Globale Azure AD-beheerder|Tijdens de installatie gebruikt<br><br>Eigenaar van de Provider standaardabonnement (DPS)|Niet van toepassing|
|

## <a name="what-tools-do-i-use-to-manage"></a>Welke hulpprogramma's kan ik gebruiken om te beheren?
 
U kunt de [beheerdersportal](azure-stack-manage-portals.md) of PowerShell voor het beheren van Azure Stack. De eenvoudigste manier voor meer informatie over de basisconcepten is via de portal. Als u PowerShell gebruiken wilt, zijn er stappen voor gegevensvoorbereiding. U moet [installeren](azure-stack-powershell-install.md) PowerShell, [downloaden](azure-stack-powershell-download.md) aanvullende modules en [configureren](azure-stack-powershell-configure-admin.md) PowerShell.

Azure Stack maakt gebruik van Azure Resource Manager als een onderliggende implementatie, beheer en organisatie-mechanisme. Als u Azure Stack beheren en ter ondersteuning van gebruikers, leert u over de Resource Manager. Zie de [aan de slag met Azure Resource Manager](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) technisch document.

## <a name="your-typical-responsibilities"></a>Uw gebruikelijke verantwoordelijkheden

Uw gebruikers willen om services te gebruiken. Vanuit het perspectief is uw belangrijkste rol aan ze beschikbaar stellen van deze services. U moet beslissen welke services te bieden, en die services beschikbaar maken door te plannen, aanbiedingen en quota's maken. Zie voor meer informatie, [overzicht van services in Azure Stack-aanbieding](azure-stack-offer-services-overview.md). 

U moet ook items toevoegen aan [marketplace](azure-stack-marketplace.md), zoals installatiekopieën voor virtuele machines. De eenvoudigste manier is om te [marketplace-items van Azure naar Azure Stack downloaden](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> Als u testen van uw plannen, aanbiedingen en services wilt, moet u de [gebruikersportal](azure-stack-manage-portals.md); niet de beheerdersportal.

Naast het leveren van services, moet u alle normale taken van een operator te houden van Azure Stack actief en werkend uitvoeren. Deze rechten omvatten het volgende:

- Voeg gebruikersaccounts toe (voor [Azure Active Directory](azure-stack-add-new-user-aad.md) implementatie of voor [Active Directory Federation Services](azure-stack-add-users-adfs.md) implementatie)
- [Toegang op rollen gebaseerd beheer (RBAC) rollen toewijzen](azure-stack-manage-permissions.md) (dit is niet beperkt tot administrators.)
- [Status van de groepsbeleidstructuur controleren](azure-stack-monitor-health.md)
- Beheren [netwerk](azure-stack-viewing-public-ip-address-consumption.md) en [opslag](azure-stack-manage-storage-accounts.md) resources
- Vervang de beschadigde hardware, bijvoorbeeld [vervangen door een niet-werkende schijf](azure-stack-replace-disk.md).

## <a name="what-to-tell-your-users"></a>Wat uw gebruikers laten weten

U moet uw gebruikers laten weten hoe u werkt met services in Azure Stack, verbinding maken met de omgeving en hoe u zich kunt abonneren op aanbiedingen. Dat kunt u uw gebruikers, kunt u naast eventuele aangepaste documentatie gebruikers aan de site van de documentatie voor Azure Stack-gebruikers sturen.

**Meer informatie over het werken met services in Azure Stack**

Er is informatie die uw gebruikers begrijpen moeten voordat ze gebruik van services en bouwen van apps in Azure Stack. Er zijn bijvoorbeeld specifieke vereisten voor PowerShell en API-versie. Er zijn ook enkele functie verschillen tussen een service in Azure en de equivalente service in Azure Stack. Zorg ervoor dat uw gebruikers, Raadpleeg de volgende artikelen:

- [Belangrijke overwegingen: met behulp van services of het bouwen van apps voor Azure Stack](user/azure-stack-considerations.md)
- [Overwegingen voor virtuele Machines in Azure Stack](user/azure-stack-vm-considerations.md)
- [Opslag: de verschillen en overwegingen met betrekking tot](user/azure-stack-acs-differences.md)

De informatie in deze artikelen bevat een overzicht van de verschillen tussen een service in Azure en Azure Stack. Vormt een aanvulling op de informatie die beschikbaar is voor een Azure-service in de globale Azure-documentatie.

**Verbinding maken met Azure Stack als een gebruiker**

In een omgeving development kit als een gebruiker heeft geen extern bureaublad-toegang tot de host development kit ze moeten een virtueel particulier netwerk (VPN)-verbinding configureren voordat ze krijgen de Azure Stack tot toegang. Zie [verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md). 

Uw gebruikers willen weten hoe u [toegang tot de gebruikersportal ](user/azure-stack-use-portal.md) of hoe u verbinding maakt via PowerShell. In een omgeving met geïntegreerde systemen verschilt de portal-mailadres van de gebruiker per implementatie. U moet uw gebruikers met de juiste URL opgeven.

Als u PowerShell gebruikt, wordt gebruikers mogelijk resourceproviders registreren voordat ze de services kunnen gebruiken. (Een service een resourceprovider beheren. For example, resources zoals virtuele netwerken, netwerkinterfaces en load balancers worden beheerd door de VPN-resourceprovider.) Ze moeten [installeren](user/azure-stack-powershell-install.md) PowerShell, [downloaden](user/azure-stack-powershell-download.md) aanvullende modules en [configureren](user/azure-stack-powershell-configure-user.md) PowerShell (met registratie van de resourceprovider).

**Abonneren op een aanbieding**

Voordat een gebruiker toegang heeft tot de services, moeten zij [abonneren op een aanbieding](azure-stack-subscribe-plan-provision-vm.md) die u hebt gemaakt als een operator.

## <a name="where-to-get-support"></a>Waar u ondersteuning krijgen

### <a name="integrated-systems"></a>Geïntegreerde systemen

Voor een geïntegreerd systeem, moet u er een gecoördineerd escalatie- en oplossingsproces tussen Microsoft en onze hardwarepartners oorspronkelijke leveranciers (OEM) is.

Als er een probleem met de cloud services, wordt ondersteuning aangeboden via Microsoft de klant ondersteuning klantenondersteuning (CSS). Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuwe ondersteuningsaanvraag**, Hiermee opent u een site waar kunt u rechtstreeks een ondersteuningsaanvraag openen.

Als er een probleem met de implementatie, patch en update, hardware (met inbegrip van veld replaceable units) en eventuele software merkproducten van hardware, zoals software die wordt uitgevoerd op de host van de levenscyclus van hardware contact op met uw hardwareleverancier OEM eerst.

Voor iets anders, neem contact op met Microsoft CSS.

### <a name="development-kit"></a>Development kit

Voor de development kit, kunt u vragen met betrekking tot ondersteuning vragen in de [Microsoft forums](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). Als u klikt u op het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en klik vervolgens op **nieuwe ondersteuningsaanvraag**, Hiermee opent u de forums-site rechtstreeks. Deze forums worden regelmatig gecontroleerd. Omdat de development kit een evaluatieomgeving is, is er geen officiële ondersteuning aangeboden via Microsoft CSS.

## <a name="next-steps"></a>Volgende stappen

[Regiobeheer in Azure Stack](azure-stack-region-management.md)


