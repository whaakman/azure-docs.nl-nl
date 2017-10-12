---
title: Aan de slag met Microsoft Azure Multi-Factor Auth Provider | Microsoft Docs
description: Informatie over het maken van een Azure Multi-Factor Authentication-provider.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: ed14a5a762bab20a1ccde699504dd21f25009b52
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Aan de slag met een Azure Multi-Factor Authentication-provider
Verificatie in twee stappen is standaard beschikbaar voor globale beheerders van Azure Active Directory- en Office 365-gebruikers. Als u echter wilt profiteren van [geavanceerde functies](multi-factor-authentication-whats-next.md), moet u de volledige versie van Azure Multi-Factor Authentication (MFA) aanschaffen.

Een Azure Multi-Factor Authentication-provider wordt gebruikt om te kunnen profiteren van de functies van de volledige versie van Azure MFA. Deze is bestemd voor gebruikers die **geen licenties via Azure MFA, Azure AD Premium of Enterprise Mobility + Security (EMS)** hebben.  Azure MFA, Azure AD Premium en EMS bevatten standaard de volledige versie van Azure MFA. Als u licenties hebt, hebt u geen Azure Multi-Factor Authentication-provider nodig.

Een Azure Multi-Factor Authentication-provider is vereist als u de SDK wilt downloaden.

> [!IMPORTANT]
> Als u de SDK wilt downloaden, moet u een Azure Multi-Factor Authentication-provider maken, zelfs als u Azure MFA, AAD Premium of EMS-licenties hebt.  Als u een Azure Multi-Factor Authentication-provider voor dit doeleinde maakt en al licenties hebt, moet u ervoor zorgen dat u de provider maakt met het model **Per ingeschakelde gebruiker**. Koppel de provider vervolgens aan de map die de licenties voor Azure MFA, Azure AD Premium of EMS bevat. Deze configuratie zorgt ervoor dat u alleen een rekening ontvangt als u meer unieke gebruikers hebt die verificatie in twee stappen gebruiken dan het aantal licenties dat u in uw bezit hebt.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Wat is een Azure Multi-Factor Authentication-provider?

Als u geen licenties voor Azure Multi-Factor Authentication hebt, kunt u een verificatieprovider maken om verificatie in twee stappen te vereisen voor uw gebruikers. Als u een aangepaste app ontwikkelt en Azure MFA wilt inschakelen, maakt u een verificatieprovider en [downloadt u de SDK](multi-factor-authentication-sdk.md).

Er zijn twee soorten verificatieproviders. Het verschil tussen beide heeft te maken met de wijze waarop uw Azure-abonnement in rekening wordt gebracht. De optie Per verificatie berekent het aantal verificaties die in een maand op uw tenant worden uitgevoerd. Deze optie wordt aanbevolen als u een aantal gebruikers hebt die slechts sporadisch verifiëren, bijvoorbeeld als u MFA vereist voor een aangepaste toepassing. De optie Per gebruiker berekent het aantal personen in uw tenant die in een maand verificatie in twee stappen uitvoeren. Deze optie wordt aanbevolen als u een aantal gebruikers met licenties hebt, maar MFA wilt uitbreiden naar meer gebruikers buiten de restricties van uw licentie.

## <a name="create-a-multi-factor-auth-provider"></a>Een Multi-Factor Authentication-provider maken
Voer de volgende stappen uit om een Azure Multi-Factor Authentication-provider te maken. Azure Multi-Factor Authentication-providers kunnen alleen worden gemaakt in de klassieke Azure Portal. Als u zich niet kunt aanmelden bij de klassieke Azure Portal, controleert u of de Azure AD-tenant is [gekoppeld aan een Azure-abonnement](../active-directory/active-directory-how-subscriptions-associated-directory.md). 

1. Meld u aan als beheerder bij de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Selecteer aan de linkerkant **Active Directory**.
3. Selecteer op de pagina Active Directory aan de bovenkant **Multi-Factor Authentication-providers**.
   
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. Klik aan de onderkant op **Nieuw**.
   
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. Onder App Services selecteert u **Multi-Factor Authentication-provider**
   
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Selecteer **Snelle invoer**.
   
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Vul de volgende velden in en selecteer **Maken**:
   1. **Naam**: de naam van de Multi-Factor Authentication-provider.
   2. **Gebruiksmodel**: kies een van de volgende twee opties:
      * Per verificatie – koopmodel waarin per verificatie wordt betaald. Wordt doorgaans gebruikt in scenario's die gebruikmaken van Azure Multi-Factor Authentication in een toepassing waarmee consumenten te maken krijgen.
      * Per ingeschakelde gebruiker – koopmodel waarin voor elke ingeschakelde gebruiker wordt betaald. Wordt doorgaans gebruikt om werknemers toegang te geven tot toepassingen zoals Office 365. Kies deze optie als sommige gebruikers al een licentie voor Azure MFA hebben.
   3. **Adreslijst**: de Azure Active Directory-tenant waaraan de Multi-Factor Authentication-provider is gekoppeld. Houd rekening met het volgende:
      * U hebt geen Azure AD-adreslijst nodig om een Multi-Factor Authentication-provider te maken. Laat dit vak leeg als u alleen van plan bent de Azure Multi-Factor Authentication-server of -SDK te downloaden.
      * De Multi-Factor Authentication-provider moet worden gekoppeld aan een Azure AD-adreslijst om te kunnen profiteren van de geavanceerde functies.
      * Er kan slechts één Multi-Factor Authentication-provider worden gekoppeld aan één Azure AD-adreslijst.  
      ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Nadat u op Maken hebt gedrukt, wordt de Multi-Factor Authentication-provider gemaakt en wordt er een bericht weergegeven: **Er is een Multi-Factor Authentication-provider gemaakt**. Klik op **OK**.  
   
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

## <a name="manage-your-multi-factor-auth-provider"></a>Een Multi-Factor Authentication-provider beheren

U kunt het gebruiksmodel (per ingeschakelde gebruiker of per verificatie) niet wijzigen nadat een MFA-provider is gemaakt. U kunt de MFA-provider echter verwijderen en vervolgens een nieuwe maken met een ander gebruiksmodel.

Als de huidige Multi-Factor Authentication-provider is gekoppeld aan een Azure AD-adreslijst (ook wel een Azure AD-tenant genoemd), kunt u de MFA-provider gerust verwijderen en er een maken die is gekoppeld aan dezelfde Azure AD-tenant. Als u voldoende licenties voor MFA, Azure AD Premium of EMS (Enterprise Mobility + Security) hebt voor alle gebruikers voor wie MFA is ingeschakeld, kunt u de MFA-provider ook helemaal verwijderen.

Als de MFA-provider niet is gekoppeld aan een Azure AD-tenant, of als u de nieuwe MFA-provider koppelt aan een andere Azure AD-tenant, worden de gebruikersinstellingen en configuratieopties niet overgedragen. Bestaande Azure MFA-servers moeten ook opnieuw worden geactiveerd met behulp van de activeringsreferenties die worden gegenereerd door de nieuwe MFA-provider. Als u de MFA-servers opnieuw activeert om ze te koppelen aan de nieuwe MFA-provider, is dit niet van invloed op telefonische verificatie en verificatie via een sms-bericht. Mobiele app-meldingen werken echter niet meer voor gebruikers totdat ze de mobiele app opnieuw activeren.

## <a name="next-steps"></a>Volgende stappen

[De Multi-Factor Authentication-SDK downloaden](multi-factor-authentication-sdk.md)

[Multi-Factor Authentication-instellingen configureren](multi-factor-authentication-whats-next.md)
