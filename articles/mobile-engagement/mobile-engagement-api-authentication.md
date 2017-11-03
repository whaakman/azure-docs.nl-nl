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
ms.openlocfilehash: b05181d9252c0a804648e01b4058019278ae5abe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Verifiëren met Mobile Engagement REST-API 's
## <a name="overview"></a>Overzicht
Dit document wordt beschreven hoe een geldige AAD-Oauth-token te verifiëren met de Mobile Engagement REST-API's ophalen. 

Er wordt van uitgegaan dat u een geldige Azure-abonnement hebt en u hebt gemaakt dat een Mobile Engagement-app met een van onze [Developer zelfstudies](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Authentication
OAuth-token wordt gebruikt voor verificatie op basis van een Microsoft Azure Active Directory. 

In volgorde van een API-verificatieaanvraag moet autorisatie-header worden toegevoegd aan elke aanvraag van de volgende notatie:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

> [!NOTE]
> Azure Active Directory-tokens verloopt over 1 uur.
> 
> 

Er zijn verschillende manieren om een token. Omdat de API's doorgaans vanuit een cloudservice aangeroepen worden, die u wilt gebruiken een API-sleutel. Een API-sleutel in de Azure-terminologie heet een Service-principal wachtwoord. De volgende procedure beschrijft een manier om het handmatig instellen.

### <a name="one-time-setup-using-script"></a>Eenmalige instelling (met behulp van script)
Volg de onderstaande instructies om uit te voeren van de installatie met behulp van een PowerShell-script dat nodig is de minimale tijd voor de installatie, maar gebruikt de standaardinstellingen van de maximaal toegestane set. Eventueel, u kunt ook de instructies in de [handmatige installatie](mobile-engagement-api-authentication-manual.md) om dit te doen in de Azure portal rechtstreeks en komen hoe fijner configuratie. 

1. De nieuwste versie van Azure PowerShell van [hier](http://aka.ms/webpi-azps). Voor meer informatie over de downloadinstructies, u kunt dit zien [koppeling](/powershell/azure/overview).  
2. Wanneer Azure PowerShell is geïnstalleerd, kunt u de volgende opdrachten om ervoor te zorgen dat u hebt de **Azure-module** geïnstalleerd:
   
    a. Zorg ervoor dat de Azure PowerShell-module is beschikbaar in de lijst met beschikbare modules. 
   
        Get-Module –ListAvailable 
   
    ![Beschikbare Azure Modules][1]
   
    b. Als u niet de Azure PowerShell-module kunt in de bovenstaande lijst vinden moet u het volgende uitvoeren:
   
        Import-Module Azure 
3. Aanmelding in de Azure Resource Manager vanuit PowerShell door de volgende opdracht uit te voeren en het leveren van uw gebruikersnaam en wachtwoord voor uw Azure-account: 
   
        Login-AzureRmAccount
4. Als u meerdere abonnementen hebt en vervolgens moet u het volgende uitvoeren:
   
    a. Een lijst met al uw abonnementen ophalen en kopieer de abonnements-id van het abonnement dat u wilt gebruiken. Zorg ervoor dat dit abonnement is hetzelfde account dat met de Mobile Engagement-App die u communiceren gaat met behulp van de API's. 
   
        Get-AzureRmSubscription
   
    b. Voer de volgende opdracht geven de abonnements-id voor het configureren van het abonnement moet worden gebruikt.
   
        Select-AzureRmSubscription –SubscriptionId <subscriptionId>
5. Kopieer de tekst voor de [nieuw AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) het script op uw lokale computer en deze opslaan als een PowerShell-cmdlet (bijvoorbeeld `APIAuth.ps1`) en voer het `.\APIAuth.ps1`. 
6. Het script wordt u gevraagd te bieden voor invoer **primaire naam**. Een geschikte naam hier opgeven dat u gebruiken wilt voor het maken van uw Active Directory-toepassing (bijvoorbeeld APIAuth). 
7. Nadat het script is voltooid, verschijnt de volgende vier waarden die u moet voor verificatie via een programma met AD dus zorg ervoor dat u deze kopiëren. 
   
    **TenantId**, **SubscriptionId**, **ApplicationId**, en **geheim**.
   
    U gebruikt TenantId als `{TENANT_ID}`, ApplicationId als `{CLIENT_ID}` en de geheime als `{CLIENT_SECRET}`.
   
   > [!NOTE]
   > Het beveiligingsbeleid van uw standaard blokkeren u een PowerShell-scripts worden uitgevoerd. Zo ja, configureren u tijdelijk de uitvoeringsbeleid zodat de uitvoering van het script met de volgende opdracht:
   > 
   > Set-ExecutionPolicy RemoteSigned
   > 
   > 
8. Hier ziet u hoe de set cmdlets PS eruit zou zien. 
   
    ![][3]
9. Controleer in de Azure Management portal of een nieuw AD-toepassing is gemaakt met de naam die u hebt opgegeven op het script met de naam **primaire naam** onder **weergeven toepassingen mijn bedrijf eigenaar is van**.
   
    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Stappen voor het ophalen van een geldig token
1. Aanroepen van de API met de volgende parameters en vervang de TENANT\_-ID, CLIENT\_-ID en CLIENT\_GEHEIM:
   
   * **Aanvraag-URL** als *https://login.microsoftonline.com/ {TENANT\_ID} / oauth2/token*
   * **HTTP Content-Type-header** als *toepassing/x-1-800-www-Dell-form-urlencoded*
   * **HTTP-aanvraagtekst** als *verlenen\_type = client\_referenties & client_id = {CLIENT\_ID} & client_secret = {CLIENT\_GEHEIM} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*
     
     Hier volgt een voorbeeld van aanvraag:
     
       POST / {TENANT_ID} / oauth2/token HTTP/1.1-Host: login.microsoftonline.com Content-Type: application/x-1-800-www-Dell-form-urlencoded grant_type = client_credentials & client_id = {CLIENT_ID} & client_secret = {CLIENT_SECRET} & reso urce=https%3A%2F%2Fmanagement.core.windows.net%2F
     
     Hier volgt een voorbeeld van antwoord:
     
       HTTP/1.1 200 OK Content-Type: application/json; charset = utf-8-Content-Length: 1234
     
       {{'token_type': 'Bearer', 'expires_in': '3599', 'expires_on': '1445395811', 'not_before': ' 144 5391911 "," resource ': 'https://management.core.windows.net/', "access_token": {ACCESS_TOKEN}}
     
     In dit voorbeeld opgenomen URL-codering van de parameters POST `resource` waarde is `https://management.core.windows.net/`. Zorg ervoor dat ook URL coderen `{CLIENT_SECRET}` als deze speciale tekens mag bevatten.
     
     > [!NOTE]
     > Voor het testen, kunt u een HTTP-clienthulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) of [Postman Chrome-uitbreiding](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 
     > 
     > 
2. Nu in elke API-aanroep zijn de autorisatie-header voor aanvraag:
   
        Authorization: Bearer {ACCESS_TOKEN}
   
    Als u een 401 statuscode geretourneerd krijgt, controleert u de antwoordtekst dit kan erop wijzen dat het token is verlopen. In dat geval een nieuw token ophalen.

## <a name="using-the-apis"></a>Met de API 's
Nu dat u een ongeldig token hebt, bent u klaar om te maken van de API-aanroepen.

1. In elke API-aanvraag moet u een geldige, niet-verlopen token dat u hebt verkregen in de vorige sectie doorgeven.
2. U moet plug-in sommige parameters in de aanvraag-URI die uw toepassing te identificeren. De aanvraag-URI lijkt op het volgende
   
        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/
   
    Om de parameters, klikt u op de toepassingsnaam van uw en klikt u op het Dashboard en u ziet een pagina zoals het volgende bij de 3 parameters zijn.
   
   * **1** `{subscription-id}`
   * **2** `{app-collection}`
   * **3** `{app-resource-name}`
   * **4** uw Resourcegroepnaam is het verstandig om **MobileEngagement** tenzij u een nieuw gemaakt. 
     
     ![Mobile Engagement API URI parameters][2]

> [!NOTE]
> <br/>
> 
> 1. Het adres van de API-hoofdmap negeren omdat dit voor de vorige API's is.<br/>
> 2. Als u de app via de klassieke Azure-portal gemaakt moet u de naam van de Resource van de toepassing dat anders dan de naam van de toepassing is gebruiken. Als u de app hebt gemaakt in de Azure Portal moet u de naam van de App zelf (Er is geen onderscheid tussen resourcenaam toepassing en de naam van de App voor apps die zijn gemaakt in de nieuwe portal) gebruiken.  
> 
> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



