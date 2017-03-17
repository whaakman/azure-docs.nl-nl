---
title: Aan de slag met Microsoft Azure Multi-Factor Auth Provider | Microsoft Docs
description: Informatie over het maken van een Azure Multi-Factor Authentication-provider.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/24/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: b0c3c5dfd695b28b75bf44c9f835b34966413a9a
ms.lasthandoff: 03/04/2017


---
# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Aan de slag met een Azure Multi-Factor Authentication-provider
Verificatie in twee stappen is standaard beschikbaar voor globale beheerders van Azure Active Directory- en Office 365-gebruikers. Als u echter wilt profiteren van [geavanceerde functies](multi-factor-authentication-whats-next.md), moet u de volledige versie van Azure Multi-Factor Authentication (MFA) aanschaffen.

> [!NOTE]
> Een Azure Multi-Factor Authentication-provider wordt gebruikt om te kunnen profiteren van de functies van de volledige versie van Azure MFA. Deze is bestemd voor gebruikers die **geen licenties via Azure MFA, Azure AD Premium of EMS** hebben.  Azure MFA, Azure AD Premium en EMS bevatten standaard de volledige versie van Azure MFA.  Als u licenties hebt, hebt u geen Azure Multi-Factor Authentication-provider nodig.

Een Azure Multi-Factor Authentication-provider is vereist als u de SDK wilt downloaden.

> [!IMPORTANT]
> Als u de SDK wilt downloaden, moet u een Azure Multi-Factor Authentication-provider maken, zelfs als u Azure MFA, AAD Premium of EMS-licenties hebt.  Als u een Azure Multi-Factor Authentication-provider voor dit doeleinde maakt en al licenties hebt, moet u ervoor zorgen dat u de provider maakt met het model **Per ingeschakelde gebruiker**. Koppel de provider vervolgens aan de map die de licenties voor Azure MFA, Azure AD Premium of EMS bevat.  Deze configuratie zorgt ervoor dat u alleen een rekening ontvangt als u meer unieke gebruikers hebt die verificatie in twee stappen gebruiken dan het aantal licenties dat u in uw bezit hebt.

## <a name="create-a-multi-factor-auth-provider"></a>Een Multi-Factor Authentication-provider maken
Voer de volgende stappen uit om een Azure Multi-Factor Authentication-provider te maken.

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
      * U hebt geen Azure AD-adreslijst nodig om een Multi-Factor Authentication-provider te maken. Laat het vak leeg als u van plan bent alleen de Azure Multi-Factor Authentication-server of -SDK te gebruiken.
      * De Multi-Factor Authentication-provider moet worden gekoppeld aan een Azure AD-adreslijst om te kunnen profiteren van de geavanceerde functies.
      * Azure AD Connect, AAD Sync of DirSync zijn alleen vereist als u uw on-premises Active Directory-omgeving synchroniseert met een Azure AD-adreslijst.  Als u alleen een Azure AD-adreslijst gebruikt die niet wordt gesynchroniseerd, is dit niet vereist.
        ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
8. Nadat u op Maken hebt gedrukt, wordt de Multi-Factor Authentication-provider gemaakt en wordt er een bericht weergegeven: **Er is een Multi-Factor Authentication-provider gemaakt**. Klik op **OK**.
   ![Een MFA-provider maken](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)


