---
title: Client testen om te vooraf valideren van een virtuele machine - Azure Marketplace | Microsoft Docs
description: Het maken van een client testen voor het valideren van de installatiekopie van een virtuele machine vooraf voor de Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal, Virtual Machine
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: pbutlerm
ms.openlocfilehash: aa46fbce9d11a8fd380a5ee1990c0e7924079a9b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58078683"
---
# <a name="create-a-self-test-client-to-pre-validate-an-azure-virtual-machine-image"></a>Maak een zelftest-client voor het vooraf valideren van een installatiekopie van een virtuele machine van Azure

Gebruik dit artikel als richtlijn voor het maken van een client-service die de API testen verbruikt. De API testen kunt u een virtuele machine (VM) om te controleren of deze voldoet aan de meest recente Azure Marketplace publishing vereisten vooraf valideren. Deze client-service kunt u een virtuele machine testen voordat u uw aanbieding voor Microsoft-certificering indienen.


## <a name="development-and-testing-overview"></a>Ontwikkeling en testen-overzicht

Als onderdeel van het proces zelf testen maakt u een lokale client die verbinding met Azure Marketplace maakt voor het valideren van een virtuele machine die wordt uitgevoerd in uw Azure-abonnement. De virtuele machine kan worden uitgevoerd als het besturingssysteem Windows of Linux.

De lokale client voert een script dat wordt geverifieerd met de API testen, verbindingsgegevens verzendt en ontvangt de resultaten.

De stappen op hoog niveau voor het maken van een client testen zijn:

1. Kies de tenant Azure Active Directory (AD) voor uw toepassing.
2. De clientapp te registreren.
3. Een token voor de client Azure AD-app maken.
4. Het token doorgeven aan de API testen.

Nadat u de client hebt gemaakt, kunt u deze testen op basis van uw virtuele machine.


### <a name="self-test-client-authorization"></a>Clientautorisatie testen

Het volgende diagram toont de werking van autorisatie voor service-to-service aanroepen met behulp van clientreferenties (gedeeld geheim of certificaat).

![Het autorisatieproces client](./media/stclient-dev-process.png)


## <a name="the-self-test-client-api"></a>De client testen-API

De API testen bevat één eindpunt die ondersteuning biedt voor alleen de POST-methode.  De volgende structuur heeft.

```
Uri:             https://isvapp.azurewebsites.net/selftest-vm
Method:          Post
Request Header:  Content-Type: “application/json”
Authorization:   “Bearer xxxx-xxxx-xxxx-xxxxx”
Request body:    The Request body parameters should use the following JSON format:
                 {
                   "DNSName":"XXXX.westus.cloudapp.azure.com",
                   "User":"XXX",
                   "Password":"XXX@1234567",
                   "OS":"XXX",
                   "PortNo":"22",
                   "CompanyName":"ABCD",
                 }

```

De volgende tabel beschrijft de API-velden.


|      Veld         |    Description    |
|  ---------------   |  ---------------  |
|  Autorisatie     |  De tekenreeks 'Bearer xxxx-xxxx-xxxx-xxxxx' bevat het clienttoken van Azure Active Directory (AD), die kan worden gemaakt met behulp van PowerShell.          |
|  DSN-naam           |  DNS-naam van de virtuele machine om te testen    |
|  Gebruiker              |  Gebruikersnaam voor de aanmelding bij de virtuele machine         |
|  Wachtwoord          |  Wachtwoord voor de aanmelding bij de virtuele machine          |
|  OS                |  Besturingssysteem van de virtuele machine: ofwel `Linux` of `Windows`          |
|  PortNo            |  Open poortnummer om verbinding te maken met de virtuele machine. Het poortnummer is doorgaans `22` voor Linux en `5986` voor Windows.          |
|  |  |


## <a name="consuming-the-api"></a>De API gebruiken

U kunt de API testen met PowerShell of cURL verbruiken.


### <a name="use-powershell-to-consume-the-api-on-the-linux-os"></a>PowerShell gebruiken om te gebruiken de API voor de Linux-besturingssysteem

Voor het aanroepen van de API in PowerShell, de volgende stappen uit:

1. Gebruik de `Invoke-WebRequest` opdracht de API aan te roepen.
2. De methode Post is en het inhoudstype is JSON, zoals wordt weergegeven in de volgende code voorbeeld en scherm vastleggen.
3. Geef de van de hoofdtekstparameters in JSON-indeling.

Het volgende codevoorbeeld toont een PowerShell-aanroep naar de API.

