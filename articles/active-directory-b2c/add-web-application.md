---
title: Toevoegen van een web-App - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het toevoegen van een web-App met uw Active Directory B2C-tenant.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: a1d098550c6fb733e088f8ad211d29f48f55d2d6
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511703"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Een web-API-toepassing naar uw Azure Active Directory B2C-tenant toevoegen

 Resources voor Web-API registreren in uw tenant, zodat ze kunnen accepteren en reageren op aanvragen door clienttoepassingen die een toegangstoken. In dit artikel wordt beschreven hoe u het registreren van de toepassing in Azure Active Directory (Azure AD) B2C.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant. Selecteer de **map- en abonnementsfilter** in het bovenste menu en kiest u de map waarin uw tenant.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld *webapi1*.
6. Selecteer voor **Inclusief webtoepassing/ web-API** en **Impliciete stroom toestaan** **Ja**.
7. Voer voor de **Antwoord-URL** een eindpunt in waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. In de productietoepassing, kunt u de antwoord-URL ingesteld op een waarde zoals `https://localhost:44332`. Voor testdoeleinden, stelt u de antwoord-URL op `https://jwt.ms`.
8. Voer voor **App-id-URI** de id in die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
9. Klik op **Create**.
10. Noteer op de eigenschappenpagina de toepassings-id die u gebruikt wanneer u de webtoepassing configureert.

## <a name="configure-scopes"></a>Bereiken configureren

Bereiken bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Bijvoorbeeld: gebruikers van de web-API kunnen zowel lees- als schrijftoegang hebben of alleen leestoegang. In deze zelfstudie gebruikt u bereiken om lees- en schrijfmachtigingen voor de web-API te definiëren.

1. Selecteer **Toepassingen** en vervolgens *webapi1*.
2. Selecteer **Gepubliceerde bereiken**.
3. Voer voor **Bereik** `Read` in en voor beschrijving `Read access to the application`.
4. Voer voor **Bereik** `Write` in en voor beschrijving `Write access to the application`.
5. Klik op **Opslaan**.

De gepubliceerde bereiken kunnen worden gebruikt om een clienttoepassingstoestemming te verlenen aan de web-API.

## <a name="grant-permissions"></a>Machtigingen verlenen

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw toepassing machtigingen geven voor de API. Bijvoorbeeld, in [Zelfstudie: Een toepassing registreren in Azure Active Directory B2C](tutorial-register-applications.md), een web-App is gemaakt in Azure AD B2C met de naam *webapp1*. Deze toepassing kunt u de web-API aanroepen.

1. Selecteer **toepassingen**, en selecteer vervolgens uw webtoepassing.
2. Selecteer **API-toegang** en vervolgens **Toevoegen**.
3. Selecteer *webapi1* in de vervolgkeuzelijst **API selecteren**.
4. In de **bereiken selecteren** vervolgkeuzelijst, selecteer de **lezen** en **schrijven** bereiken die u eerder hebt gedefinieerd.
5. Klik op **OK**.

Uw toepassing is geregistreerd voor het aanroepen van de beveiligde web-API. Een gebruiker voert een verificatie uit bij Azure AD B2C om de toepassing te kunnen gebruiken. De toepassing verkrijgt een autorisatietoekenning van Azure AD B2C voor toegang tot de beveiligde web-API.
