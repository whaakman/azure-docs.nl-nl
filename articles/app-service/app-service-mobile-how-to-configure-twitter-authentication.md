---
title: Twitter-verificatie voor uw toepassing App Services configureren
description: Informatie over het Twitter-verificatie voor uw toepassing App Services configureren.
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: afde020b7817dc58ecea24eb4a09cf93d0986eb2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Het configureren van uw App Service-toepassing Twitter aanmelding gebruiken
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Dit onderwerp leest u over het configureren van Azure App Service voor het gebruik van Twitter als een verificatieprovider.

U moet een Twitter-account met een geverifieerde e-mailadres en telefoonnummer getal hebben voor de procedure in dit onderwerp. Voor het maken van een nieuw Twitter-account, gaat u naar <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"></a>Uw toepassing registreren met Twitter
1. Meld u aan bij de [Azure-portal], en navigeer naar uw toepassing. Kopieer uw **URL**. U gebruikt dit voor het configureren van uw app Twitter.
2. Navigeer naar de [Twitter ontwikkelaars] website, meld u aan met de referenties van uw Twitter-account en klikt u op **nieuwe App maken**.
3. Typ in het **naam** en een **beschrijving** voor uw nieuwe app. Plakken in uw toepassing **URL** voor de **Website** waarde. Vervolgens voor de **retouraanroep URL**, plak de **retouraanroep URL** u eerder hebt gekopieerd. Dit is de gateway van uw mobiele App toegevoegd aan het pad */.auth/login/twitter/callback*. Bijvoorbeeld `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Zorg ervoor dat u van het HTTPS-schema gebruikmaakt.
4. Lees en accepteer de voorwaarden aan de onderkant van de pagina. Klik vervolgens op **uw Twitter-toepassing maken**. Hiermee wordt de app wordt weergegeven de toepassingsgegevens.
5. Klik op de **instellingen** tabblad selectievakje **toe dat deze toepassing worden gebruikt voor het aanmelden met Twitter**, klikt u vervolgens op **Update-instellingen**.
6. Selecteer de **sleutels en toegangstokens** tabblad. Noteer de waarden van **Consumer-sleutel (API-sleutel)** en **consumentgeheim (API geheim)**.
   
   > [!NOTE]
   > De consument geheime sleutel is een belangrijke beveiligingsreferentie. Geen dit geheim met anderen delen of deze met uw app te distribueren.
   > 
   > 

## <a name="secrets"></a>Informatie Twitter toevoegen aan uw toepassing
1. Terug in de [Azure-portal], gaat u naar uw toepassing. Klik op **instellingen**, en vervolgens **verificatie / autorisatie**.
2. Als de verificatie / autorisatie-functie niet is ingeschakeld, schakelt u de schakeloptie voor **op**.
3. Klik op **Twitter**. Plak in het App-ID en het App-geheim waarden die u eerder hebt verkregen. Klik vervolgens op **OK**.
   
   ![][1]
   
   Standaard-App Service biedt verificatie maar wordt niet geautoriseerde toegang beperkt tot uw site-inhoud en API's. U moet gebruikers machtigen in uw app-code.
4. (Optioneel) Instellen om toegang te beperken tot uw site tot alleen gebruikers die zijn geverifieerd door Twitter, **te ondernemen actie wanneer de aanvraag is niet geverifieerd** naar **Twitter**. Dit vereist dat alle aanvragen worden geverifieerd en alle niet-geverifieerde aanvragen worden omgeleid naar Twitter voor verificatie.
5. Klik op **Opslaan**.

U bent nu klaar voor gebruik van Twitter voor verificatie in uw app.

## <a name="related-content"></a>Verwante inhoud
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter ontwikkelaars]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