```powershell
$accesstoken = “Get token for your Client AAD App”
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Linux"
    "PortNo" = "22"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers; 
$Content = $res | ConvertFrom-Json
```
De volgende Schermafbeelding toont een voorbeeld voor het aanroepen van de API in PowerShell.

![Roep de API met PowerShell voor Linux-besturingssysteem](./media/stclient-call-api-ps-linuxvm.png)

Met behulp van het vorige voorbeeld, kunt u ophalen van de JSON en parseren om op te halen van de volgende gegevens:

```powershell
$testresult = ConvertFrom-Json –InputObject (ConvertFrom-Json –InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++) 
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

De volgende schermopname laat zien op welke `$res.Content`, kunt u de details van de testresultaten in JSON-indeling.

![JSON-resultaten van de PowerShell-aanroep voor Linux](./media/stclient-pslinux-rescontent-json.png)

De volgende Schermafbeelding toont een voorbeeld van JSON de resultaten weergegeven in een online JSON-viewer (bijvoorbeeld [Code Maak](https://codebeautify.org/jsonviewer) of [JSON-Viewer](https://jsonformatter.org/json-viewer)).

![JSON-resultaten van de PowerShell-aanroep voor Linux VM](./media/stclient-consume-api-pslinux-json.png)

### <a name="use-powershell-to-consume-the-api-on-the-windows-os"></a>PowerShell gebruiken om te gebruiken de API voor het Windows-besturingssysteem

Voor het aanroepen van de API in PowerShell, de volgende stappen uit:

1. Gebruik de `Invoke-WebRequest` opdracht de API aan te roepen.
2. De methode Post is en het inhoudstype is JSON, zoals wordt weergegeven in de volgende code voorbeeld en scherm vastleggen.
3. Maak de hoofdtekst van de parameters in JSON-indeling.

Het volgende codevoorbeeld toont een PowerShell-aanroep naar de API.

```powershell
$accesstoken = “Get token for your Client AAD App”
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body = @{
    "DNSName" = "XXXX.westus.cloudapp.azure.com"
    "User" = "XXX"
    "Password" = "XXX@123456"
    "OS" = "Windows"
    "PortNo" = "5986"
    "CompanyName" = "ABCD"

} | ConvertTo-Json
$res = Invoke-WebRequest -Method "Post" -Uri $uri -Body $Body -ContentType "application/json" –Headers $headers;
$Content = $res | ConvertFrom-Json
```

De volgende Schermafbeelding toont een voorbeeld voor het aanroepen van de API in PowerShell.

![Roep de API met PowerShell voor Windows VM](./media/stclient-call-api-ps-windowsvm.png)

Met behulp van het vorige voorbeeld, kunt u ophalen van de JSON en parseren om op te halen van de volgende gegevens:

```powershell
$testresult = ConvertFrom-Json –InputObject (ConvertFrom-Json –InputObject $res)

  Write-Host "OSName: $($testresult.OSName)"
  Write-Host "OSVersion: $($testresult.OSVersion)"
  Write-Host "Overall Test Result: $($testresult.TestResult)"

For ($i=0; $i -lt $testresult.Tests.Length; $i++) 
{
    Write-Host "TestID: $($testresult.Tests[$i].TestID)"
    Write-Host "TestCaseName: $($testresult.Tests[$i].TestCaseName)"
    Write-Host "Description: $($testresult.Tests[$i].Description)"
    Write-Host "Result: $($testresult.Tests[$i].Result)"
    Write-Host "ActualValue: $($testresult.Tests[$i].ActualValue)"
}
```

De volgende schermopname laat zien op welke `$res.Content`, kunt u de details van de testresultaten in JSON-indeling.

![Het aanroepen van JSON-resultaten van PowerShell voor Windows](./media/stclient-pswindows-rescontent-json.png)

De volgende schermafbeelding ziet u de resultaten weergegeven in een online JSON-viewer.
(bijvoorbeeld [Code Maak](https://codebeautify.org/jsonviewer), [JSON-Viewer](https://jsonformatter.org/json-viewer))

![JSON-resultaten van de PowerShell-aanroep voor Windows-VM](./media/stclient-consume-api-pswindows-json.png)

### <a name="use-curl-to-consume-the-api-on-the-linux-os"></a>CURL gebruiken om te gebruiken van de API voor de Linux-besturingssysteem

Volg deze stappen voor het aanroepen van de API met cURL:

1. Gebruik de opdracht curl de API aan te roepen.
2. De methode Post is en het inhoudstype is JSON, zoals wordt weergegeven in het volgende codefragment.

```
CURL POST -H "Content-Type:application/json" 
-H "Authorization: Bearer XXXXXX-Token-XXXXXXXX”
https://isvapp.azurewebsites.net/selftest-vm 
-d '{ "DNSName":"XXXX.westus.cloudapp.azure.com", "User":"XXX", "Password":"XXXX@123456", "OS":"Linux", "PortNo":"22", "CompanyName":"ABCD"}'

