---
title: Oplossing voor externe controle integreren met Azure-Stack | Microsoft Docs
description: Informatie over het Azure-Stack integreren met een oplossing voor externe controle in uw datacenter.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 856738a7-1510-442a-88a8-d316c67c757c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: mabrigg
ms.openlocfilehash: 76499ac959b77e83494bc4f9593c20a99da5c147
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="integrate-external-monitoring-solution-with-azure-stack"></a>Oplossing voor externe controle integreren met Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

Voor externe controle van de Azure-Stack-infrastructuur, moet u de Azure-Stack-software, de fysieke computers en het fysieke netwerk-switches bewaken. Elk van deze gebieden biedt een methode voor het ophalen van informatie van de status en waarschuwing.

- Azure Stack-software biedt een op basis van REST-API om op te halen van de status en waarschuwingen. (Met het gebruik van technologieën zoals opslagruimten Direct door software gedefinieerde opslag status en waarschuwingen zijn onderdeel van de bewaking van de software).
- Fysieke computers kunnen status en informatie over waarschuwingen beschikbaar maken via de baseboard management controllers (BMC).
- Fysieke netwerkapparaten kunnen status en informatie over waarschuwingen beschikbaar maken via het SNMP-protocol.

Elke Azure-Stack-oplossing wordt geleverd met een hardware-lifecycle-host. Controle van de hardwareleverancier van de Original Equipment Manufacturer (OEM)-software voor de fysieke servers en netwerkapparaten wordt uitgevoerd op deze host. Indien gewenst, kunt u deze bewakingsoplossingen omzeilen en rechtstreeks integreren met bestaande bewakingsoplossingen in uw datacenter.

> [!IMPORTANT]
> De externe bewakingsoplossing die u gebruikt moet zonder agent. U kunt van derden agents binnen Azure Stack-componenten niet installeren.

Het volgende diagram toont de verkeersstroom tussen een Azure-Stack geïntegreerd systeem, de host van de levenscyclus van hardware, een oplossing voor externe controle en een externe tickets/gegevens verzameling systeem.

![Diagram van verkeer tussen Azure Stack, bewaking en tickets oplossing.](media/azure-stack-integrate-monitor/MonitoringIntegration.png)  

In dit artikel wordt uitgelegd hoe Azure-Stack integreren met externe bewakingsoplossingen zoals System Center Operations Manager en Nagios. Dit omvat ook het werken met waarschuwingen programmatisch met behulp van PowerShell of via REST API-aanroepen.

## <a name="integrate-with-operations-manager"></a>Integreert met Operations Manager

U kunt de Operations Manager gebruiken voor externe controle van Azure-Stack. De System Center Management Pack voor Microsoft Azure-Stack kunt u voor het bewaken van meerdere Azure-Stack-implementaties met slechts één exemplaar van de Operations Manager. Het management pack maakt gebruik van de Health-resourceprovider en Update resourceprovider REST-API's om te communiceren met de Azure-Stack. Als u van plan bent voor het overslaan van de OEM bewaking van de software die wordt uitgevoerd op de host van de levenscyclus van hardware, kunt u de leverancier van management packs voor het bewaken van fysieke servers kunt installeren. U kunt ook de detectie van netwerkapparaten Operations Manager gebruiken voor het bewaken van netwerkswitches.

Het management pack voor Azure-Stack biedt de volgende mogelijkheden:

- U kunt meerdere Azure-Stack-implementaties beheren.
- Er is ondersteuning voor Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS).
- U kunt ophalen en waarschuwingen sluiten.
- Er is een status en een dashboard capaciteit.
- Bevat de onderhoudsmodus automatische detectie voor wanneer patch en bij te werken (P & U) wordt uitgevoerd.
- Taken voor het bijwerken van kracht voor implementatie en regio bevat.
- U kunt aangepaste gegevens toevoegen aan een regio.
- Ondersteunt meldingen en rapportage.

