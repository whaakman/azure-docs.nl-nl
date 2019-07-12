---
title: Onboarding een klant naar Azure gedelegeerd resourcebeheer van - naar Azure
description: Meer informatie over hoe zorgen voor onboarding een klant naar Azure overgedragen resourcebeheer, zodat hun bronnen worden geopend en beheerd via uw eigen tenant.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: 1885a6220f14de234710b6980b5d3b6a6172bb7e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809860"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Onboarding een klant naar Azure Resourcemanagement overgedragen

Dit artikel wordt uitgelegd hoe u, als een serviceprovider kunt Onboarding van een klant Azure gedelegeerde Resourcemanagement, zodat hun gedelegeerde resources (abonnementen en/of resourcegroepen) voor toegang en beheerd via uw eigen (Azure Active Directory Azure AD)-tenant. Hoewel we naar serviceproviders en klanten hier verwijzen, kunnen ondernemingen die meerdere tenants beheren hetzelfde proces gebruiken om te consolideren van hun ervaring.

U kunt dit proces herhalen als u bij het beheren van resources voor meerdere klanten. Vervolgens, wanneer een geautoriseerde gebruiker zich aanmeldt bij uw tenant, kan de gebruiker worden geautoriseerd binnen klant tenants bereiken beheerbewerkingen uitvoeren zonder te hoeven aanmelden bij elke tenant individuele klant.

U kunt uw Microsoft Partner Network (MPN)-ID koppelen aan uw onboarding-abonnementen voor het bijhouden van uw invloed op klant-engagements. Zie voor meer informatie, [een partner-ID koppelen aan uw Azure-accounts](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started).

> [!NOTE]
> Klanten die kunnen worden toegevoegd automatisch als ze een aanbieding voor beheerde services (openbaar of privé) die u hebt gepubliceerd in de Azure Marketplace kopen. Zie voor meer informatie, [beheerde Services publiceren op Azure Marketplace biedt](publish-managed-services-offers.md). U kunt ook het onboarding-proces dat hier wordt beschreven in een aanbieding op Azure Marketplace worden gepubliceerd.

Het onboarding-proces is vereist voor acties die moeten worden gemaakt op basis van vanuit zowel de van de serviceprovider-tenant en tenant van de klant. Al deze stappen worden beschreven in dit artikel.

> [!IMPORTANT]
> Op dit moment u kan onboarding niet een abonnement (of resourcegroep binnen een abonnement) voor Azure Resourcemanagement overgedragen als het abonnement gebruikmaakt van Azure Databricks. Op dezelfde manier als een abonnement is geregistreerd voor de voorbereiding op met de **Microsoft.ManagedServices** resourceprovider, kunt u zich niet voor het maken van een Databricks-werkruimte voor dat abonnement op dit moment.

## <a name="gather-tenant-and-subscription-details"></a>Tenant en hetzelfde abonnement gegevens verzamelen

Voor Onboarding van een klant tenant, het moet beschikken over een actief Azure-abonnement. U moet het volgende weten:

- De tenant-ID van de serviceprovider-tenant (waar u wilt beheren van de klant resources)
- De tenant-ID van de klant-tenant (die hebben bronnen die worden beheerd door de serviceprovider)
- De abonnement-id's voor elke specifieke abonnement in de tenant van de klant die worden beheerd door de serviceprovider (of waarin de resourcegroep(en) op die worden beheerd door de serviceprovider)

Als u deze gegevens nog niet hebt, kunt u deze in een van de volgende manieren ophalen.

### <a name="azure-portal"></a>Azure Portal

Uw tenant-ID kan worden gezien door de muiswijzer op de naam van uw account op de bovenste rechterkant van de Azure-portal of door te selecteren **schakelen tussen mappen**. Als u wilt selecteren en kopiëren van uw tenant-ID, zoeken naar 'Azure Active Directory' uit in de portal en selecteer vervolgens **eigenschappen** en kopieer de waarde die wordt weergegeven in de **map-ID** veld. Om de ID van een abonnement op te zoeken naar 'Abonnementen' en selecteer vervolgens de juiste abonnements-ID.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```


## <a name="ensure-the-customers-subscription-is-registered-for-onboarding"></a>Zorg ervoor dat de klant het abonnement is geregistreerd voor het voorbereiden

Elk abonnement moet zijn gemachtigd voor het voorbereiden door het handmatig registreren met de **Microsoft.ManagedServices** resourceprovider. De klant een abonnement kunt registreren met de volgende stappen wordt beschreven in [Azure resourceproviders en typen](../../azure-resource-manager/resource-manager-supported-services.md).

De klant kunt bevestigen dat het abonnement gereed voor onboarding in een van de volgende manieren is.

### <a name="azure-portal"></a>Azure Portal

1. Selecteer het abonnement in de Azure-portal.
1. Selecteer **resourceproviders**.
1. Bevestig dat **Microsoft.ManagedServices** wordt weergegeven als **geregistreerde**.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Set-AzContext -Subscription <subscriptionId>
Get-AzResourceProvider -ProviderNameSpace 'Microsoft.ManagedServices'
```

