---
title: Notification Hubs gebruiken om belangrijk nieuws (Windows Phone) te verzenden
description: Gebruik Azure Notification Hubs tag in registraties belangrijk nieuws verzenden naar een Windows Phone-app.
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3a6a69bf555c7267d3fbeb03ff6c03054991960f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Notification Hubs gebruiken om belangrijk nieuws te verzenden
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest u het gebruik van Azure Notification Hubs voor belangrijk nieuws meldingen naar een Windows Phone 8.0/8.1 Silverlight-app-broadcast. Als u voor Windows Store of Windows Phone 8.1-app ontwikkelt, Raadpleeg naar de [universele Windows-](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) versie. Als u klaar gaat u kunnen registreren voor nieuwscategorieën die u geïnteresseerd bent in op te splitsen en pushmeldingen voor deze categorieën ontvangen. Dit scenario is een algemene patroon voor veel apps waarbij moeten meldingen worden verzonden naar groepen gebruikers die interesse in deze, zoals RSS-lezer, apps voor muziek ventilatoren, enzovoort eerder is gedeclareerd.

Broadcast-scenario's zijn ingeschakeld door een of meer *labels* bij het maken van een registratie in de notification hub. Wanneer u meldingen worden verzonden naar een label, ontvangen alle apparaten die zijn geregistreerd voor het label de melding. Omdat tags gewoon tekenreeksen zijn, hoeven niet vooraf zijn ingericht. Raadpleeg voor meer informatie over tags [Notification Hubs-Routering en code-expressies](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Vereisten
In dit onderwerp is gebaseerd op de app die u hebt gemaakt in [aan de slag met Notification Hubs]. Voordat u deze zelfstudie begint, u moet al hebt voltooid [aan de slag met Notification Hubs].

## <a name="add-category-selection-to-the-app"></a>Categorieselectie toevoegen aan de app.
De eerste stap is het toevoegen van de UI-elementen naar uw bestaande hoofdpagina waarmee de gebruiker kan de categorieën selecteren om te registreren. De categorieën die door een gebruiker is geselecteerd worden op het apparaat opgeslagen. Wanneer de app wordt gestart, wordt de apparaatregistratie van een in uw notification hub met de geselecteerde categorieën gemaakt als labels.

1. Open het projectbestand MainPage.xaml en vervang de **raster** elementen met de naam `TitlePanel` en `ContentPanel` met de volgende code:
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. In het project, maakt u een nieuwe klasse met de naam **meldingen**, toevoegen de **openbare** aanpassingsfunctie naar de klassendefinitie, voegt u de volgende **met** instructies op het nieuwe codebestand :
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Kopieer de volgende code naar de nieuwe **meldingen** klasse:
   
        private NotificationHub hub;
   
        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // The stored categories tags are passed with the template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    Deze klasse maakt gebruik van geïsoleerde opslag voor het opslaan van de categorieën van nieuws die dit apparaat moet ontvangen. Het bevat ook methoden om te registreren voor deze categorieën met behulp van een [sjabloon](notification-hubs-templates-cross-platform-push-messages.md) registratie.


1. In het projectbestand App.xaml.cs de volgende eigenschap toevoegen aan de **App** klasse. Vervang de `<hub name>` en `<connection string with listen access>` tijdelijke aanduidingen door de naam van uw notification hub en de verbindingsreeks voor *DefaultListenSharedAccessSignature* die u eerder hebt verkregen.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Omdat de referenties die worden gedistribueerd met een client-app niet over het algemeen veilig, moet u de sleutel voor listen toegang alleen distribueren met uw clientapp. Luisteren toegang kunnen uw app registreren voor meldingen, maar bestaande registraties kan niet worden gewijzigd en kunnen niet worden meldingen verzonden. De volledige toegang tot de sleutel wordt gebruikt in een beveiligde back-endservice voor het verzenden van meldingen en bestaande registraties wijzigen.
   > 
   > 
2. Voeg de volgende regel in uw MainPage.xaml.cs:
   
        using Windows.UI.Popups;
3. In het projectbestand MainPage.xaml.cs voegt u de volgende methode toe:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    Deze methode maakt u een lijst met categorieën en gebruikt de **meldingen** klasse voor het opslaan van de lijst in de lokale opslag en registreren van de bijbehorende tags voor uw notification hub. Wanneer categorieën worden gewijzigd, wordt de registratie opnieuw gemaakt met de nieuwe categorieën.

Uw app is nu in staat een aantal categorieën opslaan in lokale opslag op het apparaat en registreren bij de notification hub wanneer de gebruiker de selectie van categorieën wijzigt.

## <a name="register-for-notifications"></a>Registreren voor meldingen
Deze stappen registreren bij de notification hub bij het opstarten met behulp van de categorieën die zijn opgeslagen in de lokale opslag.

> [!NOTE]
> Omdat de kanaal-URI is toegewezen door de Microsoft Push Notification Service (MPNS) op elk gewenst moment wijzigen kunt, kunt u moet registreren voor meldingen vaak ter voorkoming van fouten van de melding. In dit voorbeeld wordt elke keer dat de app wordt gestart voor meldingen die worden geregistreerd. Voor apps die vaak worden uitgevoerd, kunt meer dan één keer per dag, u waarschijnlijk overslaan registratie om bandbreedte te besparen als minder dan een dag is verstreken sinds de vorige registratie.
> 
> 

1. Open het bestand App.xaml.cs en voeg de **asynchrone** wijzigingsfunctie voor **Application_Launching** methode en vervang de registratie van Notification Hubs code die u hebt toegevoegd in [aan de slag met Notification Hubs] met de volgende code:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Dit zorgt ervoor dat telkens wanneer de app wordt gestart de categorieën van lokale opslag haalt en een registratie voor deze categorieën vraagt.
2. Voeg in het projectbestand MainPage.xaml.cs de volgende code waarmee de **OnNavigatedTo** methode:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    Hiermee wordt de hoofdpagina op basis van de status van de eerder opgeslagen categorieën bijgewerkt.

De app is nu voltooid en een set categorieën kunt opslaan in de lokale opslag gebruikt om u te registreren bij de notification hub wanneer de gebruiker de selectie van categorieën wijzigt van apparaat. Vervolgens definiëren we een back-end die categorie meldingen naar deze app kunt verzenden.

## <a name="sending-tagged-notifications"></a>Verzenden van meldingen met tags
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Voer de app en meldingen genereren
1. Druk in Visual Studio op F5 om te compileren en de app te starten.
   
    ![][1]
   
    Opmerking dat de UI-app biedt een reeks Schakelknoppen waarmee u kunt kiezen uit de categorieën om u te abonneren op.
2. Een of meer categorieën Schakelknoppen inschakelen en klik vervolgens op **abonneren**.
   
    De app de geselecteerde categorieën converteert naar labels en een nieuwe apparaatregistratie voor de geselecteerde codes aanvragen van de notification hub. De geregistreerde categorieën worden geretourneerd en in een dialoogvenster weergegeven.
   
    ![][2]
3. Na de ontvangst van een bevestiging dat uw categorieën abonnement voltooid zijn, moet u de console-app voor het verzenden van meldingen voor elke categorieën uitvoeren. Controleer of er alleen een melding voor de categorieën die u bent geabonneerd.
   
    ![][3]

U kunt in dit onderwerp hebt voltooid.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[aan de slag met Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

