---
title: Azure API Management Veelgestelde vragen | Microsoft Docs
description: Meer informatie over de antwoorden op veelgestelde vragen (FAQ) patronen en aanbevolen procedures in Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 1903655a262583f1ba78b728bf404a81278e2275
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-api-management-faqs"></a>Azure API Management-Veelgestelde vragen
De antwoorden op veelgestelde vragen, patronen en aanbevolen procedures voor Azure API Management.

## <a name="contact-us"></a>Contact opnemen
* [Hoe kan ik vraag het team van Microsoft Azure API Management?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
* [Wat betekent het als een functie in preview?](#what-does-it-mean-when-a-feature-is-in-preview)
* [Hoe kan ik de verbinding tussen de API Management-gateway en Mijn back-end-services beveiligen?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Hoe Kopieer mijn API Management-service-exemplaar naar een nieuw exemplaar?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [Kan ik mijn exemplaar van API Management programmatisch beheren?](#can-i-manage-my-api-management-instance-programmatically)
* [Hoe kan ik een gebruiker aan de groep Administrators toevoegen](#how-do-i-add-a-user-to-the-administrators-group)
* [Waarom is het beleid dat ik niet beschikbaar in de beleidseditor wilt toevoegen?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Hoe stel ik omgevingen met meerdere in één API](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [Kan ik SOAP gebruiken met API Management?](#can-i-use-soap-with-api-management)
* [De constante API Management gateway-IP-adres is? Kan ik deze in firewallregels gebruiken?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Kan ik een OAuth 2.0-autorisatie-server configureren met AD FS-beveiliging?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [Welke methode voor het doorsturen gebruikt API Management in implementaties van meerdere geografische locaties?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [Kan ik een Azure Resource Manager-sjabloon gebruiken voor het maken van een service-exemplaar van API Management?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Kan ik een zelf-ondertekend SSL-certificaat gebruiken voor een back-end](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Waarom krijg ik een verificatiefout opgetreden wanneer ik wil een GIT-opslagplaats klonen?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [Werkt API Management met Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
* [Waarom hebben we een specifieke subnet in het Resource Manager-stijl VNETs nodig wanneer API Management wordt geïmplementeerd in deze?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [Wat is de minimale subnetgrootte nodig bij het implementeren van API Management in een VNET?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [Kan ik een API Management-service uit één abonnement verplaatsen naar een andere?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Zijn er beperkingen op of bekende problemen met mijn API importeren?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Hoe kan ik vraag het team van Microsoft Azure API Management?
U kunt contact met ons opnemen via een van de volgende opties:

* Stel uw vragen in onze [API Management MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* Een e-mail sturen naar < mailto:apimgmt@microsoft.com >.
* Stuur ons een functie-aanvraag de [Azure Feedbackforum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Wat betekent het als een functie in preview?
Wanneer een functie in preview is, betekent dit dat we op zoek bent actief naar feedback over hoe de functie voor u werkt. Er is een functie in preview functioneel voltooid, maar het is mogelijk dat maken we een recente wijziging in reactie op feedback van klanten. Het is raadzaam dat u niet afhankelijk zijn van een functie die in het voorbeeld in uw productieomgeving. Als u feedback op de preview-functies hebt, laat ons weten via een van de opties voor contactpersonen in [hoe kan ik vraag het team van Microsoft Azure API Management?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Hoe kan ik de verbinding tussen de API Management-gateway en Mijn back-end-services beveiligen?
U hebt verschillende mogelijkheden voor het beveiligen van de verbinding tussen de API Management-gateway en de back-end-services. U kunt:

* Gebruik HTTP-basisverificatie. Zie voor meer informatie [importeren en publiceren van uw eerste API](import-and-publish.md).
* Gebruik van wederzijdse verificatie van SSL, zoals beschreven in [het beveiligen van back-end-services met behulp van client certificaatverificatie in Azure API Management](api-management-howto-mutual-certificates.md).
* IP-whitelisting op uw back-end-service gebruiken. In alle lagen van API Management, het IP-adres van de gateway constant is gebleven, met een paar [voorbehoud](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules). U kunt uw lijst met geaccepteerde toestaan dit IP-adres instellen. Op het Dashboard in de Azure portal kunt u het IP-adres van uw exemplaar van API Management ophalen.
* Verbinding maken met uw API Management-exemplaar met een virtueel netwerk van Azure.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Hoe Kopieer mijn API Management-service-exemplaar naar een nieuw exemplaar?
U hebt verschillende mogelijkheden als u wilt kopiëren van een API Management-exemplaar naar een nieuw exemplaar. U kunt:

* Gebruik de back-up en herstellen van de functie in API Management. Zie voor meer informatie [herstel na noodgevallen implementeren met behulp van service back-up en herstellen in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
* Maak uw eigen back-up en herstellen van de functie met behulp van de [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). De REST-API opslaan en terugzetten van de entiteiten van de service-exemplaar dat u wilt gebruiken.
* Configuratie van de service te downloaden met behulp van Git en upload het naar een nieuw exemplaar. Zie voor meer informatie [opslaan en de configuratie van uw API Management-service configureren met behulp van Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kan ik mijn exemplaar van API Management programmatisch beheren?
Ja, kunt u API Management programmatisch beheren door gebruik te maken:

* De [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* De [Beheerbibliotheek ApiManagement Service van Microsoft Azure SDK](http://aka.ms/apimsdk).
* De [implementatie](https://msdn.microsoft.com/library/mt619282.aspx) en [servicebeheer](https://msdn.microsoft.com/library/mt613507.aspx) PowerShell-cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Hoe kan ik een gebruiker aan de groep Administrators toevoegen
Hier ziet u hoe u een gebruiker kunt toevoegen aan de groep Administrators:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar de resourcegroep die het API Management-exemplaar dat u wilt bijwerken.
3. Wijs in API Management de **Api Management Inzender** rol aan de gebruiker.

Nu de zojuist toegevoegde Inzender kan Azure PowerShell gebruiken [cmdlets](https://msdn.microsoft.com/library/mt613507.aspx). Ga als volgt aanmelden als beheerder:

1. Gebruik de `Login-AzureRmAccount` cmdlet aan te melden.
2. De context ingesteld op het abonnement waarvoor de service met behulp van `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Ophalen van een URL met eenmalige aanmelding met behulp van `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. De URL gebruiken voor toegang tot de beheerportal.

### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Waarom is het beleid dat ik niet beschikbaar in de beleidseditor wilt toevoegen?
Als het beleid dat u wilt toevoegen wordt weergegeven, grijs of grijs weergegeven in de beleidseditor, zorg ervoor dat u in het juiste bereik voor het beleid. Elke instructie beleid is bedoeld voor u met specifieke bereiken en beleid-secties. De secties van beleid en de bereiken voor een beleid Zie sectie voor het gebruik van het beleid in [API Management-beleidsregels](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hoe stel ik omgevingen met meerdere in één API
Als u wilt instellen omgevingen met meerdere, bijvoorbeeld een testomgeving en een productie-omgeving in één API, hebt u twee opties. U kunt:

* Host verschillende API's op dezelfde tenant.
* De dezelfde API's op verschillende tenants hosten.

### <a name="can-i-use-soap-with-api-management"></a>Kan ik SOAP gebruiken met API Management?
[SOAP-pass-through](http://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) ondersteuning is nu beschikbaar. Beheerders kunnen de WSDL van de SOAP-service importeren en Azure API Management maakt een SOAP-front-end. Portal-documentatie voor ontwikkelaars, test-console, beleid en analytics zijn allemaal beschikbaar voor de SOAP-services.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>De constante API Management gateway-IP-adres is? Kan ik deze in firewallregels gebruiken?
In alle lagen van API Management is het openbare IP-adres (VIP) van de API Management-tenant met een paar uitzonderingen statisch gedurende de levensduur van de tenant. De IP-adreswijzigingen in deze omstandigheden:

* De service is verwijderd en opnieuw gemaakt.
* Het service-abonnement [onderbroken](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) of [gewaarschuwd](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (bijvoorbeeld voor nonpayment) en vervolgens opnieuw ingesteld.
* U toevoegen of verwijderen van Azure Virtual Network (u kunt virtuele netwerk alleen op de ontwikkelaar en Premium-laag).

Voor implementaties met meerdere landen/regio, het regionale adres verandert als de regio is leeggemaakt en vervolgens opnieuw ingesteld (u kunt meerdere landen/regio-implementatie alleen op de laag Premium gebruiken).

Premium-laag tenants die zijn geconfigureerd voor de implementatie van meerdere landen/regio kan één openbaar IP-adres per regio zijn toegewezen.

Op de pagina van de tenant in de Azure portal kunt u uw IP-adres (of de adressen in een implementatie met meerdere landen/regio) ophalen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kan ik een OAuth 2.0-autorisatie-server configureren met AD FS-beveiliging?
Zie voor meer informatie over het configureren van een OAuth 2.0-autorisatie-server met Active Directory Federation Services (AD FS), beveiliging, [met behulp van AD FS in API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welke methode voor het doorsturen gebruikt API Management in implementaties van meerdere geografische locaties?
API Management maakt gebruik van de [prestaties methode voor het doorsturen van verkeer](../traffic-manager/traffic-manager-routing-methods.md#priority) in implementaties van meerdere geografische locaties. Binnenkomend verkeer wordt doorgestuurd naar de dichtstbijzijnde API-gateway. Als één regio offline gaat, wordt automatisch binnenkomend verkeer naar de volgende dichtstbijzijnde gateway gerouteerd. Meer informatie over methoden voor het doorsturen in [methoden voor het doorsturen van Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kan ik een Azure Resource Manager-sjabloon gebruiken voor het maken van een service-exemplaar van API Management?
Ja. Zie de [Azure API Management-Service](http://aka.ms/apimtemplate) Quick Start-sjablonen.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kan ik een zelf-ondertekend SSL-certificaat gebruiken voor een back-end
Ja. Dit kan worden gedaan via PowerShell of door rechtstreeks verzenden naar de API. Hiermee wordt de validatie van certificaatketen uitschakelen en kunt u zelf-ondertekend of privé ondertekend om certificaten te gebruiken tijdens de communicatie van API Management met de back-end-services.

#### <a name="powershell-method"></a>PowerShell-methode ####
Gebruik de [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (voor de nieuwe back-end) of [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (voor bestaande back-end) PowerShell-cmdlets en stel de `-SkipCertificateChainValidation` -parameter voor `True`. 

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

#### <a name="direct-api-update-method"></a>Directe methode voor API-update ####
1. Maak een [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) entiteit met behulp van API Management.       
2. Stel de **skipCertificateChainValidation** eigenschap **true**.     
3. Als u niet langer toestaan dat zelfondertekende certificaten wilt, de back-end-entiteit verwijderen of stel de **skipCertificateChainValidation** eigenschap **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Waarom krijg ik een verificatiefout opgetreden wanneer ik wil een Git-opslagplaats klonen?
Als u Git Referentiebeheer gebruiken of als u een Git-opslagplaats klonen probeert met behulp van Visual Studio, kunt u in een bekend probleem met het dialoogvenster Windows-referenties uitvoeren. Het dialoogvenster beperkt wachtwoordlengte 127 tekens en het wachtwoord Microsoft gegenereerd worden afgekapt. We werken aan verkorten van het wachtwoord. Gebruik Git Bash de Git-opslagplaats klonen op dit moment.

### <a name="does-api-management-work-with-azure-expressroute"></a>Werkt API Management met Azure ExpressRoute?
Ja. API Management werkt met Azure ExpressRoute.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Waarom hebben we een specifieke subnet in het Resource Manager-stijl VNETs nodig wanneer API Management wordt geïmplementeerd in deze?
De vereiste toegewezen subnet voor API Management zijn afkomstig van het feit dat deze is gebaseerd op (PAAS-V1-laag) klassieke implementatiemodel. We kunnen implementeren in een Resource Manager VNET (V2 layer), maar er zijn die gevolgen. Het klassieke implementatiemodel in Azure is nauw niet verbonden met het Resource Manager-model en dus als u een resource in V2-laag maakt, laag V1 niet kent het en problemen kunnen optreden, zoals het gebruik van een IP-adres dat al is toegewezen aan een NIC-API-beheer  (gebaseerd op V2).
Voor meer informatie over het verschil tussen het klassieke en het Resource Manager-modellen in Azure verwijzen naar [verschil in implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>Wat is de minimale subnetgrootte nodig bij het implementeren van API Management in een VNET?
De subnetgrootte van de minimale die nodig zijn voor het implementeren van API Management is [/29](../virtual-network/virtual-networks-faq.md#configuration), dit is de minimale subnetgrootte die ondersteuning biedt voor Azure.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kan ik een API Management-service uit één abonnement verplaatsen naar een andere?
Ja. Voor meer informatie Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Zijn er beperkingen op of bekende problemen met mijn API importeren?
[Bekende problemen en beperkingen](api-management-api-import-restrictions.md) voor Open API(Swagger) WSDL en WADL indelingen.
