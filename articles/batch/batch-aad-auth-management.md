---
title: Azure Active Directory gebruiken voor verificatie van oplossingen voor Batch | Microsoft Docs
description: Toepassingen die zijn gebouwd met Azure resourcemanager en de Batch-resourceprovider verifiëren met Azure AD.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: lahugh
ms.openlocfilehash: 22cab5f22eccabf9176d777b1e3a3356cbf37c4f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57534235"
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Verifiëren van Batch Management-oplossingen met Active Directory

Toepassingen die aanroepen van de Azure Batch Management-service verifiëren met [Azure Active Directory] [ aad_about] (Azure AD). Azure AD is van Microsoft met meerdere tenants cloudgebaseerde directory en identity management-service. Azure zelf maakt gebruik van Azure AD voor de verificatie van de klanten, servicebeheerders en organisatie-gebruikers.

De Batch Management .NET-bibliotheek wordt aangegeven dat typen voor het werken met de Batch-accounts, sleutels, toepassingen en toepassingspakketten. De Batch Management .NET-bibliotheek is een Azure-resource provider-client en wordt gebruikt in combinatie met [Azure Resource Manager] [ resman_overview] deze resources op programmatische wijze beheren. Azure AD is vereist voor het verifiëren van aanvragen via een Azure-resource provider client, met inbegrip van de Batch Management .NET-bibliotheek en [Azure Resource Manager][resman_overview].

In dit artikel wordt toegelicht met behulp van Azure AD om te verifiëren van toepassingen die gebruikmaken van de Batch Management .NET-bibliotheek. Laten we zien hoe u Azure AD gebruiken voor het verifiëren van een abonnementsbeheerder of CO-beheerder, via geïntegreerde verificatie. We gebruiken de [AccountManagement] [ acct_mgmt_sample] voorbeeldproject, beschikbaar op GitHub, om te zien hoe u Azure AD met de Batch Management .NET-bibliotheek.

Zie voor meer informatie over het gebruik van de Batch Management .NET-bibliotheek en het voorbeeld AccountManagement [beheren van Batch-accounts en -quota met de Batch Management-clientbibliotheek voor .NET](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Uw toepassing registreren bij Azure AD

De Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) biedt een programmatische interface naar Azure AD voor gebruik in uw toepassingen. Om aan te roepen ADAL van uw toepassing, moet u uw toepassing registreren in een Azure AD-tenant. Als u uw toepassing registreert, kunt u Azure AD met informatie opgeven over uw toepassing, met inbegrip van een naam voor het binnen de Azure AD-tenant. Vervolgens Azure AD biedt een toepassings-ID waarmee u uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de toepassings-ID, [toepassing en service-principalobjecten in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md).

Voor het registreren van de voorbeeldtoepassing AccountManagement, volg de stappen in de [toevoegen van een toepassing](../active-directory/develop/quickstart-v1-add-azure-ad-app.md) in sectie [toepassingen integreren met Azure Active Directory] [ aad_integrate]. Geef **systeemeigen clienttoepassing** voor het type toepassing. De branche standaard OAuth 2.0-URI voor de **omleidings-URI** is `urn:ietf:wg:oauth:2.0:oob`. U kunt echter geldige URI opgeven (zoals `http://myaccountmanagementsample`) voor de **omleidings-URI**, zoals het hoeft niet te worden geen echt eindpunt:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Nadat u het registratieproces is voltooid, ziet u de toepassings-ID en het object (service-principal)-ID die wordt vermeld voor uw toepassing.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>Geef het Azure Resource Manager API-toegang tot uw toepassing

Vervolgens moet u toegang tot uw toepassing naar de Azure Resource Manager API delegeren. De Azure AD-id voor de Resource Manager-API is **Windows Azure Service Management API**.

Volg deze stappen in de Azure-portal:

