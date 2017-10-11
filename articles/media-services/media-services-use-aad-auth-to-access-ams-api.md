---
title: Toegang tot API voor Azure Media Services met Azure Active Directory-verificatie | Microsoft Docs
description: "Meer informatie over concepten en stappen voor het gebruik van Azure Active Directory (Azure AD) verifiëren van toegang tot de API van Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 0e1217afb0a37353793c64ae927b741d9fee4954
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Toegang tot de API van Azure Media Services met Azure AD-verificatie
 
De API van Azure Media Services is een RESTful-API. U kunt deze bewerkingen uitvoeren op mediabronnen met behulp van een REST-API of met behulp van de beschikbare client-SDK's. Azure Media Services biedt een Media Services-client-SDK voor .NET voor Microsoft. Als u wilt worden gemachtigd voor toegang tot Media Services-resources en de Media Services-API, moet u eerst worden geverifieerd. 

Media Services ondersteunt [Azure Active Directory (Azure AD)-gebaseerde verificatie](../active-directory/active-directory-whatis.md). De service Azure Media REST vereist dat de gebruiker of toepassing die de REST-API maakt aanvragen hebben ofwel de **Inzender** of **eigenaar** rol voor toegang tot de bronnen. Zie voor meer informatie [aan de slag met toegangsbeheer op basis van rollen in Azure portal](../active-directory/role-based-access-control-what-is.md).  

> [!IMPORTANT]
> Media Services ondersteunt momenteel, het model van Azure Access Control service-verificatie. Access Control-autorisatie wordt echter op 1 juni 2018 afgeschaft. Het is raadzaam om te migreren naar het model van Azure AD authentication zo snel mogelijk.

Dit document bevat een overzicht van hoe u toegang tot de Media Services-API met REST- of .NET API's.

## <a name="access-control"></a>Toegangsbeheer

Azure Media REST voor de aanvraag mislukt, moet de aanvragende gebruiker een rol Inzender of -eigenaar voor deze probeert te krijgen tot het Media Services-account hebben.  
Alleen een gebruiker met de rol van eigenaar kan media resource (account) toegang geven tot nieuwe gebruikers- of -apps. De rol Inzender hebben toegang tot alleen de mediabron.
Ongeautoriseerde aanvragen mislukt met de statuscode 401. Als u deze foutcode ziet, controleert u of de gebruiker de rol van inzender of -eigenaar voor Media Services-account van de gebruiker heeft. U kunt dit controleren in de Azure portal. Zoeken naar uw media-account en klik vervolgens op de **toegangsbeheer** tabblad. 

