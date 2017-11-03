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
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Verificatie met de Mobile Engagement REST-API's - handmatige installatie
Dit is een bijlage-documentatie voor [verifiëren met Mobile Engagement REST-API's](mobile-engagement-api-authentication.md). Zorg ervoor dat u het eerst gebruikt om toegang te krijgen van de context lezen. Beschrijving van een andere methode voor het eenmalig instellen voor het instellen van uw verificatie voor de Mobile Engagement REST-API's met de Azure-Portal. 

> [!NOTE]
> De onderstaande instructies zijn gebaseerd op deze [Active Directory-handleiding](../azure-resource-manager/resource-group-create-service-principal-portal.md) en aangepast voor wat vereist voor verificatie voor Mobile Engagement-API's is. Dus hiernaar wordt verwezen als u wilt weten over de stappen hieronder in detail. 
> 
> 

1. Meld u aan bij uw Azure-Account via de [klassieke portal](https://manage.windowsazure.com/).
2. Selecteer **Active Directory** in het linkerdeelvenster.
   
     ![Selecteer Active Directory][1]
3. Kies de **Active standaarddirectory** in uw Azure-portal. 
   
     ![map te kiezen][2]
   
   > [!IMPORTANT]
   > Deze methode werkt alleen als u in de standaard Active Directory van uw account werkt en niet werken als u dit doen in een Active Directory die u hebt gemaakt in uw account. 
   > 
   > 
4. Als u wilt de toepassingen in uw directory bekijken, klikt u op **toepassingen**.
   
     ![toepassingen weergeven][3]
5. Klik op **toevoegen**. 
   
     ![Toepassing toevoegen][4]
6. Klik op **mijn organisatie ontwikkelt toepassing toevoegen**
   
     ![nieuwe toepassing][5]
7. Vul in de naam van de toepassing en selecteer het type van toepassing als **WEBTOEPASSING en/of WEB-API** en klik op de knop Volgende.
   
     ![de naam van toepassing][6]
8. U kunt opgeven dat alle dummy-URL's voor **AANMELDINGS-URL** en **APP ID URI**. Ze worden niet gebruikt voor ons scenario en de URL's zelf worden niet gevalideerd.  
   
     ![Toepassingseigenschappen][7]
9. Aan het einde van dit hebt u een AAD-app met de naam die u eerder hebt opgegeven als volgt. Dit is uw **AD\_APP\_naam** en noteer deze.  
   
     ![App-naam][8]
10. Klik op de naam van de app en klik op **configureren**.
    
      ![App configureren][9]
11. Maak een notitie van de CLIENT-ID die wordt gebruikt als **CLIENT\_ID** voor uw API-aanroepen. 
    
     ![App configureren][10]
12. Schuif omlaag naar de **sleutels** gedeelte en een sleutel met bij voorkeur 2 jaar (verlopen) duur toevoegen en klik op **opslaan**. 
    
     ![App configureren][11]
13. De waarde die wordt weergegeven voor de sleutel wordt nu alleen weergegeven en wordt niet opgeslagen zodat niet meer ooit weergegeven wordt onmiddellijk te kopiëren. Als u deze verliest hebt u een nieuwe sleutel te genereren. Dit is de **CLIENT_SECRET** voor uw API-aanroepen. 
    
     ![App configureren][12]
    
    > [!IMPORTANT]
    > Deze sleutel, verloopt aan het einde van de duur die u hebt opgegeven, dus zorg ervoor dat het te vernieuwen wanneer u anders de verificatie van uw API wordt niet meer werken. U kunt ook verwijderen en opnieuw maken van deze sleutel als u denkt dat deze is geschonden.
    > 
    > 
14. Klik op **EINDPUNTEN weergeven** knop nu die wordt geopend de **App eindpunten** in het dialoogvenster. 
    
    ![][13]
15. Kopieer in het dialoogvenster App eindpunten de **OAUTH 2.0-TOKENEINDPUNT**. 
    
    ![][14]
16. Dit eindpunt worden weergegeven in de volgende notatie waarbij de GUID in de URL uw **TENANT_ID** dus maak een notitie van deze: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. We gaat nu door met het configureren van de machtigingen op deze app. Voor deze heeft u opent u de [Azure-portal](https://portal.azure.com). 
18. Klik op **resourcegroepen** en zoek de **Mobile Engagement** resourcegroep.  
    
    ![][15]
19. Klik op de **Mobile Engagement** resource groeperen en navigeer naar de **instellingen** blade hier. 
    
    ![][16]
20. Klik op **gebruikers** in de blade instellingen en klik vervolgens op **toevoegen** een gebruiker toe te voegen. 
    
    ![][17]
21. Klik op **een rol selecteren**
    
    ![][18]
22. Klik op **eigenaar**
    
    ![][19]
23. Zoeken naar de naam van uw toepassing **AD\_APP\_naam** in het zoekvak. U ziet dit hier standaard. Als u deze hebt gevonden, selecteert u deze en klik op **Selecteer** onderaan de blade. 
    
    ![][20]
24. Op de **toegang toevoegen** blade, deze wordt weergegeven als **1 gebruiker, 0 groepen**. Klik op **OK** op deze blade de wijziging te bevestigen. 
    
    ![][21]

U hebt nu de vereiste AAD-configuratie voltooid en u worden alle aanroepen van de API's. 

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
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
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



