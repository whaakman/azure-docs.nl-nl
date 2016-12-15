---
title: Aan de slag met Azure MFA in de cloud | Microsoft Docs
description: Dit is de pagina Azure Multi-Factor Authentication waarop wordt beschreven hoe u aan de slag kunt met Azure MFA in de cloud.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 6b2e6549-1a26-4666-9c4a-cbe5d64c4e66
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/17/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: c6fe00b72d95a3eb40d91f6f7989b7163518c46f


---
# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Aan de slag met Azure Multi-Factor Authentication in de cloud
In het volgende artikel staat beschreven hoe u aan de slag gaat met Azure Multi-Factor Authentication in de cloud.

> [!NOTE]
> De volgende documentatie bevat informatie over hoe u het voor gebruikers mogelijk maakt de **klassieke Azure Portal** te gebruiken. Ga naar [Multi-Factor Authentication instellen voor Office 365](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US) als u op zoek bent naar informatie over het instellen van Azure Multi-Factor Authentication voor O365-gebruikers.
> 
> 

![MFA in de Cloud](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Vereisten
Aan de volgende vereisten moet worden voldaan voordat u Azure Multi-Factor Authentication voor uw gebruikers kunt inschakelen.

1. [U moet u aanmelden voor een Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/): als u nog geen Azure-abonnement hebt, moet u er een nemen. Als u net begint en Azure MFA gebruikt, kunt u een proefabonnement nemen
2. [U moet een Multi-Factor Authentication-provider maken](multi-factor-authentication-get-started-auth-provider.md) en deze toewijzen aan uw adreslijst of [licenties toewijzen aan gebruikers](multi-factor-authentication-get-started-assign-licenses.md)

> [!NOTE]
> Licenties zijn beschikbaar voor gebruikers die Azure MFA, Azure AD Premium of Enterprise Mobility Suite (EMS) hebben.  MFA maakt deel uit van Azure AD Premium en EMS. Als u voldoende licenties hebt, hoeft u geen Multi-Factor Authentication-provider te maken.
> 
> 

## <a name="turn-on-two-step-verification-for-users"></a>Verificatie in twee stappen inschakelen voor gebruikers
Verander de status van de gebruiker van Uitgeschakeld naar Ingeschakeld om verificatie in twee stappen te activeren.  Zie voor meer informatie over de status van de gebruiker [Gebruikersstatussen in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md)

Gebruik de volgende procedure om MFA in te schakelen voor uw gebruikers.

### <a name="to-turn-on-multi-factor-authentication"></a>Multi-Factor Authentication inschakelen
1. Meld u aan als beheerder bij de [klassieke Azure Portal](https://manage.windowsazure.com).
2. Klik aan de linkerkant op **Active Directory**.
3. Klik onder Adreslijst op de adreslijst voor de gebruiker die u wilt inschakelen.
   ![Klik op Adreslijst](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4. Klik aan de bovenkant op **Gebruikers**.
5. Klik onder aan de pagina op **Multi-Factor Authentication beheren**. Er wordt een nieuw browsertabblad geopend.
   ![Klik op Adreslijst](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6. Zoek naar de gebruiker waarvoor u verificatie in twee stappen wilt inschakelen. Mogelijk moet u de weergave bovenaan wijzigen. Zorg ervoor dat de status **Uitgeschakeld is.**
   ![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7. Plaats een **vinkje** in het vak naast hun naam.
8. Klik op aan de rechterkant op **Inschakelen**.
   ![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/user1.png)
9. Klik op **Multi-Factor Auth inschakelen**.
   ![Gebruiker inschakelen](./media/multi-factor-authentication-get-started-cloud/enable2.png)
10. De status van de gebruiker zou moeten zijn gewijzigd van **Uitgeschakeld** in **Ingeschakeld**.
    ![Gebruikers inschakelen](./media/multi-factor-authentication-get-started-cloud/user.png)

Nadat u uw gebruikers hebt ingeschakeld, dient u ze hierover te informeren via e-mail. De volgende keer dat ze zich proberen aan te melden, worden ze gevraagd om hun account aan te melden voor verificatie in twee stappen. Zodra ze verificatie in twee stappen gaan gebruiken, moeten ze ook wachtwoorden instellen voor toepassingen om te voorkomen dat niet-browsertoepassingen worden geblokkeerd.

## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>PowerShell gebruiken om het inschakelen van verificatie in twee stappen te automatiseren
Als u de [status](multi-factor-authentication-whats-next.md) met [Azure AD PowerShell](/powershell/azureps-cmdlets-docs) wilt wijzigen, kunt u het volgende doen.  U kunt `$st.State` wijzigen zodat deze overeenkomt met een van de volgende statussen:

* Ingeschakeld
* Afgedwongen
* Uitgeschakeld  

> [!IMPORTANT]
> We raden het af gebruikers rechtstreeks van de status Uitgeschakeld naar de status Afgedwongen te verplaatsen. Niet-browsertoepassingen werken dan niet meer omdat de gebruiker de MFA-registratie niet heeft doorlopen en geen [toepassingswachtwoord](multi-factor-authentication-whats-next.md#app-passwords) heeft verkregen. Als u niet-browsertoepassingen hebt en toepassingswachtwoorden nodig hebt, is het raadzaam om van de status Uitgeschakeld naar de status Ingeschakeld te gaan. Dit stelt gebruikers in staat om hun toepassingswachtwoorden te registreren en op te halen. Vervolgens kunt u ze naar de status Afgedwongen verplaatsen.
> 
> 

Het gebruik van PowerShell is dan een optie om bulksgewijs gebruikers in te schakelen. Momenteel bevat Azure Portal geen functie voor het bulksgewijs inschakelen van gebruikers, en dus moet u elke gebruiker afzonderlijk selecteren. Dit kan veel tijd kosten als u een groot aantal gebruikers hebt. Als u een PowerShell-script maakt met behulp van het onderstaande, kunt u door een lijst met gebruikers gaan en ze inschakelen.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Hier volgt een voorbeeld:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Zie voor meer informatie [Gebruikersstatussen in Azure Multi-Factor Authentication](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Volgende stappen
Nu dat u Multi-Factor Authentication in de cloud hebt ingesteld, kunt u uw implementatie configureren en instellen. Zie [Azure Multi-Factor Authentication configureren](multi-factor-authentication-whats-next.md) voor meer informatie.




<!--HONumber=Dec16_HO1-->


