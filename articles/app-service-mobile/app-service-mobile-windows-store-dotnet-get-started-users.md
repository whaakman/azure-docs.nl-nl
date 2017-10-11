---
title: Verificatie toevoegen aan uw app Universal Windows Platform (UWP) | Microsoft Docs
description: 'Informatie over het gebruik van Azure App Service Mobile Apps voor verificatie van gebruikers van uw Universal Windows Platform (UWP)-app met een aantal identiteitsproviders, waaronder: AAD, Google, Facebook, Twitter en Microsoft.'
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 47da343d4ec956ec2e669757f56e853675f887a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-windows-app"></a>Verificatie toevoegen aan uw Windows-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Dit onderwerp leest u hoe u cloud-gebaseerde verificatie toevoegen aan uw mobiele app. In deze zelfstudie maakt toevoegen u verificatie aan de Universal Windows Platform (UWP)-Quick Start-project voor mobiele Apps met behulp van een id-provider die wordt ondersteund door Azure App Service. Na wordt is geverifieerd en gemachtigd door uw back-end voor de mobiele App, wordt de waarde van de gebruiker-ID weergegeven.

Deze zelfstudie is gebaseerd op de Snelstartgids Mobile Apps. U moet eerst Voltooi de zelfstudie [aan de slag met Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Uw app registreren voor verificatie en de App Service configureren
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uw app toevoegen aan de toegestane externe Omleidings-URL 's

Veilige verificatie vereist dat u een nieuwe URL-schema voor uw app definiëren. Hierdoor kan de verificatiesysteem terug te keren naar uw app zodra het verificatieproces voltooid is. In deze zelfstudie gebruiken we het URL-schema _appname_ in. U kunt echter een URL-schema dat u kiest. Deze moet uniek zijn voor uw mobiele App. De omleiding op de server inschakelen:

1. Selecteer in de [Azure-portal] uw App Service.

2. Klik op de **verificatie / autorisatie** menuoptie.

3. In de **toegestaan externe Omleidings-URL's**, voer `url_scheme_of_your_app://easyauth.callback`.  De **url_scheme_of_your_app** in deze tekenreeks wordt het URL-schema voor uw mobiele toepassing.  Deze moet voldoen aan de normale URL-specificatie voor een protocol (Gebruik letters en cijfers alleen en begin met een letter).  U moet een notitie van de tekenreeks die u naar wens aanpassen van uw mobiele toepassingscode met het URL-schema op verschillende plaatsen.

4. Klik op **OK**.

5. Klik op **Opslaan**.

## <a name="permissions"></a>Machtigingen beperken voor geverifieerde gebruikers
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kunt u controleren of anonieme toegang tot uw back-end is uitgeschakeld. Met de UWP-appproject ingesteld als het opstartproject, implementeren en uitvoeren van de app; Controleer of dat een niet-verwerkte uitzondering met een statuscode van 401 (niet-geautoriseerd) treedt op nadat de app wordt gestart. Dit komt doordat de app probeert te krijgen tot uw mobiele App-Code als een niet-geverifieerde gebruiker, maar de *TodoItem* tabel nu is verificatie vereist.

Vervolgens kunt u de app om gebruikers te verifiëren voordat u resources van uw App Service wordt bijgewerkt.

## <a name="add-authentication"></a>Verificatie toevoegen aan de app.
1. In de UWP app bestand MainPage.xaml.cs project en voeg het volgende codefragment toe:
   
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
   
    Deze code verifieert de gebruiker met een Facebook-aanmelding. Als u van een id-provider dan Facebook gebruikmaakt, wijzigt u de waarde van **MobileServiceAuthenticationProvider** boven aan de waarde voor de provider.
2. Vervang de **OnNavigatedTo()** methode in MainPage.xaml.cs. Vervolgens voegt u een **aanmelden** knop naar de app waarmee de verificatie wordt geactiveerd.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Het volgende codefragment aan MainPage.xaml.cs toevoegen:
   
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
5. Het volgende codefragment toevoegen aan de App.xaml.cs:

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
6. Package.appxmanifest bestand openen, gaat u naar **declaraties**in **beschikbaar declaraties** vervolgkeuzelijst, selecteer **Protocol** en klik op **toevoegen** knop. Nu configureren de **eigenschappen** van de **Protocol** declaratie. In **weergavenaam**, de naam die u wilt weergeven aan gebruikers van uw toepassing toevoegen. In **naam**, uw {url_scheme_of_your_app} toevoegen.
7. Druk op F5 bij het uitvoeren van de app en klik op de **aanmelden** knop en meld u aan bij de app met uw gekozen id-provider. Nadat de aanmeldingspagina geslaagd is, de app wordt uitgevoerd zonder fouten en kunt u uw back-end doorzoeken en updates aanbrengen in de gegevens.

## <a name="tokens"></a>Het verificatietoken opslaan op de client
Het vorige voorbeeld blijkt een standaard aanmelden, die vereist dat de client te maken met zowel de id-provider en de App Service elke keer dat de app wordt gestart. Is deze methode niet alleen inefficiënt, u kunt uitvoeren in gebruik-betrekking heeft problemen moeten veel klanten proberen app starten op hetzelfde moment. Er is een betere benadering voor het verificatietoken dat wordt geretourneerd door de Service van uw App in de cache en probeer het eerst moet worden gebruikt deze voordat u een provider gebaseerde aanmelden.

> [!NOTE]
> U kunt het token dat is uitgegeven door App Services, ongeacht of u van verificatie van client beheerd of service gebruikmaakt-cache. Deze zelfstudie maakt gebruik van authentication service beheerd.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u deze basisverificatie-zelfstudie hebt voltooid, overweeg dan u verder gaat u aan bij een van de volgende zelfstudies:

* [Pushmeldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van ondersteuning van pushmeldingen aan uw app en het configureren van de backend voor mobiele apps voor gebruik van Azure Notification Hubs voor het verzenden van pushmeldingen.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informatie over het toevoegen van offlineondersteuning aan uw app met een back-end voor mobiele apps. Met offlinesynchronisatie kunnen eindgebruikers interactie aangaan met een mobiele app&mdash;gegevens weergeven, toevoegen of wijzigen&mdash;ook als er geen netwerkverbinding is.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
