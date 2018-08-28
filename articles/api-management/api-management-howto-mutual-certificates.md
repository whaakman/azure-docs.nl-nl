---
title: 'Beveiligd back-endservices met behulp van client-certificaatverificatie: Azure API Management | Microsoft Docs'
description: Meer informatie over het beveiligen van back-endservices met behulp van verificatie van clientcertificaten in Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 2b0db6f9a1e9cd660ce1b1e7af3e7e1c85815c16
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045245"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Over het beveiligen van back-endservices met behulp van client-certificaatverificatie in Azure API Management

API Management kunnen voor het beveiligen van toegang tot de back-end-service van een API met behulp van clientcertificaten. Deze handleiding wordt beschreven hoe u voor het beheren van certificaten in de Azure API Management-service-exemplaar in de Azure-portal. Ook wordt uitgelegd hoe u een API voor het gebruik van een certificaat voor toegang tot een back-end-service configureren.

Zie voor meer informatie over het beheren van certificaten met behulp van de API Management REST API <a href="https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">Azure API Management REST API-Beheercertificaat entiteit</a>.

## <a name="prerequisites"> </a>Vereisten

Deze handleiding laat zien hoe u uw API Management service-exemplaar voor het gebruik van verificatie van clientcertificaten voor toegang tot de back-end-service voor een API configureren. Voordat u de stappen in dit artikel te volgen, moet u uw back-endservice geconfigureerd voor verificatie van clientcertificaten ([certificaatverificatie in Azure WebSites configureren Raadpleeg dit artikel] [ to configure certificate authentication in Azure WebSites refer to this article]). In dat geval moet u toegang tot het certificaat en het wachtwoord in voor het uploaden naar de API Management-service.

## <a name="step1"> </a>Een certificaat uploaden

![Clientcertificaten toevoegen](media/api-management-howto-mutual-certificates/apim-client-cert.png)

Volg de stappen hieronder om een nieuw certificaat te uploaden. Als u nog geen exemplaar van API Management-service hebt gemaakt, Zie de zelfstudie [API Management service-exemplaar maken][Create an API Management service instance].

1. Navigeer naar uw Azure API Management service-exemplaar in de Azure-portal.
2. Selecteer **clientcertificaten** in het menu.
3. Klik op de **+ toevoegen** knop.  
    ![Clientcertificaten toevoegen](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)  
4. Zoeken naar het certificaat, de ID en wachtwoord opgeven.  
5. Klik op **Create**.

> [!NOTE]
> Het certificaat moet zich in **pfx** indeling. Zelfondertekende certificaten zijn toegestaan.

Zodra het certificaat is geüpload, wordt in de **clientcertificaten**.  Hebt u veel certificaten, noteer de vingerafdruk van het gewenste certificaat om [een API voor het gebruik van een clientcertificaat voor gatewayverificatie configureren][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Als u wilt uitschakelen validatie van certificaatketen wanneer u bijvoorbeeld een zelf-ondertekend certificaat, volgt u de stappen in deze Veelgestelde vragen over [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Verwijderen van een clientcertificaat

Als u wilt een certificaat wilt verwijderen, klikt u op in het contextmenu **...**  en selecteer **verwijderen** naast het certificaat.

![Verwijderen van clientcertificaten](media/api-management-howto-mutual-certificates/apim-client-cert-delete.png)

Als het certificaat gebruikt door een API, wordt en vervolgens een waarschuwing wordt weergegeven. Als u wilt verwijderen van het certificaat, moet u eerst het certificaat verwijderen van API's die zijn geconfigureerd om deze te gebruiken.

![Verwijderen van de client-certificaten is mislukt](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Een API voor het gebruik van een clientcertificaat voor gatewayverificatie configureren

1. Klik op **API's** uit de **API Management** menu aan de linkerkant en navigeer naar de API.  
    ![Clientcertificaten inschakelen](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. In de **ontwerp** tabblad, klikt u op een potloodpictogram van de **back-end** sectie. 
3. Wijziging de **gatewayreferenties** naar **clientcertificaat** en selecteert u het certificaat in de vervolgkeuzelijst.  
    ![Clientcertificaten inschakelen](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. Klik op **Opslaan**. 

> [!WARNING]
> Deze wijziging wordt onmiddellijk van kracht en aanroepen van bewerkingen van die API het certificaat wordt gebruikt om te verifiëren op de back-endserver.


> [!TIP]
> Wanneer een certificaat is opgegeven voor de gatewayverificatie voor de back-end-service van een API, maakt deel uit van het beleid voor die API, en kunnen worden weergegeven in de beleidseditor.

## <a name="self-signed-certificates"></a>Zelfondertekende certificaten

Als u zelfondertekende certificaten gebruikt, moet u in de volgorde voor API Management om te communiceren met het systeem back-end-validatie van certificaatketen uitschakelen. Anders wordt een 500 foutcode geretourneerd. Als u wilt dit configureren, kunt u de [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (voor de nieuwe back-end) of [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (voor bestaande back-end) PowerShell-cmdlets en stel de `-SkipCertificateChainValidation` parameter `True`.

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
