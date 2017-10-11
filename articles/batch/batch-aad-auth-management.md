---
title: Azure Active Directory gebruiken voor verificatie van oplossingen voor het beheer van de Batch | Microsoft Docs
description: "Toepassingen die zijn gebouwd met Azure resourcemanager en de Batch-resourceprovider zich verifiëren met Azure AD."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 26d4adf4f74f9aacc4cf8cf24be293ebdb4d63c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Verificatie van oplossingen voor het beheer van de Batch met Active Directory

Toepassingen die de Azure Batch Management-service aanroepen verifiëren met [Azure Active Directory] [ aad_about] (Azure AD). Azure AD is Microsoft multitenant-cloudgebaseerde adreslijst en identity management-service. Azure zelf gebruikt Azure AD voor de verificatie van zijn klanten, servicebeheerders en organisatie-gebruikers.

De Batch Management .NET-bibliotheek wordt typen voor het werken met de Batch-accounts, toegangscodes, toepassingen en toepassingspakketten. De Batch Management .NET-bibliotheek is een Azure-resource provider-client en wordt gebruikt in combinatie met [Azure Resource Manager] [ resman_overview] voor het beheren van deze resources via een programma. Azure AD is vereist voor de verificatie via een Azure-resource provider-client, met inbegrip van de Batch Management .NET-bibliotheek en aanvragen [Azure Resource Manager][resman_overview].

In dit artikel verkennen we using Azure AD om te verifiëren van toepassingen die gebruikmaken van de Batch Management .NET-bibliotheek. Laten we zien hoe u Azure AD gebruikt om een abonnementsbeheerder of medebeheerder met geïntegreerde verificatie te verifiëren. We gebruiken de [AccountManagment] [ acct_mgmt_sample] voorbeeldproject, beschikbaar op GitHub te doorlopen gebruikmaken van Azure AD met de Batch Management .NET-bibliotheek.

