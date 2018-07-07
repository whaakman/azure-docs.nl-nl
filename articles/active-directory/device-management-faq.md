---
title: Apparaatbeheer via Azure Active Directory Veelgestelde vragen over | Microsoft Docs
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
ms.openlocfilehash: 864f790db48d3d4542ed56a4c7272a198df5bd56
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901133"
---
# <a name="azure-active-directory-device-management-faq"></a>Apparaatbeheer via Azure Active Directory Veelgestelde vragen



**V: hoe kan ik een macOS-apparaat registreren?**

**A:** om macOS-apparaat te registreren:

1.  [Een nalevingsbeleid maken](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definieer een beleid voor voorwaardelijke toegang voor macOS-apparaten](active-directory-conditional-access-azure-portal.md) 

**Opmerking:**

- De gebruikers die zijn opgenomen in uw beleid voor voorwaardelijke toegang moeten een [een ondersteunde versie van Office voor Mac OS](active-directory-conditional-access-technical-reference.md#client-apps-condition) voor toegang tot resources. 

- Uw gebruikers wordt gevraagd tijdens de eerste poging van de toegang tot het apparaat met behulp van de bedrijfsportal-App te registreren.

---

**V: ik het apparaat recent geregistreerd. Waarom zie ik het apparaat niet onder Mijn gebruikersgegevens in Azure portal?**

**A:** Windows 10-apparaten die zijn toegevoegd aan hybrid Azure AD, worden niet weergegeven bij de apparaten van de gebruiker.
U moet alle apparaten weergeven in Azure portal gebruiken. U kunt ook PowerShell gebruiken [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Alleen de volgende apparaten worden vermeld in de apparaten van de gebruiker:

- Alle persoonlijke apparaten die niet hybride Azure AD zijn toegevoegd. 
- Alle niet - Windows 10 / Windows Server 2016-apparaten.
- Alle niet-Windows-apparaten 

--- 

**V: hoe weet ik wat de status van de apparaatregistratie van de client is?**

**A:** u kunt de Azure-portal gebruiken, gaat u naar alle apparaten en zoeken naar het apparaat met behulp van apparaat-ID. Controleer de waarde onder de kolom van het type join.

Als u controleren van de status van de lokale apparaatregistratie vanaf een geregistreerde apparaten wilt:

- Voor Windows 10 en Windows Server 2016 of hoger, voert u dsregcmd.exe status.
- Voor eerdere versies van het besturingssysteem, voert u "%programFiles%\Microsoft werkplek Join\autoworkplace.exe"

---

**V: ik hebt verwijderd in Azure portal of met behulp van Windows PowerShell, maar de lokale staat op het apparaat nog steeds is geregistreerd, zegt?**

**A:** dit is standaard. Het apparaat geen toegang tot resources in de cloud. 

Als u opnieuw opnieuw te registreren wilt, moet een handmatige actie moeten worden uitgevoerd op het apparaat. 

Schakel de join-status van Windows 10 en Windows Server 2016 die on-premises AD-domein:

1.  Open de opdrachtprompt als beheerder.

2.  Type `dsregcmd.exe /debug /leave`

3.  Meld u af en meld u aan voor het activeren van de geplande taak die het apparaat wordt geregistreerd bij Azure AD het opnieuw. 

Voor versies van het eerdere Windows-besturingssysteem die zich on-premises AD-domein:

1.  Open de opdrachtprompt als beheerder.
2.  Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---
**V: hoe ik loskoppelen van een Azure AD join-apparaat lokaal op het apparaat?**

**A:** 
- Zorg dat u automatische inschrijving uitschakelen zodat de geplande taak het apparaat niet opnieuw registreren voor hybride Azure AD verbonden apparaten. Open vervolgens de opdrachtprompt als beheerder en typ `dsregcmd.exe /debug /leave`. U kunt ook worden deze opdracht uitgevoerd als een script op meerdere apparaten bulksgewijs loskoppelen.

- Voor pure Azure AD verbonden apparaten, zorg ervoor dat u hebt een offline lokale administrator-account of maak een, kunt u zich niet aanmelden met de referenties van een Azure AD-gebruiker. Ga vervolgens naar **instellingen** > **Accounts** > **toegang tot werk of School**. Selecteer uw account en klik op **verbinding verbreken**. Volg de aanwijzingen en geef de referenties van de lokale beheerder wanneer hierom wordt gevraagd. Start opnieuw op het apparaat om het loskoppelen-proces te voltooien.

---

**V: Mijn gebruikers kunnen niet zoeken naar printers uit Azure AD verbonden apparaten. Hoe kan ik afdrukken vanuit Azure AD Joined-apparaten inschakelen?**

**A:** voor het implementeren van printers voor Azure AD verbonden apparaten, Zie [hybride cloud print](https://docs.microsoft.com/en-us/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). U moet een Windows-Server on-premises implementatie van hybride cloud print. Afdrukservice cloud-gebaseerd is op dit moment niet beschikbaar. 

---

**V: Waarom zie ik dubbele apparaatvermeldingen in Azure portal?**

**A:**

-   Voor Windows 10 en Windows Server 2016, als er herhaalde pogingen voor loskoppelen en opnieuw aanmelden voor hetzelfde apparaat, kunnen er dubbele vermeldingen. 

-   Als u toevoegen werk of School-Account hebt gebruikt, wordt een nieuwe record van apparaat in elke windows-gebruiker die gebruikmaakt van toevoegen werk of School-Account maken met dezelfde naam van het apparaat.

-   Voor versies van het eerdere Windows-besturingssysteem die zich on-premises maakt AD domein met behulp van automatische registratie een nieuwe record van apparaat met dezelfde naam voor elke domeingebruiker die zich bij het apparaat aanmeldt. 

-   Een Azure AD is toegevoegd aan virtuele machine die is gewist, opnieuw geïnstalleerd en opnieuw worden samengevoegd met dezelfde naam wordt weergegeven als een andere record met dezelfde naam van het apparaat.

---

**V: waarom een gebruiker nog steeds toegang tot bronnen vanaf een apparaat dat ik in Azure portal hebt uitgeschakeld?**

**A:** het kan een uur duren voor een intrekken moet worden toegepast.

>[!Note] 
>Voor ingeschreven apparaten, wordt u aangeraden wissen van het apparaat om ervoor te zorgen dat gebruikers geen toegang heeft tot de resources. Zie voor meer informatie, [apparaten inschrijven voor beheer in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**V: Waarom kunnen mijn gebruikers zien 'U kunt daar niet komen vanaf hier'?**

**A:** als u bepaalde regels voor voorwaardelijke toegang om te vereisen dat de status van een specifiek apparaat hebt geconfigureerd en het apparaat voldoet niet aan de criteria, gebruikers worden geblokkeerd en wordt dit bericht wordt weergegeven. De beleidsregels voor voorwaardelijke toegang evalueren en zorg ervoor dat het apparaat kunnen voldoen aan de criteria om te voorkomen dat dit bericht.

---

**V: Waarom doe deel van mijn gebruikers niet ophalen MFA prompts in Azure AD gekoppelde apparaten?**

**A:** als gebruiker joins of een apparaat wordt geregistreerd bij Azure AD met behulp van multi-factor Authentication, wordt het apparaat zelf een vertrouwde tweede factor voor deze bepaalde gebruiker. Later, wanneer de gebruiker zich aanmeldt bij het apparaat en toegang verkrijgt een toepassing tot, Azure AD rekening gehouden met het apparaat als een tweede factor en kan die gebruiker naadloos toegang krijgen tot hun toepassingen, zonder extra prompts voor MFA. Dit gedrag is niet van toepassing op een andere gebruiker het apparaat aan te melden, zodat andere gebruikers met toegang tot het apparaat wordt nog steeds gevraagd met een MFA-controle voordat u toegang tot toepassingen waarvoor MFA is vereist.

---

**V: ik Zie de record van apparaat onder de gegevens van de gebruiker in Azure portal en ziet de status geregistreerd op het apparaat. Kom dat ik correct instellen voor het gebruik van voorwaardelijke toegang?**

**A:** de apparaatstatus join, weergegeven in de apparaat-id, moet overeenkomen met die in Azure AD en voldoen aan de evaluatiecriteria van een voor voorwaardelijke toegang. Zie voor meer informatie, [aan de slag met Azure Active Directory-apparaatregistratie](active-directory-device-registration.md).

---

**V: Waarom krijg ik een bericht 'gebruikersnaam of wachtwoord is onjuist' voor een apparaat dat ik net hebt toegevoegd aan Azure AD?**

**A:** veelvoorkomende redenen waarom voor dit scenario zijn:

- Uw gebruikersreferenties zijn niet meer geldig.

- Uw computer staat kan niet communiceren met Azure Active Directory. Controleer of eventuele problemen met de netwerkverbinding.

- De vereisten voor Azure AD Join is niet voldaan aan. Zorg ervoor dat u de stappen hebt gevolgd [cloudmogelijkheden uitbreiden naar Windows 10-apparaten via Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Federatieve aanmelding is vereist voor uw federation-server voor de ondersteuning van een actieve WS-Trust-eindpunt. 

- U kunt Pass through-verificatie hebt ingeschakeld en de gebruiker heeft een tijdelijk wachtwoord dat moet worden gewijzigd bij de aanmelding.

---

**V: Waarom zie ik de ' Oeps... is een fout opgetreden! "dialoogvenster wanneer ik probeer Azure AD kan deelnemen aan mijn PC?**

**A:** dit is het resultaat van het instellen van Azure Active Directory-inschrijving met Intune. Zorg ervoor dat de gebruiker die probeert te doen van Azure AD join juiste Intune-licentie is toegewezen heeft. Zie voor meer informatie, [Windows device management instellen](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**V: Waarom is mijn poging voor deelname aan een PC mislukt maar ik heb foutgegevens niet ontvangen?**

**A:** een waarschijnlijke oorzaak is dat de gebruiker is aangemeld bij het apparaat met behulp van het ingebouwde lokale beheerdersaccount. Maak een andere lokale account voordat u Azure Active Directory Join gebruikt om de installatie te voltooien. 

---

**V: waar vind ik de instructies voor het instellen van automatische device Registration service?**

**A:** Zie voor gedetailleerde instructies [automatische registratie van Windows-domein gekoppelde apparaten configureren met Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**V: waar vind ik het oplossen van informatie over de automatische apparaatregistratie?**

**A:** voor informatie over probleemoplossing, Zie:

- [Het oplossen van automatische registratie van domein lid zijn van computers met Azure AD: Windows 10 en Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Het oplossen van automatische registratie van domein lid zijn van computers met Azure AD voor Windows downlevel-clients](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 

---

