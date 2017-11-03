---
title: Azure Mobile Engagement-gebruikersinterface - instellingen
description: Informatie over het beheren van de algemene instellingen van uw toepassing met behulp van Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 858f4cb4-14de-4bb5-826f-28cadbfc928b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af5c81df2b9f288161b38625d3ac2adde8fb195d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-the-global-settings-of-your-application"></a>Het beheren van de algemene instellingen van uw toepassing
De **instellingen** menuopties beschikbaar voor een toepassing zijn verschillend, afhankelijk van het platform van de toepassing en de machtigingen die u hebt gekregen voor de toepassing. De volgende instellingen zijn: Details, projecten, Native Pushbericht, Push snelheid, Tag (app-info) en commerciële druk is. De Tag (app-info) menuoptie van de sectie instellingen kan worden beheerd door uw toepassing (met behulp van de SDK) of door uw back-end (met de apparaat-API). 

> [!NOTE]
> Veel secties van de **Mobile Engagement** portal UI bevatten de **HELP weergeven** knop. Druk op deze knop om meer contextuele informatie over een sectie.
> 
> 

## <a name="details"></a>Details
Hiermee kunt u de naam en beschrijving wijzigen van uw toepassing, de eigenaar van uw toepassing en de rolmachtigingen van uw weergeven. 

Analytics-configuratie kunt u de weken op de starten dag en de bewaartijd in dag(en) weergeven of wijzigen.

  ![settings1][46]

## <a name="projects"></a>Projecten
Hiermee kunt u alle projecten die u wilt dat uw toepassing worden weergegeven in selecteren. 

U kunt ook zoeken naar een project en de naam, beschrijving, eigenaar en uw rolmachtigingen van elk project dat uw toepassing deel uit van maakt weergeven.

Zie voor meer informatie: [UI documentatie – Start][Link 13]

  ![settings3][48]

## <a name="native-push"></a>Native Pushberichten
Hiermee kunt u een nieuw certificaat of verwijderen en een bestaand certificaat voor gebruik met native pushberichten registreren. Native Pushberichten Azure Mobile Engagement te pushen naar uw toepassing op elk gewenst moment kunt zelfs wanneer deze niet wordt uitgevoerd. 

Na het opgeven van referenties of certificaten voor ten minste één Native Pushbericht service, kunt u 'Elke keer' bij het maken van Reach-campagnes en gebruik de parameter 'melder' in de PUSH-API.

### <a name="apple-push-notification-service-apns"></a>Apple Push Notification Service (APNS)
U moet uw certificaat registreren om in te schakelen Native Push via Apple Push Notification Service. U moet het type certificaat opgeven als (DEV) ontwikkeling of productie (PROD). U wordt vervolgens moet upload uw certificaat en het wachtwoord.

Zie voor meer informatie: [SDK-documentatie - iOS: het voorbereiden van uw aanvraag voor Apple pushmeldingen][Link 5]

![settings4][49]

### <a name="windows-push-notification-service-wpns"></a>Windows Push Notification Service (WPNS)
Als u native push via Windows Notification Service wilt inschakelen, moet u de referenties van uw toepassing opgeven. U moet uw pakket beveiligings-id (SID) en de geheime sleutel.

![settings5][50]

### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud Messaging voor Android (GCM)
Om in te schakelen Native Push via GCM, moet u de instructies van Google. Vervolgens moet u een eenvoudige API serversleutel plakken geconfigureerd zonder IP-beperkingen. Integratie met de SDK vereist voor Android v1.12.0 +.

Zie voor meer informatie: 

* [SDK-documentatie Android het integreren van GCM][Link 5]
* [Google GCM ontwikkelaarshandleiding](http://developer.android.com/guide/google/gcm/gs.html)

### <a name="amazon-device-messaging-for-android-adm"></a>Amazon Device Messaging for Android (ADM)
U moet Amazon opgeven zodat Native Push via ADM <OAuth credentials> die bestaan uit een Client-ID en Clientgeheim (integratie met SDK voor Android v2.1.0 + vereist).

Zie voor meer informatie: 

* [SDK-documentatie Android het integreren van ADM][Link 5]
* [Amazon ADM-documentatie voor ontwikkelaars](https://developer.amazon.com/sdk/adm/credentials.html#Getting)

![settings6][51]

## <a name="push-speed"></a>Pushsnelheid
Geeft de huidige pushsnelheid van uw toepassing en kunt u de pushsnelheid van uw toepassing definiëren.

  ![settings7][52]

## <a name="tag-app-info"></a>Tag (app-info)
![settings11][56]

## <a name="commercial-pressure"></a>Commerciële druk
![settings12][57]

## <a name="see-also"></a>Zie ook
* [Concepten][Link 6]
* [Troubleshooting Guide-Service][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

