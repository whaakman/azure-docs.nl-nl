---
title: Azure Notification Hubs gebruiken om belangrijk nieuws (Universal Windows Platform) te verzenden
description: Azure Notification Hubs met labels in de registratie gebruiken voor belangrijk nieuws te verzenden naar een Universal Windows Platform-app.
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d510e7e665adec9607aeee80802c466b363d5d5b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Notification Hubs gebruiken om belangrijk nieuws te verzenden
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest u het gebruik van Azure Notification Hubs voor belangrijk nieuws meldingen naar een Windows Store of Windows Phone 8.1 (zonder Silverlight) app-broadcast. Als u voor Windows Phone 8.1 Silverlight ontwikkelt, raadpleegt u de [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) versie. 

Nadat u deze procedure hebt voltooid, kunt u registreren voor de nieuwste nieuwscategorieën waarin u geïnteresseerd bent en u ontvangt pushmeldingen voor deze categorieën alleen. Dit scenario is gebruikelijk voor veel apps (bijvoorbeeld RSS-lezers of apps voor muziek ventilatoren) waar meldingen voor gebruikers die zijn gedeclareerd interesse in deze groepen moeten worden verzonden. 

U kunt broadcast-scenario's inschakelen door een of meer *labels* wanneer u een registratie maakt in de notification hub. Wanneer meldingen worden verzonden naar een label, ontvangen alle apparaten die zijn geregistreerd voor het label de melding. Omdat tags gewoon tekenreeksen zijn, hoeven niet vooraf worden ingesteld. Zie voor meer informatie over tags [Notification Hubs Routering en code-expressies](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store en Windows Phone-projectversies 8.1 en lager worden niet ondersteund in Visual Studio 2017. Zie [Geschikte platforms voor Visual Studio 2017 en compatibiliteit](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) voor meer informatie. 

