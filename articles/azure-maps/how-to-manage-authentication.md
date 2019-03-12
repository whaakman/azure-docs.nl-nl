---
title: Verificatie van Azure Maps beheren | Microsoft Docs
description: U kunt de Azure-portal gebruiken om verificatie in Azure-kaarten te beheren.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: fe0e325543d245e4415fea925fcdade32eb00614
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57570501"
---
# <a name="manage-authentication-in-azure-maps"></a>Verificatie van Azure Maps beheren

Nadat u een Azure kaarten-account hebt gemaakt, worden een client-ID en sleutels gemaakt ter ondersteuning van Azure Active Directory (Azure AD) of gedeelde sleutelverificatie.

## <a name="view-authentication-details"></a>Verificatie-details weergeven

De verificatie-informatie vindt u in Azure portal. Ga naar uw account en selecteer **verificatie** op de **instellingen** menu.

![Verificatiegegevens](./media/how-to-manage-authentication/how-to-view-auth.png)

 Zie voor meer informatie, [verificatie met Azure Maps](https://aka.ms/amauth).


## <a name="set-up-azure-ad-app-registration"></a>Azure AD-app-registratie instellen

Nadat u een Azure kaarten-account hebt gemaakt, moet u een koppeling tussen uw Azure AD-tenant en de bron van de Azure-kaarten.

1. Ga naar de blade Azure AD en maak een app-registratie. Geef een naam voor de registratie. In de **aanmeldings-URL** geeft u de startpagina van de web-app / API (bijvoorbeeld https://localhost/). Als u al een geregistreerde app hebt, gaat u naar stap 2.

    ![App-registratie](./media/how-to-manage-authentication/app-registration.png)

    ![Details van de App-registratie](./media/how-to-manage-authentication/app-create.png)

2. Als u wilt gedelegeerde API-machtigingen toewijzen aan Azure-kaarten, gaat u naar de toepassing onder **App-registraties**, en selecteer vervolgens **instellingen**.  Selecteer **vereiste machtigingen**, en selecteer vervolgens **toevoegen**. Zoek en selecteer **Azure Maps** onder **Select an API**, en selecteer vervolgens de **Selecteer** knop.

    ![API-App-machtigingen](./media/how-to-manage-authentication/app-permissions.png)

3. Onder **machtigingen selecteren**, selecteer **toegang tot Azure Maps**, en selecteer vervolgens de **Selecteer** knop.

    ![Selecteer API app-machtigingen](./media/how-to-manage-authentication/select-app-permissions.png)

4. Volledige stap een of b, afhankelijk van de verificatiemethode.

    1. Als uw toepassing gebruikerstoken verificatie met Azure Maps Web SDK gebruikt, schakelt u `oauthEnableImplicitFlow` door deze te stellen op ' True ' in de Manifest-sectie van de detailpagina van uw app-registratie.
    
       ![App-manifest](./media/how-to-manage-authentication/app-manifest.png)

    2. Als uw toepassing gebruikmaakt van verificatie van de server/toepassing, gaat u naar de **sleutels** -blade in de app-registratie en maak een wachtwoord of een openbare-sleutelcertificaat dat uploaden naar de app-registratie. Als u een wachtwoord maken nadat u hebt geselecteerd **opslaan**, kopieert u het wachtwoord voor later en bewaar deze zorgvuldig. U gebruikt dit wachtwoord aan te schaffen tokens van Azure AD.

       ![App-sleutels](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Verleen RBAC voor Azure Maps

Nadat u een Azure kaarten-account aan uw Azure AD-tenant koppelt, kunt u toegangsbeheer door het toewijzen van een gebruiker of toepassing naar een of meer Azure-kaarten toegangsbeheerrollen verlenen.

1. Ga naar **toegangsbeheer (IAM)**, selecteer **roltoewijzingen**, en selecteer vervolgens **roltoewijzing toevoegen**.

    ![RBAC verlenen](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. In de **roltoewijzing toevoegen** venster onder **rol**, selecteer **Azure Maps datum Reader (Preview)**. Onder **toegang toewijzen aan**, selecteer **Azure AD-gebruiker, groep of service-principal**. Onder **Selecteer**, selecteer de gebruiker of toepassing. Selecteer **Opslaan**.

    ![Roltoewijzing toevoegen](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Beschikbare Azure Maps RBAC-rollen weergeven

Als u wilt weergeven op basis van rollen (RBAC) toegangsbeheerrollen die beschikbaar voor Azure-kaarten zijn, gaat u naar **toegangsbeheer (IAM)**, selecteer **rollen**, en vervolgens zoeken naar functies vanaf **Azure Maps**. Dit zijn de functies die u toegang tot kunt verlenen.

![Beschikbare rollen weergeven](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Azure Maps RBAC weergeven

RBAC biedt nauwkeurig toegangsbeheer.

Als u wilt weergeven van gebruikers en apps die zijn verleend RBAC voor Azure-kaarten, gaat u naar **Access Control (IAM)**, selecteer **roltoewijzingen**, en vervolgens filteren op **Azure Maps**.

![Gebruikers en apps die zijn verleend RBAC weergeven](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Aanvragen van tokens voor Azure-kaarten

Nadat u uw app registreren en deze met Azure-kaarten die zijn gekoppeld, kunt u toegangstokens aanvragen.

* Als uw toepassing gebruikerstoken verificatie met Azure Maps Web SDK gebruikt, moet u het configureren van uw HTML-pagina met de Azure-kaarten client-ID en de Azure AD-app-ID.

* Als uw toepassing gebruikmaakt van server/toepassing verificatie, moet u een token uit Azure AD-aanmelding eindpunt aanvragen `https://login.microsoftonline.com` met de Azure AD-resource-ID `https://atlas.microsoft.com/`, de Azure-kaarten client-ID, de Azure AD-app-ID en de Azure AD-app-registratie wachtwoord of certificaat.

Zie voor meer informatie over het aanvragen van tokens voor toegang van Azure AD voor gebruikers en service-principals [Verificatiescenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure AD-verificatie en de Web-SDK van Azure Maps, [Azure AD en Azure Maps Web SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).