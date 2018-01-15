---
title: Het gebruik van een Azure VM beheerde Service-identiteit voor het verkrijgen van een toegangstoken
description: Stapsgewijze-instructies en voorbeelden voor het gebruik van een Azure VM-MSI voor het verkrijgen van een OAuth toegangstoken.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 6a02b52e7103c9b6e60b09617026fbf6010e76c8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Het gebruik van een Azure VM beheerde Service identiteit (MSI) voor de aanschaf van token 

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Dit artikel bevat verschillende voorbeelden van code en scripts voor het token verkrijgen, evenals richtlijnen over belangrijke onderwerpen, zoals het verwerken van verlopen van het token en HTTP-fouten.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u gebruiken in de Azure PowerShell-voorbeelden in dit artikel wilt, moet u Installeer de nieuwste versie van [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Verbinding'-functie in de Azure portal op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van PowerShell, CLI, een sjabloon of een Azure-SDK). 

## <a name="overview"></a>Overzicht

Een clienttoepassing een MSI-bestand kan aanvragen [app alleen-lezen toegangstoken](develop/active-directory-dev-glossary.md#access-token) voor toegang tot een bepaalde resource. Het token is [op basis van de MSI-service-principal](msi-overview.md#how-does-it-work). Hierdoor is het niet nodig voor de client zich registreert om op te halen van een toegangstoken onder een eigen service-principal. Het token is geschikt voor gebruik als een bearer-token in [service-naar-service roept vereisen clientreferenties](active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Een met behulp van HTTP-token ophalen](#get-a-token-using-http) | Gegevens van het protocol voor het eindpunt van het MSI-token |
| [Een met C#-token ophalen](#get-a-token-using-c) | Voorbeeld van het gebruik van de REST van de MSI-eindpunt van een C#-client |
| [Een token met Ga ophalen](#get-a-token-using-go) | Voorbeeld van het gebruik van het eindpunt van de REST van de MSI van een client Ga |
| [Een token met Azure PowerShell ophalen](#get-a-token-using-azure-powershell) | Voorbeeld van het gebruik van de REST van de MSI-eindpunt van een PowerShell-client |
| [Een token met CURL ophalen](#get-a-token-using-curl) | Voorbeeld van het gebruik van de REST van de MSI-eindpunt van een client Bash/CURL |
| [Verlopen van het token verwerken](#handling-token-expiration) | Richtlijnen voor het verwerken van de toegangstokens te verlopen |
| [Foutafhandeling](#error-handling) | Richtlijnen voor het verwerken van HTTP-fouten geretourneerd door het eindpunt van het MSI-token |
| [Resource-id voor Azure-services](#resource-ids-for-azure-services) | Waar u resource-id's voor ondersteunde Azure-services |

## <a name="get-a-token-using-http"></a>Een met behulp van HTTP-token ophalen 

De fundamentele interface voor het ophalen van een toegangstoken is gebaseerd op REST, toegankelijk maken voor elke clienttoepassing uitgevoerd op de virtuele machine die u kunt u HTTP REST-aanroepen. Dit is vergelijkbaar met het Azure AD-programmeermodel, met uitzondering van de client gebruikt een localhost-eindpunt op de virtuele machine (tegenover een Azure AD-eindpunt).

Voorbeeld van een aanvraag:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Element | Beschrijving |
| ------- | ----------- |
| `GET` | De HTTP-term waarmee u gegevens wilt ophalen van het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://localhost:50342/oauth2/token` | Het MSI-eindpunt, waarbij 50342 is de standaardpoort en kan worden geconfigureerd. |
| `resource` | Een queryreeksparameter opgeven, die aangeeft van de App ID URI van de doelbron. Dit wordt ook weergegeven de `aud` (doelgroep) claim van het gepubliceerde token. In dit voorbeeld vraagt een token voor toegang tot Azure Resource Manager, die een App ID URI van https://management.azure.com/ heeft. |
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
| `access_token` | Het aangevraagde toegangstoken. Wanneer u een beveiligde REST-API aanroept, het token is ingesloten in de `Authorization` aanvraag-header-veld als een token 'bearer', zodat de API voor verificatie van de aanroeper. | 
| `refresh_token` | Niet gebruikt door MSI. |
| `expires_in` | Het aantal seconden dat het toegangstoken geldig is, blijft voordat het verloopt na uitgifte. Tijd van uitgifte vindt u in het token `iat` claim. |
| `expires_on` | De timespan wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `exp` claim). |
| `not_before` | De timespan wanneer het toegangstoken van kracht en kan worden geaccepteerd. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `nbf` claim). |
| `resource` | De resource die het toegangstoken is aangevraagd voor die overeenkomt met de `resource` querytekenreeksparameter van de aanvraag. |
| `token_type` | Het type token, dat een toegangstoken 'Bearer', wat betekent dat de resource kan toegang geven tot de houder van dit token is. |

## <a name="get-a-token-using-c"></a>Een met C#-token ophalen

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

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

## <a name="get-a-token-using-go"></a>Een token met Ga ophalen

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
    msi_endpoint, err := url.Parse("http://localhost:50342/oauth2/token")
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

Het volgende voorbeeld laat zien hoe het MSI-REST-eindpunt van een PowerShell-client om te gebruiken:

1. Verkrijgen van een toegangstoken.
2. Gebruik het toegangstoken aanroepen van een REST-API van Azure Resource Manager en informatie ophalen over de virtuele machine. Zorg ervoor dat uw abonnements-ID, de Resourcegroepnaam en de naam van de virtuele machine voor Vervang `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>`, en `<VM-NAME>`respectievelijk.

```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                              -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>Een token met CURL ophalen

```bash
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Verlopen van het token verwerken

Het lokale MSI-subsysteem caches tokens. Daarom kunt u deze aanroepen zo vaak als u wilt en een aanroep van de kabel naar Azure AD resulteert alleen als:
- een cache ontbreekt deze gebeurtenis treedt op omdat het geen token in de cache
- het token is verlopen

Als u het token in uw code cache, moet u zijn voorbereid voor het afhandelen van scenario's waar de resource geeft aan dat het token is verlopen.

## <a name="error-handling"></a>Foutafhandeling 

Het MSI-eindpunt signalen fouten via het statusveld van de berichtkop voor HTTP-antwoord als 4xx of 5xx-fouten:

| Statuscode | Foutreden | Hoe moet worden verwerkt |
| ----------- | ------------ | ------------- |
| 4XX fout in de aanvraag. | Een of meer van de aanvraagparameters is onjuist. | Niet opnieuw.  Raadpleeg de foutdetails voor meer informatie.  4XX fouten zijn ontwerptijd fouten.|
| 5XX tijdelijke fout van de service. | Onderliggende MSI-systeem of Azure Active Directory een tijdelijke fout geretourneerd. | Het is veilig om opnieuw te proberen na een wachttijd van ten minste 1 seconde.  Als u opnieuw te snel of te vaak, kan Azure AD een snelheid limietfout (429) geretourneerd.|

Als er een fout optreedt, bevat de bijbehorende HTTP-antwoordtekst JSON met de details van fout:

| Element | Beschrijving |
| ------- | ----------- |
| error   | Fout-id. |
| error_description | Uitgebreide beschrijving van de fout. **Beschrijvingen van de fouten kunnen op elk gewenst moment wijzigen. Geen code die vertakkingen op basis van waarden in de foutbeschrijving schrijven.**|

### <a name="http-response-reference"></a>Referentie voor HTTP-antwoord

In dit gedeelte worden de mogelijke foutberichten. Een ' 200 OK ' status is een geslaagde reactie en het toegangstoken is opgenomen in de hoofdtekst van de reactie JSON, in het element access_token.

| Statuscode | Fout | Foutbeschrijving | Oplossing |
| ----------- | ----- | ----------------- | -------- |
| 400 onjuiste aanvraag | invalid_resource | AADSTS50001: De toepassing met de naam  *\<URI\>*  is niet gevonden in de tenant met de naam  *\<TENANT-ID\>*. Dit kan gebeuren als de toepassing niet is geïnstalleerd door de beheerder van de tenant of toestemming gegeven om deze door een gebruiker in de tenant. Misschien hebt u verzonden authenticatie-aanvraag naar de verkeerde tenant. \ | (Alleen voor Linux) |
| 400 onjuiste aanvraag | bad_request_102 | Vereiste metagegevens-header is niet opgegeven | Ofwel de `Metadata` aanvraag-header-veld uit uw aanvraag ontbreken of onjuist is ingedeeld. De waarde moet worden opgegeven als `true`, in alle kleine letters. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld.|
| 401-niet toegestaan | unknown_source | Onbekende bron  *\<URI\>* | Controleer of uw HTTP GET-aanvraag URI is juist geformatteerd. De `scheme:host/resource-path` gedeelte moet worden opgegeven als `http://localhost:50342/oauth2/token`. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld.|
|           | invalid_request | De aanvraag ontbreekt een vereiste parameter, bevat een ongeldige parameterwaarde, bevat een parameter meer dan één keer of anders is onjuist gevormd. |  |
|           | unauthorized_client | De client is niet gemachtigd om aan te vragen van een toegangstoken die met deze methode. | Veroorzaakt door een aanvraag die lokale loopback niet gebruiken voor het aanroepen van de extensie, of op een virtuele machine die niet een MSI-bestand juist geconfigureerd. Zie [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md) als u hulp bij het VM-configuratie nodig. |
|           | ACCESS_DENIED | De resource-eigenaar of autorisatie de aanvraag is geweigerd. |  |
|           | unsupported_response_type | De autorisatie-server biedt geen ondersteuning voor het verkrijgen van een toegangstoken die met deze methode. |  |
|           | invalid_scope | Het aangevraagde bereik is ongeldig, onbekend of onjuist gevormd. |  |
| Interne serverfout 500 | onbekend | Kan geen token ophalen uit Active directory. Zie voor meer informatie Logboeken in  *\<bestandspad\>* | Controleer of MSI is ingeschakeld op de virtuele machine. Zie [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md) als u hulp bij het VM-configuratie nodig.<br><br>Controleer ook of uw HTTP GET-aanvraag URI correct is ingedeeld, met name de URI in de queryreeks opgegeven bron. Zie 'voorbeeldaanvraag' in de [voorgaande sectie REST](#rest) voor een voorbeeld of [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met services en hun respectieve resource-id.

## <a name="resource-ids-for-azure-services"></a>Resource-id voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met bronnen die ondersteuning voor Azure AD en met MSI zijn getest en hun respectieve resource-id.


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor het inschakelen van MSI op een Azure VM [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.








