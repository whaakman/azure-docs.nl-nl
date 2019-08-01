---
title: Azure Active Directory-verificatie (Azure AD)
titleSuffix: Azure Cognitive Services
description: Naslag informatie voor de insluitende lezer-SDK
services: cognitive-services
author: rwaller
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 07/22/2019
ms.author: rwaller
ms.openlocfilehash: de6a29e1c4c102aa7d4038185c1635544ba9dfe2
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688797"
---
# <a name="use-azure-active-directory-azure-ad-authentication-with-the-immersive-reader-service"></a>Azure Active Directory-verificatie (Azure AD) gebruiken met de insluitende lezer-service

In de volgende secties gebruikt u de Azure Cloud Shell-omgeving of de Azure CLI om een nieuwe insluitende lezer-resource te maken met een aangepast subdomein en vervolgens Azure AD te configureren in uw Azure-Tenant. Na het volt ooien van de eerste configuratie roept u Azure AD op om een toegangs token te verkrijgen, vergelijkbaar met de manier waarop deze wordt uitgevoerd wanneer u de insluitende lezer-SDK gebruikt. Als u vastloopt, worden er links in elke sectie weer gegeven met alle beschik bare opties voor elk van de Azure CLI-opdrachten.

## <a name="create-an-immersive-reader-resource-with-a-custom-subdomain"></a>Een insluitende lezer-resource maken met een aangepast subdomein

