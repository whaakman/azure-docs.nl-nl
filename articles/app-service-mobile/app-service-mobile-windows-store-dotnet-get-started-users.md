---
title: Verificatie toevoegen aan uw Universal Windows Platform (UWP)-app | Microsoft Docs
description: 'Informatie over het gebruik van Azure App Service Mobile Apps voor verificatie van gebruikers van uw Universal Windows Platform (UWP)-app met verschillende identiteitsproviders, waaronder: AAD, Google, Facebook, Twitter en Microsoft.'
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: panarasi
editor: ''
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 4cc597f8aca13445034c8a1691b41018d4d9bc4b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38306571"
---
# <a name="add-authentication-to-your-windows-app"></a>Verificatie toevoegen aan uw Windows-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp ziet u hoe u cloud-gebaseerde authenticatie toevoegen aan uw mobiele app. In deze zelfstudie hebt toevoegen u verificatie aan de Universal Windows Platform (UWP)-Quick Start-project voor mobiele Apps met behulp van een id-provider die wordt ondersteund door Azure App Service. Na wordt is geverifieerd en geautoriseerd door de back-end van uw mobiele App, wordt de waarde van de gebruiker-ID weergegeven.

In deze zelfstudie is gebaseerd op de Mobile Apps quickstart. U moet eerst de zelfstudie hebt voltooid [aan de slag met Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie is vereist dat u een nieuwe URL-schema voor uw app definiëren. Hiermee kunt het verificatiesysteem terug te keren naar uw app nadat het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer uw App Service in de [Azure-portal].

2. Klik op de **verificatie / autorisatie** menu-optie.

3. In de **toegestane externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze reeks wordt het URL-schema voor uw mobiele toepassing.  Het moet normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en beginnen met een letter) volgen.  U moet een notitie van de tekenreeks die u kiest, aangezien u nodig hebt om aan te passen van de code van uw mobiele toepassing met de URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren dat anonieme toegang tot uw back-end is uitgeschakeld. Met de UWP-appproject instellen als het opstartproject, implementeren en uitvoeren van de app; Controleer of dat een niet-verwerkte uitzondering met een statuscode 401 (niet-gemachtigd) is gegenereerd nadat de app wordt gestart. Dit komt doordat de app probeert te krijgen tot uw mobiele App-Code als een niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu verificatie is vereist.

Vervolgens werkt u de app om gebruikers te verifiëren voordat u resources van uw App Service aanvraagt.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
1. In de UWP app bestand MainPage.xaml.cs-project en voeg het volgende codefragment toe:
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Deze code verifieert de gebruiker met een Facebook-aanmelding. Als u een id-provider dan Facebook gebruikt, wijzigt u de waarde van **MobileServiceAuthenticationProvider** boven aan de waarde voor de provider.
2. Vervang de **OnNavigatedTo()** methode in MainPage.xaml.cs. Vervolgens voegt u een **aanmelden** knop naar de app die verificatie wordt geactiveerd.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Het volgende codefragment toevoegen aan de MainPage.xaml.cs:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. Open het projectbestand MainPage.xaml, zoek het element dat definieert de **opslaan** knop en vervang deze door de volgende code:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. Voeg het volgende codefragment toe aan de App.xaml.cs:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. Package.appxmanifest bestand te openen, naar **declaraties**in **beschikbaar declaraties** vervolgkeuzelijst **Protocol** en klikt u op **toevoegen** knop. Nu configureren voor de **eigenschappen** van de **Protocol** declaratie. In **weergavenaam**, de naam die u wilt weergeven voor gebruikers van uw toepassing toevoegen. In **naam**, uw {url_scheme_of_your_app} toevoegen.
7. Druk op de F5-toets naar de app uitvoeren, klikt u op de **aanmelden** knop en meld u aan bij de app met uw gekozen identiteitsprovider. Wanneer de aanmelding geslaagd is, de app wordt uitgevoerd zonder fouten en kunt u uw back-end doorzoeken en updates aanbrengen in gegevens.

## <a name="tokens"></a>Het verificatietoken Store op de client
Het vorige voorbeeld hebt u een standaard geleerd aanmelden, waarvoor de client verbinding maken met zowel de id-provider en de App Service elke keer dat de app wordt gestart. Is deze methode niet alleen inefficiënt, u kunt uitvoeren naar gebruik-betrekking heeft problemen met veel klanten proberen op te starten app op hetzelfde moment. Een betere benadering is het verificatietoken dat is geretourneerd door uw App Service in de cache en probeer het gebruik van deze eerste voordat u een aanmelding op basis van een provider.

> [!NOTE]
> U kunt het token dat is uitgegeven door App Services, ongeacht of u van verificatie-client of de service worden beheerd gebruikmaakt in de cache. In deze zelfstudie maakt gebruik van beheerde service-verificatie.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u basisverificatie-zelfstudie hebt voltooid, kunt u doorgaan naar een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van ondersteuning van pushmeldingen aan uw app en het configureren van de backend voor mobiele apps voor gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het toevoegen van offlineondersteuning aan uw app met een back-end voor mobiele apps. Met offlinesynchronisatie kunnen eindgebruikers interactie aangaan met een mobiele app&mdash;gegevens weergeven, toevoegen of wijzigen&mdash;ook als er geen netwerkverbinding is.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
