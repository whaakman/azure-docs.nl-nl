---
title: 'Azure Active Directory Domain Services: wachtwoordsynchronisatie inschakelen | Microsoft Docs'
description: Aan de slag met Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 51e4665b6372859021237144a0bf7f6763a2638b
ms.lasthandoff: 04/14/2017


---
# <a name="enable-password-synchronization-with-azure-active-directory-domain-services"></a>Wachtwoordsynchronisatie inschakelen met Azure Active Directory Domain Services
Tijdens de vorige taken hebt u Azure Active Directory Domain Services (AD DS) ingeschakeld voor uw Azure Active Directory-tenant (Azure AD). De volgende taak bestaat uit het inschakelen van referentie-hashes, die nodig zijn voor de synchronisatie van NTLM- (NT LAN Manager) en Kerberos-verificatie met Azure Active Directory Domain Services. Wanneer u de referentiesynchronisatie hebt ingesteld, kunnen gebruikers zich bij het beheerde domein aanmelden met hun zakelijke referenties.

De procedures verschillen, afhankelijk van of uw organisatie een Azure AD-tenant in de cloud heeft of kan synchroniseren met uw on-premises directory via Azure AD Connect.

> [!div class="op_single_selector"]
> * [Azure AD-tenant in de cloud](active-directory-ds-getting-started-password-sync.md)
> * [Gesynchroniseerde Azure AD-tenant](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

## <a name="task-5-enable-password-synchronization-with-azure-active-directory-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Taak 5: wachtwoordsynchronisatie met Azure Active Directory Domain Services inschakelen voor een Azure AD-tenant die zich alleen in de cloud bevindt
Voor de verificatie van gebruikers in het beheerde domein heeft Azure Active Directory Domain Services referentie-hashes nodig in een indeling die geschikt is voor NTLM- en Kerberos-verificatie. Tenzij u Azure Active Directory Domain Services voor uw tenant inschakelt, maakt of bewaart Azure AD geen referentie-hashes in de vereiste indeling voor NTLM- of Kerberos-verificatie. Om veiligheidsredenen slaat Azure AD ook geen referenties op in niet-gecodeerde vorm. Azure AD biedt daarom geen manier voor het genereren van deze NTLM- of Kerberos-referentie-hashes op basis van bestaande referenties van gebruikers.

> [!NOTE]
> Als uw organisatie een Azure AD-tenant in de cloud heeft, moeten gebruikers die Azure Active Directory Domain Services willen gebruiken, hun wachtwoord wijzigen.
>
>

Door deze wachtwoordwijziging worden de referentie-hashes die door Azure Active Directory Domain Services zijn vereist voor Kerberos- en NTLM-verificatie, gegenereerd in Azure AD. U kunt wachtwoorden laten verlopen voor alle gebruikers in de tenant die Azure Active Directory Domain Services moeten gebruiken of aan de gebruikers doorgeven dat ze hun wachtwoord moeten wijzigen.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Het genereren van hashes voor NTLM- en Kerberos-referenties inschakelen voor een Azure AD-tenant in de cloud
Hier vindt u de instructies voor het wijzigen van het wachtwoord die u moet doorgeven aan gebruikers:

1. Ga naar de pagina [Azure AD-toegangsvenster](http://myapps.microsoft.com) voor uw organisatie.
2. Selecteer het tabblad **Profiel** in het venster Toegangsvenster.
3. Klik op de tegel **Wachtwoord wijzigen**.

    ![De tegel Wachtwoord wijzigen in het Azure AD-toegangsvenster](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Als u de optie **Wachtwoord wijzigen** niet ziet in het venster Toegangsvenster, controleert u of uw organisatie [Wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md) heeft geconfigureerd.
   >
   >
4. Op de pagina **Wachtwoord wijzigen** typt u uw bestaande (oude) wachtwoord, typt u een nieuw wachtwoord en bevestigt u dit.

    ![Maak een virtueel netwerk voor Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klik op **Verzenden**.

Een paar minuten nadat u uw wachtwoord hebt gewijzigd, kunt u het nieuwe wachtwoord gebruiken in Azure Active Directory Domain Services. Na ongeveer 20 minuten kunt u zich met uw gewijzigde wachtwoord aanmelden bij computers die zijn gekoppeld aan het beheerde domein.

## <a name="next-steps"></a>Volgende stappen
* [Uw eigen wachtwoord bijwerken](../active-directory/active-directory-passwords-update-your-own-password.md#reset-my-password)
* [Aan de slag met wachtwoordbeheer in Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Wachtwoordsynchronisatie met Azure Active Directory Domain Services inschakelen voor een gesynchroniseerde Azure AD-tenant](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Een beheerd domein van Azure Active Directory Domain Services beheren](active-directory-ds-admin-guide-administer-domain.md)
* [Een virtuele Windows-computer toevoegen aan een beheerd domein van Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Een virtuele Red Hat Enterprise Linux-computer toevoegen aan een beheerd domein van Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

