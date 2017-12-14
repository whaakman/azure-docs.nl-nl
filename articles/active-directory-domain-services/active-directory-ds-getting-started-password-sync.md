---
title: 'Azure Active Directory Domain Services: wachtwoordsynchronisatie inschakelen | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/18/2017
ms.author: maheshu
ms.openlocfilehash: ea65659b1d35246bc3e7235b3faed8d2a5368010
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Wachtwoordsynchronisatie inschakelen voor Azure Active Directory Domain Services
Tijdens de vorige taken hebt u Azure Active Directory Domain Services ingeschakeld voor uw Azure Active Directory-tenant (Azure AD). De volgende taak bestaat uit het inschakelen dat referentie-hashes voor NTLM- (NT LAN Manager) en Kerberos-verificatie moeten worden gesynchroniseerd met Azure AD Domain Services. Wanneer u de referentiesynchronisatie hebt ingesteld, kunnen gebruikers zich bij het beheerde domein aanmelden met hun zakelijke referenties.

De vereiste stappen voor gebruikersaccounts in de cloud en gebruikersaccounts die worden gesynchroniseerd vanuit uw on-premises map met Azure AD Connect, verschillen. 

<br>
| **Type gebruikersaccount** | **Stappen die moeten worden uitgevoerd** |
| --- |---|
| **Cloud-gebruikersaccounts die zijn gemaakt in Azure AD** |**&#x2713;** [Volg de instructies in dit artikel](active-directory-ds-getting-started-password-sync.md#task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts) |
| **Gebruikersaccounts die zijn gesynchroniseerd vanuit een on-premises map** |**&#x2713;**: [synchroniseer de wachtwoorden voor gebruikersaccounts die zijn gesynchroniseerd vanuit de on-premises AD met het beheerde domein](active-directory-ds-getting-started-password-sync-synced-tenant.md) | 

<br>

> [!TIP]
> **Mogelijk moet u beide sets met stappen voltooien.**
> Als de Azure AD-tenant een combinatie heeft van cloudgebruikers en gebruikers van on-premises AD, moet u beide sets met stappen uitvoeren.
>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts"></a>Taak 5: wachtwoordsynchronisatie met uw beheerde domein voor cloudgebruikersaccounts inschakelen
Voor de verificatie van gebruikers in het beheerde domein heeft Azure Active Directory Domain Services referentie-hashes nodig in een indeling die geschikt is voor NTLM- en Kerberos-verificatie. Totdat u Azure Active Directory Domain Services voor uw tenant inschakelt, maakt of bewaart Azure AD geen referentie-hashes in de vereiste indeling voor NTLM- of Kerberos-verificatie. Om veiligheidsredenen slaat Azure AD ook geen wachtwoorden op in niet-gecodeerde vorm. Azure AD biedt daarom geen manier voor het automatisch genereren van deze NTLM- of Kerberos-referentie-hashes op basis van bestaande referenties van gebruikers.

> [!NOTE]
> **Als uw organisatie gebruikersaccounts in de cloud heeft, moeten alle gebruikers die Azure Active Directory Domain Services willen gebruiken, hun wachtwoord wijzigen.** Een cloudgebruikersaccount is een account dat is gemaakt in uw Azure AD-directory via de Azure portal of Azure AD PowerShell-cmdlets. Deze gebruikersaccounts zijn niet gesynchroniseerd vanuit een on-premises map.
>
>

Door deze wachtwoordwijziging worden de referentie-hashes die door Azure Active Directory Domain Services zijn vereist voor Kerberos- en NTLM-verificatie, gegenereerd in Azure AD. U kunt wachtwoorden laten verlopen voor alle gebruikers in de tenant die Azure Active Directory Domain Services moeten gebruiken of aan de gebruikers doorgeven dat ze hun wachtwoord moeten wijzigen.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Het genereren van hashes voor NTLM- en Kerberos-referenties inschakelen voor een cloudgebruikersaccount
Hier vindt u de instructies voor het wijzigen van het wachtwoord die u moet doorgeven aan gebruikers:

1. Ga naar de pagina [Azure AD-toegangsvenster](http://myapps.microsoft.com) voor uw organisatie.

    ![Open het deelvenster Azure AD Access](./media/active-directory-domain-services-getting-started/access-panel.png)

2. In de rechterbovenhoek klikt u op uw naam en selecteert u **Profiel** in het menu.

    ![Selecteer het profiel](./media/active-directory-domain-services-getting-started/select-profile.png)

3. Op de pagina **Profiel** klikt u op **Wachtwoord wijzigen**.

    ![Klik op 'Wachtwoord wijzigen'](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!TIP]
   > Als u de optie **Wachtwoord wijzigen** niet ziet in het venster Toegangsvenster, controleert u of uw organisatie [Wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md) heeft geconfigureerd.
   >
   >
4. Op de pagina **Wachtwoord wijzigen** typt u uw bestaande (oude) wachtwoord, typt u een nieuw wachtwoord en bevestigt u dit.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klik op **Verzenden**.

Een paar minuten nadat u uw wachtwoord hebt gewijzigd, kunt u het nieuwe wachtwoord gebruiken in Azure Active Directory Domain Services. Na ongeveer 20 minuten kunt u zich met het gewijzigde wachtwoord aanmelden bij computers die zijn gekoppeld aan het beheerde domein.

## <a name="related-content"></a>Gerelateerde inhoud
* [Uw eigen wachtwoord bijwerken](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Aan de slag met wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Wachtwoordsynchronisatie met Azure Active Directory Domain Services inschakelen voor een gesynchroniseerde Azure AD-tenant](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Een beheerd domein van Azure Active Directory Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Een virtuele Windows-computer toevoegen aan een beheerd domein van Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een virtuele Red Hat Enterprise Linux-computer toevoegen aan een beheerd domein van Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