Zie voor meer informatie over het gebruik van de Batch Management .NET-bibliotheek en de steekproef AccountManagement, [beheren Batch-accounts en quota's met de Batch-Management-clientbibliotheek voor .NET](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Uw toepassing registreren met Azure AD

De Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) biedt een programmatische interface naar Azure AD voor gebruik binnen uw toepassingen. Om aan te roepen ADAL van uw toepassing, moet u uw toepassing registreren in een Azure AD-tenant. Wanneer u uw toepassing registreert, kunt u Azure AD met informatie geven over uw toepassing, met inbegrip van een naam voor het in de Azure AD-tenant. Vervolgens Azure AD levert een toepassings-ID waarmee u uw toepassing koppelen aan Azure AD tijdens runtime. Zie voor meer informatie over de toepassings-ID, [toepassing en service-principal objecten in Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

Voor het registreren van de voorbeeldtoepassing AccountManagement, volg de stappen in de [een toepassing toe te voegen](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) in sectie [toepassingen integreren met Azure Active Directory][aad_integrate]. Geef **systeemeigen clienttoepassing** voor het type van de toepassing. De branche standaard OAuth 2.0-URI voor de **omleidings-URI** is `urn:ietf:wg:oauth:2.0:oob`. U kunt echter geldige URI opgeven (zoals `http://myaccountmanagementsample`) voor de **omleidings-URI**, zoals het hoeft niet te worden van een echte eindpunt:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Wanneer u het registratieproces hebt voltooid, ziet u de toepassings-ID en het object (service-principal)-ID voor uw toepassing wordt vermeld.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>De Azure Resource Manager-API toegang verlenen tot uw toepassing

Vervolgens moet u toegang tot uw toepassing om de Azure Resource Manager-API te delegeren. De Azure AD-id voor de Resource Manager-API is **Windows Azure Service Management API**.

Volg deze stappen in de Azure portal:

1. Kies in het navigatiedeelvenster links van de Azure portal, **meer Services**, klikt u op **App registraties**, en klik op **toevoegen**.
2. Zoeken naar de naam van uw toepassing in de lijst van app-rapporten:

    ![Zoeken naar de toepassingsnaam van uw](./media/batch-aad-auth-management/search-app-registration.png)

3. Weergave de **instellingen** blade. In de **API-toegang** sectie **vereist machtigingen**.
4. Klik op **toevoegen** toevoegen van een nieuwe vereiste machtiging. 
5. Typ in stap 1, **Windows Azure Service Management API**, selecteert u die API in de lijst met resultaten en klikt u op de **Selecteer** knop.
6. In stap 2, schakel het selectievakje in naast **klassieke implementatiemodel van Azure toegang als gebruikers organisatie**, en klik op de **Selecteer** knop.
7. Klik op de **gedaan** knop.

De **Required Permissions** blade wordt nu weergegeven dat de machtigingen voor uw toepassing worden toegekend aan de ADAL- en het resourcemanager-API's. Machtigingen worden verleend adal standaard, wanneer u uw app eerst bij Azure AD registreren.

![Machtigingen delegeren aan de API van Azure Resource Manager](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD-eindpunten

Verificatie van uw Batch-beheeroplossingen met Azure AD, moet u twee bekende eindpunten.

- De **algemene Azure AD-eindpunt** biedt een algemene referentie interface wanneer een specifieke tenant niet is opgegeven, zoals het geval van geïntegreerde verificatie te verzamelen:

    `https://login.microsoftonline.com/common`

- De **Azure Resource Manager-eindpunt** wordt gebruikt voor het verkrijgen van een token voor het verifiëren van aanvragen naar de service Batch management:

    `https://management.core.windows.net/`

De voorbeeldtoepassing AccountManagement definieert constanten voor deze eindpunten. Laat deze constanten ongewijzigd:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Verwijst naar de toepassings-ID 

De clienttoepassing gebruikt de toepassings-ID (ook wel de client-ID genoemd) voor toegang tot Azure AD tijdens runtime. Nadat u uw toepassing hebt geregistreerd in de Azure portal, worden uw code voor het gebruik van de toepassings-ID die is opgegeven door Azure AD voor de geregistreerde toepassing bijgewerkt. In de voorbeeldtoepassing AccountManagement kopieert u de toepassings-ID van de Azure-portal op de juiste constante:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
De omleidings-URI die u hebt opgegeven tijdens het registratieproces ook kopiëren. Omleidings-URI in uw code opgegeven de moet overeenkomen met de omleidings-URI die u hebt opgegeven bij de registratie van de toepassing.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Een Azure AD-verificatietoken ophalen

Nadat u het voorbeeld AccountManagement in de Azure AD-tenant registreren en de voorbeeldcode van de bron met de waarden bijwerken, is het voorbeeld gereed om te verifiëren met behulp van Azure AD. Wanneer u het voorbeeld uitvoert, probeert de ADAL geen verificatietoken ophalen. Bij deze stap wordt u gevraagd uw referenties Microsoft: 

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

Nadat u uw referenties opgeeft, wordt de voorbeeldtoepassing kunt doorgaan met het uitgeven van geverifieerde aanvragen naar de service Batch management. 

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over die wordt uitgevoerd de [AccountManagement voorbeeldtoepassing][acct_mgmt_sample], Zie [beheren Batch-accounts en quota's met de Batch-Management-clientbibliotheek voor .NET](batch-management-dotnet.md).

Zie voor meer informatie over Azure AD, de [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/). Gedetailleerde voorbeelden ziet u het gebruik van ADAL zijn beschikbaar in de [Azure codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotheek.

Als u wilt verifiëren Batch-service-toepassingen met behulp van Azure AD, Zie [oplossingen van de service Batch verifiëren met Active Directory](batch-aad-auth.md). 


[aad_about]: ../active-directory/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie-scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