Dit moet resultaten die vergelijkbaar is met het volgende:

```output
ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationDefinitions}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {registrationAssignments}
Locations         : {}

ProviderNamespace : Microsoft.ManagedServices
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {}
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set –subscription <subscriptionId>
az provider show –namespace "Microsoft.ManagedServices" –-output table
```

Dit moet resultaten die vergelijkbaar is met het volgende:

```output
Namespace                  RegistrationState
-------------------------  -------------------
Microsoft.ManagedServices  Registered
```

## <a name="define-roles-and-permissions"></a>Rollen en machtigingen definiëren

Als een serviceprovider kunt u het gebruik van meerdere aanbiedingen met één klant, waarbij verschillende toegang voor verschillende bereiken.

Beheer om eenvoudiger te maken, wordt u aangeraden Azure AD-gebruikersgroepen voor elke rol, zodat u kunt toevoegen of verwijderen van afzonderlijke gebruikers aan de groep in plaats van rechtstreeks aan deze gebruiker toewijzen van machtigingen. U kunt ook rollen toewijzen aan een service-principal. Zorg ervoor dat het principe van minimale bevoegdheden volgen zodat gebruikers hebben alleen de machtigingen die nodig zijn om hun taak te voltooien waardoor de kans dat onbedoeld fouten. Zie voor meer informatie, [aanbevolen procedures voor beveiliging](../concepts/recommended-security-practices.md).

