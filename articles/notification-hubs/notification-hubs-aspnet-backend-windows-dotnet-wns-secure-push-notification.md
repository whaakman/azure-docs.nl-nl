---
title: Azure Notification Hubs beveiligde Push
description: Informatie over het veilig om pushmeldingen te verzenden in Azure. Codevoorbeelden geschreven in C# met .NET API.
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 9c626ec1534c4899588150a58c0da57b9d963f6f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs beveiligde Push
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Overzicht
Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een eenvoudig te gebruiken, meerdere platforms, uitgebreid pushinfrastructuur, die aanzienlijk de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele platforms vereenvoudigt.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding die via de standaard push notification-infrastructuur kan niet worden verzonden. Deze zelfstudie wordt beschreven hoe dezelfde ervaring bereiken door te sturen van gevoelige gegevens via een veilige en geverifieerde verbinding tussen de client-apparaat en de back-end voor de app.

Op een hoog niveau is de stroom als volgt uit:

1. De back-end app:
   * Winkels beveiligde nettolading in back-enddatabase.
   * De ID van deze melding verzendt naar het apparaat (geen beveiligde gegevens verzonden).
2. De app op het apparaat tijdens het ontvangen van de melding:
   * Het apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de voorgaande stroom (en in deze zelfstudie), gaan we ervan uit dat het apparaat geen verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze ervaring als het apparaat de melding van de beveiligde nettolading met dit token kan ophalen. Als uw toepassing geen verificatietokens opslaat op het apparaat, of als deze tokens kunnen verlopen, moet een algemene melding dat de gebruiker wordt gevraagd om de app te starten door de app apparaat bij ontvangst van de melding worden weergegeven. De app vervolgens verifieert de gebruiker en toont de nettolading van de meldingen.

Deze Secure Push-zelfstudie laat zien hoe veilig een pushmelding verzenden. De zelfstudie bouwt voort op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) zelfstudie, dus u moet eerst de stappen in deze zelfstudie voltooien.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Ook, houd er rekening mee dat Windows Phone 8.1 (geen Windows Phone) voor Windows-referenties vereist en achtergrondtaken werken niet op Windows Phone 8.0 of Silverlight 8.1. Voor Windows Store-toepassingen, kunt u meldingen via een achtergrondtaak ontvangen alleen als de app vergrendelingsscherm ingeschakeld is (Klik op het selectievakje in de Appmanifest).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Het Windows Phone-Project wijzigen
1. In de **NotifyUserWindowsPhone** project en voeg de volgende code toe aan App.xaml.cs registreren van de push-achtergrondtaak. Voeg de volgende coderegel toe aan het einde van de `OnLaunched()` methode:
   
        RegisterBackgroundTask();
2. Voeg in App.xaml.cs nog steeds de volgende code direct na de `OnLaunched()` methode:
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. Voeg de volgende `using` instructies aan het begin van het bestand App.xaml.cs:
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. Klik in het menu **Bestand** in Visual Studio op **Alles opslaan**.

## <a name="create-the-push-background-component"></a>Het onderdeel van de achtergrond Push maken
De volgende stap is het maken van het onderdeel van de achtergrond push.

1. Klik in Solution Explorer met de rechtermuisknop op het bovenste knooppunt van de oplossing (**oplossing SecurePush** in dit geval), klikt u vervolgens op **toevoegen**, klikt u vervolgens op **nieuw Project**.
2. Vouw **Store-Apps**, klikt u vervolgens op **Windows Phone-Apps**, klikt u vervolgens op **Windows Runtime-onderdeel (Windows Phone)**. Noem het project **PushBackgroundComponent**, en klik vervolgens op **OK** om het project te maken.
   
    ![][12]
3. Klik in Solution Explorer met de rechtermuisknop op de **PushBackgroundComponent (Windows Phone 8.1)** project en klik vervolgens op **toevoegen**, klikt u vervolgens op **klasse**. Naam van de nieuwe klasse **PushBackgroundTask.cs**. Klik op **toevoegen** voor het genereren van de klasse.
4. Vervang de volledige inhoud van de **PushBackgroundComponent** naamruimtedefinitie door de volgende code, waarbij u de tijdelijke aanduiding vervangt `{back-end endpoint}` met het back-end-eindpunt dat is verkregen tijdens de implementatie van uw back-end:
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. Klik in Solution Explorer met de rechtermuisknop op de **PushBackgroundComponent (Windows Phone 8.1)** project en klik vervolgens op **NuGet-pakketten beheren**.
6. Aan de linkerkant en klik op **Online**.
7. In de **Search** in het vak **HTTP-Client**.
8. Klik in de lijst met resultaten op **Microsoft HTTP-clientbibliotheken**, en klik vervolgens op **installeren**. De installatie voltooid.
9. Terug in de NuGet **Search** in het vak **Json.net**. Installeer de **Json.NET** van het pakket en sluit vervolgens het venster NuGet Package Manager.
10. Voeg de volgende `using` instructies aan het begin van de **PushBackgroundTask.cs** bestand:
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. Klik in Solution Explorer in de **NotifyUserWindowsPhone (Windows Phone 8.1)** project, met de rechtermuisknop op **verwijzingen**, klikt u vervolgens op **verwijzing toevoegen...** . In het dialoogvenster Reference Manager, schakel het selectievakje in naast **PushBackgroundComponent**, en klik vervolgens op **OK**.
12. Dubbelklik in Solution Explorer op **Package.appxmanifest** in de **NotifyUserWindowsPhone (Windows Phone 8.1)** project. Onder **meldingen**stelt **Toast geschikt** naar **Ja**.
    
    ![][3]
13. Nog steeds in **Package.appxmanifest**, klikt u op de **declaraties** menu aan de bovenkant. In de **beschikbaar declaraties** dropdown, klikt u op **achtergrondtaken**, en klik vervolgens op **toevoegen**.
14. In **Package.appxmanifest**onder **eigenschappen**, Controleer **Push-melding**.
15. In **Package.appxmanifest**onder **Appinstellingen**, type **PushBackgroundComponent.PushBackgroundTask** in de **toegangspunt** veld.
    
    ![][13]
16. Klik in het menu **Bestand** op **Alles opslaan**.

## <a name="run-the-application"></a>De toepassing uitvoeren
Voor het uitvoeren van de toepassing, het volgende doen:

1. In Visual Studio, voert u de **AppBackend** Web API-toepassing. Een ASP.NET-webpagina wordt weergegeven.
2. In Visual Studio, voert u de **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app. De Windows Phone-emulator wordt uitgevoerd en de app automatisch wordt geladen.
3. In de **NotifyUserWindowsPhone** app-gebruikersinterface een gebruikersnaam en wachtwoord invoeren. Deze kunnen zich een willekeurige tekenreeks, maar moeten dezelfde waarde.
4. In de **NotifyUserWindowsPhone** app-gebruikersinterface Klik **aanmelden en registreren**. Klik vervolgens op **push verzenden**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
