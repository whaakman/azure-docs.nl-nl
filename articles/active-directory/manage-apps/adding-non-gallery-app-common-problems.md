---
title: Probleem met het toevoegen van een toepassing buiten de galerie | Microsoft Docs
description: Informatie over veelvoorkomende problemen met mensen gezichten bij het toevoegen van aangepaste niet-galerietoepassingen
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: barbkess
ms.openlocfilehash: 56cf9837bf67187f71eb94e73b516b613b8d6c6a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259883"
---
# <a name="problem-adding-a-non-gallery-application"></a>Probleem met het toevoegen van een toepassing buiten de galerie

In dit artikel krijgt u inzicht in het gezicht voor mensen van algemene problemen bij het toevoegen van **aangepaste niet-galerietoepassingen** en wat u kunt doen om ze op te lossen. 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Ik op de knop 'toevoegen' geklikt en mijn toepassing duurde het lang om te worden weergegeven

Onder bepaalde omstandigheden, duurt het 1-2 minuten (en soms langer) voor een toepassing worden weergegeven na het toe te voegen aan uw directory. Hoewel dit niet de verwachte normale prestaties is, ziet u het toevoegen van de toepassing wordt uitgevoerd door te klikken op de **meldingen** pictogram (de klok) in de rechterbovenhoek van de [Azure-portal](https://portal.azure.com/) en bekijken voor een **In voortgang** of **voltooid** melding met het label **-toepassing maken**.

Als uw toepassing wordt nooit toegevoegd of er een fout optreedt wanneer u op de **toevoegen** knop, ziet u een **melding** in een **fout** staat. Als u meer informatie over de fout voor meer informatie op of delen met een ondersteuningsmedewerker wilt, kunt u meer informatie over de fout zien met de volgende stappen in de [hoe ziet u de details van een portalmelding](#how-to-see-the-details-of-a-portal-notification) sectie.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Ik op de knop 'toevoegen' geklikt en mijn toepassing niet weergegeven

Soms vanwege tijdelijke problemen toevoegen netwerkproblemen, of een bug van een toepassing is mislukt. Kunt u zien dat dit gebeurt wanneer u klikt op de **meldingen** pictogram (de klok) in de rechterbovenhoek van de Azure-portal en u ziet een rood (!) naast uw **-toepassing maken** melding. Dit geeft aan dat er is een fout opgetreden bij het maken van de toepassing.

Als u er een fout optreedt wanneer u op de **toevoegen** knop, ziet u een **melding** in een **fout** staat. Als u meer informatie over de fout voor meer informatie op of delen met een ondersteuningsmedewerker wilt, kunt u meer informatie over de fout zien met de volgende stappen in de [hoe ziet u de details van een portalmelding](#how-to-see-the-details-of-a-portal-notification) sectie.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Ik weet niet hoe ik het instellen van mijn toepassing nadat ik deze heb toegevoegd

Als u informatie over aangepaste toepassingen moet de [Azure AD-toepassingen-documentbibliotheek](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) kunt u voor meer informatie over eenmalige aanmelding met Azure AD en hoe het werkt.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Hoe ziet u de details van een portalmelding

U kunt de details van een portalmelding zien door de onderstaande stappen te volgen:

1.  Klik op de **meldingen** pictogram (de klok) in de rechterbovenhoek van de Azure-portal

2.  Selecteer een bericht in een **fout** status (die met een rode (!) ernaast).

   >[!NOTE]
   >U kunt niet klikken meldingen in een **geslaagd** of **In voortgang** staat.
   >
   >

4.  Gebruik de informatie onder **Meldingsdetails** om meer informatie over het probleem te begrijpen.

5.  Als u nog steeds hulp nodig hebt, kunt u ook deze informatie delen met een ondersteuningsmedewerker of de productgroep voor hulp bij het probleem.

6.  Klik op de **kopieerpictogram** aan de rechterkant van de **fout kopiëren** tekstvak kopiëren alle details van de melding om te delen met een medewerker van de groep ondersteuning of product.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Hulp krijgen door de details van melding verzenden naar een ondersteuningsmedewerker

Het is heel belangrijk dat u deelt **alle onderstaande details** met de ondersteuningstechnicus als u hulp nodig hebt, zodat deze u snel kunnen ondersteunen. U kunt dit gemakkelijk door doen **u een schermopname** of door te klikken op de **fout kopieerpictogram**, gevonden aan de rechterkant van de **fout kopiëren** tekstvak.

## <a name="notification-details-explained"></a>Details van melding uitgelegd

Zie de volgende beschrijvingen voor meer informatie over de meldingen.

### <a name="essential-notification-items"></a>Essentiële kennisgeving Items

-   **Titel** – de beschrijvende titel van de melding
   *  Voorbeeld: **Application proxy-instellingen**

-   **Beschrijving** : de beschrijving van wat is het gevolg van de bewerking

   *  Voorbeeld: **opgegeven interne url wordt al gebruikt door een andere toepassing**

-   **Meldings-ID** : de unieke ID van de melding

   *  Voorbeeld: **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Clientaanvraag-ID** – de specifieke aanvraag-ID die is gemaakt door uw browser

   *  Voorbeeld: **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Tijd stempel UTC** – de tijdstempel gedurende welke de melding is opgetreden, in UTC

   *  Voorbeeld: **2017-03-23T19:50:43.7583681Z**

-   **Interne transactie-ID** : de interne ID die we kunnen gebruiken voor de fout opzoeken in onze systemen

   *  Voorbeeld: **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – de gebruiker die de bewerking heeft uitgevoerd

   *  Voorbeeld: **tperkins@f128.info**

-   **Tenant-ID** : de unieke ID van de tenant die de gebruiker die de bewerking heeft uitgevoerd, een lid van is

   *  Voorbeeld: **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Object-ID gebruiker** : de unieke ID van de gebruiker die de bewerking heeft uitgevoerd

 *  Voorbeeld: **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Gedetailleerde melding Items

-   **Weergavenaam** – **(mag leeg zijn)** een meer gedetailleerde weergavenaam voor de fout

  *  Voorbeeld: **Application proxy-instellingen**

-   **Status** – de specifieke status van de melding

   *  Voorbeeld: **is mislukt**

-   **Object-ID** – **(mag leeg zijn)** de object-ID op basis waarvan de bewerking is uitgevoerd

   *  Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Details** : de gedetailleerde beschrijving van wat is het gevolg van de bewerking

   *  Voorbeeld: **interne url 'https://bing.com/' is ongeldig omdat deze al in gebruik**

-   **Fout kopiëren** : klik op de **kopieerpictogram** aan de rechterkant van de **fout kopiëren** tekstvak kopiëren alle details van de melding om te delen met een groep ondersteuning of product 
-   technicus

   *  Voorbeeld ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```




