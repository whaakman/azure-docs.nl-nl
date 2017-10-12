---
title: Aan de slag met Azure Mobile Engagement voor universele Windows-apps
description: Informatie over het gebruik van Azure Mobile Engagement met analyses en pushmeldingen voor universele Windows-apps.
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 40db7e4dd151ec391c754dc6d4145aeeb8058eca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Aan de slag met Azure Mobile Engagement voor universele Windows-apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp leest u hoe u Azure Mobile Engagement gebruikt om inzicht te krijgen in het gebruik van uw apps, en om pushmeldingen te verzenden aan gesegmenteerde gebruikers van een universele Windows-toepassing.
Deze zelfstudie laat een eenvoudig broadcast-scenario met Mobile Engagement zien. In deze zelfstudie maakt u een lege universele Windows-app die basisgegevens verzamelt en pushmeldingen ontvangt via Windows Notification Service (WNS).

> [!NOTE]
> De Azure Mobile Engagement-service wordt in maart 2018 beëindigd en is momenteel alleen beschikbaar voor bestaande klanten. Zie [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>Mobile Engagement instellen voor uw universele Windows-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uw app verbinden met de back-end van Mobile Engagement
Deze zelfstudie toont een ‘basisintegratie’, de minimale set die vereist is voor het verzamelen van gegevens en verzenden van een pushmelding. De volledige integratiedocumentatie is te vinden in de [Mobile Engagement universele Windows SDK-integratie](mobile-engagement-windows-store-sdk-overview.md).

U maakt een eenvoudige app met Visual Studio ter illustratie van de integratie.

### <a name="create-a-windows-universal-app-project"></a>Een nieuw project voor een universele Windows-app maken
In de volgende stappen wordt ervan uitgegaan dat u Visual Studio 2015 gebruikt, hoewel de stappen hetzelfde zijn in eerdere versies van Visual Studio.

1. Start Visual Studio en selecteer **New Project** in het scherm **Home**.
2. Selecteer in het pop-upvenster **Windows** -> **Universal** -> **Blank App (Universal Windows)** (Universele lege Windows-app (Universeel Windows)). Vul de velden **Name** en **Solution name** in voor de app en klik vervolgens op **OK**.

    ![][1]

U hebt nu een universele Windows-app gemaakt waarin u vervolgens de Azure Mobile Engagement SDK integreert.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met de back-end van Mobile Engagement
1. Installeer het [MicrosoftAzure.MobileEngagement] Nuget-pakket in uw project. Als u ontwikkelt voor zowel het Windows- als het Windows Phone-platform, moet u dit voor beide projecten doen. Voor Windows 8.x en Windows Phone 8.1 plaatst hetzelfde NuGet-pakket de juiste platformspecifieke binaire bestanden in elk project.
2. Open **Package.appxmanifest** en zorg dat de volgende mogelijkheid is toegevoegd:

        Internet (Client)

    ![][2]
3. Plak nu de verbindingsreeks die u eerder hebt gekopieerd voor uw Mobile Engagement-app in het bestand `Resources\EngagementConfiguration.xml`, tussen de tags `<connectionString>` en `</connectionString>`:

    ![][3]

    > [!TIP]
    > Als u een app maakt voor zowel Windows als Windows Phone, moet u nog steeds twee Mobile Engagement-toepassingen maken, een voor elk ondersteund platform. Met twee apps zorgt u ervoor dat u de juiste segmentering van de doelgroep kunt maken en op de juiste wijze gerichte meldingen voor elk platform kunt verzenden.

    > [!IMPORTANT]
    > Met NuGet worden de SDK-resources in uw Windows 10 UWP-toepassing niet automatisch gekopieerd. U moet dit handmatig doen door de stappen te volgen die worden weergegeven (Leesmij.txt) wanneer het Nuget-pakket wordt geïnstalleerd.  

1. In het bestand `App.xaml.cs`:

    a. Voeg de instructie `using` toe:

            using Microsoft.Azure.Engagement;

    b. Voeg een methode toe die de overeenkomst initialiseert:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of the code
           }

    c. Initialiseer de SDK in de methode **OnLaunched**:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

    c. Voeg het volgende toe in de methode **OnActivated** en voeg de methode toe als die nog niet aanwezig is:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

## <a id="monitor"></a>Realtime-bewaking inschakelen
U dient ten minste één scherm (activiteit) naar de back-end van Mobile Engagement te sturen om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actief zijn.

1. Voeg in **MainPage.xaml.cs** de volgende instructie `using` toe:

    met behulp van Microsoft.Azure.Engagement.Overlay;
