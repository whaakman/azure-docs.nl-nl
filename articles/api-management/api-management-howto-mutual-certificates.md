---
title: Beveiligen van back-end-services met behulp van client certificaatverificatie - Azure API Management | Microsoft Docs
description: Informatie over het beveiligen van back-end-services met behulp van verificatie van clientcertificaten in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 844a7ea1c2dd8f7dbb4984fc148575529ac154db
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292854"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Het beveiligen van back-end-services met behulp van client certificaatverificatie in Azure API Management

API Management kunnen toegang tot de back-end-service van een API beveiligen met behulp van clientcertificaten. Deze handleiding laat zien hoe voor het beheren van certificaten in het Azure API Management-service-exemplaar in de Azure portal. Ook wordt uitgelegd hoe u een API voor het gebruik van een certificaat voor toegang tot een back-end-service configureert.

Zie voor meer informatie over het beheren van certificaten met behulp van API Management REST API <a href="https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API certificaat entiteit</a>.

## <a name="prerequisites"> </a>Vereisten

Deze handleiding laat zien hoe uw API Management-service-exemplaar voor het gebruik van verificatie van clientcertificaten voor toegang tot de back-end-service voor een API te configureren. Voordat u de stappen in dit artikel uitvoert, moet u uw back-end-service die is geconfigureerd voor verificatie van clientcertificaten hebben ([certificaatverificatie in Azure WebSites configureren Raadpleeg dit artikel] [ to configure certificate authentication in Azure WebSites refer to this article]). In dat geval moet u toegang tot het certificaat en het wachtwoord in voor het uploaden naar de API Management-service.

## <a name="step1"> </a>Een certificaat uploaden

![Toevoegen van clientcertificaten](media/api-management-howto-mutual-certificates/apim-client-cert.png)

Volg onderstaande stappen voor het uploaden van een nieuw certificaat. Als u nog geen exemplaar van API Management-service hebt gemaakt, Zie de zelfstudie [API Management service-exemplaar maken][Create an API Management service instance].

1. Navigeer naar uw Azure API Management-service-exemplaar in de Azure portal.
2. Selecteer **clientcertificaten** in het menu.
3. Klik op de **+ toevoegen** knop.  
    ![Toevoegen van clientcertificaten](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)  
4. Zoeken naar het certificaat, geven de-ID en wachtwoord.  
5. Klik op **Create**.

> [!NOTE]
> Het certificaat moet **.pfx** indeling. Zelfondertekende certificaten zijn toegestaan.

Zodra het certificaat is geüpload, wordt in de **clientcertificaten**.  Als er veel certificaten, noteer de vingerafdruk van het gewenste certificaat om [configureren van een API voor het gebruik van een clientcertificaat voor gatewayverificatie][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Als u wilt uitschakelen validatie van certificaatketen wanneer u bijvoorbeeld een zelfondertekend certificaat, volg de stappen in deze Veelgestelde vragen [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Een clientcertificaat verwijderen

Als u wilt een certificaat wilt verwijderen, klikt u op contextmenu **...**  en selecteer **verwijderen** naast het certificaat.

![Verwijderen van clientcertificaten](media/api-management-howto-mutual-certificates/apim-client-cert-delete.png)

Als het certificaat gebruikt door een API wordt en vervolgens een waarschuwing wordt weergegeven. Als u wilt verwijderen van het certificaat, moet u eerst het certificaat verwijderen van een API's die zijn geconfigureerd voor gebruik.

![Verwijderen van de client certificaten is mislukt](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Een API voor het gebruik van een certificaat voor gatewayverificatie configureren

1. Klik op **API's** van de **API Management** menu aan de linkerkant en navigeer naar de API.  
    ![Clientcertificaten inschakelen](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. In de **ontwerp** tabblad, klikt u op een potloodpictogram van de **back-end** sectie. 
3. Wijzig de **gatewayreferenties** naar **Client cert** en uw certificaat selecteert in de vervolgkeuzelijst.  
    ![Clientcertificaten inschakelen](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. Klik op **Opslaan**. 

> [!WARNING]
> Deze wijziging wordt onmiddellijk van kracht en aanroepen naar de bewerkingen van deze API gebruikt het certificaat te verifiëren op de back-endserver.


> [!TIP]
> Wanneer een certificaat voor gatewayverificatie voor de back-end-service van een API is opgegeven, wordt een onderdeel van het beleid voor die API en kunnen worden weergegeven in de beleidseditor.

## <a name="self-signed-certificates"></a>Zelfondertekende certificaten

Als u zelfondertekende certificaten gebruikt, moet u API Management om te communiceren met het back endsysteem om validatie van certificaatketen uitschakelen. Anders wordt een 500 foutcode geretourneerd. Om dit te configureren, kunt u de [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (voor de nieuwe back-end) of [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (voor bestaande back-end) PowerShell-cmdlets en stel de `-SkipCertificateChainValidation` -parameter voor `True`.

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps
