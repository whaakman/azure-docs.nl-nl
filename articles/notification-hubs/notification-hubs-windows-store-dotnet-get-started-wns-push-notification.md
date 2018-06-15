---
title: Meldingen verzenden naar Universal Windows Platform-apps met behulp van Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u Azure Notification Hubs gebruikt om meldingen naar een Windows Universal Platform-toepassing te pushen.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: c3bb170800508d5a546573850f445b2a8991ea8c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777059"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-by-using-azure-notification-hubs"></a>Zelfstudie: Meldingen verzenden naar Universal Windows Platform-apps met behulp van Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In deze zelfstudie maakt u een Notification Hub om pushmeldingen te verzenden naar een UWP-app (Universal Windows Platform). U maakt een lege Windows Store-app die pushmeldingen ontvangt via Windows Push Notification Service (WNS). Vervolgens kunt u uw Notification Hub gebruiken om pushmeldingen te verzenden naar alle apparaten waarop uw app wordt uitgevoerd.

> [!NOTE]
> De volledige code voor deze zelfstudie vindt u op [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een app maken in Windows Store
> * Een Notification Hub maken
> * Een voorbeeld-app voor Windows maken
> * Testmeldingen verzenden


## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
- [Microsoft Visual Studio Community 2015 of hoger](https://www.visualstudio.com/products/visual-studio-community-vs).
- [Ontwikkelhulpprogramma's voor UWP-apps geïnstalleerd](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Een actief Windows Store-account

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor UWP-apps.

## <a name="create-an-app-in-windows-store"></a>Een app maken in Windows Store
Als u pushmeldingen naar UWP-apps wilt verzenden, koppelt u uw app aan de Windows Store. Vervolgens configureert u de Notification Hub voor integratie met WNS.

1. Navigeer naar het [Windows-ontwikkelaarscentrum](https://dev.windows.com/overview), meld u aan met uw Microsoft-account en selecteer vervolgens **Een nieuwe app maken**.

    ![Knop Nieuwe app](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
1. Typ een naam voor uw app en selecteer **Productnaam reserveren**. Hiermee maakt u een nieuwe Windows Store-registratie voor uw app.

    ![Naam van de Store-app](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
1. Vouw **Appbeheer** uit, selecteer **WNS/MPNS**, selecteer **WNS/MPNS** en selecteer vervolgens **Live Services site**. Meld u aan bij uw Microsoft-account. De **portal voor app-registratie** wordt in een nieuw tabblad geopend. U kunt ook rechtstreeks naar de [portal voor app-registratie](http://apps.dev.microsoft.com) gaan en daar de naam van uw app selecteren om deze pagina te openen.

    ![WNS MPNS-pagina](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
1.   Noteer het wachtwoord voor **Toepassingsgeheim** en de **beveiligings-id (SID) van uw pakket**.

        >[!WARNING]
        >Het toepassingsgeheim en de pakket-SID zijn belangrijke beveiligingsreferenties. Deel deze waarden met niemand en distribueer ze niet met uw app.

## <a name="create-a-notification-hub"></a>Een Notification Hub maken
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


### <a name="configure-wns-settings-for-the-hub"></a>WNS-instellingen voor de hub configureren

1. Selecteer **Windows (WNS)** in de categorie **MELDINGSINSTELLINGEN**. 
2. Voer waarden in voor **pakket-SID** en **Beveiligingssleutel** die u in de vorige sectie hebt genoteerd. 
3. Selecteer **Opslaan** op de werkbalk.

    ![De vakken Pakket-SID en Beveiligingssleutel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

De Notification Hub is nu geconfigureerd om met WNS te kunnen werken. U hebt de verbindingsreeksen om uw app te registreren en meldingen te verzenden.

## <a name="create-a-sample-windows-app"></a>Een voorbeeld-app voor Windows maken
1. Selecteer in Visual Studio **Bestand**, wijs **Nieuw** aan en selecteer **Project**. 
2. Voer in het dialoogvenster **Nieuw project** de volgende stappen uit: 

    1. Vouw **Visual C#** uit.
    2. Selecteer **Windows Universal**. 
    3. Selecteer **Lege app (Universal Windows)**. 
    4. Voer een **naam** in voor het project. 
    5. Selecteer **OK**. 

        ![Het dialoogvenster Nieuw project](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)
1. Accepteer de standaardwaarden voor het **doel** en de **minimale** platformversies en selecteer **OK**. 
2. Klik in Solution Explorer met de rechtermuisknop op het Windows Store-app-project, selecteer **Store** en klik vervolgens op **App aan de Store koppelen**. Hierop wordt de wizard **Uw app koppelen aan Windows Store** weergegeven.
3. Meld u in de wizard aan met uw Microsoft-account.
4. Selecteer de app die u in stap 2 hebt geregistreerd, selecteer **Volgende** en selecteer vervolgens **koppelen**. Hierdoor worden de vereiste registratiegegevens voor Windows Store toegevoegd aan het toepassingsmanifest.
5. Klik met de rechtermuisknop op de oplossing in Visual Studio en selecteer **Manage NuGet Packages**. Het venster **Manage NuGet Packages** wordt geopend.
6. Voer in het zoekvak **WindowsAzure.Messaging.Managed** in, selecteer **Install** en accepteer de gebruiksvoorwaarden.
   
    ![Het venster NuGet-pakketten beheren][20]
   
    Met deze actie wordt een verwijzing gedownload, geïnstalleerd en toegevoegd. Deze verwijst naar de Azure Notification Hubs-bibliotheek voor Windows door gebruik te maken van het [Microsoft.Azure.NotificationHubs NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs).

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

6. Selecteer de toets **F5** als u de app wilt uitvoeren. Er wordt een dialoogvenster met de registratiesleutel weergegeven. Selecteer **OK** om het dialoogvenster te sluiten. 

    ![Registratie voltooid](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

Uw app is nu gereed om pop-upmeldingen te ontvangen.

## <a name="send-test-notifications"></a>Testmeldingen verzenden
U kunt het ontvangen van meldingen in de app snel testen door meldingen te verzenden in [Azure Portal](https://portal.azure.com/). 

1. Ga in de Azure Portal naar het tabblad Overzicht en selecteer **Test verzenden** op de werkbalk.     

    ![Knop Verzenden testen](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. Voer in het venster **Test verzenden** de volgende acties uit: 
    1. Selecteer **Windows** bij **Platforms**.
    2. Selecteer **Pop-up** bij **Meldingstype**. 
    3. Selecteer **Verzenden**. 
    
        ![Het venster Verzenden testen](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Bekijk het resultaat van de verzendbewerking in de lijst **Resultaat** onder in het venster. U ziet ook een waarschuwingsbericht. 

    ![Resultaat van de verzendbewerking](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
1. U ziet de melding **Testbericht** op het bureaublad. 

    ![Melding](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u meldingen verzonden naar al uw Windows-apparaten via de portal of een console-app. Ga verder met de volgende zelfstudie als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke apparaten: 

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke apparaten](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)


<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 
