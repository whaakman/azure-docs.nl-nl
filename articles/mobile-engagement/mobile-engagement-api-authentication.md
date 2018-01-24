---
title: "Verifiëren met Mobile Engagement REST-API 's"
description: Hierin wordt beschreven hoe u verificatie met Azure Mobile Engagement REST API 's
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: da82cb36-957a-4e19-a805-b44733cf6597
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: 574e699a1cfca2caef0cf20872570bbb8650117b
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Verifiëren met Mobile Engagement REST-API 's

## <a name="overview"></a>Overzicht

Dit document wordt beschreven hoe een geldige Azure Active Directory (Azure AD) OAuth-token te verifiëren met de Mobile Engagement REST-API's ophalen.

Deze procedure wordt ervan uitgegaan dat u een geldige Azure-abonnement hebt en een Mobile Engagement-app hebt gemaakt met een van de [developer zelfstudies](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Verificatie

Er is een Microsoft Azure Active Directory gebaseerde OAuth-token gebruikt voor verificatie. 

Verificatie van een API-aanvraag moet een autorisatie-header worden toegevoegd aan elke aanvraag. De autorisatie-header is in de volgende indeling:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory-tokens verlopen in één uur.
> 
> 

Er zijn verschillende manieren om een token. Omdat de API's worden aangeroepen vanuit een cloudservice, die u wilt gebruiken een API-sleutel. Een API-sleutel in de Azure-terminologie heet een Service-Principal-wachtwoord. De volgende procedure beschrijft een manier om deze functie handmatig instellen.

### <a name="one-time-setup-using-a-script"></a>Eenmalige instelling (met behulp van een script)

Als u wilt de installatie met behulp van een PowerShell-script uitvoeren, moet u de stappen uitvoeren in de volgende instructies. Een PowerShell-script de minimale hoeveelheid tijd voor de installatie vereist, maar gebruikt de maximaal toegestane standaardinstellingen. 

Eventueel, u kunt ook de instructies in de [handmatige installatie](mobile-engagement-api-authentication-manual.md) voor het rechtstreeks uitvoeren vanuit de Azure-portal. Bij het instellen van vanuit de Azure-portal, kunt u een meer gedetailleerde configuratie kunt doen.

1. De nieuwste versie van Azure PowerShell door [downloaden](http://aka.ms/webpi-azps). Zie voor meer informatie over de downloadinstructies [dit overzicht](/powershell/azure/overview).

2. Nadat u PowerShell hebt geïnstalleerd, kunt u de volgende opdrachten om ervoor te zorgen dat u hebt de **Azure-module** geïnstalleerd:

    a. Zorg ervoor dat de Azure PowerShell-module is beschikbaar in de lijst met beschikbare modules.

        Get-Module –ListAvailable

    ![Beschikbare Azure modules][1]

    b. Als u de Azure PowerShell-module niet in de vorige lijst vinden, moet u om uit te voeren:

        Import-Module Azure
3. Meld u in Azure Resource Manager vanuit PowerShell door de volgende opdracht uit te voeren. Geef de gebruikersnaam en het wachtwoord voor uw Azure-account: 

        Login-AzureRmAccount
4. Als u meerdere abonnementen hebt, kunt u de volgende stappen uitvoeren:

    a. Een lijst van al uw abonnementen ophalen. Kopieer de **SubscriptionId** van het abonnement dat u wilt gebruiken. Zorg ervoor dat dit abonnement heeft de Mobile Engagement-app. U gaat deze app gebruiken om te communiceren met de API's. 

        Get-AzureRmSubscription

    b. Voer de volgende opdracht. Geef de **SubscriptionId** voor het configureren van het abonnement dat u wilt gebruiken:

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Kopieer de tekst voor de [nieuw AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) script op uw lokale computer. Vervolgens opslaan als een PowerShell-cmdlet (bijvoorbeeld `APIAuth.ps1`), en voer deze uit.

         `.\APIAuth.ps1`.

6. Het script vraagt u een invoer voor **primaire naam**. Geef een geschikte naam die u wilt gebruiken voor uw Active Directory-toepassing (bijvoorbeeld APIAuth). 

7. Nadat het script is voltooid, wordt de volgende vier waarden weergegeven. Vergeet te kopiëren, omdat u deze voor verificatie via een programma met de Active Directory nodig: 

   - **TenantId**
   - **Abonnements-id**
   - **ApplicationId**
   - **Geheim**

   Gebruik van TenantId als `{TENANT_ID}`, ApplicationId als `{CLIENT_ID}` en de geheime als `{CLIENT_SECRET}`.

   > [!NOTE]
   > Het beveiligingsbeleid van uw standaard blokkeren u mogelijk PowerShell-scripts worden uitgevoerd. Als dit het geval is, gebruik de volgende opdracht tijdelijk uw om uitvoeringsbeleid te configureren zodat de uitvoering van script:
   > 
   > Set-ExecutionPolicy RemoteSigned
8. Hier volgt de weergave van de reeks PowerShell-cmdlets.
    ![PowerShell-cmdlets][3]
9. Ga in de Azure portal naar Active Directory, selecteer **App registraties**, en vervolgens zoeken naar uw app om te controleren of deze bestaat.
    ![Zoeken naar uw app][4]

### <a name="steps-to-get-a-valid-token"></a>Stappen voor het ophalen van een geldig token

1. Roept de API met de volgende parameters. Zorg ervoor dat u **TENANT\_ID**, **CLIENT\_ID**, en **CLIENT\_GEHEIM**:
   
   * **Aanvraag-URL** als`https://login.microsoftonline.com/{TENANT_ID}/oauth2/token`

   * **HTTP Content-Type-header** als`application/x-www-form-urlencoded`
   
   * **HTTP-aanvraag hoofdtekst** als`grant_type=client\_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&resource=https%3A%2F%2Fmanagement.core.windows.net%2F`
     
    Hier volgt een voorbeeld van aanvraag:
    ```
    POST /{TENANT_ID}/oauth2/token HTTP/1.1
    Host: login.microsoftonline.com
    Content-Type: application/x-www-form-urlencoded
    grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
    urce=https%3A%2F%2Fmanagement.core.windows.net%2F
    ```
    Hier volgt een voorbeeld van antwoord:
    ```
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Content-Length: 1234

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
    5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}
    ```
     Dit voorbeeld bevat de URL-codering van de parameters POST waarin `resource` waarde is `https://management.core.windows.net/`. Zorg ervoor dat ook URL coderen `{CLIENT_SECRET}`, omdat deze mogelijk speciale tekens bevat.

     > [!NOTE]
     > Voor het testen, kunt u een HTTP-clienthulpprogramma, zoals [Fiddler](http://www.telerik.com/fiddler) of [Chrome Postman extensie](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop). 
     > 
     > 
2. Nu in elke API-aanroep zijn de autorisatie-header voor aanvraag:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Als uw aanvraag een 401 statuscode retourneert, controleert u de antwoordtekst. Dit kan erop wijzen dat het token is verlopen. In dat geval een nieuw token ophalen.

## <a name="use-the-apis"></a>Gebruik de API 's
Nu dat u een ongeldig token hebt, bent u klaar om te maken van de API-aanroepen.

1. In elke API-aanvraag moet u een geldige, niet-verlopen token doorgeven. U hebt de lopende token in de vorige sectie hebt verkregen.

2. Sluit een aantal parameters voor de aanvraag-URI die uw toepassing te identificeren. De aanvraag URI ziet eruit als de volgende code:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Als u de parameters, selecteer de toepassingsnaam van uw. Selecteer vervolgens **Dashboard**. U ziet een pagina met alle drie parameters, als volgt:
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** uw Resourcegroepnaam is het verstandig om **MobileEngagement** tenzij u een nieuw gemaakt. 

> [!NOTE]
> Het adres van de basis-API negeren omdat deze voor de vorige API's.
> 
> Als u de app met behulp van de Azure-portal hebt gemaakt, moet u de naam van de Resource van de toepassing, dat van de naam van de App afwijkt gebruiken. Als u de app in de Azure-portal hebt gemaakt, moet u de App-naam gebruiken. (Er is geen onderscheid tussen de bron-naam van de toepassing en de naam van de App voor apps die zijn gemaakt in de nieuwe portal.)
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/search-app.png



