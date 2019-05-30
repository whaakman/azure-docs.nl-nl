---
title: Testen van toepassingen van Microsoft Authentication Library (MSAL) | Azure
description: Meer informatie over het testen van toepassingen van Microsoft Authentication Library (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/28/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3495ad5f691ac57b69ab5d3efcd5436cc66e9a6e
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307526"
---
# <a name="testing-msal-applications"></a>Testen van toepassingen met MSAL
Dit artikel bevat suggesties voor het testen van toepassingen van Microsoft Authentication Library (MSAL).

## <a name="unit-testing"></a>Moduletests uitvoeren
De MSAL-API's gebruiken de [builder patroon](https://wikipedia.org/wiki/Builder_pattern) intensief. Builders zijn moeilijke en tijdrovende aan het model. In plaats daarvan wordt aangeraden dat u alle verificatielogica achter een interface te verpakken en model die in uw app.

## <a name="end-to-end-testing"></a>End-to-end-testen
Voor het testen van end-to-end, kunt u testaccounts instellen, testen van toepassingen of zelfs afzonderlijke mappen. Gebruikersnaam en wachtwoorden kunnen worden geïmplementeerd via de pijplijn voor continue integratie (bijvoorbeeld geheim build variabelen in Azure DevOps). Een andere strategie bestaat uit referenties testen houden [Key Vault](/azure/key-vault/) en configureren van de machine waarop de tests voor toegang tot Key Vault), bijvoorbeeld door het installeren van een certificaat. Gebruik van MSAL gerust [strategie voor toegang tot Key Vault](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/master/tests/Microsoft.Identity.Test.LabInfrastructure/KeyVaultSecretsProvider.cs#L112).

Zodra het ophalen van tokens optreedt, worden zowel een toegangstoken en een vernieuwingstoken in cache opgeslagen. De eerste heeft een levensduur van een uur, de laatste van enkele maanden. Wanneer het toegangstoken is verlopen, wordt MSAL automatisch het vernieuwingstoken dat gebruikt om te verkrijgen van een nieuwe, zonder tussenkomst van de gebruiker. U kunt vertrouwen op dit gedrag voor het inrichten van uw tests uit.

Als u voorwaardelijke toegang geconfigureerd hebt, is automatiseren rond het is moeilijk. Het is gemakkelijker om een handmatige stap die deals met voorwaardelijke toegang (bijvoorbeeld, meervoudige verificatie), waardoor de tokens toevoegen aan de MSAL-cache en vervolgens afhankelijk zijn van token aankopen op de achtergrond, dat wil zeggen, afhankelijk van een eerder aangemelde gebruiker zijn.

## <a name="web-apps"></a>Web-apps
Seleen of een equivalente technologie gebruiken voor het automatiseren van de web-app. Ophalen van gebruikersnamen en het wachtwoord van een veilige locatie.

## <a name="daemon-apps"></a>Daemon-apps
Daemon-apps vooraf geïmplementeerde geheimen (wachtwoorden of certificaten) om te communiceren aan Azure Active Directory (AAD) gebruiken. U kunt het implementeren van een geheim aan uw testomgeving of het token in cache opslaan techniek gebruiken voor het inrichten van uw tests uit. De [client-referentietoekenning](msal-authentication-flows.md#client-credentials) niet ophalen van vernieuwingstokens, alleen toegang tot tokens die in één uur verlopen.

## <a name="native-client-apps"></a>Systeemeigen client-apps
Er zijn verschillende manieren om te testen voor systeemeigen clients:

* Gebruik de [gebruikersnaam en wachtwoord verlenen](msal-authentication-flows.md#usernamepassword) voor het ophalen van een token op een niet-interactieve wijze. Deze stroom wordt niet aanbevolen in de productieomgeving, maar kan men redelijkerwijs te gebruiken voor het testen.

* Een framework gebruikt, zoals Appium of Xamarin.Test, die voorziet in een automatiseringsinterface voor uw app en de MSAL browser gemaakt.

* MSAL wordt aangegeven dat een uitbreidingspunt waarmee ontwikkelaars kunnen hun eigen ervaring browser invoeren. De MSAL-team dit wordt intern gebruikt voor het testen van interactieve scenario's. Bekijk [deze .NET testproject](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/master/tests/Microsoft.Identity.Test.Integration.net45/SeleniumTests/InteractiveFlowTests.cs) voor informatie over het injecteren een [seleen aangestuurde browser](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/tree/master/tests/Microsoft.Identity.Test.Integration.net45/Infrastructure) kunt die verificatie afhandelt.

## <a name="xamarin-apps"></a>Xamarin-apps
Het team MSAL worden momenteel tests uitgevoerd op een Xamarin-app die gebruikmaakt van MSAL.NET; We gebruiken [App Center](https://appcenter.ms/apps) apparaten wilt beheren, test wordt uitgevoerd enzovoort. De test-Framework [Xamarin.UITest](/appcenter/test-cloud/uitest/). Een beperking die we hebben ontdekt dat is dat er kan geen system-browsers, test alleen ingesloten browsers.

Bij het evalueren van een testframework, is het waard is om ook eens naar de Appium en andere testframeworks, evenals andere CI/CD-providers die in de mobiele-ruimte.

## <a name="testing-the-token-cache"></a>De tokencache testen
Ongeacht welk platform dat u gebruikt, slaat MSAL tokens in de cache van een token. Op sommige platformen moet hoe u MSAL het serialiseren van deze cache. Op de mobiele platforms serialiseert MSAL de cache voor u. Vanuit het perspectief van een toepassing is de tokencache die verantwoordelijk is voor de drie dingen:

* tokens in de cache opslaan nadat ze zijn verkregen (bijvoorbeeld met behulp van de `AcquireTokenInteractive` methode)
* tokens ophalen uit de cache bij het aanroepen van de `AcquireTokenSilent` methode
* account metagegevens ophalen uit de cache bij het aanroepen van de `GetAccount` methode

Dus als u testen van scenario's voor cache wilt, kunt u een scenario dat zou schrijven:

* een of meer tokens verkrijgen (bijvoorbeeld met behulp van [gebruikersnaam en wachtwoord verlenen](msal-authentication-flows.md#usernamepassword) stroom, en voor het testen van de eenvoudigste)
* Controleer `GetAccounts` werkt
* Controleer `AcquireTokenSilent` werkt

U kunt ook testen of:

* de app opnieuw te starten, wordt de cache niet verwijderd
* `AcquireTokenSilent` het vernieuwingstoken dat niet wordt vernieuwd (dat wil zeggen, netwerk aanroep van AAD), maar het toegangstoken fungeert als deze niet is verlopen. U kunt deze en andere scenario's voor HTTP-gerelateerde bereiken door het nodig de controle van de HTTP-client.  Zie voor een .NET-voorbeeld [leveren van uw eigen httpclient maakt](msal-net-provide-httpclient.md)

## <a name="feedback"></a>Feedback
U kunt [problemen melden](developer-support-help-options.md#create-a-github-issue) of vragen stellen met betrekking tot het testen. Leveren van een goede test is een van de doelstellingen van het team.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het implementeren van [logboekregistratie](msal-logging.md) in uw toepassing MSAL.