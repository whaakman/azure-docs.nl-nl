---
title: Azure Active Directory voorwaardelijke toegang voor VPN-verbinding (preview) | Microsoft Docs
description: 'Meer informatie over de werking van voorwaardelijke toegang van Azure Active Directory voor VPN-verbinding. '
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 511df58befe9aed4aa65fc6944cae3a8e2c74c2d
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-conditional-access-for-vpn-connectivity-preview"></a>Azure Active Directory voorwaardelijke toegang voor VPN-verbinding (preview)

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), kunt u aanpassen hoe gemachtigde gebruikers toegang uw resources tot. U kunt uw VPN-verbindingen beveiligen met voorwaardelijke toegang van Azure AD voor een virtueel particulier netwerk (VPN)-verbinding.


Voor het configureren van voorwaardelijke toegang voor VPN-verbinding, moet u de volgende stappen uit: 

1.  Configureer de VPN-server.
2.  Configureer uw VPN-client.
3.  Uw beleid voor voorwaardelijke toegang configureren.


## <a name="before-you-begin"></a>Voordat u begint

In dit onderwerp wordt ervan uitgegaan dat u bekend met de volgende onderwerpen bent:

- [Voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal.md)
- [VPN- en voorwaardelijke toegang](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Zie voor het verkrijgen van inzicht in hoe deze functie wordt geïmplementeerd in Microsoft [verbeteren van externe toegang in Windows 10 aan een automatische VPN-profiel](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).   


## <a name="prerequisites"></a>Vereisten

Voor het configureren van voorwaardelijke toegang van Azure Active Directory voor VPN-verbinding, moet u een VPN-server hebt geconfigureerd. 



## <a name="step-1-configure-your-vpn-server"></a>Stap 1: De VPN-server configureren 

Deze stap configureert de basiscertificaten voor VPN-verificatie met Azure AD. Voor het configureren van voorwaardelijke toegang voor VPN-verbinding, moet u:

1. Maak een VPN-certificaat in de Azure portal.
2. Het VPN-certificaat downloaden.
2. Implementeer het certificaat op de VPN-server.

Azure AD gebruikt de VPN-certificaat voor ondertekening van certificaten die zijn uitgegeven aan Windows 10-clients verificatie bij Azure AD voor VPN-verbinding. Het token dat de client voor Windows 10 vraagt is een certificaat dat deze geeft vervolgens naar de toepassing in dit geval de VPN-server wordt.

![Download het certificaat voor voorwaardelijke toegang](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

In de Azure portal kunt u twee certificaten voor het beheren van de overgang wanneer één certificaat is verlopen. Wanneer u een certificaat maakt, kunt u kiezen of het is het primaire certificaat voor, die wordt gebruikt tijdens de verificatie voor het ondertekenen van het certificaat voor de verbinding.

Een VPN-certificaat maken:

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als globale beheerder.

2. Klik in het menu links op **Azure Active Directory**. 

    ![Selecteer Azure Active Directory](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Op de **Azure Active Directory** pagina in de **beheren** sectie, klikt u op **voorwaardelijke toegang**.

    ![Selecteer de voorwaardelijke toegang](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Op de **voorwaardelijke toegang** pagina in de **beheren** sectie, klikt u op **VPN-verbinding (preview)**.

    ![Selecteer de VPN-verbinding](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Op de **VPN-verbinding** pagina, klikt u op **nieuw certificaat**.

    ![Selecteer het nieuwe certificaat](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Op de **nieuw** pagina, voert u de volgende stappen uit:

    ![Selecteer de duur en primaire](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. Voor **Selecteer duur**, selecteer **1 jaar**.

    b. Voor **primaire**, selecteer **Ja**.

    c. Klik op **Create**.

7. Klik op de pagina VPN-verbinding op **certificaat downloaden**.


U kunt nu uw nieuwe certificaat op de VPN-server implementeren. Voeg op de VPN-server, het gedownloade certificaat als een *vertrouwde basis-CA voor VPN-verificatie*.

Voor implementaties op basis van Windows RRAS toevoegen op de NPS-server het basiscertificaat in de *Enterprise NTauth* opslaan met de volgende opdrachten:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2-configure-your-vpn-client"></a>Stap 2: Uw VPN-client configureren 

In deze stap maakt u uw VPN-client verbinding-profiel configureren zoals wordt beschreven in [VPN en voorwaardelijke toegang](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).


## <a name="step-3-configure-your-conditional-access-policy"></a>Stap 3: Uw beleid voor voorwaardelijke toegang configureren

Deze sectie biedt instructies voor het configureren van uw beleid voor voorwaardelijke toegang voor VPN-verbinding.


1. Op de **voorwaardelijke toegang** pagina in de werkbalk bovenaan, klikt u op **toevoegen**.

    ![Selecteer toevoegen op de pagina voor voorwaardelijke toegang](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Op de **nieuw** pagina in de **naam** typt u een naam voor uw beleid. Typ bijvoorbeeld **VPN-beleid**.

    ![Naam voor beleid op de pagina voor voorwaardelijke toegang toevoegen](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. In de **toewijzing** sectie, klikt u op **gebruikers en groepen**.

    ![Gebruikers en groepen selecteren](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Op de **gebruikers en groepen** pagina, voert u de volgende stappen uit:

    ![Selecteer testgebruiker](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Klik op **gebruikers en groepen selecteren**.

    b. Klik op **Selecteren**.

    c. Op de **Selecteer** pagina en klik vervolgens op uw testgebruiker Selecteer **Selecteer**.

    d. Op de **gebruikers en groepen** pagina, klikt u op **gedaan**.

7. Op de **nieuw** pagina, voert u de volgende stappen uit:

    ![Selecteer cloud-apps](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. In de **toewijzingen** sectie, klikt u op **Cloud-apps**.

    b. Op de **Cloud-apps** pagina, klikt u op **apps selecteren**, en klik vervolgens op **Selecteer**.

    c. Op de **Selecteer** pagina in de **toepassingen** in het vak **vpn**.

    d. Selecteer **VPN-Server**.

    e. Klik op **Selecteren**.


13. Op de **nieuw** pagina, open de **Grant** pagina de **besturingselementen** sectie, klikt u op **Grant**.

    ![Selecteer verlenen](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Op de **Grant** pagina, voert u de volgende stappen uit:

    ![Selecteer multi-factor authentication-server is vereist](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Selecteer **meervoudige authenticatie**.

    b. Klik op **Selecteren**.

15. Op de **nieuw** pagina onder **beleid inschakelen**, klikt u op **op**.

    ![Beleid inschakelen](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Op de **nieuw** pagina, klikt u op **maken**.



## <a name="next-steps"></a>Volgende stappen

Zie voor het verkrijgen van inzicht in hoe deze functie wordt geïmplementeerd in Microsoft [verbeteren van externe toegang in Windows 10 aan een automatische VPN-profiel](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).    

