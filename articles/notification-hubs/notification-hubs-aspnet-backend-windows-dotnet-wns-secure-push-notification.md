---
title: Azure Notification Hubs Secure Push
description: Informatie over het verzenden van beveiligde push-meldingen in Azure. Codevoorbeelden geschreven in C# met .NET API.
documentationcenter: windows
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: cf23ef5df3bdcaad23841da111fa06cc36b4cd57
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54475676"
---
# <a name="securely-push-notifications-from-azure-notification-hubs"></a>Veilig vanaf Azure Notification Hubs pushmeldingen verzenden

> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

## <a name="overview"></a>Overzicht

Push notification-ondersteuning in Microsoft Azure kunt u toegang tot een eenvoudig te gebruiken, meerdere platforms, uitgebreide pushinfrastructuur, die de implementatie van pushmeldingen voor consumenten- en enterprise-toepassingen voor mobiele apparaten vereenvoudigt platforms.

Vanwege regelgeving of veiligheidsbeperkingen, soms een toepassing wilt iets opnemen in de melding dat kan niet worden verzonden via de infrastructuur voor pushmeldingen voor standard. In deze zelfstudie wordt beschreven hoe u dezelfde ervaring bereiken door het verzenden van gevoelige gegevens via een veilige, geverifieerde verbinding tussen het clientapparaat en de back-end.

Op hoog niveau is de stroom als volgt uit:

1. De app-back-end:
   * Winkels beveiligde nettolading in back-end-database.
   * De ID van deze melding verzonden naar het apparaat (geen beveiligde gegevens wordt verzonden).
2. De app op het apparaat bij de ontvangst van de melding:
   * Het apparaat neemt contact op met de back-end de nettolading van de beveiligde aanvragen.
   * De app kan de nettolading van de weergegeven als een melding op het apparaat.

Het is belangrijk te weten dat in de vorige stroom (en in deze zelfstudie), gaan we ervan uit dat het apparaat een verificatietoken in de lokale opslag opslaat nadat de gebruiker zich aanmeldt. Dit garandeert een volledig naadloze ervaring als het apparaat van de melding van de beveiligde nettolading met behulp van dit token kunt ophalen. Als uw toepassing geen verificatietokens op het apparaat slaat, of als deze tokens kunnen worden verlopen, een algemene melding waarin de gebruiker wordt gevraagd om de app te starten moet worden weergegeven aan de apparaat-app, nadat de melding is ontvangen. De app vervolgens verifieert de gebruiker en de nettolading van de melding ziet.

