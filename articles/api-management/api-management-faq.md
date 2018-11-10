---
title: Azure API Management Veelgestelde vragen over | Microsoft Docs
description: Informatie over de antwoorden op veelgestelde vragen (FAQ), patronen en best practices in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 760feae2c9b58e162dae487e240dda72099ed91b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227974"
---
# <a name="azure-api-management-faqs"></a>Veelgestelde vragen over de Azure API Management
De antwoorden op veelgestelde vragen, patronen en best practices voor Azure API Management.

## <a name="contact-us"></a>Contact opnemen
* [Hoe kan ik van de Microsoft Azure API Management-team een vraag stellen?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen
* [Wat betekent het dat als een functie beschikbaar als preview is?](#what-does-it-mean-when-a-feature-is-in-preview)
* [Hoe kan ik de verbinding tussen de API Management-gateway en mijn back-endservices beveiligen?](#how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services)
* [Hoe kopieer ik mijn API Management service-exemplaar naar een nieuw exemplaar?](#how-do-i-copy-my-api-management-service-instance-to-a-new-instance)
* [Kan ik mijn API Management-exemplaar via een programma beheren?](#can-i-manage-my-api-management-instance-programmatically)
* [Hoe voeg ik een gebruiker aan de groep Administrators?](#how-do-i-add-a-user-to-the-administrators-group)
* [Waarom is het beleid dat ik niet beschikbaar in de beleidseditor wilt toevoegen?](#why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor)
* [Hoe stel ik meerdere omgevingen in één API?](#how-do-i-set-up-multiple-environments-in-a-single-api)
* [Kan ik SOAP gebruiken met API Management?](#can-i-use-soap-with-api-management)
* [De constante API Management gateway-IP-adres is? Kan ik deze in de firewall-regels gebruiken?](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules)
* [Kan ik een OAuth 2.0-autorisatie-server configureren met AD FS-beveiliging?](#can-i-configure-an-oauth-20-authorization-server-with-adfs-security)
* [Welke routeringsmethode gebruikt API Management in implementaties van verschillende geografische locaties?](#what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations)
* [Kan ik een Azure Resource Manager-sjabloon gebruiken voor het maken van een exemplaar van API Management-service?](#can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance)
* [Kan ik een zelf-ondertekend SSL-certificaat voor een back-end gebruiken?](#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)
* [Waarom krijg ik een verificatiefout wanneer ik probeer om een gitopslagplaats te klonen?](#why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository)
* [Werkt de API Management met Azure ExpressRoute?](#does-api-management-work-with-azure-expressroute)
* [Waarom hebben we een toegewezen subnet in Resource Manager-stijl VNETs nodig wanneer u API Management wordt geïmplementeerd in deze?](#why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them)
* [Wat is de minimale subnetgrootte die nodig zijn bij het implementeren van API Management in een VNET?](#what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet)
* [Kan ik een API Management-service van één abonnement verplaatsen naar een andere?](#can-i-move-an-api-management-service-from-one-subscription-to-another)
* [Zijn er beperkingen met betrekking tot of bekende problemen met het importeren van mijn API?](#are-there-restrictions-on-or-known-issues-with-importing-my-api)

### <a name="how-can-i-ask-the-microsoft-azure-api-management-team-a-question"></a>Hoe kan ik van de Microsoft Azure API Management-team een vraag stellen?
U kunt contact met ons opnemen met behulp van een van de volgende opties:

* Plaats uw vraag in onze [API Management MSDN-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azureapimgmt).
* Stuur een e-mail naar <mailto:apimgmt@microsoft.com>.
* Stuur ons een functie-aanvraag de [Azure-Feedbackforum](https://feedback.azure.com/forums/248703-api-management).

### <a name="what-does-it-mean-when-a-feature-is-in-preview"></a>Wat betekent het dat als een functie beschikbaar als preview is?
Wanneer een functie in preview is, betekent dit dat we op zoek bent actief naar feedback over hoe de functie voor u werkt. Een functie in preview is functioneel is voltooid, maar het is mogelijk dat we maken een belangrijke wijziging in reactie op feedback van klanten. Het is raadzaam dat u niet afhankelijk van een functie die een Preview-versie in uw productieomgeving. Als u feedback over de preview-functies hebt, laat het ons weten via een van de opties voor contactpersonen in [hoe kan ik een vraag stellen het team van Microsoft Azure API Management?](#how-can-i-ask-the-microsoft-azure-api-management-team-a-question).

### <a name="how-can-i-secure-the-connection-between-the-api-management-gateway-and-my-back-end-services"></a>Hoe kan ik de verbinding tussen de API Management-gateway en Mijn back-end-services beveiligen?
U hebt verschillende mogelijkheden voor het beveiligen van de verbinding tussen de API Management-gateway en uw back-end-services. U kunt:

* Gebruik HTTP-basisverificatie. Zie voor meer informatie, [importeren en publiceren van uw eerste API](import-and-publish.md).
* Gebruik van wederzijdse verificatie van SSL, zoals beschreven in [over het beveiligen van back-end-services met behulp van client-certificaatverificatie in Azure API Management](api-management-howto-mutual-certificates.md).
* IP-whitelists gebruiken voor uw back-end-service. In alle categorieën van API Management wordt het IP-adres van de gateway ongewijzigd, met een paar [onder voorbehoud](#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules). U kunt uw lijst met geaccepteerde toestaan dit IP-adres instellen. U kunt het IP-adres van uw API Management-exemplaar ophalen op het Dashboard in Azure portal.
* Verbinding maken met uw exemplaar van API Management met een Azure-netwerk.

### <a name="how-do-i-copy-my-api-management-service-instance-to-a-new-instance"></a>Hoe kopieer ik mijn API Management service-exemplaar naar een nieuw exemplaar?
U hebt verschillende opties als u wilt kopiëren van een API Management-exemplaar naar een nieuw exemplaar. U kunt:

* Gebruik de back-up en herstellen van de functie in API Management. Zie voor meer informatie, [herstel na noodgevallen implementeren met behulp van service back-up en herstellen in Azure API Management](api-management-howto-disaster-recovery-backup-restore.md).
* Maak uw eigen back-up en herstellen van de functie met behulp van de [API Management REST API](https://msdn.microsoft.com/library/azure/dn776326.aspx). De REST-API voor het opslaan en terugzetten van de entiteiten van de service-exemplaar dat u wilt gebruiken.
* Configuratie van de service met behulp van Git downloaden en vervolgens te uploaden naar een nieuw exemplaar. Zie voor meer informatie, [opslaan en de configuratie van uw API Management-service configureren met behulp van Git](api-management-configuration-repository-git.md).

### <a name="can-i-manage-my-api-management-instance-programmatically"></a>Kan ik mijn API Management-exemplaar via een programma beheren?
Ja, kunt u de API Management via een programma beheren met behulp van:

* De [API Management REST-API](https://msdn.microsoft.com/library/azure/dn776326.aspx).
* De [Beheerbibliotheek ApiManagement-Service van Microsoft Azure SDK](https://aka.ms/apimsdk).
* De [implementatie](https://docs.microsoft.com/powershell/module/wds) en [servicebeheer](https://docs.microsoft.com/powershell/azure/servicemanagement/overview) PowerShell-cmdlets.

### <a name="how-do-i-add-a-user-to-the-administrators-group"></a>Hoe voeg ik een gebruiker aan de groep Administrators?
Hier ziet u hoe u een gebruiker kunt toevoegen aan de groep Administrators:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar de resourcegroep met de API Management-exemplaar dat u wilt bijwerken.
3. In API Management, wijzen de **Inzender voor Api Management** rol aan de gebruiker.

Nu de pas toegevoegde Inzender kunt Azure PowerShell gebruiken [cmdlets](https://docs.microsoft.com/powershell/azure/overview). Dit is hoe u zich aanmelden als beheerder:

1. Gebruik de `Connect-AzureRmAccount` cmdlet aan te melden.
2. Stelt u de context voor het abonnement waarvoor u de service met behulp van `Set-AzureRmContext -SubscriptionID <subscriptionGUID>`.
3. Een URL met eenmalige aanmelding ophalen met behulp van `Get-AzureRmApiManagementSsoToken -ResourceGroupName <rgName> -Name <serviceName>`.
4. Gebruik de URL voor toegang tot de beheerportal.

### <a name="why-is-the-policy-that-i-want-to-add-unavailable-in-the-policy-editor"></a>Waarom is het beleid dat ik niet beschikbaar in de beleidseditor wilt toevoegen?
Als het beleid dat u wilt toevoegen wordt grijs weergegeven of ervoor dat u het juiste bereik voor het beleid wordt grijs weergegeven in de beleidseditor, worden weergegeven. Elke beleidsverklaring is ontworpen voor gebruik in specifieke scopes en -beleid voor secties. De secties van beleid en bereiken voor een beleid, Zie sectie voor het gebruik in van het beleid [API Management-beleidsregels](https://msdn.microsoft.com/library/azure/dn894080.aspx).

### <a name="how-do-i-set-up-multiple-environments-in-a-single-api"></a>Hoe stel ik meerdere omgevingen in één API?
Om in te stellen meerdere omgevingen, bijvoorbeeld een testomgeving en een productie-omgeving in één API, hebt u twee opties. U kunt:

* Host verschillende API's op dezelfde tenant.
* Dezelfde API's op verschillende tenants hosten.

### <a name="can-i-use-soap-with-api-management"></a>Kan ik SOAP gebruiken met API Management?
[SOAP-Passthrough](https://blogs.msdn.microsoft.com/apimanagement/2016/10/13/soap-pass-through/) ondersteuning is nu beschikbaar. Beheerders kunnen de WSDL van de SOAP-service importeren en Azure API Management maakt u een SOAP-front-end. Portal-documentatie voor ontwikkelaars, testconsole, beleidsregels en analyses zijn beschikbaar voor de SOAP-services.

### <a name="is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules"></a>De constante API Management gateway-IP-adres is? Kan ik deze in de firewall-regels gebruiken?
In alle lagen van API Management is het openbare IP-adres (VIP) van de API Management-tenant met enkele uitzonderingen statisch gedurende de levensduur van de tenant. De IP-adres verandert in deze omstandigheden:

* De service is verwijderd en opnieuw gemaakt.
* Het serviceabonnement is [onderbroken](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) of [gewaarschuwd](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/subscription-lifecycle-api-reference.md#subscription-states) (bijvoorbeeld voor betaald) en vervolgens opnieuw ingesteld.
* U toevoegen of verwijderen van Azure Virtual Network (kunt u Virtueelnetwerk alleen op de Developer en Premium-laag).

Voor implementaties in meerdere regio's, het regionale adres verandert als de regio is leeggemaakt en vervolgens opnieuw ingesteld. (u kunt implementatie voor meerdere regio's alleen op de Premium-laag gebruiken).

Premium-laag tenants die zijn geconfigureerd voor implementatie in meerdere regio's zijn toegewezen één openbaar IP-adres per regio.

U kunt uw IP-adres (of de adressen in een implementatie met meerdere regio's) op de pagina van de tenant in Azure portal ophalen.

### <a name="can-i-configure-an-oauth-20-authorization-server-with-ad-fs-security"></a>Kan ik een OAuth 2.0-autorisatie-server configureren met AD FS-beveiliging?
Zie voor informatie over het configureren van een OAuth 2.0-autorisatie-server met Active Directory Federation Services (AD FS)-beveiliging, [met behulp van AD FS in API Management](https://phvbaars.wordpress.com/2016/02/06/using-adfs-in-api-management/).

### <a name="what-routing-method-does-api-management-use-in-deployments-to-multiple-geographic-locations"></a>Welke routeringsmethode gebruikt API Management in implementaties van verschillende geografische locaties?
API Management maakt gebruik van de [prestaties routeringsmethode voor verkeer](../traffic-manager/traffic-manager-routing-methods.md#performance) in implementaties van verschillende geografische locaties. Inkomend verkeer wordt doorgestuurd naar de dichtstbijzijnde API-gateway. Als één regio offline gaat, wordt het inkomende verkeer automatisch doorgestuurd naar de volgende dichtstbijzijnde gateway. Meer informatie over methoden voor het doorsturen in [methoden voor het doorsturen van Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="can-i-use-an-azure-resource-manager-template-to-create-an-api-management-service-instance"></a>Kan ik een Azure Resource Manager-sjabloon gebruiken voor het maken van een exemplaar van API Management-service?
Ja. Zie de [Azure API Management-Service](https://aka.ms/apimtemplate) QuickStart-sjablonen.

### <a name="can-i-use-a-self-signed-ssl-certificate-for-a-back-end"></a>Kan ik een zelf-ondertekend SSL-certificaat voor een back-end gebruiken?
Ja. Dit kan worden gedaan via PowerShell of door rechtstreeks verzenden naar de API. Hiermee wordt de validatie van certificaatketen uitschakelen en kunt u het gebruik van zelf-ondertekend of privé-ondertekende certificaten tijdens de communicatie van API Management met de back-end-services.

#### <a name="powershell-method"></a>PowerShell-methode ####
Gebruik de [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (voor de nieuwe back-end) of [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (voor bestaande back-end) PowerShell-cmdlets en stel de `-SkipCertificateChainValidation` parameter `True`. 

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

#### <a name="direct-api-update-method"></a>Directe methode voor API bijwerken ####
1. Maak een [back-end](https://msdn.microsoft.com/library/azure/dn935030.aspx) entiteit met behulp van API Management.       
2. Stel de **skipCertificateChainValidation** eigenschap **waar**.     
3. Als u niet meer toestaan dat zelfondertekende certificaten wilt, de back-end-entiteit niet verwijderen of stel de **skipCertificateChainValidation** eigenschap **false**.

### <a name="why-do-i-get-an-authentication-failure-when-i-try-to-clone-a-git-repository"></a>Waarom krijg ik een verificatiefout wanneer ik probeer om een gitopslagplaats te klonen?
Als u Git Credential Manager, of als u probeert te klonen van een Git-opslagplaats met behulp van Visual Studio, kunt u uitvoeren in een bekend probleem met het dialoogvenster Windows-referenties. In het dialoogvenster beperkt wachtwoordlengte 127 tekens lang en het wachtwoord Microsoft gegenereerd worden afgekapt. Er wordt gewerkt aan het verkorten van het wachtwoord. Gebruik Git-Bash om uw Git-opslagplaats te klonen op dit moment.

### <a name="does-api-management-work-with-azure-expressroute"></a>Werkt de API Management met Azure ExpressRoute?
Ja. API Management werkt met Azure ExpressRoute.

### <a name="why-do-we-require-a-dedicated-subnet-in-resource-manager-style-vnets-when-api-management-is-deployed-into-them"></a>Waarom hebben we een toegewezen subnet in Resource Manager-stijl VNETs nodig wanneer u API Management wordt geïmplementeerd in deze?
De toegewezen subnet-vereiste voor API Management zijn afkomstig uit het feit, dat is gebouwd op het implementatiemodel klassiek (PAAS V1-laag). Hoewel we kunt implementeren in een Resource Manager VNET (V2-laag), zijn er gevolgen die. Het klassieke implementatiemodel in Azure is niet nauw gekoppeld met het Resource Manager-model en dus als u een resource in V2-laag maakt, de V1-laag niet weet en problemen kunnen optreden, zoals API Management is het gebruik van een IP-adres dat al aan een NIC toegewezen is  (gebaseerd op versie 2).
Voor meer informatie over het verschil tussen de klassieke en Resource Manager-modellen in Azure verwijzen naar [verschil in implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md).

### <a name="what-is-the-minimum-subnet-size-needed-when-deploying-api-management-into-a-vnet"></a>Wat is de minimale subnetgrootte die nodig zijn bij het implementeren van API Management in een VNET?
De subnetgrootte van de minimale die nodig zijn voor het implementeren van API Management is [/29](../virtual-network/virtual-networks-faq.md#configuration), dit is de minimale subnetgrootte die ondersteuning biedt voor Azure.

### <a name="can-i-move-an-api-management-service-from-one-subscription-to-another"></a>Kan ik een API Management-service van één abonnement verplaatsen naar een andere?
Ja. Voor meer informatie Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="are-there-restrictions-on-or-known-issues-with-importing-my-api"></a>Zijn er beperkingen met betrekking tot of bekende problemen met het importeren van mijn API?
[Bekende problemen en beperkingen](api-management-api-import-restrictions.md) voor Open API(Swagger) WSDL en WADL indelingen.
