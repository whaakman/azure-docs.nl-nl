#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Het iOS-project in Xamarin Studio configureren
1. Open in Xamarin.Studio, **Info.plist**, en werk de **bundel-id** met de bundel-ID die u eerder hebt gemaakt met uw nieuwe app-ID.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Schuif omlaag naar **Achtergrondmodi**. Selecteer de **Enable Background Modes** vak en de **Remote notifications** vak.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Dubbelklik op het project in het deelvenster oplossing openen **Projectopties**.
4. Onder **bouwen**, kies **iOS bundel ondertekening**, en selecteer de identiteit van de bijbehorende inrichting profiel u zojuist hebt ingesteld om voor dit project.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor het ondertekenen van programmacode. Zie voor de officiële Xamarin apparaat inrichten documentatie, [Xamarin apparaten inrichten].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Het iOS-project in Visual Studio configureren
1. Met de rechtermuisknop op het project in Visual Studio en klik vervolgens op **eigenschappen**.
2. Klik in de eigenschappenpagina's, op de **iOS toepassing** tabblad, en werk de **id** met de ID die u eerder hebt gemaakt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. In de **iOS bundel ondertekening** tabblad, selecteert u de identiteit van de bijbehorende en inrichtingsprofiel u net hebt ingesteld voor dit project.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dit zorgt ervoor dat het project wordt gebruikt voor het nieuwe profiel voor het ondertekenen van programmacode. Zie voor de officiële Xamarin apparaat inrichten documentatie, [Xamarin apparaten inrichten].
4. Dubbelklik op Info.plist te openen en schakel vervolgens **RemoteNotifications** onder **Achtergrondmodi**.

[Xamarin apparaten inrichten]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
