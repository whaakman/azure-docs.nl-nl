---
title: Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren | Microsoft Docs
description: Informatie over het configureren van beveiligingswaarschuwingen voor Azure AD directory-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4e1cb47989011f179c54061bd29ae55b4ff86d80
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669408"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Beveiligingswaarschuwingen voor Azure AD directory-rollen in PIM configureren
## <a name="security-alerts"></a>Beveiligingswaarschuwingen
Azure Privileged Identity Management (PIM) genereert waarschuwingen wanneer er verdachte of unsafe activiteit in uw omgeving. Wanneer een waarschuwing wordt geactiveerd, wordt deze weergegeven op de PIM-dashboard. Selecteer de waarschuwing om te zien van een rapport met de gebruikers of rollen die de waarschuwing is geactiveerd.

![PIM dashboard beveiligingswaarschuwingen - schermafbeelding](./media/pim-how-to-configure-security-alerts/PIM_security_dash.png)

| Waarschuwing | Severity | Trigger | Aanbeveling |
| --- | --- | --- | --- |
| **Rollen worden toegewezen buiten PIM** |Hoog |Een gebruiker is definitief toegewezen aan een bevoorrechte rol, buiten de PIM-interface. |Bekijk de gebruikers in de lijst en de toewijzing van rollen die zijn toegewezen buiten PIM bevoorrechte. |
| **Rollen worden te vaak wordt geactiveerd.** |Middelgroot |Er zijn te veel heractiveringen van dezelfde rol binnen de tijd die is toegestaan in de instellingen. |Neem contact op met de gebruiker om te zien waarom ze hebben geactiveerd de rol zo vaak. Misschien is de tijdslimiet te kort voor ze hun taken uit te voeren, of misschien bent met behulp van scripts automatisch een rol wilt activeren. Zorg ervoor dat de duur van de activering voor hun rol lang genoeg is voor hun taken uit te voeren is ingesteld. |
| **Rollen vereisen geen meervoudige verificatie voor activering** |Middelgroot |Er zijn functies zonder MFA is ingeschakeld in de instellingen. |We MFA vereisen voor de meest maximaal beschermde rollen, maar raden ten zeerste aan dat u MFA voor de activering van alle rollen inschakelen. |
| **Gebruikers worden niet met behulp van hun bevoorrechte rollen** |Laag |Er zijn in aanmerking komende beheerders die hun rollen onlangs nog niet hebt geactiveerd. |Een toegangscontrole om te bepalen van de gebruikers die toegang meer hoeft te starten. |
| **Er zijn te veel globale beheerders** |Laag |Er zijn meer globale beheerders dan aanbevolen. |Als u een groot aantal globale beheerders hebt, is het waarschijnlijk dat gebruikers meer machtigingen krijgen dan ze nodig hebben. Verplaatsen van gebruikers tot minder bevoorrechte rollen of Maak enkele van deze in aanmerking komen voor de rol in plaats van definitief toegewezen. |

### <a name="severity"></a>Severity
* **Hoge**: direct actie vereist vanwege een schending van het beleid. 
* **Gemiddeld**: geen directe actie is vereist, maar geeft een mogelijke schending van het beleid.
* **Lage**: geen directe actie is vereist, maar stelt u een beter dan beleidswijziging.

## <a name="configure-security-alert-settings"></a>Instellingen voor beveiligingswaarschuwingen configureren
U kunt enkele van de beveiligingswaarschuwingen in PIM om te werken met uw omgeving en beveiligingsdoelen. Volg deze stappen voor het bereiken van de instellingenblade:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) en selecteer de **Azure AD Privileged Identity Management** tegel vanuit het dashboard.
2. Selecteer **beheerd bevoorrechte rollen** > **instellingen** > **waarschuwingsinstellingen**.
   
    ![Navigeer naar de beveiligingsinstellingen voor waarschuwingen](./media/pim-how-to-configure-security-alerts/PIM_security_settings.png)

### <a name="roles-are-being-activated-too-frequently-alert"></a>Waarschuwing "Rollen worden geactiveerd te vaak"
Deze waarschuwing wordt geactiveerd als een gebruiker dezelfde bevoorrechte rol meerdere keren binnen een opgegeven periode activeert. U kunt zowel de periode en het aantal activeringen configureren.

* **Activeringsvernieuwing**: opgeven in dagen, uren, minuten en de periode die u gebruiken wilt voor het bijhouden van verdachte vernieuwingen seconde.
* **Aantal vernieuwingen activering**: Geef het aantal activeringen van 2 tot en met 100, waarmee u rekening houden daling van waarschuwing in de periode die u hebt gekozen. U kunt deze instelling wijzigen door de schuifregelaar of een getal in het tekstvak te typen.

### <a name="there-are-too-many-global-administrators-alert"></a>"Er zijn te veel globale beheerders" waarschuwing
PIM wordt deze waarschuwing geactiveerd als twee verschillende criteria wordt voldaan, en u kunt beide providers configureren. Eerst moet u een bepaalde drempelwaarde voor globale beheerders bereiken. Ten tweede moet een bepaald percentage van het totaal aantal roltoewijzingen globale beheerders. Als u alleen een van deze metingen aan, wordt de waarschuwing niet weergegeven.  

* **Minimum aantal globale beheerders**: Geef het aantal globale beheerders, uit 2 tot en met 100, dat u rekening houden met een onveilige bedrag.
* **Percentage globale beheerders**: Geef op welk percentage van de beheerders die globale beheerders, van 0% tot 100%, die is onveilig in uw omgeving.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Waarschuwing "Beheerders worden niet met behulp van hun bevoorrechte rollen"
Deze waarschuwing wordt geactiveerd wanneer een gebruiker een bepaalde hoeveelheid tijd opgegeven zonder een rol activeren.

* **Aantal dagen**: Geef het aantal dagen tussen 0 en 100, die een gebruiker zonder het activeren van een rol kan raadplegen.

## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
