---
title: Cloudresources beveiligen met Azure MFA en AD FS
description: Dit is de pagina Azure Multi-Factor Authentication waarop wordt beschreven hoe u aan de slag kunt met Azure MFA en AD FS in de cloud.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 0927fc67-8090-4fdd-913a-b3cfed3fbe77
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 0a9ab0aca1a77245f360d0d8976aa9b8f59f15a0


---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Cloudresources beveiligen met Azure Multi-Factor Authentication en AD FS
Als uw organisatie is gefedereerd met behulp van Azure Active Directory, kunt u Azure Multi-Factor Authentication of Active Directory Federation Services gebruiken om resources te beveiligen die worden gebruikt door Azure AD. Gebruik de volgende procedures voor het beveiligen van Azure Active Directory-resources met ofwel Azure Multi-Factor Authentication of Active Directory Federation Services.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Azure AD-resources beveiligen met behulp van AD FS
Als u uw cloudresource wilt beveiligen, moet u eerst een account inschakelen voor gebruikers en een claimregel instellen. Volg deze procedure om de stappen te doorlopen:

1. Gebruik de stappen die worden beschreven in [Multi-Factor Authentication inschakelen](multi-factor-authentication-get-started-cloud.md#turn-on-two-step-verification-for-users) voor gebruikers die een account willen inschakelen.
2. Start de AD FS-beheerconsole.
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Navigeer naar de **Relying Party-vertrouwensrelaties** en klik met de rechtermuisknop op de Relying Party-vertrouwensrelatie. Selecteer **Claimregels bewerken...**
4. Klik op **Regel toevoegen...**
5. Selecteer in de vervolgkeuzelijst **Claimregels verzenden met een aangepaste regel** en klik op **Volgende**.
6. Voer een naam in voor de claimregel.
7. Voeg onder Aangepaste regel de volgende tekst toe:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Overeenkomende claim:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```
8. Klik op **OK** en vervolgens op **Voltooien**. Sluit de AD FS-beheerconsole.

Gebruikers kunnen zich vervolgens aanmelden via de on-premises methode (zoals smartcard).

## <a name="trusted-ips-for-federated-users"></a>Goedgekeurde IP-adressen voor federatieve gebruikers
Met goedgekeurde IP-adressen zijn beheerders in staat om verificatie in twee stappen te omzeilen voor bepaalde IP-adressen of voor federatieve gebruikers met verzoeken die afkomstig zijn uit hun eigen intranet. In de volgende secties wordt beschreven hoe goedgekeurde IP-adressen van Azure Multi-Factor Authentication bij federatieve gebruikers moeten worden geconfigureerd en hoe verificatie in twee stappen kan worden omzeild als een verzoek afkomstig is uit het intranet van een federatieve gebruiker. Dit wordt bereikt door AD FS zo te configureren dat deze gebruikmaakt van een passthrough of een binnenkomende claimsjabloon filtert met het claimtype Binnen bedrijfsnetwerk.

In dit voorbeeld wordt Office 365 gebruikt voor onze Relying Party-vertrouwensrelaties.

### <a name="configure-the-ad-fs-claims-rules"></a>De claimregels van AD FS configureren
Het eerste wat we moeten doen is de AD FS-claims configureren. We gaan twee claimregels maken, één voor het claimtype Binnen bedrijfsnetwerk en een extra regel om onze gebruikers aangemeld te houden.

1. Open AD FS-beheer.
2. Selecteer **Relying Party-vertrouwensrelaties** aan de linkerkant.
3. Klik met de rechtermuisknop op het **identiteitsplatform van Microsoft Office 365** en selecteer **Claimregels bewerken...**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Klik bij Uitgifte transformatieregels op**Regel toevoegen.**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. Selecteer in de wizard Transformatieclaimregels toevoegen **Passthrough of Een binnenkomende claim filteren** in de vervolgkeuzelijst en klik op **Volgende**.
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Geef de claimregel een naam in het vak bij Naam claimregel. Bijvoorbeeld: BinnenBedrijfsNet.
7. In de vervolgkeuzelijst naast Binnenkomend claimtype, selecteert u **Binnen bedrijfsnetwerk**.
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Klik op **Voltooien**.
9. Klik bij Uitgifte transformatieregels op**Regel toevoegen.**.
10. Selecteer in de wizard Transformatieclaimregels toevoegen **Claim verzenden met een aangepaste regel** in de vervolgkeuzelijst en klik op **Volgende**.
11. In het vak onder Naam claimregel typt u *Gebruikers aangemeld houden*.
12. In het vak Aangepaste regel typt u:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Klik op **Voltooien**.
14. Klik op **Toepassen**.
15. Klik op **OK**.
16. Sluit AD FS-beheer.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Goedgekeurde IP-adressen van Azure Multi-Factor Authentication configureren bij federatieve gebruikers
Nu de claims zijn gemaakt, kunnen we goedgekeurde IP-adressen gaan configureren.

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Klik aan de linkerkant op **Active Directory**.
3. Selecteer onder Adreslijst de adreslijst waar u de goedgekeurde IP-adressen wilt instellen.
4. Klik op **Configureren** op de Adreslijst die u hebt geselecteerd.
5. Klik in de sectie Multi-Factor Authentication op **Service-instellingen beheren**.
6. Selecteer op de pagina Service-instellingen onder Goedgekeurde IP-adressen, **Meervoudige verificatie overslaan voor aanvragen van federatieve gebruikers op mijn intranet.**
   ![Cloud](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Klik op **Opslaan**.
8. Nadat de updates zijn toegepast, klikt u op **Sluiten**.

Dat is alles. Vanaf dit moment hoeven Office 365-gebruikers alleen MFA te gebruiken wanneer een claim afkomstig is van buiten het bedrijfsintranet.



<!--HONumber=Dec16_HO1-->


