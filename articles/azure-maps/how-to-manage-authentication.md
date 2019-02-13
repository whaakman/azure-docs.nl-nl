---
title: Verificatie van Azure Maps beheren | Microsoft Docs
description: U kunt de Azure-portal gebruiken om verificatie in Azure-kaarten te beheren.
author: walsehgal
ms.author: v-musehg
ms.date: 02/09/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: db0450ab8b765ec158201c6db149a7de7d135b98
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56143784"
---
# <a name="manage-authentication-in-azure-maps"></a>Verificatie van Azure Maps beheren

Nadat u een Azure kaarten-account hebt gemaakt, worden drie GUID's worden gemaakt ter ondersteuning van Azure Active Directory (Azure AD) of gedeelde sleutelverificatie.

U kunt uw verificatie-informatie vinden door te navigeren naar de **verificatie** pagina onder **instellingen** in Azure portal. Navigeer naar uw account. Selecteer vervolgens **verificatie** in het menu.


## <a name="view-authentication-details"></a>Verificatie-details weergeven

Uw verificatie om details te bekijken, gaat u naar de **verificatie** optie in het instellingenmenu.

![Weergave-verificatie](./media/how-to-manage-authentication/how-to-view-auth.png)

 Zie voor meer informatie over verificatie en Azure-kaarten, [verificatie met Azure Maps](https://aka.ms/amauth).


## <a name="configure-azure-ad-app-registration"></a>Azure AD-App-registratie configureren

Zodra een overzicht van Azure-account is gemaakt, wordt een koppeling tussen uw Azure AD-tenant en de Azure Maps Azure-resource is vereist. 

1. Ga naar de AAD-blade en de registratie van een App maken met een naam en het teken in de URL als de startpagina van de web-app / API, zoals 'https://localhost/'. Als u al een geregistreerde app hebt, gaat u verder met stap 2.

    ![App-registratie](./media/how-to-manage-authentication/app-registration.png)

    ![App-registratie](./media/how-to-manage-authentication/app-create.png)

2. Gedelegeerde API-machtigingen toewijzen aan Azure-kaarten door te navigeren naar de toepassing onder App-registraties en klik op **instellingen**.  Selecteer **vereiste machtigingen** en selecteer **toevoegen**. Zoeken en vervolgens selecteert u Azure Maps onder **Select an API** en klikt u op **Selecteer**.

    ![Api-App-machtigingen](./media/how-to-manage-authentication/app-permissions.png)

3. Ten slotte onder Selecteer machtigingen kiest u toegang tot Azure-kaarten en klik op selecteren.

    ![Selecteer de api-app-machtigingen](./media/how-to-manage-authentication/select-app-permissions.png)

4. Ga als volgt stap een of b, afhankelijk van de verificatie-implementatie.

    1. Als de toepassing wilde maken is voor gebruik van token gebruikersverificatie met onze Azure Maps Web SDK, moet u inschakelen de `oauthEnableImplicitFlow` door deze te stellen op ' True ' in de Manifest-sectie van de detailpagina van uw app-registratie. 
    
       ![App-manifest](./media/how-to-manage-authentication/app-manifest.png)

    2. Als de verificatie van uw toepassing maakt gebruik van server/toepassing gaat u naar de **sleutels** blade in de app-registratie en een wachtwoord maken of uploaden van een openbare-sleutelcertificaat dat naar de app-registratie. Als u een wachtwoord, nadat u **opslaan**, kopieert u het wachtwoord voor later, en bewaar deze zorgvuldig, u dit wilt gebruiken voor het verkrijgen van tokens van AAD. 

       ![App-sleutels](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Verleen RBAC voor Azure Maps

Wanneer een Azure kaarten-account gekoppeld aan uw Azure AD-tenant is, kan toegangsbeheer worden verleend door de gebruiker of toepassing met beschikbare Azure Maps access control rollen toewijzen.

1. Navigeer naar **Access Control** optie, klikt u op **roltoewijzingen**, en **roltoewijzing toevoegen**.

    ![RBAC verlenen](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Selecteer op de roltoewijzing pop-outmodus venster, Azure Maps datum Reader (Preview) **rol**, **toegang toewijzen aan**: Azure AD-gebruiker, groep of service-principal **Selecteer** gebruiker of toepassing in vervolgkeuzelijst en **opslaan**.

    ![Roltoewijzing toevoegen](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Beschikbare Azure Maps RBAC-rollen weergeven

Als u wilt weergeven beschikbaar op basis van rollen toegangsbeheerrollen voor Azure-kaarten die toegang kan worden verleend aan, gaat u naar **Access Control (IAM)** optie, klikt u op **rollen**, en zoeken naar functies vanaf **Azure Maps**.

![Beschikbare rollen weergeven](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-role-based-access-control-rbac"></a>Azure-kaarten weergeven op rollen gebaseerd toegangsbeheer (RBAC)

Azure AD maakt voor de brongroep gedetailleerd toegangsbeheer via op rollen gebaseerd toegangsbeheer (RBAC). 

Als u wilt weergeven van gebruikers en apps die zijn verleend RBAC voor Azure-kaarten, gaat u naar **Access Control (IAM)** optie, selecteer **roltoewijzingen**, en filteren op **Azure Maps**. Alle beschikbare rollen wordt hieronder weergegeven.

![Weergave RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Aanvragen van tokens voor Azure-kaarten

Als uw app is geregistreerd en die zijn gekoppeld aan Azure-kaarten, kunt u nu toegangstokens aanvragen.

* Als u de toepassing is wilde maken gebruik van token gebruikersverificatie met onze Azure Maps Web SDK (Web), moet u de HTML-pagina configureren met Azure Maps Client-ID en de Azure AD-App-ID.


* Voor toepassingen die gebruikmaken van verificatie van de server/toepassing, een token aanvragen van Azure AD-aanmelding eindpunt https://login.microsoftonline.com met Azure Maps Client-ID, Azure AD-App-ID en Azure AD-App-registratie-wachtwoord of certificaat.

Zie voor meer informatie over het aanvragen van tokens voor toegang van Azure AD voor gebruikers en service-principals [Verificatiescenario's voor Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over Azure AD-verificatie en de Web-SDK van Azure Maps, [Azure AD en Azure Maps Web SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).