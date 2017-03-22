---
title: 'Azure Active Directory B2C: toepassingsregistratie | Microsoft Docs'
description: Uw toepassing registreren met Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: bryanla
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/13/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 541849501335fb25d96cffa81b8119adc158cdd7
ms.lasthandoff: 03/14/2017


---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: uw toepassing registreren

> [!IMPORTANT]
> Toepassingen die zijn gemaakt vanaf de blade Azure AD B2C in Azure Portal, moeten vanaf dezelfde locatie worden beheerd. Als u de B2C-toepassingen bewerkt met PowerShell of een andere portal, worden de toepassingen niet meer ondersteund en werken ze waarschijnlijk niet met Azure AD B2C.
> 
> 

## <a name="prerequisite"></a>Vereiste
Als u een toepassing wilt maken waarin consumenten zich kunnen registreren en aanmelden, moet u de toepassing eerst registreren bij een Azure Active Directory B2C-tenant. Haal uw eigen tenant op aan de hand van de stappen in [Een Azure AD B2C-tenant maken](active-directory-b2c-get-started.md). Als u alle stappen in dit artikel hebt uitgevoerd, is de blade B2C-functies vastgemaakt aan uw Startboard.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Ga naar de blade B2C-functies
Als u de blade B2C-functies hebt vastgemaakt aan uw Startboard, ziet u de blade zodra u zich aanmeldt bij de [Azure Portal](https://portal.azure.com/) als globale beheerder van de B2C-tenant.

U kunt de blade ook openen door te klikken op **Meer services** en vervolgens te zoeken op **Azure AD B2C** in het linkernavigatiedeelvenster in [Azure Portal](https://portal.azure.com/).

> [!IMPORTANT]
> U moet een globale beheerder van de B2C-tenant zijn om de blade B2C-functies te kunnen openen. Een globale beheerder van andere tenant of een gebruiker van een tenant heeft hiertoe geen toegang.  U kunt overschakelen naar uw B2C-tenant met behulp van de tenantwisselaar in de rechterbovenhoek van Azure Portal.
> 
> 

## <a name="register-a-web-application"></a>Een web-app registreren
1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
2. Klik op **+Toevoegen** boven aan de blade.
3. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld Contoso B2C-app opgeven.
4. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**. De **Antwoord-URL's** zijn eindpunten waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Geef bijvoorbeeld `https://localhost:44316/` op.
5. Klik op **Opslaan** om de toepassing te registreren.
6. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.


## <a name="register-a-web-api"></a>Een web-API registreren
1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
2. Klik op **+Toevoegen** boven aan de blade.
3. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld 'Contoso B2C-API' opgeven.
4. Zet de schakeloptie **Inclusief web-app/web-API** op **Ja**. De **Antwoord-URL's** zijn eindpunten waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. Geef bijvoorbeeld `https://localhost:44316/` op.
5. Klik op **Opslaan** om de toepassing te registreren.
6. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.


## <a name="register-a-mobilenative-application"></a>Een mobiele/native toepassing registreren
1. Klik op de blade B2C-functies in de Azure Portal op **Toepassingen**.
2. Klik op **+Toevoegen** boven aan de blade.
3. Geef een **Naam** op voor de toepassing waarmee deze wordt beschreven voor uw consumenten. U kunt bijvoorbeeld Contoso B2C-app opgeven.
4. Zet de schakeloptie **Inclusief native client** op **Ja**.
5. Voer bij **Omleidings-URI** een URI met een aangepast schema in. Typ bijvoorbeeld com.onmicrosoft.contoso.app-naam://redirect/pad. Zorg ervoor dat u een [goede omleidings-URI](#choosing-a-redirect-uri) kiest.
6. Klik op **Opslaan** om de toepassing te registreren.
7. Klik op de toepassing die u net hebt gemaakt en noteer de globaal unieke **toepassingsclient-id** die u later in uw code gebruikt.

### <a name="choosing-a-redirect-uri"></a>Een omleidings-URI kiezen
Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI voor mobiele/native toepassingen: 
* **Uniek**: het schema van de omleidings-URI moet voor elke toepassing uniek zijn. In ons voorbeeld (com.onmicrosoft.contoso.app-naam://redirect/pad) gebruiken we com.onmicrosoft.contoso.app-naam als het schema. We raden aan dit patroon te volgen. Als twee toepassingen hetzelfde schema delen, ziet de gebruiker een dialoogvenster voor het kiezen van een app. Als de gebruiker een onjuiste keuze maakt, mislukt de aanmelding. 
* **Volledig**: de omleidings-URI moet een schema en een pad hebben. Het pad moet ten minste één forward slash bevatten na het domein (zo werkt //contoso/ bijvoorbeeld wel en //contoso niet). 

## <a name="build-a-quick-start-application"></a>Een snelstarttoepassing maken
Nu u een toepassing hebt die is geregistreerd bij Azure AD B2C, kunt u snel aan de slag gaan aan de hand van een van onze snelstartzelfstudies. Hier volgen enkele aanbevelingen:

[!INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]


