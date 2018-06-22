---
title: Veelgestelde vragen over Azure Active Directory device management | Microsoft Docs
description: Azure Active Directory Apparaatbeheer Veelgestelde vragen over.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 60b77f5956cb627905eb955995652098337c4dea
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309860"
---
# <a name="azure-active-directory-device-management-faq"></a>Veelgestelde vragen over Azure Active Directory device management



**V: hoe kan ik een Mac OS-apparaat registreren?**

**A:** kunt u Mac OS-apparaat registreren:

1.  [Een nalevingsbeleid maken](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Beleid voor voorwaardelijke toegang voor Mac OS apparaten definiëren](active-directory-conditional-access-azure-portal.md) 

**Opmerkingen:**

- De gebruikers die zijn opgenomen in uw beleid voor voorwaardelijke toegang moeten een [ondersteunde versie van Office voor Mac OS](active-directory-conditional-access-technical-reference.md#client-apps-condition) voor toegang tot bronnen. 

- Tijdens de eerste poging van de toegang tot uw gebruikers gevraagd het apparaat via de bedrijfsportal te registreren.

---

**V: ik het apparaat onlangs geregistreerd. Waarom zie ik het apparaat niet onder Mijn gebruikersgegevens in de Azure portal?**

**A:** Windows 10-apparaten die zijn hybride die lid zijn van Azure AD, worden niet weergegeven onder de apparaten van gebruikers.
U moet alle apparaten weergeven in Azure-portal gebruiken. U kunt ook PowerShell [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Alleen de volgende apparaten worden vermeld in de apparaten van gebruikers:

- Alle persoonlijke apparaten die niet hybride Azure AD zijn toegevoegd. 
- Alle niet - Windows 10 / apparaten met Windows Server 2016.
- Alle niet-Windows-apparaten 

--- 

**V: hoe weet wat de status van het apparaat registratie van de client is?**

**A:** u kunt de Azure portal gebruiken, gaat u naar alle apparaten en zoek naar het apparaat met apparaat-ID. Controleer de waarde onder de kolom van het type join.

Als u controleren van de status van het lokale apparaat registratie van een geregistreerd apparaat wilt:

- Voer voor Windows 10 en Windows Server 2016 of hoger, dsregcmd.exe/status.
- Voor eerdere versies van het besturingssysteem, voert u '%programFiles%\Microsoft werkplek Join\autoworkplace.exe'

---

**V: ik hebt verwijderd in de Azure portal of via Windows PowerShell, maar de lokale staat op het apparaat staat nog steeds wordt geregistreerd?**

**A:** dit is standaard. Het apparaat geen toegang tot bronnen in de cloud. 

Als u wilt opnieuw opnieuw te registreren, moet een handmatige actie moet worden uitgevoerd op het apparaat. 

Schakel de join-status van Windows 10 en Windows Server 2016 die on-premises AD-domein:

1.  Open de opdrachtprompt als beheerder.

2.  type `dsregcmd.exe /debug /leave`

3.  Meld u af en meld u aan voor het activeren van de geplande taak die het apparaat wordt geregistreerd met Azure AD opnieuw. 

Voor eerdere Windows-besturingssysteem versies die zich on-premises AD-domein:

1.  Open de opdrachtprompt als beheerder.
2.  Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---
** V: Hoe ik loskoppelen van een Azure AD join-apparaat lokaal op het apparaat?
**A:** 
- Zorg dat u automatische inschrijving uitschakelen zodat de geplande taak voor het apparaat niet opnieuw registreren voor hybride Azure AD join-apparaten. Open vervolgens opdrachtprompt als beheerder en typ `dsregcmd.exe /debug /leave`. U kunt ook worden deze opdracht uitgevoerd als een script op meerdere apparaten bulksgewijs loskoppelen.

- Voor pure Azure AD join apparaten, zorg ervoor dat u beschikt over offline lokaal administrator-account of maak een, als u niet meer kunnen aanmelden met de referenties van een Azure AD-gebruiker. Ga vervolgens naar **instellingen** > **Accounts** > **toegang werk of School**. Selecteer uw account en klik op **Disconnect**. Volg de aanwijzingen en geef de referenties van de lokale beheerder als u wordt gevraagd. Start opnieuw op het apparaat om het loskoppelen proces te voltooien.

---

**V: Waarom zie ik dubbele apparaatvermeldingen in Azure-portal?**

**A:**

-   Voor Windows 10 en Windows Server 2016, als er herhaalde pogingen tot het loskoppelen van en opnieuw aanmelden voor hetzelfde apparaat, kunnen er dubbele vermeldingen. 

-   Als u toevoegen werk of School-Account hebt gebruikt, wordt elke windows-gebruiker die voegen werk- of Schoolaccount gebruikt een nieuwe apparaatrecord maken met dezelfde naam van het apparaat.

-   Voor eerdere Windows-besturingssysteem versies die zich on-premises maakt AD-domein met behulp van automatische registratie een nieuwe apparaatrecord met dezelfde naam voor elke domeingebruiker die zich bij het apparaat aanmeldt. 

-   Een gekoppelde Azure AD-computer die is gewist, opnieuw geïnstalleerd en opnieuw toegevoegd met dezelfde naam wordt weergegeven als een andere record met dezelfde naam van het apparaat.

---

**V: waarom een gebruiker nog steeds toegang tot bronnen vanaf een apparaat dat ik hebt uitgeschakeld in de Azure portal?**

**A:** kan duren tot een uur voor een intrekken moet worden toegepast.

>[!Note] 
>U wordt aangeraden wissen van het apparaat om ervoor te zorgen dat gebruikers geen toegang de bronnen tot voor ingeschreven apparaten. Zie voor meer informatie [apparaten inschrijven voor beheer in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**V: Waarom kunnen mijn gebruikers zien 'U geen toegang vanaf hier'?**

**A:** als u bepaalde regels voor voorwaardelijke toegang om ervoor te zorgen, de status van een specifiek apparaat hebt geconfigureerd en het apparaat niet aan de criteria voldoet, gebruikers worden geblokkeerd en dit bericht wordt weergegeven. De beleidsregels voor voorwaardelijke toegang evalueren en zorg ervoor dat het apparaat kunnen voldoen aan de criteria om te voorkomen dat dit bericht.

---


**V: ik Zie de record van apparaat onder de gebruikersgegevens in de Azure portal en kunt zien hoe de status op het apparaat is geregistreerd. Kan dat ik een correct instellen voor het gebruik van voorwaardelijke toegang?**

**A:** de apparaatstatus join, weergegeven door de apparaat-id, moet overeenkomen met die op Azure AD en voldoen aan de evaluatiecriteria van een voor voorwaardelijke toegang. Zie voor meer informatie [aan de slag met Azure Active Directory-apparaatregistratie](active-directory-device-registration.md).

---

**V: Waarom krijg ik een bericht 'gebruikersnaam of wachtwoord is onjuist' voor een apparaat dat ik zojuist hebt toegevoegd aan Azure AD?**

**A:** veelvoorkomende redenen voor dit scenario zijn:

- Uw referenties zijn niet langer geldig.

- Uw computer staat kan niet communiceren met Azure Active Directory. Controleer of eventuele problemen met de netwerkverbinding.

- Azure AD Join aan de vereisten is niet voldaan aan. Zorg ervoor dat u de stappen hebt gevolgd [cloudfuncties uitbreiden naar Windows 10-apparaten via Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Federatieve aanmeldingen vereist uw federatieserver ter ondersteuning van een actieve WS-Trust-eindpunt. 

- Pass through-verificatie is ingeschakeld en de gebruiker is een tijdelijk wachtwoord dat moet worden gewijzigd bij aanmelding.

---

**V: Waarom zie ik de "... Oops is een fout opgetreden!" dialoogvenster wanneer ik probeer doen van Azure AD join mijn computer?**

**A:** dit is een resultaat van het instellen van Azure Active Directory-inschrijving met Intune. Controleer of de gebruiker probeert Azure AD join over de juiste Intune-licentie toegewezen. Zie voor meer informatie [Windows Apparaatbeheer instellen](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**V: waarom niet mijn poging om te nemen van een PC maar ik heb informatie over de fout niet ontvangen?**

**A:** een waarschijnlijke oorzaak is dat de gebruiker is aangemeld bij het apparaat met het ingebouwde lokale beheerdersaccount. Maak een andere lokale account voordat u Azure Active Directory Join gebruikt om de installatie te voltooien. 

---

**V: waar vind ik instructies voor het instellen van automatische apparaatregistratie**

**A:** Zie voor gedetailleerde instructies [automatische registratie van Windows-domein-apparaten met Azure Active Directory configureren](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**V: waar vind ik het oplossen van informatie over de automatische apparaatregistratie?**

**A:** voor informatie over probleemoplossing, Zie:

- [Het oplossen van automatische registratie van domein computers toegevoegd aan Azure AD. – Windows 10 en Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Het oplossen van automatische registratie van domein computers toegevoegd aan Azure AD. voor Windows downlevel-clients](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

