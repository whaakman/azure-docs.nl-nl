---
ms.openlocfilehash: a69df0cc9ea14a2c9fa172c77663afb1d6861f9b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097529"
---
#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Het iOS-project in Xamarin Studio configureren
1. Open in Xamarin.Studio, **Info.plist**, en werk de **bundel-id** met de bundel-ID die u eerder hebt gemaakt met uw nieuwe app-ID.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Schuif omlaag naar **Achtergrondmodi**. Selecteer de **Enable Background Modes** vak en de **Remote notifications** vak.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Dubbelklik op uw project in het deelvenster van de oplossing openen **Projectopties**.
4. Onder **bouwen**, kiest u **iOS-bundel ondertekening**, selecteert u de overeenkomstige id en het inrichtingsprofiel voor u zojuist hebt ingesteld om voor dit project.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Dit zorgt ervoor dat het project gebruikmaakt van het nieuwe profiel voor het ondertekenen van code. Zie voor de officiële Xamarin voor apparaatinrichting documentatie, [Xamarin voor Apparaatinrichting].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Configureren van het iOS-project in Visual Studio
1. Met de rechtermuisknop op het project in Visual Studio en klik vervolgens op **eigenschappen**.
2. Klik in de eigenschappen van pagina's, op de **iOS-toepassing** tabblad en werk de **id** met de ID die u eerder hebt gemaakt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. In de **iOS-bundel ondertekening** tabblad, selecteert u de identiteit van de bijbehorende en inrichtingsprofiel dat u net hebt ingesteld voor dit project.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dit zorgt ervoor dat het project gebruikmaakt van het nieuwe profiel voor het ondertekenen van code. Zie voor de officiële Xamarin voor apparaatinrichting documentatie, [Xamarin voor Apparaatinrichting].
4. Dubbelklik op Info.plist te openen en schakel vervolgens **RemoteNotifications** onder **Achtergrondmodi**.

[Xamarin voor Apparaatinrichting]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/