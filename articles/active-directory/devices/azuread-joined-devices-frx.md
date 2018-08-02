---
title: Een nieuwe Windows 10-apparaat met Azure AD tijdens de uitvoering van een eerste toevoegen | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe gebruikers kunnen Azure AD Join instellen tijdens de eerste ervaring van uitvoeren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1376f011d056aac33333f6ac31ee2eaadaf3ef4a
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414999"
---
# <a name="join-a-new-windows-10-device-with-azure-ad-during-a-first-run"></a>Een nieuwe Windows 10-apparaat met Azure AD tijdens de uitvoering van een eerste toevoegen

Met Apparaatbeheer in Azure Active Directory (Azure AD), kunt u ervoor zorgen dat uw gebruikers toegang hebben tot de bronnen van apparaten die voldoen aan uw normen voor beveiliging en naleving. Zie voor meer informatie de [Inleiding tot Apparaatbeheer in Azure Active Directory](overview.md).

Met Windows 10, kunt u een nieuw apparaat toevoegen aan Azure AD tijdens de eerste sessie (FRX).  
Hiermee kunt u krimp apparaten aan uw werknemers of studenten distribueren.

Als u Windows 10 Professional of Windows 10 Enterprise op een apparaat is geïnstalleerd hebt, is de ervaring standaard ingesteld op het installatieproces voor bedrijfsapparaten.

In de Windows *out-of-box experience*, lid worden van een on-premises Active Directory (AD)-domein wordt niet ondersteund. Als u van plan bent een computer toevoegen aan een AD-domein tijdens de installatie, moet u de koppeling **Windows instellen met een lokaal account**. U kunt vervolgens deelnemen aan het domein van de instellingen op uw computer.
 


## <a name="before-you-begin"></a>Voordat u begint

Als u wilt deelnemen aan een Windows 10-apparaat, moet de device registratieservice om te registreren van apparaten worden geconfigureerd. Naast de machtiging voor het toevoegen van apparaten in uw Azure AD-tenant, moet u minder apparaten die zijn geregistreerd dan het geconfigureerde maximum hebben. Zie voor meer informatie, [apparaatinstellingen configureren](device-management-azure-portal.md#configure-device-settings).

Als uw tenant is gefedereerd, moet uw id-provider ook ondersteuning voor eindpunt voor WS-Federation en WS-Trust gebruikersnaam en wachtwoord. Dit is versie 1.3 of 2005. De ondersteuning van dit protocol is vereist voor zowel het apparaat toevoegen aan Azure AD en meld u aan bij het apparaat met een wachtwoord.

## <a name="joining-a-device"></a>Een apparaat toevoegen

**Op een Windows 10-apparaat toevoegen aan Azure AD tijdens FRX:**


1. Wanneer u het nieuwe apparaat inschakelen en het installatieproces wordt gestart, ziet u de **gereed ophalen** bericht. Volg de aanwijzingen voor het instellen van uw apparaat.

2. Begin met het aanpassen van uw regio en taal. Accepteer de licentievoorwaarden voor Microsoft-Software.
 
    ![Aanpassen voor uw regio](./media/azuread-joined-devices-frx/01.png)

3. Selecteer het netwerk die u gebruiken wilt om verbinding te maken met Internet.

4. Klik op **dit apparaat behoort tot mijn organisatie**. 

    ![Wie is eigenaar van dit scherm PC](./media/azuread-joined-devices-frx/02.png)

5. Voer de referenties die aan u zijn verstrekt door uw organisatie en klik vervolgens op **aanmelden**.

    ![Aanmeldingsscherm](./media/azuread-joined-devices-frx/03.png)

6. Uw apparaat wordt gezocht naar een overeenkomende tenant in Azure AD. Als u zich in een federatief domein, wordt u omgeleid naar uw on-premises Secure Token Service (STS) server, bijvoorbeeld Active Directory Federation Services (AD FS).

7. Als u een gebruiker in een niet-gefedereerde domein, Voer uw referenties rechtstreeks op de pagina in Azure AD worden gehost. 

8. U wordt gevraagd voor een uitdaging meervoudige verificatie. 
 
9. Azure AD wordt gecontroleerd of een inschrijving in beheer van mobiele apparaten vereist is.

10. Windows het apparaat wordt geregistreerd in de map van de organisatie in Azure AD en in beheer van mobiele apparaten inschrijft, indien van toepassing.

11. Als u bent:
    - Een beheerde gebruiker Windows gaat u naar het bureaublad door het proces voor automatische aanmelding.

    - Een federatieve gebruiker u worden doorgestuurd naar de Windows-aanmeldingsscherm uw referenties in te voeren.

## <a name="verification"></a>Verificatie

Om te controleren of een apparaat wordt toegevoegd aan uw Azure AD, Controleer de **toegang tot werk of school** dialoogvenster op uw Windows-apparaat. Het dialoogvenster aangeven dat u met uw Azure AD-directory verbonden bent.

![Toegang tot werk of school](./media/azuread-joined-devices-frx/13.png)


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie de [Inleiding tot Apparaatbeheer in Azure Active Directory](overview.md).

- Zie voor meer informatie over het beheren van apparaten in de Azure AD-portal [apparaten beheert met de Azure-portal](device-management-azure-portal.md).
