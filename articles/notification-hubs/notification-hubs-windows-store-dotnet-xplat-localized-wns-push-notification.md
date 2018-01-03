---
title: Notification Hubs gelokaliseerd belangrijk nieuws zelfstudie
description: Informatie over het gebruik van Azure Notification Hubs om gelokaliseerde belangrijk nieuws meldingen te verzenden.
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8f205188bd68e53b187b71981ed36dcf9129ec62
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Notification Hubs gebruiken om gelokaliseerde belangrijk nieuws te verzenden
> [!div class="op_single_selector"]
> * [Windows Store in C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
Dit onderwerp leest u hoe u de **sjabloon** functie van Azure Notification Hubs voor belangrijk nieuws meldingen die door de taal en het apparaat zijn gelokaliseerd uitzending. In deze zelfstudie begint u met de Windows Store-app gemaakt in [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Als u klaar is, kunt u zich registreren voor u geïnteresseerd bent in categorieën, geeft u de taal waarin u de meldingen ontvangen en pushmeldingen voor de geselecteerde categorieën in die taal ontvangen.

Er zijn twee delen in dit scenario:

* de Windows Store-app kan client apparaten een taal op te geven en zich abonneren op andere belangrijk nieuwscategorieën;
* de back-end zendt de meldingen, met behulp van de **tag** en **sjabloon** funcites van Azure Notification Hubs.

## <a name="prerequisites"></a>Vereisten
U moet al hebt voltooid de [Notification Hubs gebruiken om belangrijk nieuws te verzenden] zelfstudie en hebben de code die beschikbaar zijn, omdat deze zelfstudie is gebaseerd rechtstreeks op die code.

U moet ook Visual Studio 2012 of later.

## <a name="template-concepts"></a>Sjabloon-concepten
In [Notification Hubs gebruiken om belangrijk nieuws te verzenden] u een app die gebruikt gebouwd **labels** abonneren op meldingen voor verschillende nieuwscategorieën.
Veel apps echter meerdere markten zijn gericht en lokalisatie vereisen. Dit betekent dat de inhoud van de meldingen zelf hebt kunnen worden gelokaliseerd en verzonden naar de juiste set met apparaten.
In dit onderwerp wordt wordt getoond hoe u de **sjabloon** functie van Notification Hubs eenvoudig leveren gelokaliseerde belangrijk nieuws meldingen.

Opmerking: Er is een manier om gelokaliseerde meldingen te verzenden is om meerdere versies van elke tag te maken. Bijvoorbeeld ter ondersteuning van Engels, Frans en Mandarijn, zou moeten we drie verschillende tags voor wereldnieuws: 'world_en', 'world_fr' en 'world_ch'. Er moet een gelokaliseerde versie van de wereldnieuws verzenden naar elk van deze tags. In dit onderwerp gebruiken we sjablonen om te voorkomen dat de komst van tags en de vereiste van meerdere berichten verzenden.

Sjablonen zijn op een hoog niveau een manier om op te geven hoe een specifiek apparaat een melding moet ontvangen. De sjabloon geeft de indeling van de exacte nettolading door te verwijzen naar de eigenschappen die deel van het bericht is verzonden door back-end van uw app uitmaken. In ons geval ontvangt van ons een landinstelling networkdirect-bericht met alle ondersteunde talen:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vervolgens we zorgt ervoor dat apparaten registreren met een sjabloon die naar de juiste eigenschap verwijst. Windows Store-Apps die wil een eenvoudige pop-up bericht wordt bijvoorbeeld registreren voor de volgende sjabloon met een overeenkomende labels:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Sjablonen zijn een zeer krachtige functie voor meer informatie over in onze [sjablonen](notification-hubs-templates-cross-platform-push-messages.md) artikel. 

## <a name="the-app-user-interface"></a>De gebruikersinterface van de app
Er wordt nu de op te splitsen nieuws-app die u hebt gemaakt in het onderwerp wijzigen [Notification Hubs gebruiken om belangrijk nieuws te verzenden] gelokaliseerd belangrijk nieuws met behulp van sjablonen om te verzenden.

In de Windows Store-app:

Uw MainPage.xaml als u wilt opnemen een combobox landinstelling wijzigen:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-the-windows-store-client-app"></a>De client Windows Store-app bouwen
1. In uw klasse meldingen toevoegen een landinstellingenparameter voor uw *StoreCategoriesAndSubscribe* en *SubscribeToCateories* methoden.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    Houd er rekening mee dat in plaats van het aanroepen van de *RegisterNativeAsync* methode we noemen *RegisterTemplateAsync*: we bij het registreren van een specifieke notification-indeling op waarin de sjabloon is afhankelijk van de landinstelling. We bieden ook een naam voor de sjabloon ('localizedWNSTemplateExample'), omdat we willen mogelijk meer dan één sjabloon (bijvoorbeeld één voor de pop-upmeldingen) en één voor tegels registreren en moeten deze de naam om te kunnen bijwerken of verwijderen.
   
    Houd er rekening mee dat als een apparaat meerdere sjablonen met dezelfde tag registreert, een binnenkomende bericht doelen die tag in meerdere meldingen resulteren geleverd aan het apparaat (één voor elke sjabloon). Dit gedrag is handig wanneer hetzelfde logische bericht moet resulteren in meerdere visual meldingen, bijvoorbeeld zowel een badge en een pop-up wordt weergegeven in een Windows Store-toepassing.
2. Voeg de volgende methode voor het ophalen van de opgeslagen landinstellingen:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. De knop bijwerken in uw MainPage.xaml.cs, klikt u op handler door bij het ophalen van de huidige waarde van de landinstelling keuzelijst met invoervak en beschikbaar stellen de aanroep van de klasse meldingen, zoals wordt weergegeven:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. Controleer ten slotte, in het bestand App.xaml.cs om bij te werken uw `InitNotificationsAsync` methode voor het ophalen van de landinstelling en deze gebruiken als u zich abonneert:
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a>Gelokaliseerde meldingen verzenden vanuit uw back-end
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
