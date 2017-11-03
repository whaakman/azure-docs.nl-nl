---
title: Azure Notification Hubs gebruikers waarschuwen met .NET back-end
description: Informatie over het veilig om pushmeldingen te verzenden in Azure. Codevoorbeelden geschreven in C# met .NET API.
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: c0b963ef661612b1a176dd8e5f01d56e61eb5acb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure Notification Hubs gebruikers waarschuwen met .NET back-end
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Overzicht
Push notification-ondersteuning in Azure kunt u toegang tot een eenvoudig te gebruiken, multiplatform en uitgebreid pushinfrastructuur, die aanzienlijk de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt. In deze zelfstudie wordt uitgelegd hoe u met Azure Notification Hubs pushmeldingen kunt verzenden aan een specifieke appgebruiker op een specifiek apparaat. Een ASP.NET WebAPI-back-end wordt gebruikt om clients te verifiëren. Met behulp van de gebruiker van de geverifieerde client en de tag automatisch toegevoegd door de back-end voor registratie. Deze code wordt gebruikt om te verzenden doordat de back-end om meldingen voor een specifieke gebruiker te genereren. Zie het onderwerp richtlijnen voor meer informatie over het registreren voor meldingen met behulp van een back-end voor de app [registreren van uw app back-end](http://msdn.microsoft.com/library/dn743807.aspx). Deze zelfstudie bouwt voort op de notification hub en project dat u hebt gemaakt in de [aan de slag met Notification Hubs] zelfstudie.

Deze zelfstudie is ook de vereiste voor de [Secure Push] zelfstudie. Nadat u de stappen in deze zelfstudie hebt voltooid, kunt u doorgaan met de [Secure Push] zelfstudie waarin het wijzigen van de code in deze zelfstudie voor het veilig verzenden van een push-melding.

## <a name="before-you-begin"></a>Voordat u begint
We nemen uw feedback heel serieus. Als u problemen ondervindt tijdens het voltooien van dit onderwerp of aanbevelingen hebt voor het verbeteren van de inhoud ervan, zouden we het op prijs stellen als uw uw feedback onder aan de pagina geeft.

De volledige code voor deze zelfstudie vindt u [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers) op GitHub. 

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, moet u al hebt voltooid deze zelfstudies voor Mobile Services:

* [aan de slag met Notification Hubs]<br/>Uw notification hub te maken en de app-naam reserveren en registreren voor het ontvangen van meldingen in deze zelfstudie. Deze zelfstudie wordt ervan uitgegaan dat u al deze stappen hebt voltooid. Als dit niet het geval is, volg de stappen in [aan de slag met Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); in het bijzonder de secties [uw app registreren voor de Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) en [configureren uw Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). In het bijzonder, zorg ervoor dat u hebt ingevoerd de **pakket-SID** en **Clientgeheim** waarden in de portal in de **configureren** tabblad voor uw notification hub. Deze configuratieprocedure wordt beschreven in de sectie [uw Notification Hub configureren](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Dit is een belangrijke stap: als de referenties op de portal niet overeenkomen met de opgegeven voor de naam van de app u kiest, mislukt de pushmelding.

> [!NOTE]
> Als u mobiele Apps in Azure App Service als uw back-endservice gebruikt, raadpleegt u de [Mobile Apps-versie](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) van deze zelfstudie.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>De code voor het clientproject bijwerken
In deze sectie maakt u de code in het project dat u voor voltooid bijwerken de [aan de slag met Notification Hubs] zelfstudie. De al moet zijn gekoppeld aan het archief en geconfigureerd voor uw notification hub. In deze sectie voegt u code voor het aanroepen van de nieuwe WebAPI-back-end en deze gebruiken voor het registreren en verzenden van meldingen.

1. Open in Visual Studio de de oplossing die u hebt gemaakt voor de [aan de slag met Notification Hubs] zelfstudie.
2. Klik in Solution Explorer met de rechtermuisknop op de **(Windows 8.1)** project en klik vervolgens op **NuGet-pakketten beheren**.
3. Aan de linkerkant en klik op **Online**.
4. In de **Search** in het vak **HTTP-Client**.
5. Klik in de lijst met resultaten op **Microsoft HTTP-clientbibliotheken**, en klik vervolgens op **installeren**. De installatie voltooid.
6. Terug in de NuGet **Search** in het vak **Json.net**. Installeer de **Json.NET** Inpakken en sluit het venster NuGet Package Manager.
7. Herhaal de bovenstaande stappen voor het **(Windows Phone 8.1)** project voor het installeren van de **JSON.NET** NuGet-pakket voor het Windows Phone-project.
8. Klik in Solution Explorer in de **(Windows 8.1)** project, dubbelklikt u op **MainPage.xaml** openen in de Visual Studio-editor.
9. In de **MainPage.xaml** XML-code, vervang de `<Grid>` sectie met de volgende code. Deze code wordt een gebruikersnaam en wachtwoord tekstvak die de gebruiker wordt geverifieerd met toegevoegd. Bovendien tekstvakken voor de melding en de gebruikersnaam-tag die de melding moet ontvangen die worden toegevoegd:
   
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
10. Klik in Solution Explorer in de **(Windows Phone 8.1)** project, open **MainPage.xaml** en vervang de Windows Phone 8.1 `<Grid>` sectie met de bovenstaande die dezelfde code. De interface zijn vergelijkbaar met wat is hieronder weergegeven.
    
    ![][13]
11. Open in Solution Explorer de **MainPage.xaml.cs** bestand voor de **(Windows 8.1)** en **(Windows Phone 8.1)** projecten. Voeg de volgende `using` instructies aan het begin van beide bestanden:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. In **MainPage.xaml.cs** voor de **(Windows 8.1)** en **(Windows Phone 8.1)** projecten, het volgende lid toevoegen aan de `MainPage` klasse. Zorg ervoor dat u `<Enter Your Backend Endpoint>` met de uw werkelijke back-end-eindpunt eerder hebt verkregen. Bijvoorbeeld `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. De volgende code toevoegen aan de klasse MainPage in **MainPage.xaml.cs** voor de **(Windows 8.1)** en **(Windows Phone 8.1)** projecten.
    
    De `PushClick` methode is de handler voor de **verzenden Push** knop. Roept de back-end om te activeren van een melding aan alle apparaten met een gebruikersnaam tag die overeenkomt met de `to_tag` parameter. De melding wordt verzonden als JSON-inhoud in de aanvraagtekst.
    
    De `LoginAndRegisterClick` methode is de handler voor de **aanmelden en registreren** knop. Hierin worden opgeslagen in het basic verificatietoken in lokale opslag (Let erop dat dit verwijst naar een token uw verificatiemethode wordt gebruikt), gebruikt vervolgens `RegisterClient` registreren voor meldingen met behulp van de back-end.

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
                // The device handle used will be different depending on the device and PNS. 
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



1. Klik in Solution Explorer, onder de **gedeelde** project, open de **App.xaml.cs** bestand. De aanroep naar `InitNotificationsAsync()` in de `OnLaunched()` gebeurtenis-handler. Uitschakelen of verwijderen van de aanroep van `InitNotificationsAsync()`. De knop-handler die hierboven staan vermeld, wordt melding registraties initialiseren.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. Klik in Solution Explorer met de rechtermuisknop op de **gedeelde** project en klik vervolgens op **toevoegen**, en klik vervolgens op **klasse**. Naam van de klasse **RegisterClient.cs**, klikt u vervolgens op **OK** voor het genereren van de klasse.
   
   Deze klasse wordt de REST-aanroepen contact opnemen met de app back-end, om te registreren voor pushmeldingen verpakken. Ook lokaal worden opgeslagen de *registrationIds* gemaakt door de Notification Hub zoals beschreven in [registreren van uw app back-end](http://msdn.microsoft.com/library/dn743807.aspx). Opmerking die gebruikmaakt van een verificatietoken opgeslagen in de lokale opslag, wanneer u klikt op de **aanmelden en registreren** knop.
2. Voeg de volgende `using` instructies aan het begin van het bestand RegisterClient.cs:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Voeg de volgende code in de `RegisterClient`-klassedefinitie.
   
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
   
           if (statusCode != HttpStatusCode.Accepted)
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
4. Sla al uw wijzigingen op.

## <a name="testing-the-application"></a>De toepassing testen
1. Start de toepassing op Windows 8.1 en Windows Phone 8.1. U kunt het exemplaar in de emulator of een daadwerkelijk apparaat uitvoeren voor Windows Phone 8.1.
2. Voer in het Windows 8.1-exemplaar van de app een **gebruikersnaam** en **wachtwoord** zoals weergegeven in het scherm hieronder. Het moet verschillen van de gebruikersnaam en wachtwoord op Windows Phone.
3. Klik op **aanmelden en registreren** en controleer of er wordt een dialoogvenster weergegeven dat u zich hebt aangemeld. Hierdoor kunnen ook de **verzenden Push** knop.
   
    ![][14]
4. Voer op het Windows Phone 8.1-exemplaar, een tekenreeks met de gebruiker in zowel de **gebruikersnaam** en **wachtwoord** velden, klikt u vervolgens op **aanmelden en registreren**.
5. Klik in de **ontvanger gebruikersnaam Tag** en voer de naam van de gebruiker is geregistreerd op Windows 8.1. Geef een bericht en klik op **verzenden Push**.
   
    ![][16]
6. Alleen de apparaten die zijn geregistreerd met de overeenkomende gebruikersnaam code ontvangen de melding.
   
    ![][15]

## <a name="next-steps"></a>Volgende stappen
* Zie [Notification Hubs gebruiken om belangrijk nieuws te verzenden] als u gebruikers wilt indelen op belangengroepen.
* Zie voor meer informatie over het gebruik van Notification Hubs, [richtlijnen voor Notification Hubs].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[aan de slag met Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[richtlijnen voor Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
