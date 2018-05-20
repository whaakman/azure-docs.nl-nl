---
title: Pushmeldingen verzenden naar specifieke gebruikers met Azure Notification Hubs | Microsoft Docs
description: Lees hoe u meldingen kunt verzenden naar specifieke gebruikers met behulp van UWP-toepassingen (Universal Windows Platform).
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/04/2018
ms.author: dimazaid
ms.openlocfilehash: ee0e78402515a733731e6faf21b09886674cda51
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-send-notifications-to-specific-users-by-using-azure-notification-hubs"></a>Zelfstudie: Meldingen verzenden naar specifieke gebruikers met behulp van Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt uitgelegd hoe u met Azure Notification Hubs pushmeldingen kunt verzenden naar een specifieke appgebruiker op een specifiek apparaat. Er wordt een ASP.NET WebAPI-back-end gebruikt om clients te verifiëren. Wanneer de back-end de gebruiker van een clienttoepassing verifieert, wordt er automatisch een tag aan de registratie van de melding toegevoegd. De back-end gebruikt deze tag om meldingen te verzenden naar de specifieke gebruiker. 

> [!NOTE]
> De volledige code voor deze zelfstudie vindt u op [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Het WebAPI-project maken
> * Clients verifiëren bij de WebAPI-back-end
> * Registreren voor meldingen met behulp van de WebAPI-back-end
> * Meldingen verzenden vanuit de WebAPI-back-end
> * De nieuwe WebAPI-back-end publiceren
> * De code voor het clientproject bijwerken
> * De toepassing testen


## <a name="prerequisites"></a>Vereisten
Deze zelfstudie bouwt voort op de Notification Hub en het Visual Studio-project die u hebt gemaakt in de zelfstudie [Tutorial: Send notifications to Universal Windows Platform apps by using Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) (Zelfstudie: Meldingen verzenden naar UWP-apps (Universal Windows Platform) met behulp van Azure Notification Hubs). Het is dan ook belangrijk dat u eerst die zelfstudie voltooit voordat u aan de slag gaat met deze zelfstudie. 

> [!NOTE]
> Als u Mobile Apps in Azure App Service gebruikt als uw back-endservice, raadpleegt u de [Mobile Apps-versie](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) van deze zelfstudie.


&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>De code voor het clientproject bijwerken
In dit gedeelte werkt u de code bij in het project dat u hebt voltooid voor de zelfstudie [Tutorial: Send notifications to Universal Windows Platform apps by using Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) (Zelfstudie: Meldingen verzenden naar UWP-apps (Universal Windows Platform) met behulp van Azure Notification Hubs). Het project moet al zijn gekoppeld aan de Windows Store. Bovendien moet het project zijn geconfigureerd voor het gebruik van uw Notification Hub. In dit gedeelte voegt u code toe voor het aanroepen van de nieuwe WebAPI-back-end en gebruikt u deze voor het registreren en verzenden van meldingen.

1. Open in Visual Studio de oplossing die u hebt gemaakt voor de zelfstudie [Tutorial: Send notifications to Universal Windows Platform apps by using Azure Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) (Zelfstudie: Meldingen verzenden naar UWP-apps (Universal Windows Platform) met behulp van Azure Notification Hubs).
2. Klik in Solution Explorer met de rechtermuisknop op het project **WindowsApp** en klik vervolgens op **NuGet-pakketten beheren**.
3. Klik aan de linkerkant op **Online**.
4. Typ **Http Client** in**** het zoekvak.
5. Klik in de lijst met resultaten op **System.Net.Http** en vervolgens op **Installeren**. Voltooi de installatie.
6. Typ nu **Json.net** in**** het zoekvak van NuGet. Installeer het pakket **Newtonsoft.json** en sluit het venster NuGet Package Manager.
8. Dubbelklik in Solution Explorer in het project **WindowsApp** op het bestand **MainPage.xaml** om dit te openen in de Visual Studio-editor.
9. Vervang in de XML-code **MainPage.xaml** de sectie `<Grid>` door de volgende code (met deze code worden een gebruikersnaam en een wachtwoordvak toegevoegd waarmee de gebruiker zich kan verifiëren): Ook worden er tekstvakken toegevoegd voor de melding en voor de gebruikersnaam-tag die de melding moet ontvangen:

    ```xml   
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>

        <StackPanel Grid.Row="1" VerticalAlignment="Center">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="Auto"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition></ColumnDefinition>
                    <ColumnDefinition></ColumnDefinition>
                    <ColumnDefinition></ColumnDefinition>
                </Grid.ColumnDefinitions>
                <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>

                <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                            Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>

                <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />

                <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
            </Grid>
        </StackPanel>
    </Grid>
    ```
11. Open in Solution Explorer het bestand **MainPage.xaml.cs** voor de projecten **(Windows 8.1)** en **(Windows Phone 8.1)**. Voeg de volgende `using`-instructies toe aan het begin van beide bestanden:

    ```csharp    
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Windows.Networking.PushNotifications;
    using Windows.UI.Popups;
    using System.Threading.Tasks;
    ```
12. Voeg in **MainPage.xaml.cs** voor het project **WindowsApp** het volgende lid toe aan de klasse `MainPage`. Zorg ervoor dat u `<Enter Your Backend Endpoint>` vervangt door het eindpunt van de back-end dat u eerder hebt vastgesteld. Bijvoorbeeld `http://mybackend.azurewebsites.net`.
    
    ```csharp
    private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
    ```
13. Voeg de volgende code toe aan de klasse MainPage in **MainPage.xaml.cs** voor de projecten **(Windows 8.1)** en **(Windows Phone 8.1)**.
    
    De methode `PushClick` is de handler voor de knop **Send Push**. De methode roept de back-end aan om een melding te triggeren naar alle apparaten met een gebruikersnaam-tag die overeenkomt met de parameter `to_tag`. De melding wordt als JSON-inhoud verzonden in de aanvraagtekst.
    
    De methode `LoginAndRegisterClick` is de klik-handler voor de knop **Login and register**. De methode slaat het basisverificatie-token op in de lokale opslag, waarna `RegisterClient` wordt gebruikt voor het registreren voor meldingen met behulp van de back-end. Het token kan overigens elk token zijn dat door uw verificatieschema wordt gebruikt.

    ```csharp
    private async void PushClick(object sender, RoutedEventArgs e)
    {
        if (toggleWNS.IsChecked.Value)
        {
            await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
        }
        if (toggleGCM.IsChecked.Value)
        {
            await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
        }
        if (toggleAPNS.IsChecked.Value)
        {
            await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

        }
    }

    private async Task sendPush(string pns, string userTag, string message)
    {
        var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
            pns + "&to_tag=" + userTag;

        using (var httpClient = new HttpClient())
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                alert.ShowAsync();
            }
        }
    }

    private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
    {
        SetAuthenticationTokenInLocalStorage();

        var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

        // The "username:<user name>" tag gets automatically added by the message handler in the backend.
        // The tag passed here can be whatever other tags you may want to use.
        try
        {
            // The device handle used is different depending on the device and PNS. 
            // Windows devices use the channel uri as the PNS handle.
            await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

            var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            SendPushButton.IsEnabled = true;
        }
        catch (Exception ex)
        {
            MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
            alert.ShowAsync();
        }
    }

    private void SetAuthenticationTokenInLocalStorage()
    {
        string username = UsernameTextBox.Text;
        string password = PasswordTextBox.Password;

        var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
        ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
    }
    ```
1. Open het bestand **App.xaml.cs**. Zoek de aanroep naar `InitNotificationsAsync()` in de gebeurtenis-handler `OnLaunched()`. Maak een commentaar van de aanroep naar `InitNotificationsAsync()` of verwijder de aanroep. De knop-handler initialiseert de registratie van meldingen.

    ```csharp
    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        //InitNotificationsAsync();
    ```
1. Klik met de rechtermuisknop op het project **WindowsApp**, klik op **Add** en op **Class**. Geef de klasse de naam **RegisterClient.cs** en klik vervolgens op **OK** om de klasse te genereren.
   
   Deze klasse verpakt de REST-aanroepen die nodig zijn om contact op te nemen met de app-back-end, om registratie voor pushmeldingen mogelijk te maken. Daarnaast wordt de *registrationIds* die is gemaakt door de Notification Hub, lokaal opgeslagen. Zie [Registering from your App Backend](http://msdn.microsoft.com/library/dn743807.aspx) (Registreren vanuit uw app-back-end) voor meer informatie. De klasse maakt gebruik van een verificatietoken dat wordt opgeslagen in de lokale opslag wanneer u op de knop **Login and register** klikt.
2. Voeg aan het begin van het bestand RegisterClient.cs de volgende `using`-instructies toe:

    ```csharp   
    using Windows.Storage;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using System.Threading.Tasks;
    using System.Linq;
    ```
3. Voeg de volgende code in de `RegisterClient`-klassedefinitie.
   
    ```csharp
    private string POST_URL;

    private class DeviceRegistration
    {
        public string Platform { get; set; }
        public string Handle { get; set; }
        public string[] Tags { get; set; }
    }

    public RegisterClient(string backendEndpoint)
    {
        POST_URL = backendEndpoint + "/api/register";
    }

    public async Task RegisterAsync(string handle, IEnumerable<string> tags)
    {
        var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();

        var deviceRegistration = new DeviceRegistration
        {
            Platform = "wns",
            Handle = handle,
            Tags = tags.ToArray<string>()
        };

        var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);

        if (statusCode == HttpStatusCode.Gone)
        {
            // regId is expired, deleting from local storage & recreating
            var settings = ApplicationData.Current.LocalSettings.Values;
            settings.Remove("__NHRegistrationId");
            regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
            statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
        }

        if (statusCode != HttpStatusCode.Accepted && statusCode != HttpStatusCode.OK)
        {
            // log or throw
            throw new System.Net.WebException(statusCode.ToString());
        }
    }

    private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
    {
        using (var httpClient = new HttpClient())
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);

            var putUri = POST_URL + "/" + regId;

            string json = JsonConvert.SerializeObject(deviceRegistration);
                            var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
    {
        var settings = ApplicationData.Current.LocalSettings.Values;
        if (!settings.ContainsKey("__NHRegistrationId"))
        {
            using (var httpClient = new HttpClient())
            {
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                if (response.IsSuccessStatusCode)
                {
                    string regId = await response.Content.ReadAsStringAsync();
                    regId = regId.Substring(1, regId.Length - 2);
                    settings.Add("__NHRegistrationId", regId);
                }
                else
                {
                    throw new System.Net.WebException(response.StatusCode.ToString());
                }
            }
        }
        return (string)settings["__NHRegistrationId"];

    }
    ```
4. Sla al uw wijzigingen op.

## <a name="test-the-application"></a>De toepassing testen
1. Start de toepassing in beide versies van Windows.
2. Voer een gebruikersnaam (**Username**) en wachtwoord (**Password**) in zoals weergegeven in het scherm hieronder. Deze moeten verschillen van de gebruikersnaam en het wachtwoord voor Windows Phone.
3. Klik op **Login and register** en controleer of er een dialoogvenster wordt weergegeven met de mededeling dat u zich hebt aangemeld. Met deze code wordt ook de knop **Send Push** ingeschakeld.
   
    ![][14]
5. Voer in het veld **Recipient Username Tag** de geregistreerde gebruikersnaam in. Voer tekst in voor de melding en klik op **Send Push**.
6. Alleen de apparaten die zijn geregistreerd met de overeenkomende gebruikersnaam-tag ontvangen de melding.
   
    ![][15]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u pushmeldingen kunt verzenden naar specifieke gebruikers door een tag te koppelen aan hun registraties. Als u wilt weten hoe u locatiegebaseerde pushmeldingen kunt verzenden, gaat u verder met de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Locatiegebaseerde pushmeldingen verzenden](notification-hubs-push-bing-spartial-data-geofencing-notification.md)

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Get started with Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