2. Wijzig de basisklasse van **MainPage** van **Page** (Pagina) in **EngagementPageOverlay**:

        class MainPage : EngagementPageOverlay
3. In het bestand `MainPage.xaml`:

    a. Toevoegen aan de naamruimtedeclaraties:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. Vervang de **Page** in de XML-tagnaam met **engagement:EngagementPageOverlay**.

> [!IMPORTANT]
> Als uw pagina de methode `OnNavigatedTo` negeert, zorg er dan voor dat u `base.OnNavigatedTo(e)` aanroept. Anders wordt de activiteit niet gemeld `EngagementPage`-aanroepen `StartActivity` binnen de methode `OnNavigatedTo`). Dit is vooral belangrijk in een Windows Phone-project waarbij de standaardsjabloon een methode `OnNavigatedTo` heeft.
>
> Voor **Windows 10 universele apps** gebruikt u in plaats van de bovenvermelde methode de methode die wordt aanbevolen in het gedeelte 'Recommended method: overload your Page classes' (Aanbevolen methode: overbelasting van uw paginaklassen) van [Advanced Reporting with the Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md) (Geavanceerde rapportage met de Windows Universal Apps Engagement SDK).

## <a id="monitor"></a>App verbinden met realtime-bewaking
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Pushmeldingen en in-app-berichten inschakelen
Met Mobile Engagement kunt u communiceren met uw gebruikers en ze bereiken met pushmeldingen en in-app-berichten in de context van campagnes. Deze module heet REACH in de Mobile Engagement-portal.
In de volgende secties stelt u de app in om die te ontvangen.

### <a name="enable-your-app-to-receive-wns-push-notifications"></a>Ontvangen van WNS-pushmeldingen inschakelen voor de app
1. In het bestand `Package.appxmanifest` stelt u op het tabblad **Application** onder **Notifications** de optie **Toast capable:** in op **Yes**.

    ![][5]

### <a name="initialize-the-reach-sdk"></a>Initialiseer de REACH-SDK.
Roep in `App.xaml.cs` **EngagementReach.Instance.Init(e);** aan in de functie **InitEngagement**, direct na initialisatie van de agent:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

U kunt nu een pop-upmelding verzenden. Vervolgens controleren we of u deze basisintegratie juist hebt uitgevoerd.

### <a name="grant-access-to-mobile-engagement-to-send-notifications"></a>Mobile Engagement toegang verlenen om meldingen te verzenden
1. Open het [ontwikkelaarscentrum voor Windows Store] in uw webbrowser, meld u aan, en maak indien nodig een account.
2. Klik op **Dashboard** in de rechterbovenhoek hoek en klik vervolgens op **Create a new app** in het menu aan de linkerkant.

    ![][9]
3. Maak uw app door de naam ervan te reserveren.

    ![][10]
4. Zodra de app is gemaakt, gaat u naar **Services -> Push notifications** in het menu links.

    ![][11]
5. Klik in het gedeelte met pushmeldingen op de koppeling **Live Services site** (Live Services-site).

    ![][12]
6. Navigeer naar het gedeelte voor pushreferenties. Zorg dat u zich in de sectie **App Settings** bevindt en kopieer uw **Package SID** en **Client secret**.

    ![][13]
7. Navigeer naar de **Instellingen** van uw Mobile Engagement-portal en klik op de sectie **Native pushbericht** aan de linkerkant. Klik vervolgens op de knop **Edit** (Bewerken) om uw **Package security identifier (SID)** (Beveiligings-id van pakket (SID)) en uw **Secret Key** (Geheime sleutel) in te voeren, zoals hier weergegeven:

    ![][6]
8. Controleer ten slotte of u de app in Visual Studio hebt gekoppeld aan de app die u in de App store hebt gemaakt. Klik in Visual Studio op **Associate App with Store** (App koppelen aan Store).

    ![][7]

## <a id="send"></a>Een melding verzenden naar uw app
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Als de app wordt uitgevoerd, ziet u een melding in de app. Als de app is gesloten, ziet u een pop-upmelding.
Als u wel een in-app-melding maar geen pop-upmelding ziet terwijl u de app uitvoert in de foutopsporingsmodus in Visual Studio, gebruikt u **Lifecycle events (Levenscyclusgebeurtenissen) -> Suspend (Onderbreken)** op de werkbalk om ervoor te zorgen dat de app wordt onderbroken. Als u op de knop Start hebt geklikt bij het opsporen van fouten in de toepassing in Visual Studio, wordt de app niet altijd onderbroken; daardoor wordt er geen pop-upmelding weergegeven maar wel een in-app-melding.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[ontwikkelaarscentrum voor Windows Store]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
