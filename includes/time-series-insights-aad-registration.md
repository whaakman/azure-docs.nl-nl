---
title: bestand opnemen
description: bestand opnemen
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 07/02/2019
ms.openlocfilehash: a463e3cf475909c34054717460dc10dbba4ad8f0
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543950"
---
> [!IMPORTANT]
> * De nieuwe **Azure Active Directory** > **App-registraties** blade vervangt de oude **Azure Active Directory** > **App registraties (verouderd)** blade mei 2019.
> * App-registraties die zijn gemaakt of wordt weergegeven in de verouderde blade wordt automatisch de nieuwe blade weergegeven.
> * Lees voor uitgebreide informatie over het migreren naar de nieuwe Azure-App-registratie-ervaring, de [Azure App-registraties training handleiding](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) en [Azure Active Directory-Quickstart](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. In de [Azure-portal](https://ms.portal.azure.com/), selecteer **Azure Active Directory** > **App-registraties** > **registratie van nieuwe**.

   [![Nieuwe toepassing registreren in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > Het nieuwe deelvenster van de Azure Active Directory-App-registratie kunt u de weergegeven apps filteren door te selecteren **eigendom toepassingen**.

    Uw app wordt hier weergegeven nadat u deze hebt geregistreerd.

1. Geef de toepassing een naam en selecteer **Accounts in deze organisatie-map alleen** om op te geven de **accounttypen ondersteund** die mogelijk toegang tot de API. Kies een geldige URI om te leiden van gebruikers nadat ze, klikt u vervolgens verifiëren **registreren**.

   [![De toepassing maken in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Belangrijke informatie voor Azure Active Directory-app wordt weergegeven in de vermelde app **overzicht** blade. Selecteer uw app bij **eigendom toepassingen**, klikt u vervolgens **overzicht**.

   [![Kopieer de toepassings-ID](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Kopieer uw **(client) toepassings-ID** te gebruiken in uw clienttoepassing.

1. De **verificatie** blade belangrijk verificatie configuratie-instellingen opgegeven. 

    1. **Omleidings-URI's** moet overeenkomen met het adres dat is opgegeven door de verificatieaanvraag:

        * Selecteer voor apps die worden gehost in een lokale ontwikkelingsomgeving, **openbare client (mobiele en desktop)** . Zorg ervoor dat u stelt de **standaard clienttype** op Ja.
        * Selecteer voor apps van één pagina die wordt gehost op Azure App Service, **Web**.

    1. De impliciete stroom inschakelen door het controleren van **ID-tokens**.

   [![Een nieuwe clientgeheim maken](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   Klik op **Opslaan**.

1. Koppel uw Azure Active Directory-app Azure TIme Series Insights. Selecteer **API-machtigingen** > **toevoegen van een machtiging** > **API's maakt gebruik van mijn organisatie**. 

    [![Een API koppelen aan uw app in Azure Active Directory](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Type `Azure Time Series Insights` in het zoekvak van de balk Selecteer `Azure Time Series Insights`.

1. Daarna geeft u het type API-machtigingen die uw app nodig heeft. Standaard **overgedragen machtigingen** worden gemarkeerd. Kies een machtigingstype vervolgens, selecteer **machtigingen toevoegen**.

    [![Geef het type van de API-machtigingen die uw app nodig heeft](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)