Deze beveiligde Push-zelfstudie leert hoe u een push-bericht veilig verzendt. De zelfstudie bouwt voort op de [gebruikers waarschuwen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) zelfstudie, dus u moet eerst de stappen in deze zelfstudie voltooien.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u hebt gemaakt en uw notification hub geconfigureerd zoals beschreven in [aan de slag met Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Merk ook op dat Windows Phone 8.1 is vereist voor Windows (geen Windows Phone) referenties en achtergrondtaken werken niet op Windows Phone 8.0 of Silverlight 8.1. Voor Windows Store-toepassingen, kunt u meldingen via een achtergrondtaak ontvangt alleen als de app vergrendelingsscherm ingeschakeld is (Klik op het selectievakje in de Appmanifest).

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Wijzigen van het Windows Phone-Project

1. In de **NotifyUserWindowsPhone** project, voeg de volgende code toe App.xaml.cs voor het registreren van de push-achtergrondtaak. Voeg de volgende regel code toe aan het einde van de `OnLaunched()`-methode:

    ```c#
    RegisterBackgroundTask();
    ```
2. Nog steeds in App.xaml.cs, voeg de volgende code direct na de `OnLaunched()` methode:

    ```c#
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
    ```
3. Voeg de volgende `using` instructies aan het begin van het bestand App.xaml.cs:

    ```c#
    using Windows.Networking.PushNotifications;
    using Windows.ApplicationModel.Background;
    ```
4. Klik in het menu **Bestand** in Visual Studio op **Alles opslaan**.

## <a name="create-the-push-background-component"></a>Het onderdeel van de achtergrond Push maken

De volgende stap is het maken van het onderdeel van de achtergrond push.

1. Klik in Solution Explorer met de rechtermuisknop op het knooppunt op het hoogste niveau van de oplossing (**oplossing SecurePush** in dit geval), klikt u vervolgens op **toevoegen**, klikt u vervolgens op **nieuw Project**.
2. Vouw **Store-Apps**, klikt u vervolgens op **Windows Phone Apps**, klikt u vervolgens op **Windows Runtime-onderdeel (Windows Phone)**. Noem het project **PushBackgroundComponent**, en klik vervolgens op **OK** om het project te maken.

    ![][12]
3. Klik in Solution Explorer met de rechtermuisknop op de **PushBackgroundComponent (Windows Phone 8.1)** project en klik vervolgens op **toevoegen**, klikt u vervolgens op **klasse**. Noem de nieuwe klasse `PushBackgroundTask.cs`. Klik op **toevoegen** om de klasse te genereren.
4. Vervang de volledige inhoud van de `PushBackgroundComponent` naamruimtedefinitie door de volgende code, waarbij u de tijdelijke aanduiding vervangt door `{back-end endpoint}` met de back-end-eindpunt dat is verkregen tijdens het implementeren van uw back-end:

    ```csharp
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
    ```
5. Klik in Solution Explorer met de rechtermuisknop op de **PushBackgroundComponent (Windows Phone 8.1)** project en klik vervolgens op **NuGet-pakketten beheren**.
6. Klik aan de linkerkant op **Online**.
7. Typ **Http Client** in **het zoekvak**.
8. Klik in de lijst met resultaten op **Microsoft HTTP-clientbibliotheken**, en klik vervolgens op **installeren**. Voltooi de installatie.
9. Typ nu **Json.net** in **het zoekvak** van NuGet. Installeer de **Json.NET** verpakken en sluit vervolgens het venster NuGet Package Manager.
10. Voeg de volgende `using` instructies aan het begin van de `PushBackgroundTask.cs` bestand:

    ```c#
    using Windows.ApplicationModel.Background;
    using Windows.Networking.PushNotifications;
    using System.Net.Http;
    using Windows.Storage;
    using System.Net.Http.Headers;
    using Newtonsoft.Json;
    using Windows.UI.Notifications;
    using Windows.Data.Xml.Dom;
    ```
11. Klik in Solution Explorer in de **NotifyUserWindowsPhone (Windows Phone 8.1)** project, met de rechtermuisknop op **verwijzingen**, klikt u vervolgens op **verwijzing toevoegen...** . In het dialoogvenster Reference Manager, schakel het selectievakje in naast **PushBackgroundComponent**, en klik vervolgens op **OK**.
12. Dubbelklik in Solution Explorer op **Package.appxmanifest** in de **NotifyUserWindowsPhone (Windows Phone 8.1)** project. Onder **meldingen**, stel **Toast geschikt** naar **Ja**.

    ![][3]
13. Klik in **Package.appxmanifest**, klikt u op de **declaraties** menu aan de bovenkant. In de **beschikbaar declaraties** vervolgkeuzelijst, klikt u op **achtergrondtaken**, en klik vervolgens op **toevoegen**.
14. In **Package.appxmanifest**onder **eigenschappen**, Controleer **pushmelding**.
15. In **Package.appxmanifest**onder **App-instellingen**, type **PushBackgroundComponent.PushBackgroundTask** in de **toegangspunt** het veld.

    ![][13]
16. Klik in het menu **Bestand** op **Alles opslaan**.

## <a name="run-the-application"></a>De toepassing wordt uitgevoerd

Voor het uitvoeren van de toepassing, het volgende doen:

1. Voer in Visual Studio de **AppBackend** Web-API-toepassing. Een ASP.NET-webpagina wordt weergegeven.
2. Voer in Visual Studio de **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app. De Windows Phone-emulator wordt uitgevoerd en wordt de app automatisch geladen.
3. In de **NotifyUserWindowsPhone** app-gebruikersinterface, voer een gebruikersnaam en wachtwoord. Dit kunnen een willekeurige tekenreeks zijn, maar ze moeten dezelfde waarde.
4. In de **NotifyUserWindowsPhone** app-gebruikersinterface, klikt u op **aanmelden en registreren**. Klik vervolgens op **pushmelding verzonden**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
