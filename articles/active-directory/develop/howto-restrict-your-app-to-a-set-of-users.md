---
title: Hoe u uw app in Azure Active Directory-geregistreerde beperken tot een groep gebruikers
description: Leer hoe u toegang tot uw apps die zijn geregistreerd bij Azure AD voor een geselecteerde groep gebruikers te beperken.
services: active-directory
documentationcenter: ''
author: kalyankrishna1
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.openlocfilehash: 575677cd3ace4d5d4948b1296d923b7703c3f20b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968302"
---
# <a name="how-to-restrict-your-app-to-a-set-of-users"></a>Hoe: uw app tot een groep gebruikers beperken

Toepassingen die zijn geregistreerd in een tenant Azure Active Directory (Azure AD) zijn standaard beschikbaar voor alle gebruikers van de tenant die worden geverifieerd.

Op deze manier in geval van een [multitenant](howto-convert-app-to-be-multi-tenant.md) app, alle gebruikers in de Azure AD-tenant waar deze app is ingericht zich toegang hebben tot deze toepassing zodra ze in hun respectieve tenant verifiëren.

Tenantbeheerders en ontwikkelaars hebben vaak vereisten waar een app moet worden beperkt tot een bepaalde groep gebruikers. Ontwikkelaars hetzelfde kunnen uitvoeren met behulp van populaire autorisatie patronen, zoals rollen gebaseerd toegangsbeheer (RBAC), maar deze aanpak vereist een aanzienlijke hoeveelheid werk aan een deel van de ontwikkelaar.

Azure AD kan tenant-beheerders en ontwikkelaars een app beperken tot een specifieke set gebruikers of beveiligingsgroepen in de tenant.

## <a name="supported-app-configurations"></a>Ondersteunde app-configuraties

De optie voor een app beperken tot een specifieke set gebruikers of beveiligingsgroepen in een tenant werkt met de volgende typen toepassingen:

- Toepassingen die zijn geconfigureerd voor federatieve eenmalige aanmelding met verificatie op basis van SAML
- Application proxy toepassingen die gebruikmaken van Azure AD-vooraf-verificatie
- Toepassingen die zijn ontwikkeld rechtstreeks op het Azure AD-platform voor toepassingen die gebruikmaken van OAuth 2.0/OpenID verbinding maken met verificatie nadat een gebruiker of beheerder heeft ingestemd met de toepassing.

     > [!NOTE]
     > Deze functie is beschikbaar voor web-app/web-API en zakelijke toepassingen alleen. Apps die zijn geregistreerd als [systeemeigen](quickstart-v1-integrate-apps-with-azure-ad.md) kan niet worden beperkt tot een set van gebruikers of beveiligingsgroepen in de tenant.

## <a name="update-the-app-to-enable-user-assignment"></a>De app om in te schakelen Gebruikerstoewijzing bijwerken

1. Ga naar de [ **Azure-portal** ](https://portal.azure.com/) en aanmelden als een **globale beheerder.**
1. Selecteer op de bovenste balk het aangemelde account. 
1. Onder **Directory**, selecteert u de Azure AD-tenant waar de app worden geregistreerd.
1. Selecteer in de navigatiebalk aan de linkerkant, **Azure Active Directory**. Als Azure Active Directory niet beschikbaar in het navigatiedeelvenster is, voert u deze stappen:

    1. Selecteer **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.
    1. Typ in **Azure Active Directory** in het zoekvak van filter en selecteer vervolgens de **Azure Active Directory** item van het resultaat.

1. In de **Azure Active Directory** venster **bedrijfstoepassingen** uit de **Azure Active Directory** navigatiemenu links.
1. Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

     Als u de toepassing die u wilt dat hier weergegeven niet ziet, gebruikt u de verschillende filters aan de bovenkant van de **alle toepassingen** lijst om te beperken van de lijst of blader naar uw toepassing in de lijst.

1. Selecteer de toepassing die u wilt toewijzen van een gebruiker of beveiligingsgroep op in de lijst.
1. In de toepassingsmap **overzicht** weergeeft, schakelt **eigenschappen** in het navigatiemenu aan de van de toepassing.
1. Zoek de instelling **Gebruikerstoewijzing vereist?** en stel deze in op **Ja**. Wanneer deze optie is ingesteld op **Ja**, en vervolgens gebruikers moeten eerst worden toegewezen aan deze toepassing voordat u de mogelijkheid om deze te openen.
1. Selecteer **opslaan** deze configuratiewijziging op te slaan.

## <a name="assign-users-and-groups-to-the-app"></a>Gebruikers en groepen toewijzen aan de app.

Wanneer u uw app zodat de gebruiker is toegewezen hebt geconfigureerd, kunt u direct verder gaan en gebruikers en groepen toewijzen aan de app.

1. Selecteer de **gebruikers en groepen** deelvenster in het navigatiemenu aan de van de toepassing.
1. Aan de bovenkant van de **gebruikers en groepen** in de lijst met de **gebruiker toevoegen** te openen de **toevoegen toewijzing** deelvenster.
1. Selecteer de **gebruikers** selector vanaf de **toevoegen toewijzing** deelvenster. 

     Een lijst met gebruikers en beveiligingsgroepen worden weergegeven, samen met een tekstvak Zoeken naar en zoeken naar een bepaalde gebruiker of groep. Dit scherm kunt u meerdere gebruikers en groepen selecteren in een go.

1. Wanneer u klaar bent de gebruikers en groepen selecteren, drukt u op de **Selecteer** knop onderaan om te verplaatsen naar het volgende gedeelte.
1. Druk op de **toewijzen** knop onder aan het voltooien van de toewijzingen van gebruikers en groepen naar de app. 
1. Bevestig dat de gebruikers en groepen die u hebt toegevoegd worden weergegeven in de bijgewerkte **gebruikers en groepen** lijst.

