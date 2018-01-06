---
title: Verificatie met de Mobile Engagement REST-API's - handmatige installatie
description: Hierin wordt beschreven hoe u verificatie voor Mobile Engagement REST-API's handmatig instellen
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3b678acbae225c28223a2ee76e5be2462a529362
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Verificatie met de Mobile Engagement REST-API's - handmatige installatie
Deze documentatie is een bijlage-documentatie voor [verifiëren met Mobile Engagement REST-API's](mobile-engagement-api-authentication.md). Zorg ervoor dat u het eerst gebruikt om toegang te krijgen van de context lezen.
Hierin wordt een andere methode voor het eenmalig instellen voor het instellen van uw verificatie voor de Mobile Engagement REST-API's met de Azure portal.

> [!NOTE]
> De volgende instructies zijn gebaseerd op deze [Active Directory-handleiding](../azure-resource-manager/resource-group-create-service-principal-portal.md) en aangepast voor wat vereist voor verificatie voor Mobile Engagement-API's is. Dus hiernaar wordt verwezen als u wilt weten over de stappen hieronder in detail.

1. Aanmelden bij uw Azure-Account via de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Active Directory** in het linkerdeelvenster.

     ![Selecteer Active Directory][1]

3. Als u wilt de toepassingen in uw directory bekijken, klikt u op **App registraties**.

     ![toepassingen weergeven][3]

4. Klik op **registratie van de nieuwe toepassing**.

     ![Toepassing toevoegen][4]

5. Vul in de naam van de toepassing en het type van toepassing als laat **Web-app/API** en klik op de knop Volgende. U kunt opgeven dat alle dummy-URL's voor **AANMELDINGS-URL**: ze worden niet gebruikt voor dit scenario en de URL's zelf worden niet gevalideerd.
6. Nadat u bent, hebt u een Azure AD-app met de naam die u hebt opgegeven. Het is uw **AD\_APP\_naam**, noteer ervan.

     ![app-naam][8]

7. Klik op de naam van de app.
8. Zoeken naar **toepassings-ID**, noteer ervan, kan de CLIENT-ID die moet worden gebruikt als worden **CLIENT\_ID** voor uw API-aanroepen.

     ![App configureren][10]

9. Zoek de **sleutels** sectie aan de rechterkant.

     ![App configureren][11]

10. Maak een nieuwe sleutel en onmiddellijk kopiëren en opslaan voor gebruik. Dit wordt nooit opnieuw worden weergegeven.

     ![App configureren][12]

    > [!IMPORTANT]
    > Deze sleutel verloopt aan het einde van de duur die u hebt opgegeven, dus zorg ervoor dat het te vernieuwen wanneer u anders de verificatie van uw API wordt niet meer werken. U kunt ook verwijderen en opnieuw maken van deze sleutel als u denkt dat deze is geschonden.
    >
    >
11. Klik op **eindpunten** knop aan de bovenkant van de pagina en kopieer de **OAUTH 2.0-TOKENEINDPUNT**.

    ![][14]

16. Dit eindpunt worden weergegeven in de volgende notatie waarbij de GUID in de URL uw **TENANT_ID** dus maak een notitie van deze:`https://login.microsoftonline.com/<GUID>/oauth2/token`
17. We gaat nu door met het configureren van de machtigingen op deze app. Voor deze heeft u opent u de [Azure-portal](https://portal.azure.com). 
18. Klik op **resourcegroepen** en zoek de **Mobile Engagement** resourcegroep.

    ![][15]

19. Klik op de **Mobile Engagement** resource groeperen en navigeer naar de **instellingen** hier sectie.

    ![][16]

20. Klik op **gebruikers** sectie in de instellingen en klik vervolgens op **toevoegen** een gebruiker toe te voegen.

    ![][17]

21. Klik op **Selecteer een rol**.

    ![][18]

22. Klik op **eigenaar**.

    ![][19]

23. Zoeken naar de naam van uw toepassing **AD\_APP\_naam** in het zoekvak. U ziet dit hier standaard. Als u deze hebt gevonden, selecteert u deze en klik op **Selecteer** aan de onderkant van de sectie.

    ![][20]

24. Op de **toegang toevoegen** sectie, deze wordt weergegeven als **1 gebruiker, 0 groepen**. Klik op **OK** in deze sectie om de wijziging te bevestigen.

    ![][21]

U hebt nu de vereiste configuratie van Azure AD en u bent klaar om aan te roepen de API's.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



