---
title: Azure Mobile Engagement demo-app | Microsoft Docs
description: Hierin wordt beschreven waar u wilt downloaden, gebruiken en de voordelen van het gebruik van Azure Mobile Engagement demo-app
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
ms.openlocfilehash: 8381edb569e19a85c1259f7791b477cfa6e51ea3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement demo-app
We een Azure Mobile Engagement-demo-app voor hebt gepubliceerd **iOS**, **Android**, en **Windows** platforms om te zoeken naar nuttige informatiebronnen en meer informatie over de Mobile Engagement.

De app helpt u bij:

* Gemakkelijk nuttige koppelingen tot Mobile Engagement-bronnen zoals video's, documentatie, de ondersteuningsforum en verder verhogen functieaanvragen vinden.
* Ervaring voorbeeld meldingen die worden ondersteund door de Mobile Engagement voor ideeën voor uw eigen mobiele toepassingen.
* Een referentie-implementatie gebruiken voor de analyse van het implementeren van Mobile Engagement in uw eigen app. U leert:
  
  * Analytische gegevens verzamelen.
  * Implementeer ingewikkelde melding van typen zoals *volledig scherm verspreide* of *pop-*.
  * Enquêtes en polls geïmplementeerd.
  * Gegevens en push achtergrond push-scenario's implementeren.   

## <a name="app-installation"></a>App-installatie
Deze app is beschikbaar in de volgende app worden opgeslagen:

* **Universele Windows-demo-app**:
  
  * Download de app op de [Windows-App store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
  * De app is ontwikkeld als een Windows 10 Universal-app. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).
* **iOS app demo**:
  
  * Download de app op de [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
  * De app is ontwikkeld iOS Swift. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).
* **Android demo-app**:
  
  * Download de app op de [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
  * De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Universele Windows-demo-app][1]

![iOS app demo][2]
![Android demo-app][3]

## <a name="usage"></a>Gebruik
U kunt deze app in de volgende manieren gebruiken:

**De app op uw apparaat downloaden van de toepassing store koppelingen (eerder):**

> [!IMPORTANT]
> U hoeft niet een Azure-account of moet de app verbinden met een back-end. De app werkt onafhankelijk van elkaar.
> 
> 

