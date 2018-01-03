---
title: Het gebruik van een gebruiker toegewezen beheerde Service-identiteit voor het verkrijgen van een toegangstoken op een virtuele machine.
description: Stapsgewijze-instructies en voorbeelden voor het gebruik van een gebruiker toegewezen MSI van een virtuele machine in Azure te verkrijgen van een OAuth toegangstoken.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 5c9bf052ecb2e9c79e0eb627a0fd709d587125cd
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="acquire-an-access-token-for-a-vm-user-assigned-managed-service-identity-msi"></a>Verkrijgen van een toegangstoken voor een beheerde Service identiteit (MSI) gebruiker toegewezen VM

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Dit artikel bevat verschillende voorbeelden van code en scripts voor het token verkrijgen, evenals richtlijnen over belangrijke onderwerpen, zoals het verwerken van verlopen van het token en HTTP-fouten.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Als u gebruiken in de Azure PowerShell-voorbeelden in dit artikel wilt, moet u Installeer de nieuwste versie van [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!IMPORTANT]
> - Alle code/voorbeeldscript in dit artikel wordt ervan uitgegaan dat de client wordt uitgevoerd op een virtuele Machine van MSI-functionaliteit. Gebruik de virtuele machine 'Verbinding'-functie in de Azure portal op afstand verbinding maken met uw virtuele machine. Zie voor meer informatie over het inschakelen van MSI op een virtuele machine [configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine met Azure CLI](msi-qs-configure-cli-windows-vm.md), of een van de variant artikelen (met behulp van de Portal, PowerShell, een sjabloon of een Azure-SDK). 

## <a name="overview"></a>Overzicht

