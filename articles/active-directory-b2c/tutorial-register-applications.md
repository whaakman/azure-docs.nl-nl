---
title: Zelfstudie - Register een toepassing - Azure Active Directory B2C
description: Informatie over het registreren van een web-App in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 5c46d3153bdc5768836bce198af115f82e8469f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056279"
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

1. Zorg ervoor dat u de adreslijst gebruikt die uw Azure AD B2C-tenant bevat door te klikken op het **filter voor adreslijsten en abonnementen** in het bovenste menu en de adreslijst te kiezen waarin uw tenant zich bevindt.
2. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
3. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
4. Voer een naam in voor de toepassing. Bijvoorbeeld, *webapp1*.
5. Selecteer voor **Inclusief webtoepassing/ web-API** en **Impliciete stroom toestaan** **Ja**.
6. Voer voor de **Antwoord-URL** een eindpunt in waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Bijvoorbeeld, u kunt deze instellen op het lokaal luisteren op `https://localhost:44316`. Als u het poortnummer dat nog niet weet, kunt u een tijdelijke aanduiding opgeven en dit later wijzigen.

    Voor testdoeleinden, zoals in deze zelfstudie kunt u dit instellen op `https://jwt.ms` weergeven met de inhoud van een token voor inspectie. Voor deze zelfstudie stelt u de **antwoord-URL** naar `https://jwt.ms`.

    Het antwoord op de URL met het schema beginnen moet `https`, en alle antwoord-URL-waarden moeten één DNS-domein delen. Bijvoorbeeld, als de toepassing heeft een antwoord-URL van `https://login.contoso.com`, u kunt toevoegen aan deze net als deze URL `https://login.contoso.com/new`. Of u kunt verwijzen naar een DNS-subdomein van `login.contoso.com`, zoals `https://new.login.contoso.com`. Als u wilt dat een toepassing met `login-east.contoso.com` en `login-west.contoso.com` als antwoord-URL's, moet u deze antwoord-URL's in deze volgorde toevoegen: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste antwoord-URL, `contoso.com`.

7. Klik op **Create**.

## <a name="create-a-client-secret"></a>Een clientgeheim maken

Als uw toepassing een code voor een token uitwisselt, moet u het toepassingsgeheim van een maken.

1. In de **Azure AD B2C - toepassingen** pagina, selecteert u de toepassing die u hebt gemaakt, bijvoorbeeld *webapp1*.
2. Selecteer **sleutels** en selecteer vervolgens **sleutel genereren**.
3. Selecteer **opslaan** om de sleutel weer te geven. Noteer de waarde van **App-sleutel**. U gebruikt deze waarde als het toepassingsgeheim in de code van uw toepassing.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een web-app registreren
> * Een clientgeheim maken

Vervolgens informatie over het maken van gebruikersstromen om uw gebruikers te registreren, aanmelden en hun profielen beheren.

> [!div class="nextstepaction"]
> [Gebruikersstromen maken in Azure Active Directory B2C >](tutorial-create-user-flows.md)