* Nadat u de app op uw apparaat hebt, kunt u de koppelingen in het menu aan de linkerkant de nuttige informatiebronnen over de Mobile Engagement doorlopen.
* Toegevoegd de [van service RSS-feed](https://aka.ms/azmerssfeed) in deze toepassing zodat u altijd over de meest recente productupdates zijn bijgewerkt.
* U kunt ook gaan via de melding voorbeeldscenario's gebruikmaken van het type van de meldingen die worden ondersteund door de Mobile Engagement voor elk platform. Deze meldingen kunnen worden ervaren lokaal--die is, kunt u op de knoppen op de schermen, zodat u de meldingen-ervaring identiek is aan de meldingen verzenden vanuit de Mobile Engagement-platform.

![App-menu voor Windows][4]

![App-menu voor iOS][5]
![App-menu voor Android][6]

**Download de broncode van de GitHub koppelingen (eerder):**

* Nadat u de broncode hebt gedownload, opent u het in de respectieve ontwikkelomgeving--XCode voor iOS, Android Studio voor Android- en Visual Studio voor Windows.
* U moet vervolgens volgen onze [basisstappen voor SDK-integratie](mobile-engagement-windows-store-dotnet-get-started.md) zodat u kunt deze app kunt koppelen aan een afzonderlijk exemplaar van de back-end Mobile Engagement.
  * U moet een verbindingsreeks configureren in de app.
  * U moet ook de push notification-platform voor uw app te configureren.
* U zult zien dat de app zelf is uitgerust met Mobile Engagement. Dus als u de app opent na verbinding maken met de back-end, u zult kunnen zien van de gebruikerssessie, activiteiten, gebeurtenissen, enzovoort, op de **Monitor** tabblad.
* U zult ook mogelijk om meldingen te verzenden naar deze app van uw eigen exemplaar van de Mobile Engagement, in plaats van lokale meldingen.
  
  * Hier kunt u uw apparaat als een testapparaat toevoegen met behulp van de **de apparaat-ID ophalen** menu-item in de app. Dit biedt u een apparaat-ID die u vervolgens als een testapparaat met uw exemplaar van de back-end-platform registreren.
    
    ![Apparaat-ID in Windows][7]
    
    ![Apparaat-ID op iOS][8]
    ![op Android-apparaat-ID][9]

## <a name="key-features-of-the-demo-app"></a>Belangrijke functies van de demo-app
* Zoals eerder gezegd met deze app hebt u de belangrijkste resources voor Mobile Engagement in uw hand. U kunt gaan via de koppelingen in het menu links.
* U kunt ervaren out-van-app-meldingen voor elk platform. Deze meldingen kunnen worden geleverd als **alleen melding**, waar te klikken op de melding gewoon creëert een systeemeigen scherm van de toepassing (met behulp van **grondige koppelen**)-- of als een **Web aankondiging**, waarbij u extra HTML-inhoud van de back-end van Mobile Engagement leveren kunt moet worden weergegeven wanneer de melding wordt geklikt.
  
    ![Out-van-app-meldingen][29]
* Op iOS hebt u de app om te zien van de pushmeldingen out van de toepassing of het systeem te sluiten. U kunt zoeken op de implementatie voor het toevoegen van **bewerkingsknoppen**, zoals de waarden die worden toegevoegd aan deze melding out van de toepassing voor *Feedback* en *Share* (zodat de gebruiker actie rechts van de melding zelf uitvoeren kan).
  
    ![Out-van-app-meldingen op iOS][11] ![Out-van-app-melding worden weergegeven op iOS][14]
* Op Android-, de opties die worden ondersteund met meerdere regels tekst toevoegt (**Big tekst**) of een installatiekopie van een melding (**grote afbeelding**) op de melding samen met de **bewerkingsknoppen** (zoals ondersteund door iOS).
  
    ![Out-van-app-meldingen op Android][12] ![Out-van-app-melding worden weergegeven op Android][15]
* Op Windows 10 ziet u hoe de meldingen eruit zien op de PC. Deze melding wordt ook weergegeven in het Windows 10- **Meldingencentrum**. Er is geen ondersteuning voor het toevoegen van **bewerkingsknoppen** op het moment dat in de Windows SDK.
  
    ![Out-van-app-meldingen op Windows][10] ![Out van de toepassing worden weergegeven op Windows][13]
* U kunt ervaren standaard 'in-app' meldingen voor elk platform. Dit is een ervaring in twee stappen waarbij een **melding** venster als eerste wordt weergegeven. Wanneer u erop klikt, wordt geopend om een volledig scherm **aankondiging**, zoals weergegeven in de volgende schermafbeelding. De inhoud van deze aankondiging afkomstig van uw Mobile Engagement back-end-exemplaar. De SDK bevat de sjablonen voor meldingen en aankondigingen. U kunt ze gemakkelijk aanpassen zoals weergegeven in deze demo-app met de toevoeging van onze logo en kleuren.  
  
    ![In-app-meldingen op Windows][16]
  
    ![In-app-meldingen op iOS][17]  ![In-app-meldingen op Android][18]
  
    **iOS**, **Android**
* U kunt ook de **categorie** functie van Mobile Engagement voor het aanpassen van deze standaard-ervaring. In de demo-app hebt we twee algemene manieren om te wijzigen van de ervaring van de meldingen gedemonstreerd. Houd er rekening mee dat de categorie-functie is nog niet ondersteund in de Windows SDK.
  
    **Volledig scherm verspreide:**
  
    ![In het app-melding - verspreide categorie][30]
  
    ![Verspreide categorie op iOS][21]     ![Verspreide categorie voor Android][22]
  
    **Pop-upbericht:**
  
    ![In het app-melding - pop-categorie][31]
  
    ![Pop-upbericht op iOS][19]    ![Pop-upbericht op Android][20]

**iOS**, **Android**

* Mobile Engagement biedt ook ondersteuning voor een speciaal type melding in de app aangeroepen **Polls**. Hiermee kunt u snel enquêtes verzenden naar uw gesegmenteerde app-gebruikers. U kunt vragen en opties voor elke vraag zoals in de volgende schermafbeelding toevoegen. Dit wordt vervolgens als een in-app-melding naar de app-gebruiker worden weergegeven.   
  
    ![Poll-meldingen][32]
  
    ![Overzicht van Windows][26]
  
    ![Enquête op iOS][27]   ![Onderzoek op Android][28]

**iOS**, **Android**

* Mobile Engagement biedt ook ondersteuning voor verzenden van achtergrond **gegevens Push** meldingen. U kunt met deze meldingen gegevens verzenden van uw service (zoals de JSON-gegevens in het volgende voorbeeld), die u kunt verwerken in uw app en een bepaalde actie ondernemen. Een voorbeeld is hoe we wijzigt de prijs van een item selectief via pushmeldingen gegevens.
  
    ![Gegevens-push-melding][33]
  
    ![Gegevens-push-melding in Windows][23]
  
    ![Gegevens van pushmeldingen in iOS][24]  ![Gegevens-push-melding op Android][25]

**iOS**, **Android**

> [!NOTE]
> U kunt gedetailleerde stapsgewijze instructies voor een van deze meldingen weergeven door te klikken op **Klik hier voor instructies over het verzenden van deze meldingen van Mobile Engagement-platform** op een melding voorbeeldscherm.
> 
> 

[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
