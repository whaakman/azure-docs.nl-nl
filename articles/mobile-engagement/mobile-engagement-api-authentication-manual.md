---
title: 'Verificatie met de Mobile Engagement REST-API''s: handmatige installatie'
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
ms.openlocfilehash: 0b4a999c6778040e71f862d3a010b6635e84b26e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis-manual-setup"></a>Verificatie met de Mobile Engagement REST-API's: handmatige installatie
Deze documentatie is een bijlage bij [verifiëren met Mobile Engagement REST-API's](mobile-engagement-api-authentication.md). Zorg ervoor dat u dat artikel eerst voor inzicht in de context lezen. Hierin wordt ook een andere methode voor de eenmalige verificatie-instellingen voor de REST API's van Mobile Engagement doen met behulp van de Azure-portal.

> [!NOTE]
> De volgende instructies zijn gebaseerd op [deze Active Directory-handleiding](../azure-resource-manager/resource-group-create-service-principal-portal.md). Ze zijn afgestemd op de verificatievereisten voor Mobile Engagement-API's. Raadpleeg deze als u wilt de volgende stappen in detail begrijpen.

1. Aanmelden bij uw Azure-account via de [Azure-portal](https://portal.azure.com/).
2. Selecteer **Active Directory** in het linkerdeelvenster.

   ![Selecteer Active Directory][1]

3. Als u wilt weergeven van de toepassingen in uw directory, selecteer **App registraties**.

   ![toepassingen weergeven][3]

4. Selecteer **registratie van de nieuwe toepassing**.

   ![Toepassing toevoegen][4]

5. Naam van de toepassing ingevuld. Houd het type van toepassing als **Web-app/API**, en selecteer vervolgens de **volgende** knop. U kunt opgeven dat alle dummy-URL's voor **AANMELDINGS-URL**. Ze worden niet gebruikt voor dit scenario en de URL's zelf worden niet gevalideerd.

   Als u klaar bent, hebt u een app met Azure Active Directory (Azure AD) met de naam die u hebt opgegeven. Het is uw **AD\_APP\_naam**, dus zorg Noteer deze.

   ![App-naam][8]

7. Selecteer de naam van de app.

8. Zoeken naar **toepassings-ID** en noteer deze. Dit is de client-ID die wordt gebruikt als **CLIENT\_ID** voor uw API-aanroepen.

   ![De toepassings-ID vinden][10]

9. Zoek de **sleutels** sectie aan de rechterkant.

   ![Sectie van sleutels][11]

10. Maak een nieuwe sleutel en vervolgens onmiddellijk kopiëren. Dit wordt niet opnieuw weergegeven.

    ![Deelvenster met details van de sleutels][12]

    > [!IMPORTANT]
    > Deze sleutel verloopt aan het einde van de duur die u hebt opgegeven. Zorg ervoor dat het te vernieuwen wanneer dat nodig is, anders uw API-verificatie werkt niet meer. Als u denkt dat deze sleutel is ingebroken, kunt u deze kunt verwijderen en opnieuw maken.
    >
    
11. Selecteer de **eindpunten** knop aan de bovenkant van de pagina. Kopieer de **OAUTH 2.0-TOKENEINDPUNT**.

    ![Kopieer het eindpunt][14]

16. Dit eindpunt is in de volgende vorm, waarbij de GUID in de URL is uw **TENANT_ID**:`https://login.microsoftonline.com/<GUID>/oauth2/token`

17. Vervolgens configureert u de machtigingen op deze app. Om het te starten, gaat u naar de [Azure-portal](https://portal.azure.com).

18. Selecteer **resourcegroepen**, en gaat u naar de **MobileEngagement** resourcegroep.

    ![MobileEngagement vinden][15]

19. Selecteer de **MobileEngagement** resource groep en selecteert u vervolgens **alle instellingen**.

    ![Blader naar de instellingen voor MobileEngagement][16]

20. Selecteer **gebruikers** in de **instellingen** sectie. Selecteer vervolgens een gebruiker toevoegen, **toevoegen**.

    ![Een gebruiker toevoegen][17]

21. Klik op **Selecteer een rol**.

    ![Selecteer een rol][18]

22. Selecteer **eigenaar**.

    ![Eigenaar als de rol selecteren][19]

23. Zoeken naar de naam van uw toepassing **AD\_APP\_naam**, in het zoekvak. Deze naam is hier niet standaard. Nadat u deze hebt gevonden, selecteert u deze. Klik vervolgens op **Selecteer** aan de onderkant van de sectie.

    ![Selecteer de naam][20]

24. In de **toegang toevoegen** sectie, wordt deze weergegeven als **1 gebruiker, 0 groepen**. Selecteer om te bevestigen de wijziging, **OK**.

    ![Toegevoegde gebruiker bevestigen][21]

U hebt nu de vereiste configuratie van Azure AD en zijn ingesteld op het aanroepen van de API's.

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



