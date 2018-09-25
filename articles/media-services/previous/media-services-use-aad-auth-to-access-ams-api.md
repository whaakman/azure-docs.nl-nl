---
title: Toegang tot Azure Media Services API met Azure Active Directory-verificatie | Microsoft Docs
description: Meer informatie over concepten en stappen voor het gebruik van Azure Active Directory (Azure AD) om te verifiëren van toegang met de Azure Media Services-API.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: d6fe69588c44e489f37ad0908df569c2b5a4b96c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967809"
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Toegang tot de API van Azure Media Services met Azure AD-verificatie
 
De API van Azure Media Services is een RESTful-API. U kunt deze gebruiken voor het uitvoeren van bewerkingen op media resources met behulp van een REST-API of met behulp van de beschikbare client-SDK's. Azure Media Services biedt een Media Services-client-SDK voor .NET van Microsoft. Als u wilt worden geautoriseerd voor toegang tot Media Services-resources en de API van Media Services, moet u eerst worden geverifieerd. 

Media Services ondersteunt [Azure Active Directory (Azure AD)-verificatie op basis van](../../active-directory/fundamentals/active-directory-whatis.md). De Azure Media REST-service is vereist dat de gebruiker of toepassing waarmee de REST API vraagt een de **Inzender** of **eigenaar** rol voor toegang tot de resources. Zie voor meer informatie, [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../../role-based-access-control/overview.md).  

> [!IMPORTANT]
> Media Services ondersteunt momenteel het model van Azure Access Control service-verificatie. Access Control-autorisatie worden echter op 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

Dit document geeft een overzicht van hoe u toegang tot de Media Services-API met behulp van REST- of .NET-API's.

## <a name="access-control"></a>Toegangsbeheer

Voor de Azure Media REST-aanvraag te voltooien, moet de aanroepende gebruiker een rol Inzender of eigenaar voor de Media Services-account die deze probeert toegang te hebben.  
Alleen een gebruiker met de rol van eigenaar kunt media resource (account) toegang geven tot nieuwe gebruikers en apps. De rol Inzender hebben toegang tot alleen de mediabron.
Niet-geautoriseerde aanvragen mislukt met de statuscode 401. Als u de foutcode van deze ziet, controleert u of de gebruiker de rol van inzender of eigenaar voor Media Services-account van de gebruiker heeft. U kunt dit controleren in Azure portal. Zoeken naar uw media-account en klik vervolgens op de **toegangsbeheer** tabblad. 

