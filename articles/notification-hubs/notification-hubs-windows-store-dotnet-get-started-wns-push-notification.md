---
title: Aan de slag met Azure Notification Hubs voor Universeel Windows-platform-apps | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Notification Hubs gebruikt om meldingen naar een Windows Universal Platform-toepassing te pushen.
services: notification-hubs
documentationcenter: windows
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: c09621d1152aafbe15039130f6ca24082dc5bd21
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-notification-hubs-for-universal-windows-platform-apps"></a>Aan de slag met Notification Hubs voor Universeel Windows-platform-apps

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
In dit artikel leest u hoe u Azure Notification Hubs gebruikt om pushmeldingen te verzenden naar een Universal Windows Platform-app (UWP-app).

In dit artikel maakt u een lege Windows Store-app die pushmeldingen ontvangt via de Windows Push Notification Service (WNS). Als u klaar bent, kunt u de Notification Hub gebruiken om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd.

## <a name="before-you-begin"></a>Voordat u begint
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De volledige code voor deze zelfstudie vindt u op [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) of later
* [Ontwikkelhulpprogramma's voor UWP-apps geïnstalleerd](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Een actief Azure-account  
    Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F) voor meer informatie.
* Een actief Windows Store-account

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor UWP-apps.

## <a name="register-your-app-for-the-windows-store"></a>Uw app registreren voor Windows Store
Als u pushmeldingen naar UWP-apps wilt verzenden, koppelt u uw app aan de Windows Store. Vervolgens configureert u de Notification Hub voor integratie met WNS.

