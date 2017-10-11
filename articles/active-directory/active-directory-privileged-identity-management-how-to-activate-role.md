---
title: Hoe u een rol activeren of deactiveren | Microsoft Docs
description: Informatie over het activeren van rollen voor bevoegde identiteiten met de Azure Privileged Identity Management-toepassing.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: a143fd78eae52fda0cbadb7e74fd8209f24629a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Activeren of deactiveren van rollen in Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management vereenvoudigt de manier waarop bedrijven bevoorrechte toegang tot resources in Azure AD en andere Microsoft online services zoals Office 365 of Microsoft Intune beheert.  

Als u hebt aangebracht in aanmerking komen voor een beheerderrol, betekent dit dat kunt u die rol activeren als u wilt uitvoeren van bevoegdheden. Bijvoorbeeld, als u af en functies van Office 365 beheren, van uw organisatie bevoorrechte rol administrators mogelijk niet aanbrengen u een permanente hoofdbeheerder omdat die rol van invloed is op andere services te. In plaats daarvan zij u in aanmerking komen voor Azure AD-functies, zoals Exchange Online-beheerder. U kunt aanvragen te activeren die rol wanneer u de bevoegdheden nodig, en vervolgens u controle van de beheerder voor een vooraf vastgestelde periode hebt.

Dit artikel is bedoeld voor beheerders die voor het activeren van hun rol in Azure AD Privileged Identity Management (PIM) nodig hebben. Dit leidt u door de stappen voor het activeren van een rol wanneer u de machtigingen moet en de rol deactiveren wanneer u klaar bent. Bevoorrechte rol beheerders kunnen bovendien goedkeuring voor het activeren van een rol (Preview) vereisen. Meer informatie over [PIM goedkeuringswerkstromen](./privileged-identity-management/azure-ad-pim-approval-workflow.md) hier.

## <a name="add-the-privileged-identity-management-application"></a>De Privileged Identity Management-toepassing toevoegen
Gebruik de Azure AD Privileged Identity Management-toepassing in de [Azure-portal](https://portal.azure.com/) om aan te vragen van een rol moet worden geactiveerd, zelfs als u werkt met een andere portal of PowerShell. Als u niet de Azure AD Privileged Identity Management-toepassing in uw Azure-portal hebt, als volgt te werk om te beginnen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer uw gebruikersnaam in de rechterbovenhoek van het Azure-portal en selecteer de map waar u wilt dat u werkt.
3. Selecteer **Meer services** en gebruik het tekstvak Filteren om te zoeken naar **Azure AD Privileged Identity Management**.
4. Schakel **Vastmaken aan dashboard** in en klik op de knop **Maken**. De Privileged Identity Management-toepassing wordt geopend.

## <a name="activate-a-role"></a>Een rol activeren
Wanneer u om een rol te vervullen, kunt u de activering aanvragen door het selecteren van de **mijn rollen** navigatieoptie in de Azure AD Privileged Identity Management-toepassing de linker navigatie-kolom.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de tegel Azure AD Privileged Identity Management.
2. Selecteer **mijn rollen**. Een lijst met uw toegewezen in aanmerking komende rollen weergegeven in de groepering boven aan de pagina.
3. Selecteer een rol te activeren.
4. Selecteer **activeren**. De **rolactivering aanvragen** blade wordt weergegeven.
5. Sommige rollen is multi-factor Authentication (MFA) vereist voordat u de rol kunt activeren. U hoeft alleen te verifiëren eenmaal per sessie.
   
    ![Controleer of met MFA voor rolactivering van de - schermafbeelding][2]
6. De reden voor de activeringsaanvraag invoeren in het tekstveld.  Sommige rollen is vereist op te geven van een Ticketnummer problemen.
7. Selecteer **OK**.  Als de functie niet goedkeuring vereist, wordt nu wordt geactiveerd en de rol wordt weergegeven in de lijst met actieve rollen (direct onder de lijst van in aanmerking komende roltoewijzingen). Als de [rol is goedkeuring vereist](./privileged-identity-management/azure-ad-pim-approval-workflow.md) wilt activeren, verschijnt een pop-upmelding kort in de rechterbovenhoek van uw browser geïnformeerd wanneer de aanvraag is in afwachting van goedkeuring.

    ![Aanvraag in behandeling melding - schermafbeelding][3]

## <a name="deactivate-a-role"></a>Een rol deactiveren
Zodra een rol is geactiveerd, wordt deze automatisch uitgeschakeld wanneer de tijdslimiet (duur in aanmerking komende) is bereikt.

Als u beheertaken vroeg hebt voltooid, kunt u ook een rol handmatig in de Azure AD Privileged Identity Management-toepassing deactiveren.  Selecteer **mijn rollen**, kiest u de rol die u klaar bent met behulp van de **Active roltoewijzingen** groeperen en selecteer **deactiveren**.  

## <a name="cancel-a-pending-request"></a>Annuleren van een aanvraag in behandeling
In het geval u geen activering van een rol waarvoor goedkeuring wordt vereist, kunt u een aanvraag in behandeling op elk gewenst moment annuleren. Gewoon selecteert de **mijn rollen** navigatieoptie in de Azure AD Privileged Identity Management-toepassing de linker navigatie-kolom.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de tegel Azure AD Privileged Identity Management.
2. Selecteer **mijn rollen**. Een lijst met uw toegewezen in aanmerking komende rollen weergegeven in de groepering boven aan de pagina.
3. Selecteer een rol.
4. Selecteer de **activering is in afwachting van goedkeuring** standaardvaandel op de blade rol activering details.
5. Selecteer **annuleren** boven aan de **in afwachting van goedkeuring** blade.

   ![Annuleren van aanvraag in behandeling-schermafbeelding][4]

## <a name="next-steps"></a>Volgende stappen
Als u geïnteresseerd in meer informatie over Azure AD Privileged Identity Management bent, hebben de volgende koppelingen meer informatie.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
