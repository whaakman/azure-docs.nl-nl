---
title: Lid worden van een nieuw Windows 10-apparaat met Azure AD tijdens de uitvoering van een eerste | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe gebruikers kunnen Azure AD Join instellen tijdens het first-run experience.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 0e71df2333dee9c4eb9935d3397d343be246be65
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Lid worden van een nieuw Windows 10-apparaat met Azure AD tijdens de uitvoering van een eerste

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen vanaf apparaten die voldoen aan uw standaarden voor beveiliging en naleving. Zie voor meer informatie de [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md).

Met Windows 10, kunt u een nieuw apparaat toevoegen aan Azure AD tijdens de eerste sessie (FRX).  
Hiermee kunt u krimp apparaten aan uw werknemers of studenten distribueren.

Als u Windows 10 Professional of Windows 10 Enterprise op een apparaat geïnstalleerd hebt, de ervaring wordt standaard ingesteld op het installatieproces voor apparaten in Bedrijfseigendom.

In het Windows- *out-of-box experience*, lidmaatschap van een lokale Active Directory (AD)-domein wordt niet ondersteund. Als u van plan bent om een computer toevoegen aan een AD-domein tijdens de installatie, moet u de koppeling **Windows instellen met een lokaal account**. U kunt vervolgens deelnemen aan het domein van de instellingen op uw computer.
 


## <a name="before-you-begin"></a>Voordat u begint

Als u wilt deelnemen aan een Windows 10-apparaat, moet de device registratieservice worden geconfigureerd zodat u apparaten registreren. Naast de machtiging voor het koppelen van apparaten in uw Azure AD-tenant hebt, moet u minder apparaten geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie [apparaatinstellingen configureren](device-management-azure-portal.md#configure-device-settings).

## <a name="joining-a-device"></a>Lid worden van een apparaat

**Een Windows 10-apparaat koppelen aan Azure AD tijdens FRX:**


1. Wanneer u het nieuwe apparaat inschakelen en het installatieproces start, ziet u de **gereed ophalen** bericht. Volg de aanwijzingen voor het instellen van uw apparaat.

2. Start op het aanpassen van uw regio en taal. Accepteer de licentievoorwaarden voor Microsoft-Software.
 
    ![Aanpassen voor uw regio](./media/device-management-azuread-joined-devices-frx/01.png)

3. Selecteer het netwerk die u gebruiken wilt om verbinding te maken met Internet.

4. Klik op **dit apparaat is van mijn organisatie**. 

    ![Wie is eigenaar van dit scherm PC](./media/device-management-azuread-joined-devices-frx/02.png)

5. Geef de referenties die aan u zijn geleverd door uw organisatie en klik vervolgens op **aanmelden**.

    ![Aanmeldingsscherm](./media/device-management-azuread-joined-devices-frx/03.png)

6. Uw apparaat wordt gezocht naar een overeenkomende tenant in Azure AD. Als u zich in een federatieve domein, wordt u omgeleid naar uw lokale Secure Token Service (STS) server, bijvoorbeeld Active Directory Federation Services (AD FS).

7. Als u een gebruiker in een niet-gefedereerde domein, Geef uw referenties rechtstreeks op de Azure AD gehost pagina. 

8. U wordt gevraagd naar een multi-factorauthenticatie uitdaging. 
 
9. Azure AD wordt gecontroleerd of een inschrijving in beheer van mobiele apparaten vereist is.

10. Windows het apparaat in de map van de organisatie wordt geregistreerd in Azure AD en beheer van mobiele apparaten inschrijft, indien van toepassing.

11. Als u bent:
    - Een gebruiker met beheerde Windows gaat u naar het bureaublad door het proces voor automatische aanmelding.

    - Een federatieve gebruiker, wordt u omgeleid naar het Windows-aanmeldingsscherm uw referenties in te voeren.

## <a name="verification"></a>Verificatie

Te controleren of een apparaat is verbonden met uw Azure AD de **toegang werk of school** dialoogvenster op uw Windows-apparaat. Het dialoogvenster zou moeten aangeven dat u met uw Azure AD-directory verbonden bent.

![Toegang tot werk of school](./media/device-management-azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie de [Inleiding tot beheer van apparaten in Azure Active Directory](device-management-introduction.md).

- Zie voor meer informatie over het beheren van apparaten in de Azure AD-portal [apparaten beheert met de Azure-portal](device-management-azure-portal.md).
