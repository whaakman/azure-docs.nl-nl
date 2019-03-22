---
title: 'Zelfstudie: Maak gebruikersstromen - Azure Active Directory B2C | Microsoft Docs'
description: Informatie over het maken van de gebruikersstromen voor uw toepassingen in Azure Active Directory B2C met behulp van de Azure portal.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/01/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b76d56b17973c04992a8855917c814ced649bd0f
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58338255"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Zelfstudie: Gebruikersstromen maken in Azure Active Directory B2C

In uw toepassingen, hebt u mogelijk [gebruikersstromen](active-directory-b2c-reference-policies.md) waarmee gebruikers zich aanmelden, meld u aan of hun profiel beheren. U kunt meerdere gebruikersstromen van verschillende typen in uw Azure Active Directory (Azure AD) B2C-tenant maken en deze gebruiken in uw toepassingen, indien nodig. Gebruikersstromen kunnen worden hergebruikt binnen toepassingen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een zich kunnen registreren en aanmelden gebruikersstroom maken
> * Een gebruikersstroom voor profielbewerking maken
> * Een gebruikersstroom voor het opnieuw instellen van het wachtwoord maken

Deze zelfstudie leert u hoe u enkele aanbevolen om gebruikersstromen te maken met behulp van de Azure-portal. Als u zoekt naar informatie over het instellen van een wachtwoordreferenties van resource-eigenaar (ROPC) flow in uw toepassing, Zie [configureren de wachtwoord-referenties van de resource-eigenaar voor flow in Azure AD B2C](configure-ropc.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

[Uw toepassingen registreren](tutorial-register-applications.md) die deel uitmaken van de gebruikersstromen die u wilt maken. 

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Een zich kunnen registreren en aanmelden gebruikersstroom maken

De gebruikersstroom zich kunnen registreren en aanmelden verwerkt zowel registratie en aanmelding bij ervaringen met een configuratie voor één. Gebruikers van uw toepassing worden op het juiste pad zijn afhankelijk van de context geleid.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.

    ![Schakel over naar de abonnementsmap](./media/tutorial-create-user-flows/switch-directories.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer in het menu links **gebruikersstromen**, en selecteer vervolgens **nieuwe gebruikersstroom**.

    ![Nieuwe gebruikersstroom selecteren](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

5. Selecteer de **zich kunnen registreren en aanmelden** gebruikersstroom op het tabblad aanbevolen.

    ![Selecteer de gebruikersstroom die zich kunnen registreren en aanmelden](./media/tutorial-create-user-flows/signup-signin-type.png)

6. Voer een **Naam** in voor de gebruikersstroom. Bijvoorbeeld, *signupsignin1*.
7. Voor **id-providers**, selecteer **e-mailregistratie**.

    ![De flow-eigenschappen instellen](./media/tutorial-create-user-flows/signup-signin-properties.png)

8. Voor **gebruikerskenmerken en claims**, kies de claims en de kenmerken die u wilt verzamelen en verzenden van de gebruiker tijdens de registratie. Selecteer bijvoorbeeld **meer weergeven**, en kies vervolgens **land/regio**, **weergavenaam**, en **postcode**. Klik op **OK**.

    ![Selecteer kenmerken en claims](./media/tutorial-create-user-flows/signup-signin-attributes.png)

9. Klik op **maken** om toe te voegen van de gebruikersstroom. Het voorvoegsel *B2C_1* wordt automatisch toegevoegd aan de naam.

### <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer op de pagina overzicht van de gebruikersstroom die u hebt gemaakt, **gebruikersstroom uitvoeren**.
2. Voor **toepassing**, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **uitvoeren gebruikersstroom**, en selecteer vervolgens **Meld u nu**.

    ![De gebruikersstroom uitvoeren](./media/tutorial-create-user-flows/signup-signin-run-now.png)

4. Voer een geldig e-mailadres, klikt u op **verificatiecode verzenden**, en voer de verificatiecode in die u ontvangt.
5. Voer een nieuw wachtwoord in en Bevestig het wachtwoord.
6. Voer de naam die u wilt weergegeven, selecteer uw land en regio, een postcode, en klik vervolgens op **maken**. Het token wordt geretourneerd naar `https://jwt.ms` en u moet worden weergegeven.
7. U kunt nu de gebruikersstroom opnieuw uitvoeren en u moet aanmelden met het account dat u hebt gemaakt. Het geretourneerde token bevat de claims die u hebt geselecteerd van de naam, land/regio en postcode.

## <a name="create-a-profile-editing-user-flow"></a>Een gebruikersstroom voor profielbewerking maken

Als u dat gebruikers wilt kunnen hun profiel in uw toepassing te bewerken, gebruikt u een beleid voor profielbewerking.

1. Selecteer in het menu links **gebruikersstromen**, en selecteer vervolgens **nieuwe gebruikersstroom**.
2. Selecteer de **profielbewerking** gebruikersstroom op het tabblad aanbevolen.
3. Voer een **Naam** in voor de gebruikersstroom. Bijvoorbeeld, *profileediting1*.
4. Voor **id-providers**, selecteer **aanmelden met lokaal Account**.
5. Voor **gebruikerskenmerken**, kiest u de kenmerken die u wilt dat de klant om te kunnen bewerken in het bijbehorende profiel. Selecteer bijvoorbeeld **meer weergeven**, en kies vervolgens **weergavenaam** en **functie**. Klik op **OK**.
6. Klik op **maken** om toe te voegen van de gebruikersstroom. Het voorvoegsel *B2C_1* wordt automatisch toegevoegd aan de naam.

### <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer op de pagina overzicht van de gebruikersstroom die u hebt gemaakt, **gebruikersstroom uitvoeren**.
2. Voor **toepassing**, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **gebruikersstroom uitvoeren**, en meld u aan met het account dat u eerder hebt gemaakt.
4. U hebt nu de mogelijkheid om de titel van de weergegeven naam en de taak voor de gebruiker wijzigen. Klik op **Doorgaan**. Het token wordt geretourneerd naar `https://jwt.ms` en u moet worden weergegeven.

## <a name="create-a-password-reset-user-flow"></a>Een gebruikersstroom voor het opnieuw instellen van het wachtwoord maken

Het is mogelijk dat u om in te schakelen van de gebruiker van uw toepassing in hun wachtwoord opnieuw instellen, indien nodig. Als u wilt inschakelen voor wachtwoord opnieuw instellen, moet u een wachtwoord opnieuw instellen van de gebruikersstroom gebruiken.

1. Selecteer in het menu links **gebruikersstromen**, en selecteer vervolgens **nieuwe gebruikersstroom**.
2. Selecteer de **wachtwoordherstel** gebruikersstroom op het tabblad aanbevolen.
3. Voer een **Naam** in voor de gebruikersstroom. Bijvoorbeeld, *passwordreset1*.
4. Voor **id-providers**, inschakelen **wachtwoord opnieuw instellen met e-mailadres**.
5. Klik onder toepassingsclaims, op **meer weergeven** en kies de claims die u laten retourneren in de autorisatietokens die is verzonden naar de toepassing wilt. Selecteer bijvoorbeeld **Object-ID van gebruiker**.
6. Klik op **OK**.
7. Klik op **maken** om toe te voegen van de gebruikersstroom. Het voorvoegsel *B2C_1* wordt automatisch toegevoegd aan de naam.

### <a name="test-the-user-flow"></a>De gebruikersstroom testen

1. Selecteer op de pagina overzicht van de gebruikersstroom die u hebt gemaakt, **gebruikersstroom uitvoeren**.
2. Voor **toepassing**, selecteert u de web-App met de naam *webapp1* die u eerder hebt geregistreerd. De **antwoord-URL** moet worden weergegeven `https://jwt.ms`.
3. Klik op **gebruikersstroom uitvoeren**, en meld u aan met het account dat u eerder hebt gemaakt.
4. U hebt nu de mogelijkheid om het wachtwoord voor de gebruiker te wijzigen. Klik op **Doorgaan**. Het token wordt geretourneerd naar `https://jwt.ms` en u moet worden weergegeven.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een zich kunnen registreren en aanmelden gebruikersstroom maken
> * Een gebruikersstroom voor profielbewerking maken
> * Een gebruikersstroom voor het opnieuw instellen van het wachtwoord maken

> [!div class="nextstepaction"]
> [Id-providers toevoegen aan uw toepassingen in Azure Active Directory B2C](tutorial-add-identity-providers.md)