U kunt de System Center Management Pack voor Microsoft Azure-netwerkstack en de bijbehorende gebruikershandleiding downloaden [hier](https://www.microsoft.com/en-us/download/details.aspx?id=55184), of rechtstreeks uit Operations Manager.

Voor een ticketsysteem oplossing, kunt u Operations Manager integreren met System Center Service Manager. De geïntegreerde productconnector kunnen bidirectionele communicatie waarmee u een waarschuwing in Azure-Stack en Operations Manager sluiten nadat u een serviceaanvraag in Service Manager hebt opgelost.

Het volgende diagram toont de integratie van Azure-Stack met een bestaande implementatie van System Center. U kunt de Service Manager verder met System Center Orchestrator of Service Management Automation (SMA) voor het uitvoeren van bewerkingen in Azure-Stack automatiseren.

![Diagram van de integratie met OM-, Service-beheer- en SMA.](media/azure-stack-integrate-monitor/SystemCenterIntegration.png)

## <a name="integrate-with-nagios"></a>Integreren met Nagios

Een invoegtoepassing bewaking Nagios is ontwikkeld samen met de Cloudbase partneroplossingen die beschikbaar onder de strikte gratis software-licentie – MIT (Massachusetts Institute of Technology is).

De invoegtoepassing is geschreven in Python en maakt gebruik van de resourceprovider health REST-API. Biedt basisfuncties voor het ophalen en waarschuwingen in de Azure-Stack sluiten. Als het System Center management pack kunt u meerdere Azure-Stack implementaties toevoegen en om meldingen te verzenden.

De invoegtoepassing werkt met Nagios Enterprise en Nagios Core. U kunt dit downloaden [hier](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details). De downloadsite bevat ook de installatie en configuratie-informatie.

### <a name="plugin-parameters"></a>Parameters van de invoegtoepassing

Configureer de invoegtoepassingbestand 'Azurestack_plugin.py' met de volgende parameters:

| Parameter | Beschrijving | Voorbeeld |
|---------|---------|---------|
| *arm_endpoint* | Azure Resource Manager (beheerder)-eindpunt |https://adminmanagement.local.azurestack.external |
| *api_endpoint* | Azure Resource Manager (beheerder)-eindpunt  | https://adminmanagement.local.azurestack.external |
| *Tenant_id* | Beheerder abonnements-ID | Via de beheerdersportal of PowerShell ophalen |
| *Gebruikersnaam* | Operator abonnement gebruikersnaam | operator@myazuredirectory.onmicrosoft.com |
| *User_password* | Operator abonnement wachtwoord | mijnwachtwoord |
| *Client_id* | Client | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 * |
| *regio* |  Naam van de regio Azure Stack | lokale |
|  |  |

* De PowerShell-GUID die opgegeven is universal. U kunt deze gebruiken voor elke implementatie.

## <a name="use-powershell-to-monitor-health-and-alerts"></a>PowerShell gebruiken voor de monitor-status en waarschuwingen

Als u Operations Manager, Nagios of een oplossing op basis van Nagios niet gebruikt, kunt u PowerShell gebruiken om in te schakelen van een breed scala aan bewakingsoplossingen integreren met Azure-Stack.
 
1. Zorg ervoor dat u hebt voor het gebruik van PowerShell [PowerShell is geïnstalleerd en geconfigureerd](azure-stack-powershell-configure-quickstart.md) voor een Azure-Stack operator-omgeving. PowerShell installeren op een lokale computer die toegang heeft tot de Resource Manager (beheerder)-eindpunt (https://adminmanagement. [ de regio]. [External_FQDN]).

2. Voer de volgende opdrachten verbinding maken met de Azure-Stack-omgeving als een Azure-Stack-operator:

   ```PowerShell
   Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint https://adminmanagement.[Region].[External_FQDN]

   Login-AzureRmAccount -EnvironmentName "AzureStackAdmin"
   ```
3. Ga naar de map waarin u hebt geïnstalleerd het [extra Azure-Stack](https://github.com/Azure/AzureStack-Tools) als onderdeel van de PowerShell-installatie, bijvoorbeeld c:\azurestack-tools-master. Vervolgens wijzigen in de infrastructuur-map en voer de volgende opdracht voor het importeren van de infrastructuur-module:

   ```PowerShell
   Import-Module .\AzureStack.Infra.psm1
    ```
4. Gebruik de opdrachten zoals de volgende voorbeelden om te werken met waarschuwingen:
   ```PowerShell
   #Retrieve all alerts
   Get-AzsAlert -location [Region]

   #Filter for active alerts
   $Active=Get-AzsAlert -location [Region] | Where {$_.State -eq "active"}
   $Active

   #Close alert
   Close-AzsAlert -location [Region] -AlertID "ID"

   #Retrieve resource provider health
   Get-AzsResourceProviderHealths -location [Region]

   #Retrieve infrastructure role instance health
   Get-AzsInfrastructureRoleHealths -location [Region]
   ```

## <a name="use-the-rest-api-to-monitor-health-and-alerts"></a>De REST-API op controleprogramma status en waarschuwingen gebruiken

U kunt de REST API-aanroepen ontvang waarschuwingen, waarschuwingen sluiten en de status van de resourceproviders ophalen.

### <a name="get-alert"></a>Waarschuwing ontvangen

**Aanvraag**

De aanvraag haalt alle actieve en gesloten waarschuwingen voor de provider standaardabonnement. Er is geen aanvraaginhoud.


|Methode  |Aanvraag-URI  |
|---------|---------|
|TOEVOEGEN     |   https://{armendpoint}/Subscriptions/{subId}/resourceGroups/System. {RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts?api-version=2016-05-01 '      |
|     |         |

**Argumenten**

|Argument  |Beschrijving  |
|---------|---------|
|armendpoint     |  Het Azure Resource Manager-eindpunt van uw Azure-Stack-omgeving, in de indeling https://adminmanagement. {RegionName}. {Externe FQDN}. Als de externe FQDN-naam is bijvoorbeeld *azurestack.external* en de regionaam van de is *lokale*, dan is het Resource Manager-eindpunt https://adminmanagement.local.azurestack.external.       |
|subid     |   Abonnements-ID van de gebruiker die de oproep aan te brengen. U kunt deze API's voor query alleen met een gebruiker die gemachtigd om de standaard provider-abonnement te is gebruiken.      |
|RegionName     |    De naam van het gebied van de Azure-Stack-implementatie.     |
|API-versie     |  De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. U moet 2016-05-01.      |
|     |         |

**Antwoord**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/Alerts?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Active",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},

…
```

**Details van de reactie**


|  Argument  |Beschrijving  |
|---------|---------|
|*ID*     |      Unieke ID van de waarschuwing.   |
|*naam*     |     Interne naam van de waarschuwing.   |
|*type*     |     Resourcedefinitie.    |
|*location*     |       Regionaam.     |
|*tags*     |   Resourcelabels.     |
|*closedtimestamp*    |  UTC-tijd waarop de waarschuwing is gesloten.    |
|*createdtimestamp*     |     UTC-tijd waarop de waarschuwing is gemaakt.   |
|*Beschrijving*     |    Beschrijving van de waarschuwing.     |
|*faultid*     | Betrokken onderdeel.        |
|*alertid*     |  Unieke ID van de waarschuwing.       |
|*faulttypeid*     |  Unieke type defecte onderdeel.       |
|*lastupdatedtimestamp*     |   UTC-tijd wanneer waarschuwingsgegevens voor het laatst is bijgewerkt.    |
|*healthstate*     | Algemene health-status.        |
|*naam*     |   Naam van de specifieke waarschuwing.      |
|*fabricname*     |    Geregistreerde fabric-naam van het defecte onderdeel.   |
|*Beschrijving*     |  Beschrijving van de geregistreerde fabric-component.   |
|*servicetype*     |   Type van de geregistreerde fabric-service.   |
|*herstel*     |   Aanbevolen herstelstappen uit.    |
|*type*     |   Waarschuwingstype.    |
|*resourceRegistrationid*    |     ID van de betreffende geregistreerde resource.    |
|*resourceProviderRegistrationID*   |    ID van de geregistreerde resourceprovider van het betreffende onderdeel.  |
|*serviceregistrationid*     |    ID van de geregistreerde service.   |
|*ernst*     |     De ernst van waarschuwing.  |
|*status*     |    Waarschuwing status.   |
|*titel*     |    De naam van de waarschuwing.   |
|*impactedresourceid*     |     ID van de betrokken resource.    |
|*ImpactedresourceDisplayName*     |     De naam van de betrokken resource.  |
|*closedByUserAlias*     |   De gebruiker die heeft waarschuwing gesloten.      |

### <a name="close-alert"></a>Waarschuwing sluiten

**Aanvraag**

De aanvraag wordt gesloten voor een waarschuwing door de unieke ID.

|Methode    |Aanvraag-URI  |
|---------|---------|
|PLAATSEN     |   https://{armendpoint}/Subscriptions/{subId}/resourceGroups/System. {RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/Alerts/alertid?api-version=2016-05-01 '    |

**Argumenten**


|Argument  |Beschrijving  |
|---------|---------|
|*armendpoint*     |   Resource Manager-eindpunt van uw Azure-Stack-omgeving, in de indeling https://adminmanagement. {RegionName}. {Externe FQDN}. Als de externe FQDN-naam is bijvoorbeeld *azurestack.external* en de regionaam van de is *lokale*, dan is het Resource Manager-eindpunt https://adminmanagement.local.azurestack.external.      |
|*subid*     |    Abonnements-ID van de gebruiker die de oproep aan te brengen. U kunt deze API's voor query alleen met een gebruiker die gemachtigd om de standaard provider-abonnement te is gebruiken.     |
|*RegionName*     |   De naam van het gebied van de Azure-Stack-implementatie.      |
|*API-versie*     |    De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. U moet 2016-05-01.     |
|*alertid*     |    Unieke ID van de waarschuwing.     |

**Hoofdtekst**

```json

{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"2017-08-10T20:18:58.1584868Z",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```
**Antwoord**

```http
PUT https://adminmanagement.local.azurestack.external//subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/alerts/71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"name":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/alerts",
"location":"local",
"tags":{},
"properties":
{
"closedTimestamp":"",
"createdTimestamp":"2017-08-10T20:13:57.4398842Z",
"description":[{"text":"The infrastructure role (Updates) is experiencing issues.",
"type":"Text"}],
"faultId":"ServiceFabric:/UpdateResourceProvider/fabric:/AzurestackUpdateResourceProvider",
"alertId":"71dbd379-1d1d-42e2-8439-6190cc7aa80b",
"faultTypeId":"ServiceFabricApplicationUnhealthy",
"lastUpdatedTimestamp":"2017-08-10T20:18:58.1584868Z",
"alertProperties":
{
"healthState":"Warning",
"name":"Updates",
"fabricName":"fabric:/AzurestackUpdateResourceProvider",
"description":null,
"serviceType":"UpdateResourceProvider"},
"remediation":[{"text":"1. Navigate to the (Updates) and restart the role. 2. If after closing the alert the issue persists, please contact support.",
"type":"Text"}],
"resourceRegistrationId":null,
"resourceProviderRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"serviceRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"severity":"Warning",
"state":"Closed",
"title":"Infrastructure role is unhealthy",
"impactedResourceId":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/UpdateResourceProvider",
"impactedResourceDisplayName":"UpdateResourceProvider",
"closedByUserAlias":null
}
},
```

**Details van de reactie**


|  Argument  |Beschrijving  |
|---------|---------|
|*ID*     |      Unieke ID van de waarschuwing.   |
|*naam*     |     Interne naam van de waarschuwing.   |
|*type*     |     Resourcedefinitie.    |
|*location*     |       Regionaam.     |
|*tags*     |   Resourcelabels.     |
|*closedtimestamp*    |  UTC-tijd waarop de waarschuwing is gesloten.    |
|*createdtimestamp*     |     UTC-tijd waarop de waarschuwing is gemaakt.   |
|*Beschrijving*     |    Beschrijving van de waarschuwing.     |
|*faultid*     | Betrokken onderdeel.        |
|*alertid*     |  Unieke ID van de waarschuwing.       |
|*faulttypeid*     |  Unieke type defecte onderdeel.       |
|*lastupdatedtimestamp*     |   UTC-tijd wanneer waarschuwingsgegevens voor het laatst is bijgewerkt.    |
|*healthstate*     | Algemene health-status.        |
|*naam*     |   Naam van de specifieke waarschuwing.      |
|*fabricname*     |    Geregistreerde fabric-naam van het defecte onderdeel.   |
|*Beschrijving*     |  Beschrijving van de geregistreerde fabric-component.   |
|*servicetype*     |   Type van de geregistreerde fabric-service.   |
|*herstel*     |   Aanbevolen herstelstappen uit.    |
|*type*     |   Waarschuwingstype.    |
|*resourceRegistrationid*    |     ID van de betreffende geregistreerde resource.    |
|*resourceProviderRegistrationID*   |    ID van de geregistreerde resourceprovider van het betreffende onderdeel.  |
|*serviceregistrationid*     |    ID van de geregistreerde service.   |
|*ernst*     |     De ernst van waarschuwing.  |
|*status*     |    Waarschuwing status.   |
|*titel*     |    De naam van de waarschuwing.   |
|*impactedresourceid*     |     ID van de betrokken resource.    |
|*ImpactedresourceDisplayName*     |     De naam van de betrokken resource.  |
|*closedByUserAlias*     |   De gebruiker die heeft waarschuwing gesloten.      |

### <a name="get-resource-provider-health"></a>Provider resourcestatus ophalen

**Aanvraag**

De aanvraag Hiermee wordt de status voor alle geregistreerde resourceproviders.


|Methode  |Aanvraag-URI  |
|---------|---------|
|TOEVOEGEN    |   https://{armendpoint}/Subscriptions/{subId}/resourceGroups/System. {RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths?api-version=2016-05-01 '   |


**Argumenten**


|Argumenten  |Beschrijving  |
|---------|---------|
|*armendpoint*     |    Het Resource Manager-eindpunt van uw Azure-Stack-omgeving, in de indeling https://adminmanagement. {RegionName}. {Externe FQDN}. Bijvoorbeeld, als de externe FQDN azurestack.external is en regionaam lokaal is, is klikt u vervolgens het Resource Manager-eindpunt https://adminmanagement.local.azurestack.external.     |
|*subid*     |     Abonnements-ID van de gebruiker die de oproep aan te brengen. U kunt deze API's voor query alleen met een gebruiker die gemachtigd om de standaard provider-abonnement te is gebruiken.    |
|*RegionName*     |     De naam van het gebied van de Azure-Stack-implementatie.    |
|*API-versie*     |   De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. U moet 2016-05-01.      |


**Antwoord**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths?api-version=2016-05-01
```

```json
{
"value":[
{
"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"name":"03ccf38f-f6b1-4540-9dc8ec7b6389ecca",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths",
"location":"local",
"tags":{},
"properties":{
"registrationId":"03ccf38f-f6b1-4540-9dc8-ec7b6389ecca",
"displayName":"Key Vault",
"namespace":"Microsoft.KeyVault.Admin",
"routePrefix":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local",
"serviceLocation":"local",
"infraURI":"/subscriptions/4aa97de3-6b83-4582-86e1-65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.KeyVault.Admin/locations/local/infraRoles/Key Vault",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},
"healthState":"Healthy"
}
}

…
```
**Details van de reactie**


|Argument  |Beschrijving  |
|---------|---------|
|*ID*     |   Unieke ID van de waarschuwing.      |
|*naam*     |  Interne naam van de waarschuwing.       |
|*type*     |  Resourcedefinitie.       |
|*location*     |  Regionaam.       |
|*tags*     |     Resourcelabels.    |
|*registratie-id*     |   Unieke registratie voor de resourceprovider.      |
|*Weergavenaam*     |Weergavenaam van de resource provider.        |
|*naamruimte*     |   API-naamruimte de resourceprovider implementeert.       |
|*routePrefix*     |    De URI voor de interactie met de resourceprovider.     |
|*serviceLocation*     |   Deze resourceprovider is geregistreerd bij de regio.      |
|*infraURI*     |   De URI van de resourceprovider vermeld als een functie van de infrastructuur.      |
|*alertSummary*     |   Samenvatting van kritieke en waarschuwingsstatussen waarschuwing die is gekoppeld aan de resourceprovider.      |
|*healthState*     |    De status van de resourceprovider.     |


### <a name="get-resource-health"></a>Resourcestatus ophalen

Status van de aanvraag opgehaald voor een specifieke geregistreerde resourceprovider.

**Aanvraag**

|Methode  |Aanvraag-URI  |
|---------|---------|
|TOEVOEGEN     |     https://{armendpoint}/Subscriptions/{subId}/resourceGroups/System. {RegionName}/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/{RegionName}/serviceHealths/{RegistrationID}/resourceHealths?api-version=2016-05-01 '    |

**Argumenten**

|Argumenten  |Beschrijving  |
|---------|---------|
|*armendpoint*     |    Het Resource Manager-eindpunt van uw Azure-Stack-omgeving, in de indeling https://adminmanagement. {RegionName}. {Externe FQDN}. Bijvoorbeeld, als de externe FQDN azurestack.external is en regionaam lokaal is, is klikt u vervolgens het Resource Manager-eindpunt https://adminmanagement.local.azurestack.external.     |
|*subid*     |Abonnements-ID van de gebruiker die de oproep aan te brengen. U kunt deze API's voor query alleen met een gebruiker die gemachtigd om de standaard provider-abonnement te is gebruiken.         |
|*RegionName*     |  De naam van het gebied van de Azure-Stack-implementatie.       |
|*API-versie*     |  De versie van het protocol dat wordt gebruikt voor het maken van deze aanvraag. U moet 2016-05-01.       |
|*Registratie-id* |Registratie-ID voor een bepaalde resourceprovider. |

**Antwoord**

```http
GET https://adminmanagement.local.azurestack.external/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/03ccf38f-f6b1-4540-9dc8-ec7b6389ecca /resourceHealths?api-version=2016-05-01 HTTP/1.1
```

```json
{
"value":
[
{"id":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.InfrastructureInsights.Admin/regionHealths/local/serviceHealths/472aaaa6-3f63-43fa-a489-4fd9094e235f/resourceHealths/028c3916-ab86-4e7f-b5c2-0468e607915c",
"name":"028c3916-ab86-4e7f-b5c2-0468e607915c",
"type":"Microsoft.InfrastructureInsights.Admin/regionHealths/serviceHealths/resourceHealths",
"location":"local",
"tags":{},
"properties":
{"registrationId":"028c3916-ab86-4e7f-b5c2 0468e607915c","namespace":"Microsoft.Fabric.Admin","routePrefix":"/subscriptions/4aa97de3-6b83-4582-86e1 65a5e4d1295b/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local",
"resourceType":"infraRoles",
"resourceName":"Privileged endpoint",
"usageMetrics":[],
"resourceLocation":"local",
"resourceURI":"/subscriptions/<Subscription_ID>/resourceGroups/system.local/providers/Microsoft.Fabric.Admin/fabricLocations/local/infraRoles/Privileged endpoint",
"rpRegistrationId":"472aaaa6-3f63-43fa-a489-4fd9094e235f",
"alertSummary":{"criticalAlertCount":0,"warningAlertCount":0},"healthState":"Unknown"
}
}
…
```

**Details van de reactie**

|Argument  |Beschrijving  |
|---------|---------|
|*ID*     |   Unieke ID van de waarschuwing.      |
|*naam*     |  Interne naam van de waarschuwing.       |
|*type*     |  Resourcedefinitie.       |
|*location*     |  Regionaam.       |
|*tags*     |     Resourcelabels.    |
|*registratie-id*     |   Unieke registratie voor de resourceprovider.      |
|*resourceType*     |Type resource.        |
|*resourceName*     |   Naam van de resource.   |
|*usageMetrics*     |    Gebruik de metrische gegevens voor de resource.     |
|*resourceLocation*     |   Naam van de regio waar geïmplementeerd.      |
|*resourceURI*     |   De URI voor de resource.   |
|*alertSummary*     |   Samenvatting van kritieke en waarschuwingen, gezondheidsstatus.     |

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over ingebouwde statuscontrole [status en waarschuwingen in de Azure-Stack controleren](azure-stack-monitor-health.md).


