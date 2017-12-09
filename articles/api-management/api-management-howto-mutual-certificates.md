---
title: Beveiligen van back-end-services met behulp van client certificaatverificatie - Azure API Management | Microsoft Docs
description: Informatie over het beveiligen van back-end-services met behulp van verificatie van clientcertificaten in Azure API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 885315b9f610d5f1703acd0f292f7b3347462b34
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Het beveiligen van back-end-services met behulp van client certificaatverificatie in Azure API Management
API Management biedt de mogelijkheid voor het beveiligen van toegang tot de back-end-service van een API met behulp van clientcertificaten. Deze handleiding wordt beschreven hoe u voor het beheren van certificaten in de publicatieportal van API en het configureren van een API voor het gebruik van een certificaat voor toegang tot de back-end-service.

Zie voor meer informatie over het beheren van certificaten met behulp van API Management REST API [Azure API Management REST API certificaat entiteit][Azure API Management REST API Certificate entity].

## <a name="prerequisites"></a>Vereisten
Deze handleiding laat zien hoe uw API Management-service-exemplaar voor het gebruik van verificatie van clientcertificaten voor toegang tot de back-end-service voor een API te configureren. Voordat u de stappen in dit onderwerp, hebt u uw back-end-service die is geconfigureerd voor verificatie van clientcertificaten ([certificaatverificatie in Azure WebSites configureren Raadpleeg dit artikel] [ to configure certificate authentication in Azure WebSites refer to this article]), en hebben toegang tot het certificaat en het wachtwoord voor het certificaat voor het uploaden van in de publicatieportal van API Management.

## <a name="step1"></a>Een certificaat uploaden
Als u aan de slag wilt gaan, klikt u op **Publicatieportal** in Azure Portal voor uw API Management-service. Hiermee gaat u naar de publicatieportal van API Management.

![API-publicatieportal][api-management-management-console]

> Als u nog geen exemplaar van API Management-service hebt gemaakt, raadpleegt u [API Management service-exemplaar maken][Create an API Management service instance].
> 
> 

Klik op **beveiliging** van de **API Management** menu aan de linkerkant en klik op **clientcertificaten**.

![Clientcertificaten][api-management-security-client-certificates]

Als u wilt een nieuw certificaat uploaden, klikt u op **certificaat uploaden**.

![Certificaat uploaden][api-management-upload-certificate]

Blader naar uw certificaat en voer vervolgens het wachtwoord voor het certificaat.

> Het certificaat moet **.pfx** indeling. Zelfondertekende certificaten zijn toegestaan.
> 
> 

![Certificaat uploaden][api-management-upload-certificate-form]

Klik op **uploaden** om het certificaat te uploaden.

> Wachtwoord voor het certificaat wordt op dit moment gevalideerd. Als dit onjuist is wordt een foutbericht weergegeven.
> 
> 

![Certificaat geüpload][api-management-certificate-uploaded]

Zodra het certificaat is geüpload, wordt deze weergegeven op de **clientcertificaten** tabblad. Als er meerdere certificaten, moet u een notitie van het onderwerp of de laatste vier tekens van de vingerafdruk die worden gebruikt voor het certificaat te selecteren bij het configureren van een API voor het gebruik van certificaten, zoals beschreven in de volgende [configureren van een API gebruiken een clientcertificaat voor gatewayverificatie] [ Configure an API to use a client certificate for gateway authentication] sectie.

> Als u wilt uitschakelen validatie van certificaatketen wanneer u bijvoorbeeld een zelfondertekend certificaat, volg de stappen in deze Veelgestelde vragen [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).
> 
> 

## <a name="step1a"></a>Een clientcertificaat verwijderen
Als u wilt een certificaat wilt verwijderen, klikt u op **verwijderen** naast het gewenste certificaat.

![Certificaat verwijderen][api-management-certificate-delete]

Klik op **Ja, deze verwijderen** om te bevestigen.

![De verwijdering bevestigen][api-management-confirm-delete]

Als het certificaat gebruikt door een API wordt en vervolgens een waarschuwing wordt weergegeven. Als u wilt verwijderen van het certificaat moet u eerst het certificaat verwijderen uit alle API's die zijn geconfigureerd voor gebruik.

![De verwijdering bevestigen][api-management-confirm-delete-policy]

## <a name="step2"></a>Een API voor het gebruik van een certificaat voor gatewayverificatie configureren
Klik op **API's** van de **API Management** menu aan de linkerkant op de naam van de gewenste API en klik op de **beveiliging** tabblad.

![API-beveiliging][api-management-api-security]

Selecteer **clientcertificaten** van de **met referenties** vervolgkeuzelijst.

![Clientcertificaten][api-management-mutual-certificates]

Selecteer het gewenste certificaat van de **clientcertificaat** vervolgkeuzelijst. Als er meerdere certificaten kunt u het onderwerp of de laatste vier tekens van de vingerafdruk zoals aangegeven in de vorige sectie om te bepalen van het juiste certificaat kijken.

![Certificaat selecteren][api-management-select-certificate]

Klik op **opslaan** wijzigen van de configuratie opslaan in de API.

> Deze wijziging wordt onmiddellijk van kracht en aanroepen naar de bewerkingen van deze API gebruikt het certificaat te verifiëren op de back-endserver.
> 
> 

![Sla de wijzigingen van de API][api-management-save-api]

> Wanneer een certificaat voor gatewayverificatie voor de back-end-service van een API is opgegeven, wordt een onderdeel van het beleid voor die API en kunnen worden weergegeven in de beleidseditor.
> 
> 

![Certificaatbeleid][api-management-certificate-policy]

## <a name="self-signed-certificates"></a>Zelfondertekende certificaten

Als u zelfondertekende certificaten gebruikt, moet u API Management om te communiceren met het back endsysteem om validatie van certificaatketen uitschakelen, anders wordt er een 500 foutcode geretourneerd. Om dit te configureren, kunt u de [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (voor de nieuwe back-end) of [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (voor bestaande back-end) PowerShell-cmdlets en stel de `-SkipCertificateChainValidation` -parameter voor `True`.

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over andere manieren voor het beveiligen van uw back-endservice zoals HTTP basic of gedeelde geheime-verificatie, de volgende video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Last-mile-Security/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



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



