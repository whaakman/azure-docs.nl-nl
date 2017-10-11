---
title: Probleem met het toevoegen van een toepassing niet galerie | Microsoft Docs
description: Inzicht in het algemene problemen mensen vlak bij het toevoegen van aangepaste toepassingen voor niet-galerie
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: bb3fc7877f4e7cafc3904fc67abd87b897874d8a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="problem-adding-a-non-gallery-application"></a>Probleem met een niet-galerie-toepassing toevoegen

In dit artikel helpt u bij het begrijpen van de face voor mensen van algemene problemen bij het toevoegen van **aangepaste niet-galerie toepassingen** en wat u kunt doen om deze kunt oplossen. 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Ik op de knop 'add' geklikt en mijn toepassing lang duurde om te worden weergegeven

In sommige gevallen kan het 1-2 minuten duren voordat (en soms langer) voor een toepassing worden weergegeven nadat u deze toevoegt aan uw directory. Hoewel dit niet de verwachte normale prestaties, ziet u het toevoegen van de toepassing wordt uitgevoerd door te klikken op de **meldingen** pictogram (bel) in de rechterbovenhoek van de [Azure Portal](https://portal.azure.com/) en te zoeken voor een **In uitvoering** of **voltooid** melding met het label **-toepassing maken**.

Als uw toepassing wordt nooit toegevoegd of er een fout optreedt wanneer u op de **toevoegen** knop, ziet u een **melding** in een **fout** status. Als u meer informatie over de fout naar meer informatie over het of delen met een engingeer ondersteuning wilt, kunt u meer informatie over de fout kunt zien door de stappen in de [hoe ziet u de details van een portal melding](#how-to-see-the-details-of-a-portal-notification) sectie.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Ik op de knop 'add' geklikt en de toepassing worden niet weergegeven

Soms vanwege tijdelijke problemen toevoegen netwerkproblemen of een bug van een toepassing mislukt. Kunt u instellen dat dit gebeurt wanneer u klikt op de **meldingen** pictogram (bel) in de rechterbovenhoek van de Azure-Portal en u ziet een rood (!) naast uw **-toepassing maken** melding. Dit betekent dat er is een fout opgetreden bij het maken van de toepassing.

Als er een fout optreden wanneer u op de **toevoegen** knop, ziet u een **melding** in een **fout** status. Als u meer informatie over de fout naar meer informatie over het of delen met een engingeer ondersteuning wilt, kunt u meer informatie over de fout kunt zien door de stappen in de [hoe ziet u de details van een portal melding](#how-to-see-the-details-of-a-portal-notification) sectie.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Ik weet mijn toepassing instellen wanneer ik het toegevoegde niet

Als u meer informatie over aangepaste toepassingen hulp nodig de [Azure AD-toepassingen-documentbibliotheek](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) kunt u meer informatie over eenmalige aanmelding met Azure AD en hoe het werkt.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Hoe ziet u de details van een melding van de portal

De details van de bedrijfsportal meldingen kunt u zien door de onderstaande stappen te volgen:

1.  Klik op de **meldingen** pictogram (bel) in de rechterbovenhoek van de Azure Portal

2.  Selecteer een melding in een **fout** status (die met een rode (!) naast deze).

   >[!NOTE]
   >U kunt niet meldingen in op een **geslaagd** of **In uitvoering** status.
   >
   >

3.  Deze open de **Meldingsdetails** blade.

4.  Gebruik deze informatie zelf meer details over het probleem te begrijpen.

5.  Als u nog hulp nodig hebt, kunt u ook deze informatie delen met een ondersteuningstechnicus of de productgroep voor hulp bij het probleem.

6.  Klik op de **pictogram kopiëren** rechts van de **Kopieerfout** tekstvak voor het kopiëren van de Meldingsdetails te delen met een groep engineer van ondersteuning of product.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Help opvragen door gegevens meldingen verzenden naar een ondersteuningstechnicus

Het is heel belangrijk dat u delen **alle onderstaande details** met de ondersteuningstechnicus als u hulp nodig hebt zodat u ze kunt u snel. Dit kunt u doen door eenvoudig **u een schermopname** of door te klikken op de **foutpictogram kopiëren**vindt u rechts van de **Kopieerfout** textbox.

## <a name="notification-details-explained"></a>Details van de melding uitgelegd

De hieronder wordt uitgelegd meer wat elke van de melding betekent dat items en worden voorbeelden gegeven van elk van deze servers.

### <a name="essential-notification-items"></a>Melding van essentiële Items

-   **Titel** – de beschrijvende titel van de melding
   *  Voorbeeld: **Application proxy-instellingen**

-   **Beschrijving** – de beschrijving van wat is het gevolg van de bewerking

   *  Voorbeeld: **interne url ingevoerd wordt al gebruikt door een andere toepassing**

-   **Id van de melding** – de unieke id van de melding

   *  Voorbeeld: **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Aanvraag-Id van client** – specifieke aanvraag-id gemaakt door uw browser

   *  Voorbeeld: **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Stempel UTC-tijd** – de tijdstempel gedurende welke de melding is opgetreden, in UTC

   *  Voorbeeld: **2017-03-23T19:50:43.7583681Z**

-   **Interne transactie-Id** – de interne ID we de fout opzoeken in onze systemen kunnen gebruiken

   *  Voorbeeld: **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – de gebruiker die de bewerking uitgevoerd

   *  Voorbeeld:**tperkins@f128.info**

-   **Tenant-Id** – de unieke ID van de tenant die de gebruiker die de bewerking uitgevoerd lid van was

   *  Voorbeeld: **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Gebruikersobject-Id** – de unieke ID van de gebruiker die de bewerking uitgevoerd

 *  Voorbeeld: **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Gedetailleerde melding Items

-   **Weergavenaam** – **(kan niet leeg zijn)** een meer gedetailleerde weergavenaam voor de fout

  *  Voorbeeld: **Application proxy-instellingen**

-   **Status** – de specifieke status van de melding

   *  Voorbeeld: **is mislukt**

-   **Object-Id** – **(kan niet leeg zijn)** de object-ID op basis waarvan de bewerking is uitgevoerd

   *  Voorbeeld: **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Details** -het gedetailleerde beschrijving van wat is het gevolg van de bewerking

   *  Voorbeeld: **interne url 'http://bing.com/' is ongeldig omdat het zich al in gebruik**

-   **Fout kopiëren** : klik op de **pictogram kopiëren** rechts van de **Kopieerfout** tekstvak voor het kopiëren van de Meldingsdetails te delen met een groep engineer van ondersteuning of productupdates

   *  Voorbeeld```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)