1. Kies in het linker navigatiedeelvenster van de Azure-portal **alle services**, klikt u op **App-registraties**, en klikt u op **toevoegen**.
2. Zoeken naar de naam van uw toepassing in de lijst van app-registraties:

    ![Zoek de toepassingsnaam van uw](./media/batch-aad-auth-management/search-app-registration.png)

3. Weergave de **instellingen** blade. In de **API-toegang** sectie, selecteer **vereiste machtigingen**.
4. Klik op **toevoegen** om toe te voegen een nieuwe vereiste machtiging. 
5. Typ in stap 1, **Windows Azure Service Management API**, selecteert u die API in de lijst met resultaten en klikt u op de **selecteren** knop.
6. In stap 2, schakel het selectievakje in naast **toegang tot Azure klassieke implementatiemodel als organisatiegebruikers**, en klik op de **Selecteer** knop.
7. Klik op de **gedaan** knop.

De **vereiste machtigingen** blade nu ziet u dat de machtigingen voor uw toepassing zijn verleend aan de ADAL- en resourcemanager API's. Machtigingen worden toegekend aan ADAL standaard wanneer u uw app eerst bij Azure AD registreren.

![Het overdragen van machtigingen voor de Azure Resource Manager-API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD-eindpunten

Als u wilt verifiëren van uw Batch-Management-oplossingen met Azure AD, moet u twee bekende eindpunten.

- De **algemene Azure AD-eindpunt** biedt een algemene referentie interface wanneer een specifieke tenant niet is opgegeven, zoals het in het geval van geïntegreerde verificatie te verzamelen:

    `https://login.microsoftonline.com/common`

- De **Azure Resource Manager-eindpunt** wordt gebruikt om een token voor het verifiëren van aanvragen voor de Batch management-service te verkrijgen:

    `https://management.core.windows.net/`

De voorbeeldtoepassing AccountManagement definieert constanten voor deze eindpunten. Laat deze constanten die niet worden gewijzigd:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Verwijzen naar uw toepassings-ID 

Uw clienttoepassing maakt gebruik van de toepassings-ID (ook wel de client-ID genoemd) voor toegang tot Azure AD tijdens runtime. Nadat u uw toepassing hebt geregistreerd in Azure portal, worden uw code voor het gebruik van de toepassings-ID die is geleverd door Azure AD voor uw geregistreerde toepassing bijgewerkt. In de voorbeeldtoepassing AccountManagement kopieert u de ID van uw toepassing vanuit Azure portal op de juiste constante:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Kopieer ook de omleidings-URI die u hebt opgegeven tijdens het registratieproces. De URI die is opgegeven in uw code omleiding moet overeenkomen met de omleidings-URI die u hebt opgegeven bij de registratie van de toepassing.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Een Azure AD-verificatietoken ophalen

Nadat u het voorbeeld AccountManagement in de Azure AD-tenant registreren en de voorbeeldcode van bron met de waarden bijwerken, is het voorbeeld klaar om te verifiëren met behulp van Azure AD. Wanneer u het voorbeeld uitvoert, wordt de ADAL probeert om een verificatietoken te verkrijgen. Bij deze stap wordt u gevraagd om uw Microsoft-referenties: 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Nadat u uw referenties hebt opgegeven, wordt de voorbeeld-App kunt doorgaan met het verzenden van geverifieerde aanvragen naar de Batch management-service. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over die wordt uitgevoerd de [AccountManagement voorbeeldtoepassing][acct_mgmt_sample], Zie [beheren van Batch-accounts en -quota met de Batch Management-clientbibliotheek voor .NET](batch-management-dotnet.md) .

Zie voor meer informatie over Azure Active Directory, de [documentatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Gedetailleerde voorbeelden die laten zien hoe u het gebruik van ADAL zijn beschikbaar in de [Azure-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotheek.

Als u wilt verifiëren Batch-service-toepassingen met behulp van Azure AD, Zie [verifiëren Batch-service-oplossingen met Active Directory](batch-aad-auth.md). 


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Verificatiescenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: https://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
