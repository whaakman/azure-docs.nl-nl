---
title: Activeren of deactiveren van een rol | Microsoft Docs
description: Informatie over het activeren van rollen voor bevoegde identiteiten met de Azure Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 0ad95cab1ae8072004a528e449cf828b32500be8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590399"
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Activeren of deactiveren van rollen in Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management vereenvoudigt de manier waarop ondernemingen bevoegde toegang tot resources in Azure AD en andere Microsoft online services zoals Office 365 en Microsoft Intune beheren.  

Als u hebt aangebracht in aanmerking voor een beheerdersrol, betekent dit dat u die rol kunt activeren wanneer u nodig hebt bevoegde acties uit te voeren. Bijvoorbeeld als u Office 365-functies van tijd tot tijd beheert, beheerders met bevoegdheid van uw organisatie mogelijk niet moet u een permanente globale beheerder, omdat die rol te invloed is op andere services. In plaats daarvan moeten deze u in aanmerking voor Azure AD-rollen, zoals Exchange Online-beheerder. U kunt aanvragen om te activeren die rol wanneer u de bevoegdheden nodig hebt, en vervolgens beheer voor een vooraf vastgestelde periode hebt u.

In dit artikel is bedoeld voor beheerders die nodig zijn om hun rol in Azure AD Privileged Identity Management (PIM) te activeren. Dit helpt u de stappen beschreven voor een rol activeren wanneer u de machtigingen nodig hebt, en de rol deactiveren wanneer u klaar bent. Bovendien kunnen beheerders met bevoegdheid goedkeuring om een rol (Preview) te activeren vereisen. Meer informatie over [PIM goedkeuringswerkstromen](./azure-ad-pim-approval-workflow.md) hier.

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
Gebruik de Azure AD Privileged Identity Management-toepassing in de [Azure-portal](https://portal.azure.com/) om aan te vragen van een rol moet worden geactiveerd, zelfs als u wilt werken in een andere portal of PowerShell. Als u niet de Azure AD Privileged Identity Management-toepassing in uw Azure-portal hebt, volgt u deze stappen om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikersnaam in de rechterbovenhoek van de Azure-portal en selecteer de map waar u u wilt worden uitgevoerd.
3. Selecteer **Alle services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="activate-a-role"></a>Een rol activeren
Wanneer u nodig hebt om een rol te, kunt u de activering aanvragen door het selecteren van de **mijn rollen** navigatieoptie in de Azure AD Privileged Identity Management-toepassing de navigatiekolom links.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de tegel Azure AD Privileged Identity Management.
2. Selecteer **mijn rollen**. Een lijst met uw in aanmerking komende toegewezen rollen in de groep aan de bovenkant van de pagina weergegeven.
3. Selecteer een rol te activeren.
4. Selecteer **activeren**. De **rolactivering aanvragen** blade wordt weergegeven.
5. Sommige rollen is multi-factor Authentication (MFA) vereist voordat u de rol kunt activeren. U moet slechts één keer per sessie geverifieerd.
   
    ![Controleer of met MFA voor rolactivering van de - schermafbeelding](./media/pim-how-to-activate-role/PIM_activation_MFA.png)
6. De reden voor de activeringsaanvraag invoeren in het tekstveld.  Sommige rollen moeten u een Ticketnummer problemen opgeven.
7. Selecteer **OK**.  Als de rol geen goedkeuring is vereist, wordt deze nu geactiveerd en de rol wordt weergegeven in de lijst met actieve rollen (direct onder de lijst met in aanmerking komende roltoewijzingen). Als de [rol is goedkeuring vereist](./azure-ad-pim-approval-workflow.md) wilt activeren, verschijnt een pop-upmelding kort in de rechterbovenhoek van uw browser waarin de aanvraag is in afwachting van goedkeuring.

    ![Aanvraag in behandeling melding - schermafbeelding](./media/pim-how-to-activate-role/PIM_Request_Pending_Toast2.png)

## <a name="deactivate-a-role"></a>Een rol deactiveren
Wanneer een rol is geactiveerd, wordt deze automatisch uitgeschakeld wanneer de tijdslimiet (in aanmerking komende duur) is bereikt.

Als u al vroeg beheertaken uitvoeren, kunt u ook een rol handmatig in de Azure AD Privileged Identity Management-toepassing deactiveren.  Selecteer **mijn rollen**, kiest u de rol die u klaar bent met behulp van de **actieve roltoewijzingen** groeperen en selecteer **deactiveren**.  

## <a name="cancel-a-pending-request"></a>Een aanvraag in behandeling annuleren
In het geval u geen activering van een rol waarvoor goedkeuring wordt vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren. Selecteer de **mijn rollen** navigatieoptie in de Azure AD Privileged Identity Management-toepassing de navigatiekolom links.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de tegel Azure AD Privileged Identity Management.
2. Selecteer **mijn rollen**. Een lijst met uw in aanmerking komende toegewezen rollen in de groep aan de bovenkant van de pagina weergegeven.
3. Selecteer een rol.
4. Selecteer de **activering is in afwachting van goedkeuring** standaardvaandel op de blade rol activeren.
5. Selecteer **annuleren** aan de bovenkant van de **in afwachting van goedkeuring** blade.

   ![Schermafbeelding van de aanvraag in behandeling annuleren](./media/pim-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png)

## <a name="next-steps"></a>Volgende stappen
Als u geïnteresseerd in meer informatie over Azure AD Privileged Identity Management bent, hebben de volgende koppelingen meer informatie.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