![Access control-tabblad](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Typen verificatie 
 
Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, hebt u twee verificatieopties:

- **Verificatie van de gebruiker**. Een persoon die de app wordt gebruikt om te communiceren met resources voor Media Services worden geverifieerd. De interactieve toepassing moet eerst de gebruiker gevraagd om referenties van de gebruiker. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers om te controleren coderingstaken of live streamen. 
- **Service-principal verificatie**. Een service worden geverifieerd. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, services van de middelste laag of geplande taken worden uitgevoerd. Voorbeelden zijn web-apps, functie-apps, logic apps, API en microservices.

### <a name="user-authentication"></a>Gebruikersverificatie 

Toepassingen die gebruikmaken van de verificatiemethode voor de gebruiker moeten zijn management of bewaken van systeemeigen apps: mobiele apps en apps voor Windows-consoletoepassingen. Dit type oplossing is handig als u wilt dat menselijke tussenkomst met de service in een van de volgende scenario's:

- Dashboard van de controle voor uw coderingstaken.
- Dashboard van de controle voor uw live streams.
- Management-toepassing voor desktop- of mobiele gebruikers voor het beheren van resources in een Media Services-account.

> [!NOTE]
> Deze verificatiemethode moet niet worden gebruikt voor consumentgerichte toepassingen. 

Een systeemeigen toepassing moet eerst een toegangstoken van Azure AD te verkrijgen en vervolgens worden gebruikt wanneer u HTTP-aanvragen met de Media Services REST-API. Het toegangstoken toevoegen aan de aanvraagheader. 

Het volgende diagram toont een typische interactieve toepassing verificatie-stroom: 

![Diagram van de systeemeigen apps](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

In het voorgaande diagram vertegenwoordigen de nummers in de stroom van de aanvragen in chronologische volgorde.

> [!NOTE]
> Wanneer u de methode voor gebruikersverificatie, wordt alle apps delen dezelfde (standaard) systeemeigen toepassing client-ID en omleidings-URI systeemeigen toepassing. 

1. Een gebruiker om referenties gevraagd.
2. Aanvragen van een Azure AD-toegangstoken met de volgende parameters:  

    * Azure AD-tenant-eindpunt.

        De tenant-gegevens kan worden opgehaald uit de Azure-portal. Plaats de cursor boven de naam van de aangemelde gebruiker in de rechterbovenhoek.
    * Media Services-resource-URI. 

        Deze URI is hetzelfde voor Media Services-accounts die zich in dezelfde Azure-omgeving (bijvoorbeeld https://rest.media.azure.net).

    * Media Services (systeemeigen) toepassingsclient-id.
    * Media Services (native modus)-toepassing omleidings-URI.
    * Resource-URI voor de REST-mediaservices.
        
        De URI vertegenwoordigt de REST-API-eindpunt (bijvoorbeeld https://test03.restv2.westus.media.azure.net/api/).

    Als u waarden voor deze parameters, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md) met behulp van de optie voor verificatie.

3. Het Azure AD-toegangstoken wordt verzonden naar de client.
4. De client stuurt een aanvraag naar de Azure REST-API voor Media met de Azure AD-toegangstoken.
5. De client wordt in de gegevens van Media Services.

Zie voor meer informatie over het gebruik van Azure AD-verificatie om te communiceren met de REST-aanvragen met behulp van de client-Media Services .NET SDK [gebruik Azure AD-verificatie voor toegang tot de Media Services-API met .NET](media-services-dotnet-get-started-with-aad.md). 

Als u niet de client-Media Services .NET SDK gebruikt, moet u handmatig een aanvraag voor Azure AD een toegangstoken maken met behulp van de parameters die worden beschreven in stap 2. Zie voor meer informatie, [hoe u de Azure AD-token ophalen met de Azure AD-Verificatiebibliotheek](../../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die door services van de middelste laag en geplande taken uitvoeren: web-apps, functie-apps, logic apps, API's en microservices. Deze verificatiemethode is ook geschikt voor interactieve toepassingen waarin wilt u mogelijk een service-account gebruiken om resources te beheren.

Wanneer u de service principal verificatiemethode consumentenscenario's bouwen, wordt verificatie doorgaans uitgevoerd in de middelste laag (via een API) en niet rechtstreeks in een mobiele of bureaubladtoepassingen-toepassing. 

Als u wilt deze methode gebruikt, een Azure AD-toepassing en service-principal maken in een eigen tenant. Nadat u de toepassing hebt gemaakt, geven u de app Inzender of eigenaar roltoegang tot het Media Services-account. U kunt dit doen in de Azure-portal met behulp van de Azure CLI of met een PowerShell-script. U kunt ook een bestaande Azure AD-toepassing gebruiken. U kunt registreren en beheren van uw Azure AD-app en service-principal [in Azure portal](media-services-portal-get-started-with-aad.md). U kunt dit ook doen met behulp van [Azure CLI](media-services-use-aad-auth-to-access-ams-api.md) of [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Middelste laag apps](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Nadat u uw Azure AD-toepassing maakt, krijgt u waarden voor de volgende instellingen. U hebt deze waarden nodig voor verificatie:

- Client-id 
- Clientgeheim 

De getallen in de afbeelding hierboven gelden voor de stroom van de aanvragen in chronologische volgorde:
    
1. Een app van de middelste laag (web-API of webtoepassing) vraagt een toegangstoken van Azure AD dat de volgende parameters heeft:  

    * Azure AD-tenant-eindpunt.

        De tenant-gegevens kan worden opgehaald uit de Azure-portal. Plaats de cursor boven de naam van de aangemelde gebruiker in de rechterbovenhoek.
    * Media Services-resource-URI. 

        Deze URI is hetzelfde voor Media Services-accounts die zich in dezelfde Azure-omgeving (bijvoorbeeld https://rest.media.azure.net).

    * Resource-URI voor de REST-mediaservices.

        De URI vertegenwoordigt de REST-API-eindpunt (bijvoorbeeld https://test03.restv2.westus.media.azure.net/api/).

    * Waarden van Azure AD-toepassing: de client-ID en clientgeheim.
    
    Als u waarden voor deze parameters, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md) met behulp van de optie voor de service-principal verificatie.

2. Het Azure AD-toegangstoken wordt verzonden naar de middelste laag.
4. De middelste laag stuurt de aanvraag naar de Azure REST-API voor Media met de Azure AD-token.
5. De middelste laag wordt in de gegevens van Media Services.

Zie voor meer informatie over het gebruik van Azure AD-verificatie om te communiceren met de REST-aanvragen met behulp van de client-Media Services .NET SDK [gebruik Azure AD-verificatie voor toegang tot Azure Media Services API met .NET](media-services-dotnet-get-started-with-aad.md). 

Als u niet de client-Media Services .NET SDK gebruikt, moet u handmatig een Azure AD-tokenaanvraag maken met behulp van de parameters die worden beschreven in stap 1. Zie voor meer informatie, [hoe u de Azure AD-token ophalen met de Azure AD-Verificatiebibliotheek](../../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Problemen oplossen

Uitzondering: ' de externe server heeft een fout geretourneerd: (401) niet geautoriseerd. "

Oplossing: De aanroepende gebruiker moet een rol Inzender of eigenaar in het Media Services-account die deze probeert te krijgen tot zijn voor de Media Services REST-aanvraag te voltooien. Zie voor meer informatie de [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control) sectie.

## <a name="resources"></a>Resources

De volgende artikelen vindt u overzichten van verificatie op basis van Azure AD: 

- [Verificatiescenario's die worden opgelost door Azure AD](../../active-directory/develop/authentication-scenarios.md)
- [Toevoegen, bijwerken of verwijderen van een toepassing in Azure AD](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)
- [Configureren en toegangsbeheer op basis van rollen beheren met behulp van PowerShell](../../role-based-access-control/role-assignments-powershell.md)

## <a name="next-steps"></a>Volgende stappen

* De Azure portal gebruiken om [toegang hebben tot Azure AD-verificatie gebruiken voor Azure Media Services API](media-services-portal-get-started-with-aad.md).
* Gebruik Azure AD-verificatie te [toegang tot Azure Media Services API met .NET](media-services-dotnet-get-started-with-aad.md).

