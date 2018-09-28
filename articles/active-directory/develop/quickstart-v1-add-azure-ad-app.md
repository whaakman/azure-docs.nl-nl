---
title: Een app registreren met het Azure Active Directory v1.0-eindpunt
description: Leer hoe u een toepassing met het Azure Active Directory v 1.0-eindpunt (Azure AD) kunt toevoegen en registreren.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 7e2225a173562be28840cb0a66913b9f8c9657ff
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962042"
---
# <a name="quickstart-register-an-app-with-the-azure-active-directory-v10-endpoint"></a>Snelstart: Een app registreren bij het Azure Active Directory v1.0-eindpunt

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Enterprise-ontwikkelaars en providers van software-as-a-service (SaaS) kunnen commerciële cloudservices of bedrijfstoepassingen ontwikkelen die kunnen worden geïntegreerd met Azure Active Directory (Azure AD) om beveiligde aanmelding en autorisatie te bieden voor hun services. Om een toepassing of service met Azure Active Directory te integreren, moet een ontwikkelaar de toepassing eerst bij Azure Active Directory registreren.

Elke toepassing die de mogelijkheden van Azure Active Directory wil gebruiken, moet eerst in een Azure Active Directory-tenant worden geregistreerd. Dit registratieproces bestaat uit het verstrekken van details over uw toepassing in Azure Active Directory, zoals de URL waar de toepassing zich bevindt, de URL waar reacties naar toe moeten worden verzonden nadat een gebruiker is geverifieerd, de URI die de app identificeert, enzovoort.

In deze snelstartgids leert u hoe u een toepassing kunt registreren in Azure Active Directory.

## <a name="prerequisites"></a>Vereisten

Zorg om te beginnen dat u een Azure Active Directory-tenant hebt die u kunt gebruiken om uw apps te registreren. [Lees hier hoe u een tenant kunt verkrijgen](quickstart-create-new-tenant.md) als u er nog geen hebt.

## <a name="register-a-new-application-using-the-azure-portal"></a>Een nieuwe toepassing registreren via de Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
1. Selecteer in het navigatiedeelvenster aan de linkerkant de **Azure Active Directory**-service.
1. Selecteer **App-registraties** en selecteer daarna **Nieuwe toepassing registreren**.

    ![Een nieuwe toepassing registreren](./media/quickstart-v1-integrate-apps-with-azure-ad/add-app-registration.png)

1. Voer op de pagina **Maken** die wordt weergegeven de registratiegegevens voor uw toepassing in: 

    - **Naam:** voer een nuttige naam in voor de toepassing
    - **Toepassingstype:**
      - Selecteer **Systeemeigen** voor [clienttoepassingen](developer-glossary.md#client-application) die lokaal op een apparaat zijn geïnstalleerd. Deze instelling wordt gebruikt voor openbare [systeemeigen clients](developer-glossary.md#native-client) met OAuth.
      - Selecteer **Web-app/API** voor [Clienttoepassingen](developer-glossary.md#client-application) en [Resource/API-toepassingen](developer-glossary.md#resource-server) die op een beveiligde server zijn geïnstalleerd. Deze instelling wordt gebruikt voor vertrouwelijke [webclients](developer-glossary.md#web-client) en openbare [clients die op gebruikersagents zijn gebaseerd](developer-glossary.md#user-agent-based-client) die OAuth gebruiken. Dezelfde toepassing kan zowel een client als resource/API beschikbaar maken.
    - **Aanmeldings-URL:** geef voor 'Web-app/API'-toepassingen de basis-URL van uw app op. `http://localhost:31544` kan bijvoorbeeld de URL zijn van een web-app die op uw lokale machine wordt uitgevoerd. Gebruikers moeten deze URL gebruiken om zich bij een webclienttoepassing aan te melden. 
    - **Omleidings-URI**: geef voor systeemeigen toepassingen de URI op die door Azure Active Directory wordt gebruikt om tokenantwoorden te retourneren. Voer een waarde in die specifiek is voor uw toepassing, bijvoorbeeld `http://MyFirstAADApp`

      ![Een nieuwe toepassing registreren - maken](./media/quickstart-v1-integrate-apps-with-azure-ad/add-app-registration-create.png)

    Raadpleeg de **snelstart** in onze documentatie als u specifieke voorbeelden wilt voor webtoepassingen of systeemeigen toepassingen.

1. Selecteer **Maken** wanneer u klaar bent.

    Azure Active Directory wijst een unieke toepassing-ID toe aan uw toepassing, waarna u naar de hoofdpagina voor registratie van uw toepassing wordt doorgestuurd. Afhankelijk van of uw toepassing een webtoepassing of systeemeigen toepassing is, worden er verschillende opties geboden om extra mogelijkheden aan uw toepassing toe te voegen.

      > [!NOTE]
      > Standaard wordt een nieuw geregistreerde webtoepassing geconfigureerd om **alleen** gebruikers van dezelfde tenant toe te staan om zich bij uw toepassing aan te melden.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure AD-toestemmingsframework](consent-framework.md) voor een overzicht van toestemmingen.
- Raadpleeg [Een toepassing bijwerken in Azure Active Directory](quickstart-v1-update-azure-ad-app.md) om extra configuratiefuncties in de registratie van uw toepassing in te schakelen, zoals referenties, machtigingen en aanmelden toestaan voor gebruikers van andere tenants.
- Raadpleeg [Toepassingsobjecten en service-principal-objecten](app-objects-and-service-principals.md) voor meer informatie over de twee Azure Active Directory-objecten die een geregistreerde toepassing vertegenwoordigen en de relatie ertussen.
- Raadpleeg [Huisstijlrichtlijnen voor apps](howto-add-branding-in-azure-ad-apps.md) voor meer informatie over de huisstijlrichtlijnen die u moet gebruiken bij het ontwikkelen van toepassingen met Azure Active Directory.
