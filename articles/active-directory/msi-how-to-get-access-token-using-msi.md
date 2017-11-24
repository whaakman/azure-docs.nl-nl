---
title: Het gebruik van een Azure VM beheerde Service-identiteit voor aanmelden en tokens verkrijgen
description: Stapsgewijze service instructies voor het gebruik van een Azure VM-MSI-principal voor aanmelden en voor het ophalen van een toegangstoken.
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/17/2017
ms.author: bryanla
ms.openlocfilehash: 168b2ab3676d3f3e2830966f850e14adbe579f85
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in-and-token-acquisition"></a>Het gebruik van een Azure VM beheerde Service identiteit (MSI) voor aanmelden en tokens verkrijgen 
[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]Nadat u MSI op een virtuele machine in Azure hebt ingeschakeld, kunt u het MSI-bestand gebruiken voor aanmelden en om aan te vragen van een toegangstoken. Dit artikel ziet u de verschillende manieren voor gebruik van een MSI [service-principal](develop/active-directory-dev-glossary.md#service-principal-object) voor aanmelden, en verkrijgen een [app alleen-lezen toegangstoken](develop/active-directory-dev-glossary.md#access-token) voor toegang tot andere resources, met inbegrip van:

- Achtergrond/zonder toezicht aanmelden van PowerShell of Azure CLI
- Token verkrijgen voor de verschillende clients, waaronder .NET, PowerShell, Bash/CURL, REST
- Meld u aan met een Azure-SDK, waaronder .NET, Java, Node.js, Python, Ruby

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u de PowerShell-voorbeelden in dit artikel gebruiken wilt, moet u installeren [Azure PowerShell versie 4.3.1](https://www.powershellgallery.com/packages/AzureRM) of hoger. Als u gebruiken in de Azure CLI-voorbeelden in dit artikel wilt, hebt u drie opties:
- Gebruik [Azure Cloud Shell](../cloud-shell/overview.md) vanuit de Azure-portal.
- De ingesloten Azure Cloud Shell via de 'Deze probeer' knop, in de rechterbovenhoek van de Azure CLI codeblokken gebruiken.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 of hoger) als u liever CLI in een lokale opdrachtprompt. 


> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 
> - Om te voorkomen dat de autorisatie-fouten (403/AuthorizationFailed) in de voorbeelden code/script identiteit van de VM moet 'Lezer' toegang krijgen bij het bereik van de VM om toe te staan van Azure Resource Manager-bewerkingen op de virtuele machine. Zie [een beheerde Service identiteit (MSI) toegang toewijzen aan een resource met de Azure portal](msi-howto-assign-access-portal.md) voor meer informatie.
> - Voordat u doorgaat naar een van de volgende secties door de functie VM 'Verbinding' in de Azure-portal op afstand verbinding maken met uw VM MSI-functionaliteit te gebruiken.

## <a name="how-to-sign-in-from-powershell-or-cli-using-msi"></a>Het aanmelden met PowerShell of CLI met behulp van MSI

Voorheen uitvoeren van een script met de eigen identiteit bedoeld:
- registreren van deze als een vertrouwelijk/client-webtoepassing met Azure AD
- aanmelden met referenties voor client-ID/geheim van de toepassing

Met MSI moet uw script-client niet langer met Azure AD registreren of geef referenties op. In de volgende voorbeelden laten we zien hoe de MSI van een virtuele machine gebruiken service-principal voor aanmelden.

### <a name="azure-powershell"></a>Azure PowerShell

Het volgende script toont hoe:

- verkrijgen van een toegangstoken MSI voor een Azure VM
- Gebruik het toegangstoken aan te melden bij Azure AD onder de bijbehorende MSI service-principal
- de service-principal MSI gebruiken om te maken van een Azure Resource Manager-aanroep, voor het ophalen van de ID van de service-principal

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token

# Use the access token to sign in under the MSI service principal
Login-AzureRmAccount -AccessToken $access_token -AccountId “CLIENT”

# The MSI service principal is now signed in for this session.
# Next, a call to Azure Resource Manager is made to get the service principal ID for the VM's MSI. 
$spID = (Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>).identity.principalid
echo "The MSI service principal ID is $spID"
```
   
### <a name="azure-cli"></a>Azure CLI

Het volgende script toont hoe:

- aanmelden bij Azure AD onder de VM MSI service-principal
- de service-principal MSI gebruiken om te maken van een Azure Resource Manager-aanroep, voor het ophalen van de ID van de service-principal

```azurecli-interactive
az login --msi
spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
echo The MSI service principal ID is $spID
```

## <a name="how-to-acquire-an-access-token-from-msi"></a>Het verkrijgen van een toegangstoken van MSI

Met behulp van MSI uw client-toepassing/script niet langer nodig heeft voor het registreren met Azure AD of de client-ID en geheim voor het verkrijgen van een toegangstoken presenteren. De client kunt gewoon het lokale MSI-eindpunt voor een toegangstoken vragen zonder dat de referenties van toepassing. De app alleen-lezen toegangstoken is uitgegeven voor de MSI-service-principal, geschikt voor gebruik als een bearer-token in [service-naar-service roept vereisen clientreferenties](active-directory-protocols-oauth-service-to-service.md).

In de volgende voorbeelden laten we zien hoe de MSI van een virtuele machine gebruiken voor de aanschaf van token.

### <a name="net"></a>.NET

> [!IMPORTANT]
> De Azure AD Authentication Library (ADAL) is niet geïntegreerd met MSI. Voor een MSI met toegangstoken, controleert u een aanvraag naar het lokale eindpunt MSI in een virtuele machine. Zie voor meer informatie [MSI Veelgestelde vragen en bekende problemen](msi-known-issues.md#frequently-asked-questions-faqs).

```csharp
// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://localhost:50342/oauth2/token?resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

### <a name="azure-powershell-token"></a>Azure PowerShell

```powershell
# Get an access token from MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"
```

### <a name="bashcurl"></a>Bash/CURL

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

### <a name="rest"></a>REST

Voorbeeld van een aanvraag:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Beschrijving |
| ------- | ----------- |
| `GET` | De HTTP-term waarmee u gegevens wilt ophalen van het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://localhost:50342/oauth2/token` | Het MSI-eindpunt, waarbij 50342 is de standaardpoort en kan worden geconfigureerd. |
| `resource` | Een queryreeksparameter opgeven, die aangeeft van de App ID URI van de doelbron. Dit wordt ook weergegeven de `aud` (doelgroep) claim van het gepubliceerde token. In dit voorbeeld wordt een token voor toegang tot Azure Resource Manager, met een App ID URI van https://management.azure.com/ aanvraagt. |
| `Metadata` | Een HTTP-aanvraagveld header wordt vereist door MSI als een risicobeperking tegen aanvallen van Server Side aanvraag kunnen worden vervalst (SSRF). Deze waarde moet worden ingesteld op 'true', in alle kleine letters.

Voorbeeldreactie:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Element | Beschrijving |
| ------- | ----------- |
| `access_token` | Het aangevraagde toegangstoken. Wanneer u een REST-API aanroept, het token is ingesloten in de `Authorization` aanvraag-header-veld als een token 'bearer', zodat de API voor verificatie van de aanroeper. | 
| `refresh_token` | Niet gebruikt door MSI. |
| `expires_in` | Het aantal seconden dat het toegangstoken geldig is, blijft voordat het verloopt na uitgifte. Tijd van uitgifte vindt u in het token `iat` claim. |
| `expires_on` | De timespan wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `exp` claim). |
| `not_before` | De timespan wanneer het toegangstoken van kracht en kan worden geaccepteerd. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `nbf` claim). |
| `resource` | De resource die het toegangstoken is aangevraagd voor die overeenkomt met de `resource` querytekenreeksparameter van de aanvraag. |
| `token_type` | Het type token, dat een toegangstoken 'Bearer', wat betekent dat de resource kan toegang geven tot de houder van dit token is. |

