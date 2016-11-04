---
title: Aan de slag met Microsoft Azure Multi-Factor Auth Provider
description: Informatie over het maken van een Azure Multi-Factor Authentication-provider.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtand

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/04/2016
ms.author: kgremban

---
# Aan de slag met een Azure Multi-Factor Authentication-provider
Multi-Factor Authentication is standaard beschikbaar voor globale beheerders van Azure Active Directory- en Office 365-gebruikers. Als u echter wilt profiteren van [Geavanceerde functies](multi-factor-authentication-whats-next.md), moet u de volledige versie van Azure MFA aanschaffen.

> [!NOTE]
> Een Azure Multi-Factor Authentication-provider wordt gebruikt om te kunnen profiteren van de functies van de volledige versie van Azure MFA. Deze is bestemd voor gebruikers die **geen licenties via Azure MFA, Azure AD Premium of EMS** hebben.  Azure MFA, Azure AD Premium en EMS bevatten standaard de volledige versie van Azure MFA.  Als u licenties hebt, hebt u geen Azure Multi-Factor Authentication-provider nodig.
> 
> 

Een Azure Multi-Factor Authentication-provider is vereist als u de SDK wilt downloaden.

> [!IMPORTANT]
> Als u de SDK wilt downloaden, moet u een Azure Multi-Factor Authentication-provider maken, zelfs als u Azure MFA, AAD Premium of EMS-licenties hebt.  Als u voor dit doel een Azure Multi-Factor Authentication-provider maakt en al licenties hebt, is het een vereiste om de Provider met het model **Per Enabled User** (Per ingeschakelde gebruiker) te maken en de provider te koppelen aan de map waarin zich de Azure MFA-, Azure AD Premium- of EMS-licenties bevinden.  Dit zorgt ervoor dat er niet bij u wordt gefactureerd, tenzij u meer unieke gebruikers hebt die de SDK gebruiken dan het aantal licenties dat u in uw bezit hebt.
> 
> 

Voer de volgende stappen uit om een Azure Multi-Factor Authentication-provider te maken.

## Een Azure Multi-Factor Authentication-provider maken
- - -
1. Meld u als beheerder aan bij de **klassieke Azure-portal**.
2. Selecteer aan de linkerkant **Active Directory**.
3. Selecteer op de pagina Active Directory aan de bovenkant **Multi-Factor Authentication-providers**.
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. Klik aan de onderkant op **Nieuw**.
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. Onder **App Services** selecteert u **Multi-Factor Authentication-provider**
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Selecteer **Snelle invoer**.
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
7. Vul de volgende velden in en selecteer **Maken**:
   1. **Naam**: de naam van de Multi-Factor Authentication-provider.
   2. **Gebruiksmodel** – het gebruiksmodel van de Multi-Factor Authentication-provider.
      * Per verificatie – koopmodel waarin per verificatie wordt betaald. Wordt doorgaans gebruikt in scenario's die gebruikmaken van Azure Multi-Factor Authentication in een toepassing waarmee consumenten te maken krijgen.
      * Per ingeschakelde gebruiker – koopmodel waarin voor elke ingeschakelde gebruiker wordt betaald. Wordt doorgaans gebruikt om werknemers toegang te geven tot toepassingen zoals Office 365.
   3. **Adreslijst**: de Azure Active Directory-tenant waaraan de Multi-Factor Authentication-provider is gekoppeld. Houd rekening met het volgende:
      * U hebt geen Azure AD-adreslijst nodig om een Multi-Factor Authentication-provider te maken.  Laat het vak gewoon leeg als u van plan bent alleen de Azure Multi-Factor Authentication-server of -SDK te gebruiken.
      * De Multi-Factor Authentication-provider moet worden gekoppeld aan een Azure AD-adreslijst om te kunnen profiteren van de geavanceerde functies.
      * Azure AD Connect, AAD Sync of DirSync zijn alleen vereist als u uw on-premises Active Directory-omgeving synchroniseert met een Azure AD-adreslijst.  Als u alleen een Azure AD-adreslijst gebruikt die niet wordt gesynchroniseerd, is dit niet vereist.
        ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
8. Nadat u op Maken hebt gedrukt, wordt de Multi-Factor Authentication-provider gemaakt en wordt er een bericht weergegeven: **Er is een Multi-Factor Authentication-provider gemaakt**. Klik op **OK**.
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)

<!--HONumber=Sep16_HO3-->