1. Als u uw app nog niet hebt geregistreerd, gaat u naar het [Windows-ontwikkelaarscentrum](https://dev.windows.com/overview), meldt u zich aan met uw Microsoft-account en selecteert u **Een nieuwe app maken**.

2. Typ een naam voor uw app en selecteer **App-naam reserveren**. Hiermee maakt u een nieuwe Windows Store-registratie voor uw app.

3. Maak in Visual Studio een nieuw project voor Visual C# Store-apps met de UWP-sjabloon **Blank App** en selecteer **OK**.

4. Accepteer de standaardwaarden voor het doel en de minimale platformversies.

5. Klik in Solution Explorer met de rechtermuisknop op het Windows Store-app-project, selecteer **Store** en klik vervolgens op **App aan de Store koppelen**.  
    Hierop wordt de wizard **Uw app koppelen aan Windows Store** weergegeven.

6. Meld u in de wizard aan met uw Microsoft-account.

7. Selecteer de app die u in stap 2 hebt geregistreerd, selecteer **Volgende** en selecteer vervolgens **koppelen**. Hierdoor worden de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.

8. Als u weer terug bent op de pagina [Windows-ontwikkelaarscentrum](http://dev.windows.com/overview) voor uw nieuwe app, selecteert u de optie **Services**, selecteer vervolgens **Pushmeldingen** en daarna **WNS/MPNS**.

9. Selecteer **Nieuwe melding**.

10. Selecteer de sjabloon **Leeg (pop-up)** en selecteer vervolgens **OK**.

11. Voer een **naam** voor de melding in en een visueel **contextbericht**. Selecteer vervolgens **Opslaan als concept**.

12. Ga naar de [portal voor app-registratie](http://apps.dev.microsoft.com) en meld u aan.

13. Selecteer de naam van uw toepassing. Noteer in de platforminstellingen van de **Windows Store** het wachtwoord voor het **Toepassingsgeheim** en de **Beveiligings-id (SID) pakket**.

    >[!WARNING]
    >Het toepassingsgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Deel deze waarden met niemand en distribueer ze niet met uw app.

## <a name="configure-your-notification-hub"></a>Uw Notification Hub configureren
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Onder <b>Notification Services</b> selecteert u <b>Windows (WNS)</b> en geeft u het wachtwoord voor het toepassingsgeheim op in het vak <b>Security Key</b>. Voer in het vak <b>Pakket-SID</b> de waarde in die u in het vorige gedeelte van WNS hebt gekregen en selecteer <b>Opslaan</b>.</p>
</li>
</ol>

![De vakken Pakket-SID en Beveiligingssleutel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

De Notification Hub is nu geconfigureerd om met WNS te kunnen werken. U hebt de verbindingsreeksen om uw app te registreren en meldingen te verzenden.

## <a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de Notification Hub
1. Klik met de rechtermuisknop op de oplossing in Visual Studio en selecteer **Manage NuGet Packages**.  
    Het venster **Manage NuGet Packages** wordt geopend.

2. Voer in het zoekvak **WindowsAzure.Messaging.Managed** in, selecteer **Install** en accepteer de gebruiksvoorwaarden.
   
    ![Het venster NuGet-pakketten beheren][20]
   
    Met deze actie wordt een verwijzing gedownload, geïnstalleerd en toegevoegd aan de Azure messaging-bibliotheek voor Windows met het [WindowsAzure.Messaging.Managed NuGet-pakket](http://nuget.org/packages/WindowsAzure.Messaging).

3. Open het projectbestand App.xaml.cs en voeg de volgende `using`-instructies toe: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. Voeg in App.xaml.cs ook de volgende **InitNotificationsAsync**-methodedefinitie toe aan de klasse **App**:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Met deze code wordt de kanaal-URI voor de app opgehaald uit WNS en wordt vervolgens die kanaal-URI voor uw Notification Hub geregistreerd.
   
    >[!NOTE]
    >* Vervang de tijdelijke aanduiding voor uw **hubnaam** door de naam van de Notification Hub, zoals die wordt weergegeven in Azure Portal. 
    >* Vervang ook de tijdelijke plaatsaanduiding door de verbindingstekenreeks **DefaultListenSharedAccessSignature** die u in een eerder gedeelte hebt gekregen op de pagina **Toegangsbeleid** van uw Notification Hub.
   > 
   > 
5. Aan de bovenkant van de gebeurtenis-handler **OnLaunched** in App.xaml.cs voegt u de volgende aanroep toe aan de nieuwe **InitNotificationsAsync**-methode:
   
        InitNotificationsAsync();
   
    Met deze actie wordt gegarandeerd dat de kanaal-URI in uw Notification Hub wordt geregistreerd telkens wanneer de toepassing wordt gestart.

6. Selecteer de toets **F5** als u de app wilt uitvoeren. Er wordt een dialoogvenster met de registratiesleutel weergegeven.

Uw app is nu gereed om pop-upmeldingen te ontvangen.

## <a name="send-notifications"></a>Meldingen verzenden
U kunt het ontvangen van meldingen in de app snel testen door meldingen te verzenden in [Azure Portal](https://portal.azure.com/). Gebruik de knop **Verzenden testen** op de Notification Hub, zoals weergegeven in de volgende afbeelding:

![Het venster Verzenden testen](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Pushmeldingen worden gewoonlijk verzonden in een back-endservice als Mobile Services of ASP.NET, met behulp van een compatibele bibliotheek. U kunt de REST API ook direct gebruiken om meldingsberichten te verzenden als er geen bibliotheek beschikbaar is voor uw back-end. 

In deze zelfstudie wordt gedemonstreerd hoe u uw client-app kunt testen door meldingen te verzenden met de .NET SDK voor Notification Hubs in een consoletoepassing in plaats van een back-endservice. U kunt het beste de zelfstudie [Use Notification Hubs to push notifications to users] (Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden) doornemen voor informatie over het verzenden van meldingen vanuit een ASP.NET-back-end. U kunt echter ook meldingen verzenden met behulp van de volgende methoden:

* **REST-interface**: u kunt meldingen op elk back-endplatform ondersteunen met de [REST-interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure Notification Hubs .NET SDK**: in NuGet Package Manager voor Visual Studio voert u [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) uit.

* **Node.js**: zie [How to use Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md) (Notification Hubs gebruiken vanuit Node.js) voor meer informatie.
* **Azure Mobile Apps**: zie [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) (Pushmeldingen toevoegen voor mobiele apps) voor een voorbeeld van hoe u meldingen verzendt vanuit een Azure Mobile App die is geïntegreerd met Notification Hubs.

* **Java of PHP**: voor voorbeelden van het verzenden van meldingen met de REST API's, raadpleegt u:
    * [Java](notification-hubs-java-push-notification-tutorial.md)
    * [PHP](notification-hubs-php-push-notification-tutorial.md)

## <a name="next-steps"></a>Volgende stappen
In dit eenvoudige voorbeeld hebt u meldingen uitgezonden naar al uw Windows-apparaten via de portal of een console-app. Voor de volgende stap kunt u het beste de zelfstudie [Use Notification Hubs to push notifications to users] (Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden) doornemen. Hierin ziet u hoe u meldingen van een ASP.NET-back-end verzendt met tags voor specifieke gebruikers.

Zie [Notification Hubs gebruiken om belangrijk nieuws te verzenden] als u gebruikers wilt indelen op belangengroepen. 

Zie [Notification Hubs guidance](notification-hubs-push-notification-overview.md) (Richtlijnen voor Notification Hubs) voor meer algemene informatie over Notification Hubs.

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 
