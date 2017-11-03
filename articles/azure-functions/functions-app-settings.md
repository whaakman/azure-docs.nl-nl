---
title: App-naslaginformatie voor Azure Functions
description: Documentatie voor de app-instellingen van Azure Functions of omgevingsvariabelen.
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/26/2017
ms.author: tdykstra
ms.openlocfilehash: ce7bf2cf650b0df7e8998766b2d3f5a37c4a1b72
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="app-settings-reference-for-azure-functions"></a>App-naslaginformatie voor Azure Functions

Appinstellingen in een functie-app bevat globale configuratie-opties die invloed hebben op alle functies voor die functie-app. Wanneer u lokaal uitvoert, worden deze instellingen zijn in omgevingsvariabelen. In dit artikel vindt u de appinstellingen die beschikbaar in de functie apps zijn.

Er zijn andere globale configuratie-opties in de [host.json](functions-host-json.md) bestand en in de [local.settings.json](functions-run-local.md#local-settings-file) bestand.

## <a name="appinsightsinstrumentationkey"></a>APPINSIGHTS_INSTRUMENTATIONKEY

De Application Insights-instrumentatiesleutel als u Application Insights. Zie [bewaken van Azure Functions](functions-monitoring.md).

|Sleutel|Voorbeeldwaarde|
|---|------------|
|APPINSIGHTS_INSTRUMENTATIONKEY|5dbdd5e9-af77-484b-9032-64f83bb83bb|

## <a name="azurewebjobsdashboard"></a>AzureWebJobsDashboard

Optioneel storage-account-verbindingsreeks voor het opslaan van Logboeken en weergeven in de **Monitor** tabblad in de portal. Het opslagaccount moet een die ondersteuning biedt voor blobs, wachtrijen en tabellen voor algemene doeleinden. Zie [opslagaccount](functions-infrastructure-as-code.md#storage-account) en [account opslagvereisten](functions-create-function-app-portal.md#storage-account-requirements).

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsDashboard|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [sleutel]|

## <a name="azurewebjobsdisablehomepage"></a>AzureWebJobsDisableHomepage

`true`betekent dat de standaard landingspagina die wordt weergegeven voor de basis-URL van een functie-app niet uitschakelen. Standaard is `false`.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsDisableHomepage|De waarde True|

Wanneer deze appinstelling is weggelaten of ingesteld op `false`, een pagina zoals in het volgende voorbeeld wordt weergegeven in het antwoord naar de URL `<functionappname>.azurewebsites.net`.

![De functie app-startpagina](media/functions-app-settings/function-app-landing-page.png)

## <a name="azurewebjobsdotnetreleasecompilation"></a>AzureWebJobsDotNetReleaseCompilation

`true`betekent releasemodus gebruiken bij het compileren van .NET-code; `false` betekent foutopsporingsmodus gebruiken. Standaard is `true`.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsDotNetReleaseCompilation|De waarde True|

## <a name="azurewebjobsfeatureflags"></a>AzureWebJobsFeatureFlags

Een door komma's gescheiden lijst met functies beta om in te schakelen. Beta-functies ingeschakeld door deze vlaggen zijn niet gereed productie, maar kunnen worden ingeschakeld voor experimenteel gebruik voordat u ze.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsFeatureFlags|feature1, feature2|

## <a name="azurewebjobsscriptroot"></a>AzureWebJobsScriptRoot

Het pad naar de hoofdmap waarin de *host.json* bestands- en -mappen zich bevinden. In een functie-app, de standaardwaarde is `%HOME%\site\wwwroot`.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsScriptRoot|%Home%\site\wwwroot|

## <a name="azurewebjobssecretstoragetype"></a>AzureWebJobsSecretStorageType

Hiermee geeft u de opslagplaats of de provider moet worden gebruikt voor opslag van sleutels. De ondersteunde opslagplaatsen zijn momenteel-blob ('Blob') en het bestandssysteem ("uitgeschakeld"). De standaardwaarde is bestandssysteem ("uitgeschakeld").

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsSecretStorageType|Uitgeschakeld|

## <a name="azurewebjobsstorage"></a>AzureWebJobsStorage

De Azure Functions-runtime gebruikt deze account verbindingsreeks voor opslag voor alle functies, met uitzondering van HTTP-geactiveerde functies. Het opslagaccount moet een die ondersteuning biedt voor blobs, wachtrijen en tabellen voor algemene doeleinden. Zie [opslagaccount](functions-infrastructure-as-code.md#storage-account) en [account opslagvereisten](functions-create-function-app-portal.md#storage-account-requirements).

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobsStorage|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [sleutel]|

## <a name="azurewebjobstypescriptpath"></a>AzureWebJobs_TypeScriptPath

Pad naar de compiler voor machineschrift gebruikt. Hiermee kunt u de standaardwaarde als u wilt vervangen.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|AzureWebJobs_TypeScriptPath|%Home%\typescript|

## <a name="functionappeditmode"></a>DE FUNCTIE\_APP\_BEWERKEN\_MODUS

Geldige waarden zijn 'readwrite' en 'readonly'.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|DE FUNCTIE\_APP\_BEWERKEN\_MODUS|alleen-lezen|

## <a name="functionsextensionversion"></a>FUNCTIES\_EXTENSIE\_VERSIE

De versie van de Azure Functions-runtime moet worden gebruikt in deze functie-app. Een tilde met hoofdversie betekent dat de nieuwste versie van die primaire versie (bijvoorbeeld ' ~ 1) gebruiken. Wanneer nieuwe versies voor dezelfde hoofdversie beschikbaar zijn, worden ze automatisch geïnstalleerd in de functie-app. Om de app naar een specifieke versie, gebruikt u het nummer van de volledige versie (bijvoorbeeld ' 1.0.12345'). Standaard is '~ 1'.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|FUNCTIES\_EXTENSIE\_VERSIE|~1|

## <a name="websitecontentazurefileconnectionstring"></a>WEBSITE_CONTENTAZUREFILECONNECTIONSTRING

Voor verbruik plannen. De verbindingsreeks voor de opslagaccount waarin de functie app-code en de configuratie zijn opgeslagen. Zie [maken van een functie-app](functions-infrastructure-as-code.md#create-a-function-app).

|Sleutel|Voorbeeldwaarde|
|---|------------|
|WEBSITE_CONTENTAZUREFILECONNECTIONSTRING|DefaultEndpointsProtocol = https; AccountName = [name]; AccountKey = [sleutel]|

## <a name="websitecontentshare"></a>WEBSITE_CONTENTSHARE

Voor verbruik plannen. Het pad naar de functie app-code en configuratie. Met WEBSITE_CONTENTAZUREFILECONNECTIONSTRING gebruikt. Standaard is een unieke tekenreeks die met de naam van de functie-app begint. Zie [maken van een functie-app](functions-infrastructure-as-code.md#create-a-function-app).

|Sleutel|Voorbeeldwaarde|
|---|------------|
|WEBSITE_CONTENTSHARE|functionapp091999e2|

## <a name="websitemaxdynamicapplicationscaleout"></a>WEBSITE\_MAX\_DYNAMISCHE\_TOEPASSING\_SCALE\_UIT

Het maximum aantal exemplaren die de functie-app te kan uitbreiden. Standaard is geen limiet.

> [!NOTE]
> Deze instelling is voor een preview-functie.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|WEBSITE\_MAX\_DYNAMISCHE\_TOEPASSING\_SCALE\_UIT|10|

## <a name="websitenodedefaultversion"></a>WEBSITE\_KNOOPPUNT\_DEFAULT_VERSION

De standaardwaarde is '6.5.0'.

|Sleutel|Voorbeeldwaarde|
|---|------------|
|WEBSITE\_KNOOPPUNT\_DEFAULT_VERSION|6.5.0|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het bijwerken van app-instellingen](functions-how-to-use-azure-function-app-settings.md#manage-app-service-settings)

> [!div class="nextstepaction"]
> [Zie globale instellingen in het bestand host.json](functions-host-json.md)