![Access control-tabblad](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Typen verificatie 
 
Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, hebt u twee opties voor verificatie:

- **Gebruikersverificatie**. Verifiëren van een persoon die de app wordt gebruikt om te communiceren met Media Services-resources. De interactieve toepassing moet eerst de gebruiker gevraagd om referenties van de gebruiker. Een voorbeeld is een management console-app gebruikt door gemachtigde gebruikers codering taken bewaken of live streamen. 
- **Verificatie van de service-principal**. Een service verifiëren. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, middelste laag services of geplande taken worden uitgevoerd. Voorbeelden zijn web-apps, apps van de functie logic apps, API en microservices.

### <a name="user-authentication"></a>Gebruikersverificatie 

Toepassingen die gebruikmaken van de verificatiemethode voor de gebruiker moeten zijn management of systeemeigen apps bewaking: mobiele apps, Windows-apps en toepassingen van de Console. Dit type oplossing is handig als u wilt dat menselijke tussenkomst met de service in een van de volgende scenario's:

- Dashboard voor uw codering taken bewaken.
- Het bewakingsdashboard voor uw live-stromen.
- Management-toepassing voor pc's of mobiele gebruikers voor het beheren van resources in een Media Services-account.

> [!NOTE]
> Deze verificatiemethode moet niet worden gebruikt voor consumententoepassingen. 

Een systeemeigen toepassing moet eerst een toegangstoken van Azure AD te verkrijgen en vervolgens worden gebruikt wanneer u HTTP-aanvragen voor de REST-API van Media Services. Het toegangstoken toevoegen aan de aanvraag-header. 

Het volgende diagram toont een typische interactieve toepassing authenticatiestroom: 

![Diagram van systeemeigen apps](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

In het voorgaande diagram vertegenwoordigen de nummers in de stroom van de aanvragen in chronologische volgorde.

> [!NOTE]
> Wanneer u de methode voor gebruikersverificatie, worden alle apps delen de dezelfde (standaard) native client-ID en systeemeigen omleidings-URI. 

1. Een gebruiker om referenties gevraagd.
2. Aanvragen van een Azure AD-toegangstoken met de volgende parameters:  

    * Azure AD-tenant-eindpunt.

        De tenant-informatie kan worden opgehaald uit de Azure-portal. Plaats de cursor op de naam van de aangemelde gebruiker in de rechterbovenhoek van de rechterbovenhoek.
    * Media Services resource-URI. 

        Deze URI is hetzelfde voor Media Services-accounts die zich in de dezelfde Azure-omgeving (bijvoorbeeld https://rest.media.azure.net).

    * Media Services (systeemeigen) toepassingsclient-id.
    * Media Services (systeemeigen)-toepassing omleidings-URI.
    * Resource-URI voor de REST mediaservices.
        
        De URI vertegenwoordigt de REST-API-eindpunt (bijvoorbeeld https://test03.restv2.westus.media.azure.net/api/).

    Als u de waarden voor deze parameters, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md) met de optie voor verificatie van gebruiker.

3. Het toegangstoken van Azure AD wordt verzonden naar de client.
4. De client verzendt een aanvraag naar de Azure Media REST-API met de Azure AD-toegangstoken.
5. De client krijgt weer de gegevens van Media Services.

Zie voor meer informatie over het gebruik van Azure AD-verificatie om te communiceren met de REST-aanvragen via de client met Media Services .NET SDK [gebruik Azure AD-verificatie voor toegang tot de Media Services-API met .NET](media-services-dotnet-get-started-with-aad.md). 

Als u geen van de client-Media Services .NET SDK gebruikmaakt, moet u handmatig een tokenaanvraag van Azure AD access maken met behulp van de parameters die worden beschreven in stap 2. Zie voor meer informatie [hoe de Azure AD-token ophalen met de Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die services van de middelste laag en geplande taken uitvoeren: web-apps, apps van de functie logic apps, API's en microservices. Deze verificatiemethode is ook geschikt zijn voor interactieve toepassingen waar wilt u mogelijk een serviceaccount gebruiken om resources te beheren.

Wanneer u de service principal verificatiemethode voor het bouwen van consumentenscenario's gebruikt, wordt verificatie gewoonlijk uitgevoerd in de middelste laag (via een API) en niet rechtstreeks in een toepassing met mobiele of bureaubladtoepassingen. 

Als u deze methode gebruikt, maak een Azure AD-toepassing en service principal in een eigen tenant. Nadat u de toepassing hebt gemaakt, krijgt u de app Inzender of eigenaar rol de toegang tot het Media Services-account. U kunt dit doen in de Azure portal met behulp van Azure CLI of met een PowerShell-script. U kunt ook een bestaande Azure AD-toepassing gebruiken. U kunt registreren en beheren van uw Azure AD-app en service-principal [in de Azure portal](media-services-portal-get-started-with-aad.md). U kunt dit ook doen met behulp van [Azure CLI 2.0](media-services-use-aad-auth-to-access-ams-api.md) of [PowerShell](media-services-powershell-create-and-configure-aad-app.md). 

![Middelste laag apps](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Nadat u uw Azure AD-toepassing hebt gemaakt, kunt u waarden ophalen voor de volgende instellingen. U moet deze waarden voor verificatie:

- Client-ID 
- Clientgeheim 

De getallen in de voorgaande afbeelding gelden voor de stroom van de aanvragen in chronologische volgorde:
    
1. Een app middelste laag (web-API of webtoepassing) vraagt een Azure AD-toegangstoken dat de volgende parameters heeft:  

    * Azure AD-tenant-eindpunt.

        De tenant-informatie kan worden opgehaald uit de Azure-portal. Plaats de cursor op de naam van de aangemelde gebruiker in de rechterbovenhoek van de rechterbovenhoek.
    * Media Services resource-URI. 

        Deze URI is hetzelfde voor Media Services-accounts die zich in de dezelfde Azure-omgeving (bijvoorbeeld https://rest.media.azure.net bevinden).

    * Resource-URI voor de REST mediaservices.

        De URI vertegenwoordigt de REST-API-eindpunt (bijvoorbeeld https://test03.restv2.westus.media.azure.net/api/).

    * Waarden van Azure AD-toepassing: de client-ID en clientgeheim.
    
    Als u de waarden voor deze parameters, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md) met behulp van de service principal verificatieoptie.

2. Het toegangstoken van Azure AD wordt verzonden naar de middelste laag.
4. De middelste laag verzendt een aanvraag naar de Azure Media REST-API met de Azure AD-token.
5. De middelste laag krijgt weer de gegevens van Media Services.

Zie voor meer informatie over het gebruik van Azure AD-verificatie om te communiceren met de REST-aanvragen via de client met Media Services .NET SDK [gebruik Azure AD-verificatie voor toegang tot API voor Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md). 

Als u geen van de client-Media Services .NET SDK gebruikmaakt, moet u handmatig een Azure AD-tokenaanvraag maken met behulp van de parameters die worden beschreven in stap 1. Zie voor meer informatie [hoe de Azure AD-token ophalen met de Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Problemen oplossen

Uitzondering: ' de externe server heeft een fout geretourneerd: (401) niet geautoriseerd. "

Oplossing: Media Services REST voor de aanvraag mislukt, de aanvragende gebruiker moet een rol Inzender of eigenaar in de Media Services-account die het probeert te openen. Zie voor meer informatie de [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control) sectie.

## <a name="resources"></a>Resources

De volgende artikelen vindt u overzichten van de concepten van Azure AD-verificatie: 

- [Verificatie scenario's voor toepassingsbeheer door Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md#basics-of-authentication-in-azure-ad)
- [Toevoegen, bijwerken of verwijderen van een toepassing in Azure AD](../active-directory/develop/active-directory-integrating-applications.md)
- [Configureren en beheren van rollen gebaseerd toegangsbeheer met behulp van PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)

## <a name="next-steps"></a>Volgende stappen

* De Azure portal gebruiken om [toegang hebben tot Azure AD-verificatie gebruiken voor Azure Media Services-API](media-services-portal-get-started-with-aad.md).
* Verificatie met Azure AD [toegang tot API voor Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md).