```
Het volgende scherm toont een voorbeeld van het gebruik van curl de API aan te roepen.

![API oproepen met curl-opdracht](./media/stclient-consume-api-curl.png)

De volgende schermopname ziet u de JSON-resultaten van de curl-aanroep.

![JSON-resultaten van curl-aanroep](./media/stclient-consume-api-curl-json.png)


## <a name="choose-the-azure-ad-tenant-for-the-app"></a>Kies de Azure AD-tenant voor de app.

Gebruik de volgende stappen uit om te kiezen van de Azure AD-tenant waar u om uw toepassing te maken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Op de bovenste menubalk, selecteert u uw account en kies de Active Directory-tenant waar u uw toepassing registreren bij de adreslijst. Of Selecteer de **map + abonnement** pictogram om te zien van het filter globale abonnementen. De volgende Schermafbeelding toont een voorbeeld van dit filter.

   ![Selecteer het filter voor abonnementen](./media/stclient-subscription-filter.png)

3. Selecteer op de navigatiebalk links **alle services** en selecteer vervolgens **Azure Active Directory**.

   In de volgende stappen, moet u mogelijk de tenantnaam (of de mapnaam) of de tenant-ID (of map-ID).

   **Tenantgegevens ophalen:**
  
   In **overzicht van Azure Active Directory**, zoek naar 'Eigenschappen' en selecteer vervolgens **eigenschappen**. Met behulp van de volgende schermopname als een voorbeeld:

   - **Naam** -de tenantnaam of directory
   - **Map-ID** -het tenant-ID of map-ID of gebruik de schuifbalk te vinden van eigenschappen.

   ![Azure Active Directory-eigenschappenpagina](./media/stclient-aad-properties.png)

## <a name="register-the-client-app"></a>De client-app registreren

Gebruik de volgende stappen uit om de clientapp te registreren.

1. Selecteer op de navigatiebalk links **alle services** en selecteer vervolgens **App-registraties**.
2. Onder **App-registraties**, selecteer **+ nieuwe toepassing registreren**.
3. Onder **maken**, geeft u de gegevens die vereist zijn voor de volgende velden:

   - **Naam** – Geef een beschrijvende naam voor de app. Bijvoorbeeld: 'SelfTestClient'.
   - **Toepassingstype** – Selecteer **Web-App/API**
   - **Aanmeldings-URL** – Type "https://isvapp.azurewebsites.net/selftest-vm"

4. Selecteer **Maken**.
5. Onder **App-registraties** of **geregistreerde app**, Kopieer de **toepassings-ID**.

   ![De toepassings-ID ophalen](./media/stclient-app-id.png)

6. Selecteer in de werkbalk van de geregistreerde app **instellingen**.
7. Selecteer **vereiste machtigingen** om machtigingen voor uw toepassing te configureren.
8. Onder **vereiste machtigingen**, selecteer **+ toevoegen**.
9. Onder **API-toegang toevoegen**, kies **Select an API**.
10. Onder **Select an API**, type "Windows Azure classic deployment model" om te zoeken naar de API.
11. Kies in de lijst met zoekresultaten **klassieke implementatiemodel van Windows Azure** en klik vervolgens op **Selecteer**.

    ![Configureren van meerdere tenants voor app](./media/stclient-select-api.png)

12. Onder **API-toegang toevoegen**, kies **machtigingen selecteren**.
13. Selecteer **'Windows Azure Service Management API' toegang tot**.

    ![API-toegang voor app inschakelen](./media/stclient-enable-api-access.png)

14. Klik op **Selecteren**.
15. Selecteer **Done**.
16. Onder **instellingen**, selecteer **eigenschappen**.
17. Onder **eigenschappen**, schuif omlaag naar **multitenant**. Selecteer **Ja**.  

    ![Configureren van meerdere tenants voor app](./media/stclient-yes-multitenant.png)

18. Selecteer **Opslaan**.
19. Onder **instellingen**, selecteer **sleutels**.
20. Maken van een geheime sleutel door te selecteren van de sleutel **beschrijving** tekstvak. Configureer de volgende velden:

    - Typ de naam van een sleutel. Bijvoorbeeld, "selftestclient"
    - Op de **VERLOOPT** vervolgkeuzelijst, selecteer 'In de 1 jaar'.
    - Selecteer **opslaan** om de sleutel te genereren.
    - Onder **waarde**, Kopieer de sleutel.

      >[!Important]
      >Niet mogelijk om te zien van de sleutelwaarde nadat u sluiten de **sleutels** formulier.

    ![Sleutelwaarde formulier](./media/stclient-create-key.png)

## <a name="create-the-token-for-the-client-app"></a>Het token voor de client-app maken

U kunt een van de volgende programma's gebruiken om te maken en een met de REST-API van OAuth-token verkrijgen:

- Postman
- cURL in Linux
- C&#35;
- PowerShell

### <a name="to-create-and-get-a-token-using-postman"></a>Om te maken en een token met Postman

 Om te stellen Auth0 voor tokens voor het gebruik van uw geautoriseerde toepassingen, uitvoeren van een POST-bewerking op de [ https://login.microsoftonline.com/common/oauth2/token ](https://login.microsoftonline.com/common/oauth2/token) eindpunt met een nettolading van de volgende indeling hebben:

```
Method Type : POST
Base Url: https://login.microsoftonline.com/common/oauth2/token
```
De volgende parameters doorgeven in de hoofdtekst van de aanvraag:

```
Body Content-Type: x-www-form-urlencoded
client_id: XXX (Paste your Application ID of Web App/API Type client AD App)
grant_type: client_credentials
client_secret: XXX (Paste your Secret Key of Web App/API Type client AD App)
resource: https://management.core.windows.net
```

De volgende parameters doorgeven in de aanvraagheader:

```
Content-Type: application/x-www-form-urlencoded
```

De volgende Schermafbeelding toont een voorbeeld van een token krijgen met Postman.

![Ophalen van token met Postman](./media/stclient-postman-get-token.png)

### <a name="to-create-and-get-a-token-using-curl-in-linux"></a>Om te maken en een token met cURL in Linux

Om te stellen Auth0 voor tokens voor het gebruik van uw geautoriseerde toepassingen, uitvoeren van een POST-bewerking op de [ https://login.microsoftonline.com/common/oauth2/token ](https://login.microsoftonline.com/common/oauth2/token) eindpunt met een nettolading van de volgende indeling hebben:

```
Request:

