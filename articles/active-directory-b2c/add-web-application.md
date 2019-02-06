---
title: Toevoegen van een web-App - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het toevoegen van een web-App met uw Active Directory B2C-tenant.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: c20f455a0a325dadd3eeeb77dea7026de4834c56
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757654"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Een web-API-toepassing naar uw Azure Active Directory B2C-tenant toevoegen

Web-API-resources moeten worden geregistreerd in uw tenant voordat ze kunnen accepteren en reageren op aanvragen van de beveiligde resource door clienttoepassingen die een toegangstoken.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
4. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
5. Voer een naam voor de toepassing. Bijvoorbeeld, *webapi1*.
6. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
7. Voor **antwoord-URL**, voer een eindpunt waar Azure AD B2C moet tokens retourneert die door uw toepassing worden aangevraagd. In deze zelfstudie, het voorbeeld lokaal wordt uitgevoerd en luistert op `https://localhost:44332`.
8. Voor **App ID URI**, voer de id die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
9. Klik op **Create**.
10. Noteer de toepassings-ID die u gebruikt wanneer u de web-App configureren op de eigenschappenpagina.

## <a name="configure-scopes"></a>Scopes configureren

Scopes bieden een manier om te bepalen de toegang tot beveiligde bronnen. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Bijvoorbeeld: gebruikers van de web-API kunnen zowel lees- als schrijftoegang hebben of alleen leestoegang. In deze zelfstudie gebruikt u bereiken om lees- en schrijfmachtigingen voor de web-API te definiëren.

1. Selecteer **toepassingen**, en selecteer vervolgens *webapi1*.
2. Selecteer **gepubliceerd bereiken**.
3. Voor **bereik**, voer `Hello.Read`, en voor beschrijving, voer `Read access to hello`.
4. Voor **bereik**, voer `Hello.Write`, en voor beschrijving, voer `Write access to hello`.
5. Klik op **Opslaan**.

De gepubliceerde bereiken kunnen worden gebruikt voor het verlenen van een client toepassing machtiging voor de web-API.

## <a name="grant-permissions"></a>Machtigingen verlenen

Als u wilt een beveiligde web-API aanroepen vanuit een toepassing, moet u uw om Toepassingsmachtigingen te verlenen tot de API. In de vereiste zelfstudie hebt u een webtoepassing in Azure AD B2C met de naam gemaakt *webapp1*. Deze toepassing kunt u de web-API aanroepen.

1. Selecteer **toepassingen**, en selecteer vervolgens uw webtoepassing.
2. Selecteer **API-toegang**, en selecteer vervolgens **toevoegen**.
3. In de **API selecteren** vervolgkeuzelijst, selecteer *webapi1*.
4. In de **bereiken selecteren** vervolgkeuzelijst, selecteer de **Hello.Read** en **Hello.Write** bereiken die u eerder hebt gedefinieerd.
5. Klik op **OK**.

Uw toepassing is geregistreerd voor het aanroepen van de beveiligde web-API. Een gebruiker wordt geverifieerd met Azure AD B2C om de toepassing te gebruiken. De toepassing verkrijgt een autorisatietoekenning van Azure AD B2C voor toegang tot de beveiligde web-API.