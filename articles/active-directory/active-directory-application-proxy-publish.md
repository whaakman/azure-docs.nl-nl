---
title: Apps publiceren met een Azure AD-toepassingsproxy | Microsoft Docs
description: Publiceer on-premises toepassingen naar de cloud met Azure AD-toepassingsproxy in de klassieke portal.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 96490c0d060fe5486a7235a5aa76380c8d9b5d4f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Toepassingen publiceren met Azure AD-toepassingsproxy

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [klassieke Azure-portal](active-directory-application-proxy-publish.md)

Azure AD-toepassingsproxy helpt u externe werknemers te ondersteunen door on-premises toepassingen te publiceren voor toegang via internet. Inmiddels moet u [toepassingsproxy al hebben ingeschakeld in de klassieke Azure-portal](active-directory-application-proxy-enable.md). Dit artikel beschrijft de stappen voor het publiceren van toepassingen die worden uitgevoerd op uw lokale netwerk en het bieden van beveiligde externe toegang van buiten uw netwerk. Nadat u de stappen in dit artikel hebt uitgevoerd, bent u klaar om de toepassing te configureren met persoonlijke gegevens of beveiligingsvereisten.

> [!NOTE]
> De functie toepassingsproxy is alleen beschikbaar als u een upgrade hebt uitgevoerd naar de Premium- of Basic-editie van Azure Active Directory. Zie [Azure Active Directory-edities](active-directory-editions.md) voor meer informatie. Als u de toepassingsproxy wilt gebruiken, kunt u [toepassingen publiceren in Azure Portal](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-the-wizard"></a>Een app publiceren met de wizard
1. Meld u als beheerder aan in de [klassieke Azure-portal](https://manage.windowsazure.com/).
2. Ga naar Active Directory en selecteer de directory waarin u toepassingsproxy hebt ingeschakeld.
   
    ![Active Directory - pictogram](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Klik op de tab **Toepassingen** en klik vervolgens op de knop **Toevoegen** onder aan het scherm
   
    ![Toepassing toevoegen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Selecteer **Een toepassing publiceren die toegankelijk is van buiten uw netwerk**.
   
    ![Een toepassing publiceren die toegankelijk is van buiten uw netwerk](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Geef de volgende informatie over de toepassing op:
   
   * **Naam**: de beschrijvende naam van uw toepassing. Deze naam moet uniek zijn in uw directory.
   * **Interne URL**: het adres dat de connector voor de toepassingsproxy gebruikt om toegang tot de toepassing te verkrijgen vanuit uw particuliere netwerk. U kunt voor het publiceren een specifiek pad opgeven op de back-endserver, terwijl de rest van de server ongepubliceerd blijft. Op deze manier kunt u verschillende sites op dezelfde server publiceren en elke server een eigen naam en toegangsregels geven.
     
     > [!TIP]
     > Als u een pad publiceert, moet u ervoor zorgen dat dit alle benodigde installatiekopieën, scripts en opmaakmodellen voor uw toepassing bevat. Als uw app zich bijvoorbeeld bevindt op het pad https://uwapp/app en gebruikmaakt van installatiekopieën op https://uwapp/media, moet u https://uwapp/ publiceren als het pad.
     > 
     > 
   * **Methode met verificatie vooraf**: de manier waarop gebruikers door de toepassingsproxy worden geverifieerd voordat ze toegang krijgen tot uw toepassing. Kies een van de opties in de vervolgkeuzelijst.
     
     * Azure Active Directory: de gebruikers worden omgeleid met de toepassingsproxy zodat zij zich kunnen aanmelden met Azure AD. Hierbij worden hun machtigingen geverifieerd voor de directory en de toepassing.
     * Passthrough: gebruikers hoeven geen verificatie te doorlopen om toegang te krijgen tot de toepassing.
     
     ![Toepassingseigenschappen](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. Schakel het selectievakje onder aan het scherm in om de wizard te voltooien. De toepassing is nu gedefinieerd in Azure AD.

## <a name="assign-users-and-groups-to-the-application"></a>Gebruikers en groepen toewijzen aan de toepassing
U dient uw gebruikers individueel of in groepen toe te wijzen, zodat zij toegang krijgen tot uw gepubliceerde toepassing. (Vergeet niet om ook aan uzelf toegang te verlenen.) Elke gebruiker die u toewijst, moet beschikken over een licentie voor Azure Basic of hoger. U kunt licenties afzonderlijk of aan groepen toewijzen. Zie [Gebruikers toewijzen aan een toepassing](active-directory-applications-guiding-developers-assigning-users.md) voor meer informatie. 

Voor apps waarbij verificatie vooraf is vereist, wordt bij het toewijzen van een gebruiker een machtiging verleend voor het gebruik van de app. Voor apps waarbij geen verificatie vooraf is vereist, kan de gebruiker na het toewijzen de toepassing openen via het toegangsvenster.

1. Nadat u de wizard App toevoegen hebt voltooid, wordt de pagina Snel starten weergegeven voor uw toepassing. Selecteer **Gebruikers en groepen** om de toegang tot de app te beheren.
   
    ![Toepassingsproxy - gebruikers toewijzen via Snel starten - schermafbeelding](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Zoek naar specifieke groepen in uw directory of geef alle gebruikers weer. Klik op het vinkje om de zoekresultaten weer te geven.
   
      ![Zoeken naar groepen of gebruikers - schermafbeelding](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Selecteer elke gebruiker aan wie of groep waaraan u deze app wilt toewijzen en klik op **Toewijzen**. U wordt gevraagd deze actie te bevestigen.

> [!NOTE]
> Voor apps met geïntegreerde Windows-verificatie kunt u alleen gebruikers en groepen toewijzen die zijn gesynchroniseerd vanuit uw on-premises Active Directory. Gebruikers die zich aanmelden met een Microsoft-account en gasten kunnen niet worden toegewezen aan apps die zijn gepubliceerd met Azure Active Directory-toepassingsproxy. Zorg ervoor dat gebruikers zich aanmelden met de referenties die horen bij hetzelfde domein als de app die u publiceert.
> 
> 

## <a name="test-your-published-application"></a>Uw gepubliceerde toepassing testen
Nadat u uw toepassing hebt gepubliceerd, kunt u deze testen door te navigeren naar de URL die u hebt gepubliceerd. Zorg ervoor dat u toegang toe hebt tot uw toepassing, dat deze correct wordt weergegeven en dat alles werkt zoals verwacht. Als problemen optreden of als een foutbericht wordt weergegeven, raadpleegt u de [gids voor het oplossen van problemen](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Uw toepassing configureren
Op de pagina Configureren kunt u gepubliceerde apps aanpassen of geavanceerde opties instellen. Op deze pagina kunt u uw app aanpassen door de naam te wijzigen of een logo te uploaden. Ook kunt u toegangsregels beheren zoals de methode voor verificatie vooraf of meervoudige verificatie.

![Geavanceerde configuratie](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Nadat u toepassingen hebt gepubliceerd met Azure Active Directory-toepassingsproxy, worden ze weergegeven in de lijst Toepassingen in Azure AD en kunt u ze daar beheren.

Als u de services voor toepassingsproxy uitschakelt nadat u toepassingen hebt gepubliceerd, zijn de toepassingen niet langer toegankelijk van buiten uw particuliere netwerk. Uw gebruikers kunnen de toepassingen nog wel gewoon on-premises openen.

Als u een toepassing wilt bekijken en controleren of deze toegankelijk is, dubbelklikt u op de naam van de toepassing. Als de service voor toepassingsproxy is uitgeschakeld en de toepassing niet beschikbaar is, wordt er boven aan het scherm een waarschuwing weergegeven.

Wilt u een toepassing verwijderen, dan selecteert u de toepassing in de lijst en klikt u op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen
* [Toepassingen publiceren met uw eigen domeinnaam](active-directory-application-proxy-custom-domains.md)
* [Eenmalige aanmelding inschakelen](active-directory-application-proxy-sso-using-kcd.md)
* [Voorwaardelijke toegang inschakelen](active-directory-application-proxy-conditional-access.md)
* [Werken met claim-compatibele toepassingen](active-directory-application-proxy-claims-aware-apps.md)

Ga naar het [blog over toepassingsproxy](http://blogs.technet.com/b/applicationproxyblog/) voor nieuws en updates.