curl --request POST \
                --url 'https://login.microsoftonline.com/common/oauth2/token' \
                --header "Content-Type:application/x-www-form-urlencoded" \
                --data "grant_type=client_credentials&client_id=XXXXX-XXXX-XXXX-XXXXXXX&client_secret=XXXXXXXXX&resource=https://management.core.windows.net"

Response:

{"token":"UClCUUKxUlkdbhE1cHLz3kyjbIZYVh9eB34A5Q21Y3FPqKGSJs","expires":"2014-02-17 18:46:08"}
```

De volgende Schermafbeelding toont een voorbeeld van een token krijgen met de curl-opdracht.

![Ophalen van token met curl-opdracht](./media/stclient-curl-get-token.png)

### <a name="to-create-and-get-a-token-using-c35"></a>Om te maken en een met c#-token verkrijgen&#35;

Om te stellen Auth0 voor tokens voor het gebruik van uw geautoriseerde toepassingen, uitvoeren van een POST-bewerking op de [ https://soamtenant.auth0.com/oauth/token ](https://soamtenant.auth0.com/oauth/token) eindpunt met een nettolading van de volgende indeling hebben:

```
string clientId = "Your Application Id";
string clientSecret = "Your Application Secret";
string audience = "https://management.core.windows.net";
string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, "https://login.microsoftonline.com/common/oauth2/token");
string grantType = "client_credentials";
var client = new RestClient(authority);
var request = new RestRequest(Method.POST);
       request.AddHeader("content-type", "application/x-www-form- urlencoded");

string requestBody = "grant_type=" + grantType + "&" + "client_id=" +   clientId + "&" + "client_secret=" + clientSecret + "&" + "resource=" +  audience;

request.AddParameter("application/x-www-form-urlencoded", requestBody,  ParameterType.RequestBody);

IRestResponse response = client.Execute(request);
var content = response.Content;
var token = JObject.Parse(content)["access_token"];
```

### <a name="to-create-and-get-a-token-using-powershell"></a>Om te maken en een token met behulp van PowerShell

Om te stellen Auth0 voor tokens voor het gebruik van uw geautoriseerde toepassingen, uitvoeren van een POST-bewerking op de [ https://soamtenant.auth0.com/oauth/token ](https://soamtenant.auth0.com/oauth/token) eindpunt met een nettolading van de volgende indeling hebben:

```
$clientId = "Application Id of AD Client APP";
$clientSecret = "Secret Key of AD Client APP “
$audience = "https://management.core.windows.net";
$authority = "https://login.microsoftonline.com/common/oauth2/token"
$grantType = "client_credentials";

