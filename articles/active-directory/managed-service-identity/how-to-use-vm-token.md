---
title: Hoe u een Azure VM Managed Service Identity met een toegangstoken verkrijgen
description: Stapsgewijze-instructies en voorbeelden voor het gebruik van een Azure VM-MSI-bestand voor het verkrijgen van een OAuth toegangstoken.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: ee4702733e775051cbbcace109bd1a7ffdf50e9c
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325452"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Over het gebruik van een Azure VM Managed Service Identity (MSI) voor het ophalen van tokens 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

Dit artikel bevat verschillende voorbeelden van code en scripts voor het ophalen van tokens, alsmede informatie over belangrijke onderwerpen, zoals het verlopen van het token en het HTTP-fouten. 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u van plan bent de Azure PowerShell-voorbeelden in dit artikel gebruiken, moet u Installeer de nieuwste versie van [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine met een beheerde Service-identiteit. Gebruik de virtuele machine 'Connect'-functie in Azure portal, op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI-bestand op een virtuele machine [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 

> [!IMPORTANT]
> - De beveiligingsgrens van een beheerde Service-identiteit is de resource die wordt gebruikt op. Alle code/scripts die worden uitgevoerd op een virtuele Machine kunt aanvragen en ophalen van tokens voor alle beheerde Service-identiteit beschikbaar is op deze. 

## <a name="overview"></a>Overzicht

Een clienttoepassing kan een beheerde Service-identiteit aanvragen [alleen app-toegangstoken](../develop/active-directory-dev-glossary.md#access-token) voor toegang tot een bepaalde resource. Het token is [op basis van de MSI-service-principal](overview.md#how-does-it-work). Er is daarom niet nodig voor de client zich registreert voor een toegangstoken onder een eigen service-principal. Het token is geschikt voor gebruik als een bearer-token in [service-naar-service aanroepen vereisen clientreferenties](../develop/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Een met behulp van HTTP-token verkrijgen](#get-a-token-using-http) | Protocoldetails van het voor het eindpunt van de MSI-token |
| [Een token met de Microsoft.Azure.Services.AppAuthentication-clientbibliotheek voor .NET ophalen](#get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net) | Voorbeeld van het gebruik van de bibliotheek Microsoft.Azure.Services.AppAuthentication vanuit een .NET-client
| [Een met C#-token verkrijgen](#get-a-token-using-c) | Voorbeeld van het gebruik van het MSI-REST-eindpunt van een C#-client |
| [Een token met behulp van Go ophalen](#get-a-token-using-go) | Voorbeeld van het gebruik van het MSI-REST-eindpunt van een Go-client |
| [Een token met Azure PowerShell ophalen](#get-a-token-using-azure-powershell) | Voorbeeld van het gebruik van het MSI-REST-eindpunt van een PowerShell-client |
| [Ophalen van een token met CURL](#get-a-token-using-curl) | Voorbeeld van het gebruik van het MSI-REST-eindpunt van een client Bash/CURL |
| [Afhandeling van token in cache opslaan](#handling-token-caching) | Richtlijnen voor het verwerken van verlopen toegangstokens |
| [Foutafhandeling](#error-handling) | Richtlijnen voor het verwerken van HTTP-fouten geretourneerd door het MSI-tokeneindpunt |
| [Resource-id's voor Azure-services](#resource-ids-for-azure-services) | Waar u de resource-id's ophalen voor ondersteunde Azure-services |

## <a name="get-a-token-using-http"></a>Een met behulp van HTTP-token verkrijgen 

De fundamentele interface voor het verkrijgen van een toegangstoken is gebaseerd op REST, waardoor het toegankelijk is voor elke clienttoepassing die worden uitgevoerd op de virtuele machine om een HTTP REST-aanroepen. Dit is vergelijkbaar met het Azure AD-programmeermodel, tenzij de client maakt gebruik van een eindpunt op de virtuele machine (Visual Studio een Azure AD-eindpunt).

Voorbeeld van een aanvraag met behulp van het eindpunt van Azure Instance Metadata Service (IMDS) *(aanbevolen)*:

```
GET 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' HTTP/1.1 Metadata: true
```

| Element | Beschrijving |
| ------- | ----------- |
| `GET` | De HTTP-term, die aangeeft dat u wilt ophalen van gegevens uit het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | Het MSI-eindpunt voor de Instance Metadata Service. |
| `api-version`  | Een queryreeks-parameter, die de API-versie voor het eindpunt IMDS aangeeft. Gebruik API-versie `2018-02-01` of hoger. |
| `resource` | Een queryreeks-parameter, die wijzen op de URI van de App-ID van de doelresource. Het verschijnt ook in de `aud` claim (doelgroep) van de uitgegeven tokens. In dit voorbeeld vraagt een token voor toegang tot Azure Resource Manager heeft een URI van de App-ID van https://management.azure.com/. |
| `Metadata` | Een HTTP-aanvraag veld header wordt vereist door MSI-bestand als een bescherming tegen aanvallen van Server Side aanvraag kunnen worden vervalst (SSRF). Deze waarde moet worden ingesteld op "true", volledig in kleine letters. |
| `object_id` | (Optioneel) Een queryreeks-parameter, die de object_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere gebruiker beheerde identiteiten toegewezen heeft.|
| `client_id` | (Optioneel) Een queryreeks-parameter, die de client_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere gebruiker beheerde identiteiten toegewezen heeft.|

Voorbeeld van een aanvraag met behulp van het eindpunt van de VM-extensie voor de Managed Service Identity (MSI) *(om te worden afgeschaft)*:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Beschrijving |
| ------- | ----------- |
| `GET` | De HTTP-term, die aangeeft dat u wilt ophalen van gegevens uit het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://localhost:50342/oauth2/token` | Het MSI-eindpunt, waarbij 50342 is de standaardpoort en kan worden geconfigureerd. |
| `resource` | Een queryreeks-parameter, die wijzen op de URI van de App-ID van de doelresource. Het verschijnt ook in de `aud` claim (doelgroep) van de uitgegeven tokens. In dit voorbeeld vraagt een token voor toegang tot Azure Resource Manager heeft een URI van de App-ID van https://management.azure.com/. |
| `Metadata` | Een HTTP-aanvraag veld header wordt vereist door MSI-bestand als een bescherming tegen aanvallen van Server Side aanvraag kunnen worden vervalst (SSRF). Deze waarde moet worden ingesteld op "true", volledig in kleine letters.|
| `object_id` | (Optioneel) Een queryreeks-parameter, die de object_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere gebruiker beheerde identiteiten toegewezen heeft.|
| `client_id` | (Optioneel) Een queryreeks-parameter, die de client_id van de beheerde identiteit die u wilt dat het token voor aangeeft. Vereist, als uw virtuele machine meerdere gebruiker beheerde identiteiten toegewezen heeft.|


Het voorbeeldantwoord:

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
| `access_token` | Het aangevraagde toegangstoken. Bij het aanroepen van een beveiligde REST-API, het token is ingesloten in de `Authorization` headerveld aanvraag als een token 'bearer', zodat de API voor verificatie van de oproepende functie. | 
| `refresh_token` | Niet gebruikt door MSI-bestand. |
| `expires_in` | Het aantal seconden dat het toegangstoken nog steeds geldig zijn voordat het verloopt na uitgifte. Tijd van uitgifte kunt u vinden in van het token `iat` claim. |
| `expires_on` | De timespan wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met van het token `exp` claim). |
| `not_before` | De timespan wanneer het toegangstoken wordt van kracht en kan worden geaccepteerd. De datum wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met van het token `nbf` claim). |
| `resource` | De resource het toegangstoken is aangevraagd voor, die overeenkomt met de `resource` query-tekenreeksparameter van de aanvraag. |
| `token_type` | Het type token, dat een toegangstoken 'Bearer', wat betekent dat de resource kan toegang geven tot de houder van dit token is. |

## <a name="get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net"></a>Een token met de Microsoft.Azure.Services.AppAuthentication-clientbibliotheek voor .NET ophalen

Voor .NET-toepassingen en -functies is de eenvoudigste manier om te werken met een beheerde service-identiteit via de Microsoft.Azure.Services.AppAuthentication-pakket. Deze bibliotheek wordt u ook de mogelijkheid voor het testen van uw code lokaal op uw ontwikkelcomputer, met behulp van uw gebruikersaccount vanuit Visual Studio, de [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest), of geïntegreerde verificatie van Active Directory. Zie voor meer informatie over opties voor lokale ontwikkeling met deze bibliotheek [Microsoft.Azure.Services.AppAuthentication reference]. Deze sectie leest u hoe u aan de slag met de bibliotheek in uw code.

1. Voeg verwijzingen toe aan de [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) en [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet-pakketten aan uw toepassing.

2.  Voeg de volgende code toe aan uw toepassing:

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```
    
Zie voor meer informatie over Microsoft.Azure.Services.AppAuthentication en de bewerkingen die beschikbaar worden gemaakt, de [Microsoft.Azure.Services.AppAuthentication verwijzing](/azure/key-vault/service-to-service-authentication) en de [App Service en de Key Vault met MSI-.NET voorbeeld](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

## <a name="get-a-token-using-c"></a>Een met C#-token verkrijgen

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
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

## <a name="get-a-token-using-go"></a>Een token met behulp van Go ophalen

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for MSI token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call MSI /token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Een token met Azure PowerShell ophalen

Het volgende voorbeeld ziet u hoe u het MSI-REST-eindpunt van een PowerShell-client te gebruiken:

1. Een toegangstoken verkrijgen.
2. Gebruik het toegangstoken voor een Azure Resource Manager REST-API aanroepen en informatie over de virtuele machine. Zorg ervoor dat u vervangen door uw abonnements-ID, de naam van de resourcegroep en de naam van de virtuele machine voor `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>`, en `<VM-NAME>`, respectievelijk.

```azurepowershell
Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Headers @{Metadata="true"}
```

Voorbeeld voor het parseren van het toegangstoken uit het antwoord:
```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F `
                              -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>Ophalen van een token met CURL

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s
```


Voorbeeld voor het parseren van het toegangstoken uit het antwoord:

```bash
response=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="token-caching"></a>Token in cache opslaan

Terwijl het Managed Service Identity (MSI)-subsysteem wordt gebruikt (IMDS/MSI VM-extensie) in cache, tokens, wordt ook aangeraden voor het implementeren van token in cache opslaan in uw code. Als gevolg hiervan moet u voorbereiden voor scenario's waarin de resource geeft aan dat het token is verlopen. 

De wire-aanroepen naar Azure AD alleen resultaat wanneer:
- Cachemisser treedt op omdat er geen token in de cache van de MSI-subsysteem
- het in cache opgeslagen token is verlopen

## <a name="error-handling"></a>Foutafhandeling

Het eindpunt van de beheerde Service-identiteit signalen fouten via het statusveld van de berichtkop van HTTP-antwoord als 4xx of 5xx-fouten:

| Statuscode | Foutreden | Hoe worden verwerkt |
| ----------- | ------------ | ------------- |
| 404 niet gevonden. | IMDS eindpunt wordt bijgewerkt. | Probeer het opnieuw met Expontential uitstel. Zie de onderstaande richtlijnen. |
| 429 te veel aanvragen. |  IMDS beperken limiet is bereikt. | Opnieuw proberen met exponentieel uitstel. Zie de onderstaande richtlijnen. |
| 4XX-fout bij de aanvraag. | Een of meer van de parameters van de aanvraag is onjuist. | Probeer niet opnieuw.  Bekijk de foutdetails voor meer informatie.  4XX-fouten zijn moment van ontwerp fouten.|
| 5XX tijdelijke fout van de service. | De MSI-subsysteem of Azure Active Directory een tijdelijke fout geretourneerd. | Het is veilig om opnieuw te proberen na een wachttijd van ten minste 1 seconde.  Als u nieuwe te snel of te vaak pogingen, mogelijk IMDS en/of Azure AD een tarief limietfout (429) geretourneerd.|
| timeout | IMDS eindpunt wordt bijgewerkt. | Probeer het opnieuw met Expontential uitstel. Zie de onderstaande richtlijnen. |

Als er een fout optreedt, bevat de bijbehorende HTTP-antwoordtekst JSON met details van de fout:

| Element | Beschrijving |
| ------- | ----------- |
| error   | Fout-id. |
| error_description | Uitgebreide beschrijving van de fout. **Beschrijvingen van de fouten kunnen op elk gewenst moment wijzigen. Geen code die vertakkingen op basis van waarden in de beschrijving van de fout schrijven.**|

### <a name="http-response-reference"></a>Naslaginformatie over HTTP-antwoord

In deze sectie worden de mogelijke foutberichten. Een "200 OK ' status is een geslaagde respons en het toegangstoken is opgenomen in de antwoordtekst JSON, in het access_token-element.

| Statuscode | Fout | Foutbeschrijving | Oplossing |
| ----------- | ----- | ----------------- | -------- |
| 400-Ongeldige aanvraag | invalid_resource | AADSTS50001: De toepassing met de naam *\<URI\>* is niet gevonden in de tenant met de naam  *\<TENANT-ID\>*. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant of toegestaan door een gebruiker in de tenant. Misschien hebt u verzonden de verificatieaanvraag naar de verkeerde tenant. \ | (Alleen Linux) |
| 400-Ongeldige aanvraag | bad_request_102 | Vereiste metagegevens-header is niet opgegeven | Een van beide de `Metadata` koptekst van het veld van uw aanvraag, ontbreekt of heeft een onjuiste indeling. De waarde moet worden opgegeven als `true`, volledig in kleine letters. Zie de aanvraag"voorbeeld" in de [voorgaande sectie REST](#rest) voor een voorbeeld.|
| 401-niet toegestaan | unknown_source | Onbekende bron  *\<URI\>* | Controleer of uw aanvraag HTTP GET-URI is ingedeeld. De `scheme:host/resource-path` deel moet worden opgegeven als `http://localhost:50342/oauth2/token`. Zie de aanvraag"voorbeeld" in de [voorgaande sectie REST](#rest) voor een voorbeeld.|
|           | invalid_request | De aanvraag ontbreekt een vereiste parameter, bevat een ongeldige parameter-waarde, bevat een parameter meer dan één keer of anders is onjuist gevormd. |  |
|           | unauthorized_client | De client is niet gemachtigd om aan te vragen van een toegangstoken met deze methode. | Veroorzaakt door een aanvraag die lokale loopback hebt gebruikt voor het aanroepen van de extensie, of op een virtuele machine waaraan geen een MSI-bestand correct geconfigureerd. Zie [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md) als u hulp nodig met VM-configuratie. |
|           | ACCESS_DENIED | De resource-eigenaar of autorisatieserver server heeft de aanvraag geweigerd. |  |
|           | unsupported_response_type | De autorisatie-server biedt geen ondersteuning voor het verkrijgen van een toegangstoken met deze methode. |  |
|           | invalid_scope | Het aangevraagde bereik is ongeldig, onbekend of onjuist gevormd. |  |
| 500 Interne serverfout | onbekend | Kan geen token ophalen uit de Active directory. Zie voor meer informatie de logboeken in  *\<bestandspad\>* | Controleer of de MSI-bestand dat is ingeschakeld op de virtuele machine. Zie [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md) als u hulp nodig met VM-configuratie.<br><br>Controleer ook of dat uw aanvraag HTTP GET-URI is juist opgemaakt, met name de URI die is opgegeven in de querytekenreeks resource. Zie de aanvraag"voorbeeld" in de [voorgaande sectie REST](#rest) voor een voorbeeld of [Azure-services die ondersteuning voor Azure AD-verificatie](services-support-msi.md) voor een lijst met services en hun respectieve resource-id's.

## <a name="retry-guidance"></a>Richtlijnen voor opnieuw proberen 

Het verdient aanbeveling om opnieuw te proberen als er een 404-fout, 429 en 5xx-foutcode (Zie [foutafhandeling](#error-handling) hierboven).

Bandbreedtebeperking limieten gelden voor het aantal oproepen naar het eindpunt IMDS. Wanneer de bandbreedteregeling drempelwaarde wordt overschreden, beperkt IMDS eindpunt nieuwe aanvragen terwijl de vertraging van kracht is. Tijdens deze periode, het eindpunt IMDS retourneert de statuscode HTTP 429 ("te veel aanvragen '), en de aanvragen mislukken. 

Voor opnieuw proberen raden we aan de volgende strategie: 

| **Strategie voor opnieuw proberen** | **Instellingen** | **Waarden** | **Hoe werkt het?** |
| --- | --- | --- | --- |
|ExponentialBackoff |Aantal pogingen<br />Min. uitstel<br />Max. uitstel<br />Delta-uitstel<br />Eerste snelle poging |5<br />0 sec.<br />60 sec.<br />2 sec.<br />false |Poging 1, vertraging 0 sec.<br />Poging 2, vertraging ~2 sec.<br />Poging 3, vertraging ~6 sec.<br />Poging 4, vertraging ~14 sec.<br />Poging 5, vertraging ~30 sec. |

## <a name="resource-ids-for-azure-services"></a>Resource-id's voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD-verificatie](services-support-msi.md) voor een lijst van resources met Azure AD ondersteunen en die zijn getest met MSI-bestand en hun respectieve resource-id's.


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor het inschakelen van MSI-bestand op een Azure VM [configureren van een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.