## <a name="prerequisites"></a>Vereisten
In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met Notification Hubs][get-started]. Voordat u deze zelfstudie begint, moet u [aan de slag met Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.
De eerste stap is het toevoegen van UI-elementen naar uw bestaande hoofdpagina zodat gebruikers kunnen selecteren categorieën om u te registreren. De geselecteerde categorieën worden opgeslagen op het apparaat. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in uw notification hub, met de geselecteerde categorieën als labels gemaakt.

1. Open het projectbestand MainPage.xaml en kopieer de volgende code in de **raster** element:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>

2. Met de rechtermuisknop op de **gedeelde** project, Voeg een nieuwe klasse met de naam **meldingen**, voeg de **openbare** aanpassingsfunctie naar de klassendefinitie en voeg vervolgens de volgende **met** instructies op het nieuwe codebestand:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopieer de volgende code naar het nieuwe **meldingen** klasse:
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Deze klasse maakt gebruik van de lokale opslag voor het opslaan van de categorieën van nieuws die dit apparaat moet ontvangen. In plaats van het aanroepen van de *RegisterNativeAsync* methode, belt *RegisterTemplateAsync* registreren voor de categorieën met behulp van de registratie van een sjabloon. 
   
    Omdat u meer dan één sjabloon (bijvoorbeeld één voor de pop-upmeldingen) en één voor tegels registreren wilt mogelijk, bieden ook een sjabloonnaam (bijvoorbeeld ' simpleWNSTemplateExample'). U kunt de sjablonen naam zodat u kunt bijwerken of verwijderen.
   
    >[!NOTE]
    >Als een apparaat meerdere sjablonen met dezelfde tag registreert, wordt een binnenkomend bericht die gericht is op de tag meerdere meldingen moeten worden geleverd aan het apparaat (één voor elke sjabloon). Dit gedrag is handig wanneer hetzelfde logische bericht moet resulteren in meerdere visual meldingen (bijvoorbeeld zowel een badge en een pop-up wordt weergegeven in een Windows Store-toepassing).
   
    Zie voor meer informatie [sjablonen](notification-hubs-templates-cross-platform-push-messages.md).

4. In het projectbestand App.xaml.cs de volgende eigenschap toevoegen aan de **App** klasse:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    U kunt deze eigenschap maken en toegang tot een **meldingen** exemplaar.
   
    Vervang in de code de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen door de naam van uw notification hub en de verbindingsreeks voor *DefaultListenSharedAccessSignature*, die u eerder hebt verkregen.
   
   > [!NOTE]
   > Omdat de referenties die worden gedistribueerd met een client-app niet meestal beveiligd zijn, distribueert u alleen de sleutel voor *luisteren* toegang met uw client-app. Uw app kunt registreren voor meldingen, met toegang tot het luisteren, maar bestaande registraties kunnen niet worden gewijzigd, en kunnen niet worden meldingen verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-end-service voor het verzenden van meldingen en bestaande registraties wijzigen.
   > 
   > 
5. Voeg de volgende regel in het projectbestand MainPage.xaml.cs:
   
        using Windows.UI.Popups;

6. In het projectbestand MainPage.xaml.cs voegt u de volgende methode toe:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    Deze methode maakt u een lijst met categorieën en gebruikt de **meldingen** klasse voor het opslaan van de lijst in de lokale opslag. Deze ook de bijbehorende labels voor uw notification hub geregistreerd. Wanneer de categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app kunt nu een set categorieën opslaan in de lokale opslag op het apparaat. De app wordt geregistreerd bij de notification hub wanneer gebruikers de categorieselectie te wijzigen.

## <a name="register-for-notifications"></a>Registreren voor meldingen
In deze sectie maakt registreren u bij de notification hub bij het opstarten met behulp van de categorieën die u hebt opgeslagen in de lokale opslag.

> [!NOTE]
> Omdat de kanaal-URI die toegewezen door de Windows Notification Service (WNS) op elk gewenst moment wijzigen kunt, kunt u moet registreren voor meldingen vaak ter voorkoming van fouten van de melding. In dit voorbeeld registreert voor melding van elke keer dat de app wordt gestart. Voor apps die u regelmatig (meer dan één keer per dag) uitvoert, kunt u waarschijnlijk registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie overslaan.
> 
> 

1. Gebruik de `notifications` klasse om u te abonneren op basis van categorieën, open het bestand App.xaml.cs en werk vervolgens de **InitNotificationsAsync** methode.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Dit proces zorgt ervoor dat wanneer de app wordt gestart, het haalt de categorieën van lokale opslag en de registratie van deze categorieën vraagt. U hebt gemaakt de **InitNotificationsAsync** methode als onderdeel van de [aan de slag met Notification Hubs] [ get-started] zelfstudie.

2. In het projectbestand MainPage.xaml.cs voegt u de volgende code aan de *OnNavigatedTo* methode:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    Deze code werkt de hoofdpagina van op basis van de status van de eerder opgeslagen categorieën.

De app is nu voltooid. Dit kan een set categorieën opslaan in de lokale opslagruimte op apparaten die worden gebruikt om te registreren bij de notification hub wanneer gebruikers de categorieselectie wijzigen. In de volgende sectie definieert u een back-end die categorie meldingen naar deze app kunt verzenden.

## <a name="send-tagged-notifications"></a>Met tags meldingen verzenden
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Voer de app en meldingen genereren
1. Selecteer in Visual Studio **F5** compileren en de app te starten.  
    De UI-app biedt een reeks Schakelknoppen waarmee u kiezen van de categorieën om u te abonneren kunt op. 
   
    ![Nieuws app op te splitsen][1]

2. Een of meer categorie Schakelknoppen inschakelen en klik vervolgens op **abonneren**.
   
    De app de geselecteerde categorieën converteert naar labels en een nieuwe apparaatregistratie voor de geselecteerde codes aanvragen van de notification hub. De geregistreerde categorieën worden geretourneerd en weergegeven in het dialoogvenster.
   
    ![Categorie schakelknoppen en abonneren knop][19]

3. Een nieuwe melding van de back-end op een van de volgende manieren verzenden:

   * **Console-app**: de console-app te starten.
   * **Java/PHP**: uitvoeren van uw app of script.
     
     Meldingen voor de geselecteerde categorieën weergegeven als pop-upmeldingen.
     
     ![Pop-upmeldingen][14]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe belangrijk nieuws per categorie-broadcast. Houd rekening met het voltooien van de volgende zelfstudie, wat op een ander geavanceerde Notification Hubs-scenario:

* [Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws] in deze zelfstudie wordt beschreven hoe u de app belangrijk nieuws om in te schakelen met het verzenden van meldingen gelokaliseerde uitbreiden.

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification
[Notification Hubs gebruiken voor het uitzenden van gelokaliseerde belangrijk nieuws]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
