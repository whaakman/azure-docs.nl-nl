---
title: Zelfstudie - Register een toepassing - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het registreren van een web-App in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 02/05/2019
ms.author: davidmu
ms.openlocfilehash: 1f9a4f2f0ac44c8815e33957b49b4215c998eae3
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55754163"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Zelfstudie: Een toepassing registreren in Azure Active Directory B2C

Voordat u uw [toepassingen](active-directory-b2c-apps.md) kan communiceren met Azure Active Directory (Azure AD) B2C, ze moeten worden geregistreerd in een tenant die u beheert. Deze zelfstudie leert u hoe u een web-App met behulp van de Azure-portal registreren.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een web-app registreren
> * Een clientgeheim maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit nog niet hebt gemaakt uw eigen [Azure AD B2C-Tenant](tutorial-create-tenant.md), Maak nu een. U kunt een bestaande Azure AD B2C-tenant.

## <a name="register-a-web-application"></a>Een web-app registreren

1. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
2. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
3. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
4. Voer een naam voor de toepassing. Bijvoorbeeld, *webapp1*.
5. Voor **inclusief web-app / web-API** en **impliciete stroom toestaan**, selecteer **Ja**.
6. Voor **antwoord-URL**, voer een eindpunt waar Azure AD B2C moet tokens retourneert die door uw toepassing worden aangevraagd. Bijvoorbeeld, kunt u dit instellen op het lokaal luisteren op `https://localhost:44316` als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen. Voor testdoeleinden kunt u deze instellen op `https://jwt.ms`, wordt de inhoud van een token voor inspectie. Voor deze zelfstudie, ingesteld op `https://jwt.ms`. 

    Het antwoord op de URL met het schema beginnen moet `https`, en alle antwoord-URL-waarden moeten één DNS-domein delen. Bijvoorbeeld, als de toepassing heeft een antwoord-URL van `https://login.contoso.com`, u kunt toevoegen aan deze net als deze URL `https://login.contoso.com/new`. Of u kunt verwijzen naar een DNS-subdomein van `login.contoso.com`, zoals `https://new.login.contoso.com`. Als u wilt dat een toepassing met `login-east.contoso.com` en `login-west.contoso.com` als antwoord-URL's, moet u deze antwoord-URL's in deze volgorde toevoegen: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste antwoord-URL, `contoso.com`.

7. Klik op **Create**.

## <a name="create-a-client-secret"></a>Een clientgeheim maken

Als u geen toepassing een code voor een token uitwisselt, moet u het toepassingsgeheim van een maken.

1. Selecteer **sleutels** en klik vervolgens op **sleutel genereren**.
2. Selecteer **opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt de waarde als het toepassingsgeheim in de code van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een web-app registreren
> * Een clientgeheim maken

> [!div class="nextstepaction"]
> [Gebruikersstromen maken in Azure Active Directory B2C](tutorial-create-user-flows.md)