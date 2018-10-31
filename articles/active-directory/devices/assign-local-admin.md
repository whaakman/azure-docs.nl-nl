---
title: Over het beheren van de lokale groep administrators op de Azure AD gekoppelde apparaten | Microsoft Docs
description: Leer hoe u Azure-rollen toewijzen aan de groep lokale beheerders van een Windows-apparaat.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: markvi
ms.reviewer: ravenn
ms.openlocfilehash: 267e398305939048014038b7350b2a10481fdfbd
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238723"
---
# <a name="how-to-manage-the-local-administrators-group-on-azure-ad-joined-devices"></a>Over het beheren van de lokale groep administrators op de Azure AD gekoppelde apparaten

Voor het beheren van een Windows-apparaat, moet u lid zijn van de lokale groep administrators. Azure AD-updates als onderdeel van de join-proces van Azure Active Directory (Azure AD), het lidmaatschap van deze groep op een apparaat. U kunt het lidmaatschap bijwerken om te voldoen aan uw bedrijfsvereisten aanpassen. Een update in het groepslidmaatschap is bijvoorbeeld handig als u uw helpdeskpersoneel wilt doen taken waarvoor de administrator-rechten op een apparaat inschakelen.

In dit artikel wordt uitgelegd hoe de update lidmaatschap werkt en hoe u deze kunt aanpassen tijdens een Azure AD Join. De inhoud van dit artikel niet van toepassing op een **hybride** koppelen aan Azure AD.


## <a name="how-it-works"></a>Hoe werkt het?

Wanneer u een Windows-apparaat te met Azure AD met behulp van een Azure AD-join verbinden, wordt de volgende beveiligingsprincipes in Azure AD toegevoegd aan de groep lokale beheerders op het apparaat:

- De globale beheerdersrol van Azure AD
- De beheerdersrol van Azure AD-apparaat 
- De gebruiker die de Azure AD join   

Azure AD-rollen toevoegt aan de lokale groep administrators, kunt u de gebruikers die een apparaat in Azure AD op elk gewenst moment beheren kunnen zonder te hoeven wijzigen op het apparaat bijwerken. Op dit moment kunt u groepen toewijzen aan een rol voor beheerders.
Azure AD wordt de rol van apparaatbeheerder Azure AD ook toegevoegd aan de groep lokale beheerders voor de ondersteuning van het principe van minimale bevoegdheden (PoLP). Naast de globale beheerders, kunt u ook gebruikers die zijn inschakelen *alleen* toegewezen van de rol van apparaatbeheerder om een apparaat te beheren. 


## <a name="manage-the-global-administrators-role"></a>De rol globale beheerders beheren

Als u wilt weergeven en bijwerken van het lidmaatschap van de rol globale beheerder, Zie:

- [Alle leden van een beheerdersrol weergeven in Azure Active Directory](../users-groups-roles/directory-manage-roles-portal.md)

- [Een gebruiker toewijzen aan beheerdersrollen in Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md)


## <a name="manage-the-device-administrator-role"></a>Beheren van de rol van apparaatbeheerder 

In de Azure-portal, kunt u de rol van apparaatbeheerder beheren op de **apparaten** pagina. Om te openen de **apparaten** pagina:

1. Aanmelden bij uw [Azure-portal](https://portal.azure.com) als een globale beheerder of apparaatbeheerder van het.
2. Klik op de navigatiebalk links **Azure Active Directory**. 
3. In de **beheren** sectie, klikt u op **apparaten**.
4. Op de **apparaten** pagina, klikt u op **apparaatinstellingen**.

Configureren voor het wijzigen van de rol van apparaatbeheerder **extra lokale beheerders op Azure AD gekoppelde apparaten**.  

![Extra lokale beheerders](./media/assign-local-admin/10.png)

 
Apparaatbeheerders zijn toegewezen aan alle Azure AD gekoppelde apparaten. U kan niet het bereik van apparaatbeheerders voor een specifieke set apparaten. Bijwerken van de rol van apparaatbeheerder, niet per se een directe invloed hebben op de betrokken gebruikers. Een gebruiker is al aangemeld bij voor de apparaten, de bevoegdheid-update plaatsvindt:
     

- Wanneer een gebruiker zich aanmeldt uitschakelen.
- Na 4 uur, wanneer er een nieuwe is primaire Refresh Token uitgegeven. 




## <a name="manage-regular-users"></a>Normale gebruikers beheren

Standaard wordt de gebruiker die de Azure AD join aan de groep Administrators op het apparaat uitvoert met Azure AD toegevoegd. Als u voorkomen dat normale gebruikers wilt tegen lokale beheerders om, hebt u de volgende opties:

- [Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot) -Windows Autopilot kunt u met de mogelijkheid om te voorkomen dat de primaire gebruiker uitvoeren van de join tegen om een lokale beheerder zijn. U doet dit door [een Autopilot-profiel maken](https://docs.microsoft.com/intune/enrollment-autopilot#create-an-autopilot-deployment-profile).
 
- [Bulkinschrijving](https://docs.microsoft.com/intune/windows-bulk-enroll) -een Azure AD join die wordt uitgevoerd in de context van een bulkinschrijving gebeurt in de context van een gebruiker automatisch is gemaakt. Gebruikers aanmelden nadat een apparaat is toegevoegd, zijn niet toegevoegd aan de groep administrators.   



## <a name="manually-elevate-a-user-on-a-device"></a>Een gebruiker handmatig met verhoogde bevoegdheden op een apparaat 

Naast het gebruik van de Azure AD join-proces, kunt u ook handmatig een gewone gebruiker om te worden van een lokale beheerder op een specifiek apparaat verhogen. Deze stap moet u al een lid van de lokale groep administrators. 

Beginnen met de **Windows 10 1709** release, kunt u deze taak uitvoeren vanuit **instellingen -> Accounts andere gebruikers ->** door te selecteren **toevoegen een werk- of schoolaccount gebruiker**.
 
U kunt daarnaast ook gebruikers via de opdrachtprompt toevoegen:

- Als uw tenantgebruikers worden gesynchroniseerd vanuit on-premises Active Directory, gebruikt u `net localgroup administrators /add "Contoso\username"`.

- Als uw tenantgebruikers worden gemaakt in Azure AD, gebruiken `net localgroup administrators /add "AzureAD\UserUpn"`


## <a name="considerations"></a>Overwegingen 

U kunt groepen toewijzen aan de rol van apparaatbeheerder, zijn alleen afzonderlijke gebruikers toegestaan.

Apparaatbeheerders zijn toegewezen aan alle Azure AD verbonden apparaten. Ze kunnen niet worden toegewezen aan een specifieke set apparaten.

Wanneer u gebruikers uit de rol van apparaatbeheerder verwijdert, hebben ze nog steeds de lokale administrator-bevoegdheden op een apparaat zo lang ze zijn aangemeld bij deze. De bevoegdheden worden ingetrokken wanneer de volgende-eenmalig gebruik of na 4 uur wanneer een nieuwe primaire vernieuwingstoken dat is uitgegeven.



## <a name="next-steps"></a>Volgende stappen

- Zie [Apparaten beheren met behulp van de Azure-portal](device-management-azure-portal.md) voor een overzicht van hoe u apparaten in de Azure-portal kunt beheren.

- Zie [Beleid configureren voor apparaatgebaseerde voorwaardelijke toegang van Azure Active Directory](../conditional-access/require-managed-devices.md) voor meer informatie over apparaatgebaseerde voorwaardelijke toegang.


