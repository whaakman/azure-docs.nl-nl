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
ms.openlocfilehash: a0cfd65aa2444956336e5363d20acab61a404c68
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309175"
---
# <a name="azure-active-directory-device-management-faq"></a>Apparaatbeheer via Azure Active Directory Veelgestelde vragen

**VRAAG: Kan ik het apparaat recent geregistreerd. Waarom zie ik het apparaat niet onder Mijn gebruikersgegevens in Azure portal? Of waarom is eigenaar van het apparaat gemarkeerd als N.V.T. voor hybride Azure AD gekoppelde apparaten?**
 **A:** Windows 10-apparaten die zijn toegevoegd aan hybrid Azure AD, worden niet weergegeven bij de apparaten van de gebruiker.
U moet alle apparaten weergeven in Azure portal gebruiken. U kunt ook PowerShell gebruiken [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Alleen de volgende apparaten worden vermeld in de apparaten van de gebruiker:

- Alle persoonlijke apparaten die niet hybride Azure AD zijn toegevoegd. 
- Alle niet - Windows 10 / Windows Server 2016-apparaten.
- Alle niet-Windows-apparaten 

--- 

**VRAAG: Hoe weet ik wat de status van de apparaatregistratie van de client is?**

**A:** U kunt de Azure-portal gebruiken, gaat u naar alle apparaten en zoeken naar het apparaat met behulp van apparaat-ID. Controleer de waarde onder de kolom van het type join. Soms, kan het apparaat zijn opnieuw ingesteld of teruggezet. Daarom is het essentieel dat ook de status van de apparaatregistratie op het apparaat te controleren:

- Voor Windows 10 en Windows Server 2016 of hoger, voert u dsregcmd.exe status.
- Voor eerdere versies van het besturingssysteem, voert u "%programFiles%\Microsoft werkplek Join\autoworkplace.exe"

---

**VRAAG: Ik zie de record van apparaat onder de gegevens van de gebruiker in Azure portal en ziet de status geregistreerd op het apparaat. Ben ik correct ingesteld voor het gebruik van voorwaardelijke toegang?**

**A:** De apparaatstatus join, weergegeven in de apparaat-id, moet overeenkomen met die in Azure AD en voldoen aan de evaluatiecriteria van een voor voorwaardelijke toegang. Zie voor meer informatie, [vereisen beheerde apparaten voor toegang tot cloud-Apps met voorwaardelijke toegang](../conditional-access/require-managed-devices.md).

---

**VRAAG: Ik heb hebt verwijderd in Azure portal of met behulp van Windows PowerShell, maar de lokale staat op het apparaat nog steeds is geregistreerd, zegt?**

**A:** Dit is standaard. Het apparaat geen toegang tot resources in de cloud. 

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

**VRAAG: Waarom zie ik dubbele apparaatvermeldingen in Azure portal?**

**A:**

-   Voor Windows 10 en Windows Server 2016, als er herhaalde pogingen voor het loskoppelen van en zich weer aanmelden bij hetzelfde apparaat, kunnen er dubbele vermeldingen. 

-   Als u toevoegen werk of School-Account hebt gebruikt, wordt een nieuwe record van apparaat in elke windows-gebruiker die gebruikmaakt van toevoegen werk of School-Account maken met dezelfde naam van het apparaat.

-   Voor versies van het eerdere Windows-besturingssysteem die zich on-premises maakt AD domein met behulp van automatische registratie een nieuwe record van apparaat met dezelfde naam voor elke domeingebruiker die zich bij het apparaat aanmeldt. 

-   Een Azure AD is toegevoegd aan virtuele machine die is gewist, opnieuw geïnstalleerd, en deze met dezelfde naam wordt weergegeven als een andere record met dezelfde naam van het apparaat.

---

**VRAAG: Waarom een gebruiker nog steeds toegang tot bronnen vanaf een apparaat dat ik in Azure portal hebt uitgeschakeld?**

**A:** Het kan een uur duren voor een intrekken moet worden toegepast.

>[!Note] 
>Voor ingeschreven apparaten, wordt u aangeraden wissen van het apparaat om ervoor te zorgen dat gebruikers geen toegang heeft tot de resources. Zie voor meer informatie, [apparaten inschrijven voor beheer in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Veelgestelde vragen over Azure AD Join

**VRAAG: Hoe ik loskoppelen van een Azure AD join-apparaat lokaal op het apparaat?**

**A:** 
- Zorg dat u automatische inschrijving uitschakelen zodat de geplande taak het apparaat niet opnieuw registreren voor hybride Azure AD verbonden apparaten. Open vervolgens de opdrachtprompt als beheerder en typ `dsregcmd.exe /debug /leave`. U kunt ook worden deze opdracht uitgevoerd als een script op meerdere apparaten bulksgewijs loskoppelen.

- Voor pure Azure AD verbonden apparaten, zorg ervoor dat u hebt een offline lokale administrator-account of maak een, kunt u zich niet aanmelden met de referenties van een Azure AD-gebruiker. Ga vervolgens naar **instellingen** > **Accounts** > **toegang tot werk of School**. Selecteer uw account en klik op **verbinding verbreken**. Volg de aanwijzingen en geef de referenties van de lokale beheerder wanneer hierom wordt gevraagd. Start opnieuw op het apparaat om het loskoppelen-proces te voltooien.

---

**VRAAG: Kunnen mijn gebruikers zich aanmelden bij Azure AD gekoppelde apparaten die zijn verwijderd of uitgeschakeld in Azure AD?**
 **A:** Ja. Windows heeft in de cache opgeslagen aanmeldingsmogelijkheid om toe te staan eerder aangemelde gebruikers snel toegang tot bureaublad zelfs zonder netwerkverbinding. Wanneer een apparaat is verwijderd of uitgeschakeld in Azure AD, is het niet bekend bij de Windows-apparaat. Dus eerder zijn geregistreerd gebruikers toegang houden tot het bureaublad met aanmelding in de cache. Echter, als het apparaat wordt verwijderd of uitgeschakeld, gebruikers geen toegang tot alle bronnen worden beveiligd door een apparaat gebaseerde voorwaardelijke toegang. 

Gebruikers die nog niet hebt aangemeld heeft geen toegang tot het apparaat omdat er geen aanmelding in de cache is ingeschakeld voor deze. 

---

**VRAAG: Kunnen uitgeschakelde of verwijderde gebruikers zich aanmelden bij Azure AD gekoppelde apparaten?**
 **A:** Ja, maar alleen voor een beperkte periode. Wanneer een gebruiker is verwijderd of uitgeschakeld in Azure AD, is deze niet direct bekend bij de Windows-apparaat. Dus eerder zijn geregistreerd gebruikers hebben toegang tot het bureaublad met aanmelding in de cache. Zodra het apparaat (doorgaans in minder dan 4 uur) op de hoogte van de status van de gebruiker is, blokkeert Windows die gebruikers toegang krijgen tot het bureaublad. Als de gebruiker wordt verwijderd of uitgeschakeld in Azure AD, worden alle hun tokens, ingetrokken, zodat ze geen toegang alle resources tot. 

Verwijderde of uitgeschakelde gebruikers die eerder in dit nog niet hebt geregistreerd heeft geen toegang tot een apparaat omdat er geen aanmelding in de cache is ingeschakeld voor deze. 

---

**VRAAG: Mijn gebruikers kunnen geen printers uit Azure AD Joined-apparaten zoeken. Hoe kan ik afdrukken vanuit Azure AD Joined-apparaten inschakelen?**

**A:** Voor het implementeren van printers voor Azure AD verbonden apparaten, Zie [hybride cloud print](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). U moet een Windows-Server on-premises implementatie van hybride cloud print. Afdrukservice cloud-gebaseerd is op dit moment niet beschikbaar. 

---

**VRAAG: Hoe maak ik verbinding met een externe Azure Active Directory toegevoegd apparaat?**
 **A:** Raadpleeg het artikel https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc voor meer informatie.

---

**VRAAG: Waarom kunnen mijn gebruikers zien 'U kunt daar niet komen vanaf hier'?**

**A:** Als u bepaalde regels voor voorwaardelijke toegang om te vereisen dat de status van een specifiek apparaat hebt geconfigureerd en het apparaat voldoet niet aan de criteria, wordt gebruikers worden geblokkeerd en dit bericht wordt weergegeven. De beleidsregels voor voorwaardelijke toegang evalueren en zorg ervoor dat het apparaat kunnen voldoen aan de criteria om te voorkomen dat dit bericht is.

---

**VRAAG: Waarom worden enkele van mijn gebruikers doen niet get MFA prompts voor Azure AD gekoppelde apparaten?**

**A:** Als de gebruiker lid wordt van of een apparaat wordt geregistreerd bij Azure AD met behulp van multi-factor Authentication, wordt het apparaat zelf een vertrouwde tweede factor voor deze bepaalde gebruiker. Later, wanneer de gebruiker zich aanmeldt bij het apparaat en toegang verkrijgt een toepassing tot, Azure AD rekening gehouden met het apparaat als een tweede factor en kan die gebruiker naadloos toegang krijgen tot hun toepassingen, zonder extra prompts voor MFA. Dit gedrag is niet van toepassing op een andere gebruiker het apparaat aan te melden, zodat andere gebruikers met toegang tot het apparaat wordt nog steeds gevraagd met een MFA-controle voordat u toegang tot toepassingen waarvoor MFA is vereist.

---

**VRAAG: Waarom krijg ik een bericht 'gebruikersnaam of wachtwoord is onjuist' voor een apparaat dat ik net hebt toegevoegd aan Azure AD?**

**A:** Veelvoorkomende redenen voor dit scenario zijn:

- Uw gebruikersreferenties zijn niet meer geldig.

- Uw computer staat kan niet communiceren met Azure Active Directory. Controleer of eventuele problemen met de netwerkverbinding.

- Federatieve aanmeldingen vereist uw federation-server voor de ondersteuning van WS-Trust-eindpunten en die toegankelijk zijn. 

- U kunt Pass through-verificatie hebt ingeschakeld en de gebruiker heeft een tijdelijk wachtwoord dat moet worden gewijzigd bij de aanmelding.

---

**VRAAG: Waarom zie ik de ' Oeps... is een fout opgetreden! "dialoogvenster wanneer ik probeer Azure AD kan deelnemen aan mijn PC?**

**A:** Dit is een resultaat van het instellen van Azure Active Directory-inschrijving met Intune. Zorg ervoor dat de gebruiker die probeert te doen van Azure AD join juiste Intune-licentie is toegewezen heeft. Zie voor meer informatie, [Windows device management instellen](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**VRAAG: Waarom mijn poging tot het Azure AD join een PC is mislukt maar ik heb foutgegevens niet ontvangen?**

**A:** Een waarschijnlijke oorzaak is dat de gebruiker is aangemeld bij het apparaat met behulp van het ingebouwde lokale beheerdersaccount. Een ander lokaal account maken voordat u Azure Active Directory Join gebruikt om de installatie te voltooien. 

---

## <a name="hybrid-azure-ad-join-faq"></a>Veelgestelde vragen over de hybride Azure AD Join

**VRAAG: Waar vind ik het oplossen van informatie voor het vaststellen van hybride Azure AD join fouten?**

**A:** Zie voor meer informatie:

- [Het oplossen van automatische registratie van domein lid zijn van computers met Azure AD: Windows 10 en Windows Server 2016](troubleshoot-hybrid-join-windows-current.md)

- [Het oplossen van automatische registratie van domein lid zijn van computers met Azure AD voor Windows downlevel-clients](troubleshoot-hybrid-join-windows-legacy.md)
 

---

## <a name="azure-ad-register-faq"></a>Veelgestelde vragen over Azure AD registreren

**VRAAG: Kan ik Android of iOS BYOD-apparaten registreren?**

**A:** Ja, maar alleen bij Azure device registratieservice en voor hybride klanten. Het wordt niet ondersteund met on-premises device registratieservice in AD FS.

**VRAAG: Hoe kan ik een macOS-apparaat registreren?**

**A:** MacOS-apparaat registreren:

1.  [Een nalevingsbeleid maken](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definieer een beleid voor voorwaardelijke toegang voor macOS-apparaten](../active-directory-conditional-access-azure-portal.md) 

**Opmerking:**

- De gebruikers die zijn opgenomen in uw beleid voor voorwaardelijke toegang moeten een [een ondersteunde versie van Office voor Mac OS](../conditional-access/technical-reference.md#client-apps-condition) voor toegang tot resources. 

- Uw gebruikers wordt gevraagd tijdens de eerste poging van de toegang tot het apparaat met behulp van de bedrijfsportal-App te registreren.

---