Een clienttoepassing een MSI-bestand kan aanvragen [app alleen-lezen toegangstoken](~/articles/active-directory/develop/active-directory-dev-glossary.md#access-token) voor toegang tot een bepaalde resource. Het token is [op basis van de MSI-service-principal](msi-overview.md#how-does-it-work). Hierdoor is het niet nodig voor de client zich registreert om op te halen van een toegangstoken onder een eigen service-principal. Het token is geschikt voor gebruik als een bearer-token in [service-naar-service roept vereisen clientreferenties](~/articles/active-directory/active-directory-protocols-oauth-service-to-service.md).

|  |  |
| -------------- | -------------------- |
| [Een met behulp van HTTP-token ophalen](#get-a-token-using-http) | Gegevens van het protocol voor het eindpunt van het MSI-token |
| [Een token met CURL ophalen](#get-a-token-using-curl) | Voorbeeld van het gebruik van de REST van de MSI-eindpunt van een client Bash/CURL |
| [Verlopen van het token verwerken](#handling-token-expiration) | Richtlijnen voor het verwerken van de toegangstokens te verlopen |
| [Foutafhandeling](#error-handling) | Richtlijnen voor het verwerken van HTTP-fouten geretourneerd door het eindpunt van het MSI-token |
| [Resource-id voor Azure-services](#resource-ids-for-azure-services) | Waar u resource-id's voor ondersteunde Azure-services |

## <a name="get-a-token-using-http"></a>Een met behulp van HTTP-token ophalen 

De fundamentele interface voor het ophalen van een toegangstoken is gebaseerd op REST, toegankelijk maken voor elke clienttoepassing uitgevoerd op de virtuele machine die u kunt u HTTP REST-aanroepen. Dit is vergelijkbaar met het Azure AD-programmeermodel, met uitzondering van de client gebruikt een localhost-eindpunt op de virtuele machine (tegenover een Azure AD-eindpunt).

Voorbeeld van een aanvraag:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F&client_id=712eac09-e943-418c-9be6-9fd5c91078bl HTTP/1.1
Metadata: true
```

| Element | Beschrijving |
| ------- | ----------- |
| `GET` | De HTTP-term waarmee u gegevens wilt ophalen van het eindpunt. In dit geval een OAuth-toegangstoken. | 
| `http://localhost:50342/oauth2/token` | Het MSI-eindpunt, waarbij 50342 is de standaardpoort en kan worden geconfigureerd. |
| `resource` | Een queryreeksparameter opgeven, die aangeeft van de App ID URI van de doelbron. Dit wordt ook weergegeven de `aud` (doelgroep) claim van het gepubliceerde token. In dit voorbeeld vraagt een token voor toegang tot Azure Resource Manager, die een App ID URI van https://management.azure.com/ heeft. |
| `client_id` | Een queryreeksparameter opgeven, die de client-ID (ook wel bekend als een App-ID) voor de service-principal voor de gebruiker toegewezen MSI aangeeft. Deze waarde wordt geretourneerd als de `clientId` eigenschap tijdens het maken van een MSI gebruiker toegewezen. In dit voorbeeld vraagt een token voor client-ID '712eac09-e943-418c-9be6-9fd5c91078bl'. |
| `Metadata` | Een HTTP-aanvraagveld header wordt vereist door MSI als een risicobeperking tegen aanvallen van Server Side aanvraag kunnen worden vervalst (SSRF). Deze waarde moet worden ingesteld op 'true', in alle kleine letters.

Voorbeeldreactie:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
  "client_id":"712eac09-e943-418c-9be6-9fd5c91078bl"
}
```

| Element | Beschrijving |
| ------- | ----------- |
| `access_token` | Het aangevraagde toegangstoken. Wanneer u een beveiligde REST-API aanroept, het token is ingesloten in de `Authorization` aanvraag-header-veld als een token 'bearer', zodat de API voor verificatie van de aanroeper. | 
| `expires_in` | Het aantal seconden dat het toegangstoken geldig is, blijft voordat het verloopt na uitgifte. Tijd van uitgifte vindt u in het token `iat` claim. |
| `expires_on` | De timespan wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `exp` claim). |
| `not_before` | De timespan wanneer het toegangstoken van kracht en kan worden geaccepteerd. De datum die wordt weergegeven als het aantal seconden van ' 1970-01-01T0:0:0Z UTC ' (komt overeen met het token `nbf` claim). |
| `resource` | De resource die het toegangstoken is aangevraagd voor die overeenkomt met de `resource` querytekenreeksparameter van de aanvraag. |
| `token_type` | Het type token, dat een toegangstoken 'Bearer', wat betekent dat de resource kan toegang geven tot de houder van dit token is. |
| `client_id` | De client-ID (ook wel bekend als een App-ID) van de service-principal voor de gebruiker toegewezen MSI waarvoor het token is aangevraagd. |

## <a name="get-a-token-using-curl"></a>Een token met CURL ophalen

Zorg ervoor dat de vervangen door de client-ID (ook wel bekend als een App-ID) van de gebruiker toegewezen MSI service-principal voor de <MSI CLIENT ID> waarde van de `client_id` parameter. Deze waarde wordt geretourneerd als de `clientId` eigenschap tijdens het maken van een MSI gebruiker toegewezen.

   ```bash
   response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=<MSI CLIENT ID>" -H Metadata:true -s)
   access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   echo The MSI access token is $access_token
   ```

   Voorbeeld van antwoorden:

   ```bash
   user@vmLinux:~$ response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/&client_id=9d484c98-b99d-420e-939c-z585174b63bl" -H Metadata:true -s)
   user@vmLinux:~$ access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
   user@vmLinux:~$ echo The MSI access token is $access_token
   The MSI access token is eyJ0eXAiOiJKV1QiLCJhbGciO...
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
| 400 onjuiste aanvraag | bad_request_102 | Vereiste metagegevens-header is niet opgegeven | Ofwel de `Metadata` aanvraag-header-veld uit uw aanvraag ontbreken of onjuist is ingedeeld. De waarde moet worden opgegeven als `true`, in alle kleine letters. Zie 'voorbeeldaanvraag' in de [ophalen van een token met behulp van HTTP](#get-a-token-using-http) sectie voor een voorbeeld.|
| 401-niet toegestaan | unknown_source | Onbekende bron  *\<URI\>* | Controleer of uw HTTP GET-aanvraag URI is juist geformatteerd. De `scheme:host/resource-path` gedeelte moet worden opgegeven als `http://localhost:50342/oauth2/token`. Zie 'voorbeeldaanvraag' in de [ophalen van een token met behulp van HTTP](#get-a-token-using-http) sectie voor een voorbeeld.|
|           | invalid_request | De aanvraag ontbreekt een vereiste parameter, bevat een ongeldige parameterwaarde, bevat een parameter meer dan één keer of anders is onjuist gevormd. |  |
|           | unauthorized_client | De client is niet gemachtigd om aan te vragen van een toegangstoken die met deze methode. | Veroorzaakt door een aanvraag die lokale loopback niet gebruiken voor het aanroepen van de extensie, of op een virtuele machine die niet een MSI-bestand juist geconfigureerd. Zie [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md) als u hulp bij het VM-configuratie nodig. |
|           | ACCESS_DENIED | De resource-eigenaar of autorisatie de aanvraag is geweigerd. |  |
|           | unsupported_response_type | De autorisatie-server biedt geen ondersteuning voor het verkrijgen van een toegangstoken die met deze methode. |  |
|           | invalid_scope | Het aangevraagde bereik is ongeldig, onbekend of onjuist gevormd. |  |
| Interne serverfout 500 | onbekend | Kan geen token ophalen uit Active directory. Zie voor meer informatie Logboeken in  *\<bestandspad\>* | Controleer of MSI is ingeschakeld op de virtuele machine. Zie [configureren van een VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md) als u hulp bij het VM-configuratie nodig.<br><br>Controleer ook of uw HTTP GET-aanvraag URI correct is ingedeeld, met name de URI in de queryreeks opgegeven bron. Zie 'voorbeeldaanvraag' in de [ophalen van een token met behulp van HTTP](#get-a-token-using-http) sectie voor een voorbeeld of [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met services en hun respectieve resource-id.

## <a name="resource-ids-for-azure-services"></a>Resource-id voor Azure-services

Zie [Azure-services die ondersteuning voor Azure AD authentication](msi-overview.md#azure-services-that-support-azure-ad-authentication) voor een lijst met bronnen die ondersteuning voor Azure AD en met MSI zijn getest en hun respectieve resource-id.


## <a name="next-steps"></a>Volgende stappen

- Zie voor het inschakelen van MSI op een Azure VM [configureren van een gebruiker toegewezen beheerde Service identiteit (MSI) voor een virtuele machine met Azure CLI](msi-qs-configure-cli-windows-vm.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.








