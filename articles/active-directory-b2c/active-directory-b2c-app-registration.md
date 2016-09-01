<properties
    pageTitle="Azure Active Directory B2C: toepassingsregistratie | Microsoft Azure"
    description="Uw toepassing registreren met Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/22/2016"
    ms.author="swkrish"/>


# Azure Active Directory B2C: uw toepassing registreren

## Vereiste

Als u een toepassing wilt maken waarin consumenten zich kunnen registreren en aanmelden, moet u de toepassing eerst registreren bij een Azure Active Directory B2C-tenant. Haal uw eigen tenant op aan de hand van de stappen in [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md). Als u alle stappen in dit artikel hebt uitgevoerd, is de blade B2C-functies vastgemaakt aan uw Startboard.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Ga naar de blade B2C-functies

Als u de blade B2C-functies hebt vastgemaakt aan uw Startboard, ziet u de blade zodra u zich aanmeldt bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van de B2C-tenant.

U kunt de blade ook openen door achtereenvolgens op **Bladeren** en **Azure AD B2C** te klikken in het linkernavigatiedeelvenster in de [Azure Portal](https://portal.azure.com/).

> [AZURE.IMPORTANT] U moet een globale beheerder van de B2C-tenant zijn om de blade B2C-functies te kunnen openen. Een globale beheerder van andere tenant of een gebruiker van een tenant heeft hiertoe geen toegang.  U kunt overschakelen naar uw B2C-tenant met behulp van de tenantwisselaar in de rechterbovenhoek van de Azure Portal.

## Een toepassing registreren

1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
2. Klik op **+Toevoegen** boven aan de blade.
3. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld Contoso B2C-app opgeven.
4. Als u een webtoepassing schrijft, stelt u de schakeloptie **Web-app/web-API opnemen** in op **Ja**. De **Antwoord-URL's** zijn eindpunten waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Geef bijvoorbeeld `https://localhost:44321/` op. Als uw webtoepassing ook web-API’s aanroept die beveiligd zijn door Azure AD B2C, kunt het best **toepassingsgeheim** maken door te klikken op de knop **Sleutel genereren**.
5. Als u een mobiele toepassing maakt, stelt u de schakeloptie **Systeemeigen client opnemen** in op **Ja**. Noteer de standaard**omleidings-URI** die automatisch wordt gemaakt.
6. Klik op **Maken** om uw toepassing te registreren.
7. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.

> [AZURE.NOTE] Een **toepassingsgeheim** is een belangrijke beveiligingsreferentie en moet op de juiste wijze worden beveiligd.

## Een snelstarttoepassing maken

Nu u een toepassing hebt die is geregistreerd bij Azure AD B2C, kunt u snel aan de slag gaan aan de hand van een van onze snelstartzelfstudies. Hier volgen enkele aanbevelingen:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!--HONumber=ago16_HO4-->