> [!NOTE]
> Roltoewijzingen moeten op rollen gebaseerd toegangsbeheer (RBAC) gebruiken [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles). Alle ingebouwde rollen worden momenteel ondersteund met Azure gedelegeerde resource Manager, met uitzondering van de eigenaar en een ingebouwde rollen met [DataActions](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#dataactions) machtiging. De ingebouwde rol van beheerder van gebruikerstoegang wordt ondersteund voor beperkt gebruik, zoals hieronder wordt beschreven. Aangepaste rollen en [klassiek abonnement beheerdersrollen](https://docs.microsoft.com/azure/role-based-access-control/classic-administrators) worden ook niet ondersteund.

Om te definiëren autorisaties, moet u weet welke ID-waarden voor elke gebruiker, groep of service-principal waartoe u toegang wilt verlenen. U moet ook de roldefinitie-ID voor elke ingebouwde rol die u wilt toewijzen. Als u ze nog niet hebt, kunt u ze in een van de volgende manieren ophalen.



### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>').objectId

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list –-query "[?displayName == '<yourGroupName>'].objectId" –-output tsv

# To retrieve the objectId for an Azure AD user
az ad user show –-upn-or-object-id "<yourUPN>" –-query "objectId" –-output tsv

# To retrieve the objectId for an SPN
az ad sp list –-query "[?displayName == '<spDisplayName>'].objectId" –-output tsv

# To retrieve role definition IDs
az role definition list –-name "<roleName>" | grep name
```

## <a name="create-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon maken

Om te onboarden uw klant, moet u maken een [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/) sjabloon met het volgende:

|Veld  |Definitie  |
|---------|---------|
|**mspName**     |Naam van serviceprovider         |
|**mspOfferDescription**     |Een korte beschrijving van uw aanbieding (bijvoorbeeld ' Contoso VM management-aanbieding")      |
|**managedByTenantId**     |Uw tenant-ID         |
|**Autorisaties**     |De **principalId** waarden voor de gebruikers/groepen/SPN-namen van uw tenant, elk met een **principalIdDisplayName** om u te helpen uw klant leert wat het doel van de autorisatie en toegewezen aan een ingebouwde **roleDefinitionId** waarde om op te geven van het niveau van toegang         |

Voor de Onboarding van een klant-abonnement, gebruikt u de juiste Azure Resource Manager-sjabloon die wij verstrekken in onze [voorbeelden opslagplaats](https://github.com/Azure/Azure-Lighthouse-samples/), samen met een overeenkomstige parameterbestand dat u wijzigen om overeenkomt met uw configuratie te definiëren uw autorisaties. Afzonderlijke sjablonen vindt u afhankelijk van of u onboarding, een hele abonnement, een resourcegroep of meerdere resourcegroepen binnen een abonnement. We bieden ook een sjabloon die voor klanten die hebben gekocht van een beheerde service-aanbieding die u hebt gepubliceerd in de Azure Marketplace, als u liever vrijgeven hun abonnement op deze manier kan worden gebruikt.

|**Gebruik dit**  |**Deze Azure Resource Manager-sjabloon gebruiken**  |**En dit parameterbestand wijzigen** |
|---------|---------|---------|
|Subscription   |[delegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Resource group   |[rgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Meerdere resourcegroepen binnen een abonnement   |[multipleRgDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Abonnement (bij met behulp van een aanbieding op Azure Marketplace publiceren)   |[marketplaceDelegatedResourceManagement.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[marketplaceDelegatedResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/Azure-Delegated-Resource-Management/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> De hier beschreven proces vereist een afzonderlijke implementatie voor elk abonnement onboarding wordt uitgevoerd.
> 
> Afzonderlijke implementaties zijn ook vereist als u onboarding meerdere in verschillende abonnementen resourcegroepen. Echter, onboarding meerdere resourcegroepen binnen één abonnement kunnen worden gedaan in een implementatie.

Het volgende voorbeeld toont een gewijzigde **resourceProjection.parameters.json** -bestand dat wordt gebruikt voor de Onboarding van een abonnement. De bronbestanden van de groep-parameter (zich in de [rg-gedelegeerde-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management) map) zijn vergelijkbaar, maar ook een **rgName** parameter voor het identificeren van de specifieke resourcegroepen bevinden om te worden Onboarding uitgevoerd.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```
De laatste autorisatie in het bovenstaande voorbeeld wordt een **principalId** met de rol Administrator voor gebruikerstoegang (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9). Bij het toewijzen van deze rol, moet u bevatten de **delegatedRoleDefinitionIds** eigenschap en een of meer van de ingebouwde rollen. De gebruiker heeft gemaakt in deze toestemming zich deze ingebouwde rollen toewijzen aan beheerde identiteiten. Houd er rekening mee dat er geen andere machtigingen die doorgaans gepaard gaan met de rol Administrator voor gebruikerstoegang op deze gebruiker toepast.

## <a name="deploy-the-azure-resource-manager-templates"></a>De Azure Resource Manager-sjablonen implementeren

Nadat u de parameter-bestand hebt bijgewerkt, moet de klant de Resource Manager-sjabloon in de tenant van de klant implementeren als een implementatie op abonnementsniveau. Een afzonderlijke installatie nodig is voor elk abonnement dat u vrijgeven aan Azure-gedelegeerde Resourcemanagement (of voor elk abonnement dat resourcegroepen die u wenst wilt te onboarden bevat).

> [!IMPORTANT]
> De implementatie moet worden uitgevoerd door een niet-gastaccount in van de klant-tenant waaraan de [ingebouwde rol van eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) voor het abonnement onboarding wordt uitgevoerd (of waarin de resourcegroepen die toegevoegd worden).

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment create –-name <deploymentName> \
                     --location <AzureRegion> \
                     --template-file <pathToTemplateFile> \
                     --parameters <parameters/parameterFile> \
                     --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment create –-name <deploymentName \
                     –-location <AzureRegion> \
                     --template-uri <templateUri> \
                     --parameters <parameterFile> \
                     --verbose
```

## <a name="confirm-successful-onboarding"></a>Succesvolle voorbereiding bevestigen

Wanneer een klantabonnement toegevoegd aan Azure-gedelegeerde Resourcemanagement is is, is om te zien van het abonnement en de daarbij behorende bronnen (als ze toegang tot deze via het bovenstaande proces hebben gekregen gebruikers in de tenant van de serviceprovider mogelijk een afzonderlijk of als lid van een Azure AD-groep met de juiste machtigingen). U kunt dit controleren, controleert u of dat het abonnement wordt weergegeven in een van de volgende manieren.  

### <a name="azure-portal"></a>Azure Portal

In de tenant van de serviceprovider:

1. Navigeer naar de [Mijn pagina klanten](view-manage-customers.md).
2. Selecteer **klanten**.
3. Bevestig dat u de abonnementen met de naam van de aanbieding die u hebt opgegeven in het Resource Manager-sjabloon kunt zien.

In de tenant van de klant:

1. Navigeer naar de [serviceproviders pagina](view-manage-service-providers.md).
2. Selecteer **Service provider aanbiedingen**.
3. Bevestig dat u de abonnementen met de naam van de aanbieding die u hebt opgegeven in het Resource Manager-sjabloon kunt zien.

> [!NOTE]
> Het duurt een paar minuten nadat de implementatie voltooid is voordat de updates worden weergegeven in de Azure-portal.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ervaringen voor cross-tenant](../concepts/cross-tenant-management-experience.md).
- [Weergeven en beheren van klanten](view-manage-customers.md) door te gaan naar **mijn klanten** in Azure portal.