## <a name="how-to-use-msi-with-azure-sdk-libraries"></a>Het gebruik van MSI met Azure SDK-bibliotheken

Azure ondersteunt meerdere programming platforms door een reeks [Azure SDK's](https://azure.microsoft.com/downloads). Meerdere zijn bijgewerkt ter ondersteuning van aanmelden met een MSI-bestand en bijbehorende voorbeelden om te demonstreren gebruik opgeven. Deze lijst wordt bijgewerkt als aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET | [Implementeren van een VM van Windows-Service-identiteit beheerd met een ARM-sjabloon](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core | [Azure-services aanroepen vanuit een Linux-VM met behulp van de Service-identiteit beheerd](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js| [Beheren van resources met behulp van de Service-identiteit beheerd](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python | [MSI gebruiken om te verifiëren gewoon uit in een virtuele machine](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby   | [Resources beheren van een VM MSI-ingeschakeld](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) | 

## <a name="additional-information"></a>Aanvullende informatie

### <a name="token-expiration"></a>Verlopen van het token

Het lokale MSI-subsysteem caches tokens. Daarom kunt u deze aanroepen zo vaak als u wilt en een aanroep van de kabel naar Azure AD resulteert alleen als:
- een cache ontbreekt deze gebeurtenis treedt op omdat het geen token in de cache
- het token is verlopen

Als u het token in uw code cache, moet u zijn voorbereid voor het afhandelen van scenario's waar de resource geeft aan dat het token is verlopen.

### <a name="resource-ids-for-azure-services"></a>Resource-id voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met services die ondersteuning bieden voor MSI en hun respectieve resource-id.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="sign-in-or-token-acquisition-fails"></a>Aanmelden of token overname mislukt

Controleer of het MSI-bestand juist is ingeschakeld. Ga terug naar de Azure VM in de [Azure-portal](https://portal.azure.com) en:

- Bekijk de pagina 'Configuration' en zorg ervoor dat MSI ingeschakeld = "Yes".
- Bekijk de pagina 'Extensies' en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u wellicht de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt.

### <a name="http-request-error-responses"></a>Foutberichten voor HTTP-aanvraag

- *bad_request_102: vereiste metagegevens-header is niet opgegeven*

  Ofwel de `Metadata` aanvraag-header-veld uit uw aanvraag ontbreken of onjuist is ingedeeld. De waarde moet worden opgegeven als `true`, in alle kleine letters. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld.

- *Onbekende: token ophalen uit Active directory is mislukt. Zie voor meer informatie Logboeken in \<bestandspad\>*

  Controleer of dat uw HTTP GET-aanvraag URI correct is ingedeeld, met name de URI in de queryreeks opgegeven bron. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld of [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met services en hun respectieve resource-id.

- *unknown_source: onbekende bron \<URI\>*

  Controleer of uw HTTP GET-aanvraag URI is juist geformatteerd. De `scheme:host/resource-path` gedeelte moet worden opgegeven als `http://localhost:50342/oauth2/token`. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor het inschakelen van MSI op een Azure VM [configureren van een Azure VM beheerde Service identiteit (MSI) met behulp van PowerShell](msi-qs-configure-powershell-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

