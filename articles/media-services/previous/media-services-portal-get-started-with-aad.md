---
title: Aan de slag met Azure AD-verificatie met behulp van Azure portal | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal voor toegang tot Azure Active Directory (Azure AD)-verificatie gebruiken de API van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: juliako
ms.openlocfilehash: bae134d30bd175656e4da9121bca8dbfcfe7fcdd
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50229456"
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Aan de slag met Azure AD-verificatie met behulp van Azure portal

Informatie over het gebruik van de Azure-portal voor toegang tot Azure Active Directory (Azure AD)-verificatie voor toegang tot de API van Azure Media Services.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u geen account hebt, beginnen met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media Services-account. Zie voor meer informatie, [een Azure Media Services-account maken met behulp van de Azure-portal](media-services-portal-create-account.md).
- Zorg ervoor dat u de [toegang tot Azure Media Services API met Azure AD-verificatieoverzicht](media-services-use-aad-auth-to-access-ams-api.md). 

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, hebt u twee verificatieopties:

- **Verificatie van de gebruiker**. Een persoon die de app wordt gebruikt om te communiceren met resources voor Media Services worden geverifieerd. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers om te controleren coderingstaken of live streamen. 
- **Service-principal verificatie**. Een service worden geverifieerd. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, services van de middelste laag of geplande taken worden uitgevoerd: web-apps, functie-apps, logic apps, API's of een microservice.

> [!IMPORTANT]
> Media Services ondersteunt momenteel het model van Azure Access Control service-verificatie. Access Control-autorisatie worden echter op 1 juni 2018 afgeschaft. We raden u aan om zo snel mogelijk naar het Azure Active Directory-verificatiemodel te migreren.

## <a name="select-the-authentication-method"></a>Selecteer de verificatiemethode

1. In de [Azure-portal](https://portal.azure.com/), selecteert u uw Media Services-account.
2. Selecteer hoe u verbinding maakt met de Media Services-API.

    ![Selecteer de pagina methode voor verbinding](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Gebruikersverificatie

Voor verbinding met de API van Media Services met behulp van de optie voor verificatie, moet de client-app om aan te vragen van een Azure AD-token met de volgende parameters:  

* Azure AD-tenant-eindpunt
* Media Services-resource-URI
* Client-ID van Media Services (native modus)-toepassing 
* Media Services (native modus)-toepassing omleidings-URI 
* Resource-URI voor de REST-mediaservices

U kunt de waarden voor deze parameters krijgen bij de **API van Media Services met verificatie van de gebruiker** pagina. 

![Verbinding maken met de pagina voor verificatie van gebruiker](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Als u verbinding met de Media Services-API maken met behulp van Microsoft voor Media Services .NET SDK, is de vereiste waarden zijn beschikbaar voor u als onderdeel van de SDK. Zie voor meer informatie, [gebruik Azure AD-verificatie voor toegang tot de API van Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md).

Als u niet de client-Media Services .NET SDK gebruikt, moet u handmatig een Azure AD-tokenaanvraag maken met behulp van de parameters die eerder zijn besproken. Zie voor meer informatie, [hoe u de Azure AD-token ophalen met de Azure AD-Verificatiebibliotheek](../../active-directory/develop/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Verificatie van service-principal

Voor verbinding met de API van Media Services met behulp van de service-principal-optie, moet uw app middelste laag (web-API of webtoepassing) om aan te vragen van een Azure AD-token met de volgende parameters:  

* Azure AD-tenant-eindpunt
* Media Services-resource-URI 
* Resource-URI voor de REST-mediaservices
* Waarden van Azure AD-toepassing: de **client-ID** en **clientgeheim**

U kunt de waarden voor deze parameters krijgen bij de **verbinding maken met de API van Media Services met service-principal** pagina. Gebruik deze pagina voor het maken van een nieuwe Azure AD-toepassing of Selecteer een bestaande resourcegroep. Nadat u de Azure AD-app hebt geselecteerd, kunt u ophalen van de client-ID (toepassings-ID) en de waarden van client secret (sleutel) gegenereerd. 

![Verbinding maken met de principal-servicepagina](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

Wanneer de **Service-Principal** blade wordt geopend, de eerste Azure AD-toepassing die voldoet aan de volgende criteria is ingeschakeld:

- Het is een geregistreerde Azure AD-toepassing.
- Inzender of Owner Role-Based Access Control-machtigingen heeft voor het account.

Nadat u maakt of selecteert u een Azure AD-app, kunt u maken en kopiëren van een clientgeheim (sleutel) en de client-ID (toepassings-ID). Het clientgeheim en de client-ID moet het toegangstoken ophalen in dit scenario.

Als u geen machtigingen voor het maken van Azure AD-apps in uw domein, de Azure AD app besturingselementen aan de blade worden niet weergegeven en een waarschuwingsbericht wordt weergegeven.

Als u verbinding maken met de Media Services-API met behulp van de Media Services .NET SDK, [gebruik Azure AD-verificatie voor toegang tot de API van Azure Media Services met .NET](media-services-dotnet-get-started-with-aad.md).

Als u niet de client-Media Services .NET SDK gebruikt, moet u handmatig een tokenaanvraag van Azure AD met behulp van de parameters die eerder zijn besproken. Zie voor meer informatie, [hoe u de Azure AD-token ophalen met de Azure AD-Verificatiebibliotheek](../../active-directory/develop/active-directory-authentication-libraries.md).

### <a name="get-the-client-id-and-client-secret"></a>De client-ID en clientgeheim ophalen

Nadat u een bestaande Azure AD-app selecteren of de optie voor het maken van een nieuwe, wordt de volgende knoppen weergegeven:

![Beheren van de knop machtigingen en knop van de toepassing beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

De Azure AD-toepassing om blade te openen, klikt u op **toepassing beheren**. Op de **toepassing beheren** blade, krijgt u de app client-ID (toepassings-ID). Voor het genereren van een clientgeheim (sleutel), selecteer **sleutels**.

![Optie voor de App blade sleutels beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a>Machtigingen en de toepassing beheren

Nadat u de Azure AD-toepassing selecteert, kunt u de toepassing en machtigingen beheren. Als u uw Azure AD-toepassing voor toegang tot andere toepassingen instelt, klikt u op **machtigingen beheren**. Voor beheertaken, zoals het wijzigen van sleutels en antwoord-URL's of te bewerken van het toepassingsmanifest, klikt u op **toepassing beheren**.

### <a name="edit-the-apps-settings-or-manifest"></a>Manifest of van de app-instellingen bewerken

Als u wilt de instellingen van de app of het manifest bewerken, klikt u op **toepassing beheren**.

![Toepassing beheren](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