1. Begin met het openen van de [Azure Cloud shell](https://docs.microsoft.com/azure/cloud-shell/overview). [Selecteer vervolgens een abonnement](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0#description):

   ```azurecli-interactive
   Select-AzSubscription -SubscriptionName <YOUR_SUBSCRIPTION>
   ```

2. Maak vervolgens [een insluitende lezer-resource](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) met een aangepast subdomein.

   >[!NOTE]
   > De naam van het subdomein wordt gebruikt in insluitende Reader SDK bij het starten van de lezer met de functie launchAsync.

   -SkuName kan F0 (gratis laag) of S0 (Standard-laag, ook gratis tijdens de open bare preview) zijn. De S0-laag heeft een hogere limiet voor de aanroep frequentie en geen maandelijks quotum voor het aantal aanroepen.

   -Locatie kan een van de volgende zijn: `eastus`, `westus`, `australiaeast`, `centralindia`, `japaneast` `northeurope`,,`westeurope`

   -CustomSubdomainName moet wereld wijd uniek zijn en mag geen speciale tekens bevatten, zoals: ".", "!", ",".


   ```azurecli-interactive
   $resource = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME> -Type ImmersiveReader -SkuName S0 -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>

   // Display the Resource info
   $resource
   ```

   Als dit lukt, moet het resource- **eind punt** de subdomeinnaam weer geven die uniek is voor uw resource.

   Hier wordt het zojuist gemaakte resource-object vastgelegd in een **$resource** variabele, omdat het later wordt gebruikt bij het verlenen van toegang tot deze bron.


   >[!NOTE]
   > Als u een resource maakt in de Azure Portal, wordt de resource naam gebruikt als het aangepaste subdomein. U kunt de naam van het subdomein controleren in de portal door naar de pagina Resource overzicht te gaan en het subdomein te vinden in het eind punt dat `https://[SUBDOMAIN].cognitiveservices.azure.com/`hier wordt vermeld. U kunt dit ook later controleren wanneer u het subdomein wilt ophalen voor de integratie met de SDK.

   Als de resource is gemaakt in de portal, kunt u nu ook [een bestaande resource ophalen](https://docs.microsoft.com/powershell/module/az.cognitiveservices/get-azcognitiveservicesaccount?view=azps-1.8.0) .

   ```azurecli-interactive
   $resource = Get-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME>

   // Display the Resource info
   $resource
   ```

## <a name="assign-a-role-to-a-service-principal"></a>Een rol toewijzen aan een Service-Principal

Nu u een aangepast subdomein hebt dat aan uw resource is gekoppeld, moet u een rol toewijzen aan een service-principal.

1. Eerst gaan we [een Azure AD-toepassing maken](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0).

   >[!NOTE]
   > Het wacht woord, ook wel bekend als ' client geheim ', wordt gebruikt bij het verkrijgen van verificatie tokens.

   ```azurecli-interactive
   $password = "<YOUR_PASSWORD>"
   $secureStringPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
   $aadApp = New-AzADApplication -DisplayName ImmersiveReaderAAD -IdentifierUris http://ImmersiveReaderAAD -Password $secureStringPassword

   // Display the Azure AD app info
   $aadApp
   ```

   Hier wordt het zojuist gemaakte Azure AD-App-object vastgelegd in een **$aadApp** variabele voor gebruik in de volgende stap.

2. Vervolgens moet u [een service-principal maken](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0) voor de Azure AD-toepassing.

   ```azurecli-interactive
   $principal = New-AzADServicePrincipal -ApplicationId $aadApp.ApplicationId

   // Display the service principal info
   $principal
   ```

   Hier wordt het nieuw gemaakte service-principal-object vastgelegd in een **$Principal** variabele voor gebruik in de volgende stap.


3. De laatste stap is het [toewijzen van de rol ' Cognitive Services gebruiker '](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0) aan de Service-Principal (bereik van de resource). Door een rol toe te wijzen, verleent u de Service-Principal toegang tot deze resource. U kunt dezelfde service-principal toegang verlenen tot meerdere resources in uw abonnement.

   ```azurecli-interactive
   New-AzRoleAssignment -ObjectId $principal.Id -Scope $resource.Id -RoleDefinitionName "Cognitive Services User"
   ```

   >[!NOTE]
   > Deze stap moet worden uitgevoerd voor elke insluitende lezer-resource die u maakt. Als u bijvoorbeeld meerdere resources voor verschillende globale regio's maakt, moet u deze stap uitvoeren voor elk van deze regionale resources, zodat de Azure AD-verificatie voor al deze bronnen werkt. Of als u op een later tijdstip een nieuwe resource maakt, moet u deze functie toewijzings stap ook uitvoeren voor die nieuwe resource.


## <a name="obtain-an-azure-ad-token"></a>Een Azure AD-token verkrijgen

In dit voor beeld wordt uw wacht woord gebruikt voor het verifiëren van de service-principal voor het verkrijgen van een Azure AD-token.

1. Down load uw **TenantId**:
   ```azurecli-interactive
   $context = Get-AzContext
   $context.Tenant.Id
   ```

2. Een Token ophalen:
   ```azurecli-interactive
   $authority = "https://login.windows.net/" + $context.Tenant.Id
   $resource = "https://cognitiveservices.azure.com/"
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $aadApp.ApplicationId, $password
   $token = $authContext.AcquireTokenAsync($resource, $clientCredential).Result
   $token
   ```

   >[!NOTE]
   > De insluitende lezer-SDK gebruikt de eigenschap AccessToken van het token, bijvoorbeeld $token. AccessToken. Raadpleeg de SDK- [naslag](reference.md) informatie en code [voorbeelden](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples) voor meer informatie.

U kunt de Service-Principal ook verifiëren met een certificaat. Naast een service-principal worden ook gebruikers-principals ondersteund door machtigingen te delegeren via een andere Azure AD-toepassing. In dit geval, in plaats van wacht woorden of certificaten, wordt gebruikers gevraagd om twee ledige verificatie bij het ophalen van tokens.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de [zelf studie](./tutorial.md) om te zien wat u nog meer kunt doen met de SDK voor insluitende lezers
* Verken de insluitende [Lezer SDK](https://github.com/microsoft/immersive-reader-sdk) en de referentie voor de insluitende [Lezer SDK](./reference.md)