$requestBody = "grant_type=" + $grantType + "&" + "client_id=" +  $clientId   + "&" + "client_secret=" + $clientSecret + "&" + "resource=" + $audience;

$headers = New-Object  "System.Collections.Generic.Dictionary[[String],[String]]"
             $headers.Add("ContentType", "application/x-www-form-urlencoded")
resp = Invoke-WebRequest -Method Post -Uri $authority -Headers $headers -ContentType 'application/x-www-form-urlencoded' -Body $requestBody

$token = $resp.Content | ConvertFrom-Json
$token.AccessToken

```

## <a name="pass-the-client-app-token-to-the-api"></a>App-token van de client doorgeeft aan de API

Het token doorgeven aan de API testen met behulp van de volgende code in de autorisatie-header:

```
$redirectUri = ‘https://isvapp.azurewebsites.net/selftest-vm’
$accesstoken = ‘place your token here’

$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Bearer $accesstoken")
$Body =
@{
      'DNSName'="XXXX.cloudapp.azure.com";
      'User'="XXX";
      'Password'="XXXX@12345";
      'OS'="Linux";
      'PortNo'="22"
  } | ConvertTo-Json

$result=Invoke-WebRequest -Method Post -Uri $redirectUri -Headers $headers -ContentType 'application/json' -Body $Body
$result
echo 'Test Results:'
$result.Content

```

## <a name="test-your-self-test-client"></a>Testen van uw client testen

Als u wilt testen van de client, de volgende stappen uit:

1. Implementeer de virtuele machine die u wilt testen.
2. Roep de API testen met behulp van uw client-app-token voor autorisatie.
3. De resultaten van de ophalen in JSON-indeling.

### <a name="test-result-examples"></a>Voorbeelden van resultaat testen

De volgende fragmenten ziet de resultaten in JSON-indeling.

**Testresultaten voor een Windows-VM:**

```
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Microsoft Windows Server 2016 Datacenter",
  "OSVersion": "10.0.14393",
  "CreatedDate": "06/01/2018 07:48:04",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "Tes tID": "1.1.4",
      "TestCaseName": "OS Architecture",
      "Description": "Azure supports 64bit Operating System Only.",
      "Result": "Passed",
      "ActualValue": "64 Bit",
      "Re quiredValue": "OS Should be 64 bit"
    },
    {
      "TestID": "1.1.5",
      "TestCaseName": "User account dependency",
      "Description": "Application execution should not have de pendency on administrator account.",
      "Result": "Passed",
      "ActualValue": "isv",
      "RequiredValue": "Logged in user should not be an administrator"
    },
    {
      "TestID": " 1.1.6",
      "TestCaseName": "Failover Cluster",
      "Description": "Windows Server Failover Clustering feature is not yet supported, Application should not have dependency on this feature.",
      "Result": "Passed",
      "ActualValue": "Disabled",
      "RequiredValue": "Failover Clustering feature is disabled"
    },
```

**Testresultaten voor een Linux-VM:**

```
{
  "SchemaVersion": 1,
  "AppCertificationCategory": "Microsoft Single VM Certification",
  "ProviderID": "050DE427-2A99-46D4-817C-5354D3BF2AE8",
  "OSName": "Ubuntu  16.04",
  "OSVersion": "16.04",
  "CreatedDate": "06/01/2018 07:04:24",
  "TestResult": "Pass",
  "APIVersion": "1.1",
  "Tests": [
    {
      "TestID": "48",
      "TestCaseName": "Bash Hi story",
      "Description": "Bash history files should be cleared before creating the VM image.",
      "Result": "Passed",
      "ActualValue": "No file Exist",
      "RequiredVal ue": "1024"
    },
    {
      "TestID": "39",
      "TestCaseName": "Linux Agent Version",
      "Description": "Azure Linux Agent 2.2.10 and above should be installed. ",
      "Result": "Pas sed",
      "ActualValue": "2.2.20",
      "RequiredValue": "2.2.10"
    },
    {
      "TestID": "40",
      "TestCaseName": "Required Kernel Parameters",
      "Description": "Verifies the following  kernel parameters are set console=ttyS0, earlyprintk=ttyS0, rootdelay=300",
      "Result": "Passed",
      "ActualValue": "Matched Parameter: console=ttyS0,earlypri ntk=ttyS0,rootdelay=300",
      "RequiredValue": "console=ttyS0#earlyprintk=ttyS0#rootdelay=300"
    },
```

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt uw virtuele machine van Azure is getest, kunt u [de aanbieding publiceren](./cpp-publish-offer.md).
