---
title: Apparaatbeheer via Azure Active Directory Veelgestelde vragen over | Microsoft Docs
description: Azure Active Directory Apparaatbeheer Veelgestelde vragen over.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: markvi
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9517ccac4006edec473e25c5e6524ce62d4e1259
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210099"
---
# <a name="azure-active-directory-device-management-faq"></a>Apparaatbeheer via Azure Active Directory Veelgestelde vragen

**V: Kan ik het apparaat recent geregistreerd. Waarom zie ik het apparaat niet onder Mijn gebruikersgegevens in Azure portal? Of waarom is de eigenaar van het apparaat gemarkeerd als N.V.T. voor hybride Azure Active Directory (Azure AD) apparaten gekoppelde?**

**A:** Windows 10-apparaten die zijn toegevoegd aan hybrid Azure AD niet worden weergegeven onder **Gebruikersapparaten**.
Gebruik de **alle apparaten** weergeven in Azure portal. U kunt ook een PowerShell [Get-MsolDevice](https://docs.microsoft.com/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Alleen de volgende apparaten worden vermeld in **Gebruikersapparaten**:

- Alle persoonlijke apparaten die niet van hybride Azure AD is gekoppeld. 
- Alle Windows 10 of Windows Server 2016-apparaten.
- Alle niet-Windows-apparaten. 

--- 

**V: Hoe weet ik wat de status van de apparaatregistratie van de client is?**

**A:** In de Azure-portal, gaat u naar **alle apparaten**. Zoeken naar het apparaat met behulp van de apparaat-ID. Controleer de waarde onder de kolom van het type join. Soms kan het apparaat pas opnieuw instellen of teruggezet. Het is daarom essentieel om te controleren ook de status van de apparaatregistratie op het apparaat:

- Voor Windows 10 en Windows Server 2016 of hoger, voert u `dsregcmd.exe /status`.
- Voor eerdere versies van het besturingssysteem, voert u `%programFiles%\Microsoft Workplace Join\autoworkplace.exe`.

---

**V: Ik zie de record van apparaat onder de gegevens van de gebruiker in Azure portal. En ik Zie de status geregistreerd op het apparaat. Ben ik correct ingesteld voor het gebruik van voorwaardelijke toegang?**

**A:** De apparaatstatus join, wordt weergegeven door **deviceID**, moet overeenkomen met de status in Azure AD en voldoen aan de evaluatiecriteria van een voor voorwaardelijke toegang. Zie voor meer informatie, [vereisen beheerde apparaten voor toegang tot cloud-Apps met voorwaardelijke toegang](../conditional-access/require-managed-devices.md).

---

**V: Kan ik mijn apparaat in Azure portal of met behulp van Windows PowerShell is verwijderd. Maar er volgens de status van de lokale op het apparaat moet dat deze nog steeds geregistreerd.**

**A:** Met deze bewerking is standaard. Het apparaat geen toegang heeft tot resources in de cloud. 

Als u wilt opnieuw wilt registreren, moet u een handmatige actie ondernemen op het apparaat. 

Hiermee schakelt u de status van Windows 10 en Windows Server 2016 die on-premises Active Directory-domein is gekoppeld aan de werkplek, moet u de volgende stappen uitvoeren:

1.  Open de opdrachtprompt als beheerder.

2.  Voer `dsregcmd.exe /debug /leave` in.

3.  Meld u af en meld u aan voor het activeren van de geplande taak die het apparaat opnieuw met Azure AD wordt geregistreerd. 

Voor versies van het eerdere Windows-besturingssysteem die zich on-premises Active Directory-domein is toegevoegd, de volgende stappen uit:

1.  Open de opdrachtprompt als beheerder.
2.  Voer `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` in.
3.  Voer `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` in.

---

**V: Waarom zie ik dubbele apparaatvermeldingen in Azure portal?**

**A:**

-   Voor Windows 10 en Windows Server 2016, herhaalde pogingen en loskoppelen van het apparaat weer kunnen leiden tot dubbele vermeldingen. 

-   Elke Windows-gebruiker die gebruikmaakt van **toevoegen werk of School-Account** maakt een nieuwe apparaatrecord met dezelfde naam van het apparaat.

-   Automatische registratie wordt een nieuwe record van apparaat voor downlevel Windows OS-versies die on-premises Azure Directory-domein is gekoppeld, gemaakt met dezelfde naam voor elke domeingebruiker die zich aanmeldt bij het apparaat. 

-   Een Azure AD is toegevoegd aan virtuele machine die is gewist, opnieuw geïnstalleerd en deze met dezelfde naam wordt weergegeven als een andere record met dezelfde naam van het apparaat.

---

**V: Waarom een gebruiker nog steeds toegang tot bronnen vanaf een apparaat dat ik uitgeschakeld in de Azure-portal?**

**A:** Het duurt maximaal een uur een intrekken moet worden toegepast.

>[!NOTE] 
>Voor ingeschreven apparaten, wordt u aangeraden dat u het apparaat om ervoor te zorgen dat gebruikers geen toegang tot de resources te wissen. Zie voor meer informatie, [wat is apparaatinschrijving?](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Azure AD join Veelgestelde vragen

**V: Hoe ik loskoppelen van een Azure AD gekoppelde apparaat lokaal op het apparaat?**

**A:** 
- Hybrid Azure AD zorg gekoppelde apparaten, er voor automatische registratie uitschakelen. De geplande taak registreert niet u het apparaat opnieuw. Vervolgens open een opdrachtprompt als beheerder en voer `dsregcmd.exe /debug /leave`. Of voer deze opdracht uit als een script op verschillende apparaten bulksgewijs loskoppelen.

- Pure Azure AD zorg gekoppelde apparaten, er voor u een offline lokale administrator-account hebt of maakt u er een. U kunt zich niet aanmelden met de referenties van een Azure AD-gebruiker. Ga vervolgens naar **instellingen** > **Accounts** > **toegang tot werk of School**. Selecteer uw account en selecteer **verbinding verbreken**. Volg de aanwijzingen en geef de referenties van de lokale beheerder wanneer hierom wordt gevraagd. Start opnieuw op het apparaat om het proces loskoppelen.

---

**V: Kunnen mijn gebruikers zich aanmelden bij Azure AD gekoppelde apparaten die zijn verwijderd of uitgeschakeld in Azure AD?**

**A:** Ja. Windows heeft in de cache gebruikersnaam en wachtwoord mogelijkheid waardoor gebruikers die u eerder hebt aangemeld bij de toegang tot het bureaublad snel zelfs zonder netwerkverbinding. 

Wanneer een apparaat is verwijderd of uitgeschakeld in Azure AD, het niet bekend bij de Windows-apparaat. Gebruikers die eerder aangemeld blijven dus toegang tot het bureaublad met de cache gebruikersnaam en wachtwoord. Maar als het apparaat wordt verwijderd of uitgeschakeld, gebruikers geen toegang tot alle bronnen worden beveiligd door een apparaat gebaseerde voorwaardelijke toegang. 

Gebruikers die zich niet in eerder heeft geen toegang tot het apparaat. Er is geen in de cache gebruikersnaam en wachtwoord voor deze ingeschakeld. 

---

**V: Kunnen uitgeschakelde of verwijderde gebruikers zich aanmelden bij Azure AD gekoppelde apparaten?**

**A:** Ja, maar alleen voor een beperkte periode. Wanneer een gebruiker is verwijderd of uitgeschakeld in Azure AD, het niet direct naar het Windows-apparaat bekend. Gebruikers die eerder is aangemeld hebben dus toegang tot het bureaublad met de gebruikersnaam in de cache en het wachtwoord. 

Het apparaat is normaal gesproken op de hoogte van de status van de gebruiker in minder dan vier uur. Windows blokkeert vervolgens die gebruikers toegang tot het bureaublad. Als de gebruiker wordt verwijderd of uitgeschakeld in Azure AD, worden alle hun tokens worden ingetrokken. Ze geen dus toegang tot bronnen. 

Verwijderd of uitgeschakelde gebruikers die zich niet in eerder heeft geen toegang tot een apparaat. Er is geen in de cache gebruikersnaam en wachtwoord voor deze ingeschakeld. 

---

**V: Waarom mijn gebruikers hebben problemen in Azure AD gekoppelde apparaten na het wijzigen van de UPN?**

**A:** UPN-wijzigingen worden op dit moment niet volledig ondersteund in Azure AD gekoppelde apparaten. Zo mislukt de verificatie met Azure AD nadat hun UPN is gewijzigd. Als gevolg hiervan hebben gebruikers eenmalige aanmelding en problemen met voorwaardelijke toegang op hun apparaten. Op dit moment moeten gebruikers zich aanmeldt bij Windows via de tegel 'Andere gebruiker' is de nieuwe UPN gebruiken om dit probleem te verhelpen. Er zijn momenteel gewerkt aan het aanpakken van dit probleem. Gebruikers aanmelden met Windows Hello voor bedrijven kunnen echter niet maken met dit probleem. 

---

**V: Mijn gebruikers kunnen geen printers uit Azure AD gekoppelde apparaten zoeken. Hoe kan ik afdrukken vanaf deze apparaten inschakelen?**

**A:** Printers implementeren voor Azure AD gekoppelde apparaten, Zie [Windows Server hybride Cloud Print is implementeren met verificatie vooraf](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). U moet een Windows-Server on-premises implementatie van hybride cloud print. Afdrukservice cloud-gebaseerd is op dit moment niet beschikbaar. 

---

**V: Hoe maak ik verbinding met een externe Azure Active Directory toegevoegd apparaat?**

**A:** Zie [verbinding maken met de externe Azure Active Directory-apparaat moet een PC](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc).

---

**V: Waarom kunnen mijn gebruikers zien *u daar niet komen vanaf hier*?**

**A:** U bepaalde regels voor voorwaardelijke toegang om te vereisen dat de status van een specifiek apparaat configureren? Als het apparaat niet voldoet aan de criteria, gebruikers worden geblokkeerd en zien ze dit bericht. De beleidsregels voor voorwaardelijke toegang evalueren. Zorg ervoor dat het apparaat voldoet aan de criteria om te voorkomen dat het bericht.

---

**V: Waarom niet sommige van mijn gebruikers Azure multi-factor Authentication prompts opvragen in Azure AD gekoppelde apparaten?**

**A:** Een gebruiker kan deelnemen aan of een apparaat registreren bij Azure AD met behulp van multi-factor Authentication. Vervolgens wordt het apparaat zelf een vertrouwde tweede factor voor die gebruiker. Wanneer de gebruiker zich aanmeldt bij het apparaat en toegang verkrijgt een toepassing tot, beschouwd Azure AD het apparaat als een tweede factor. Hiermee kunt naadloos toegang krijgen tot toepassingen zonder extra prompts van multi-factor Authentication voor die gebruiker. 

Dit gedrag is niet van toepassing op een andere gebruiker die zich aanmeldt op dat apparaat. Dus alle andere gebruikers die toegang hebben tot dat apparaat krijgen een multi-Factor Authentication-uitdaging. Ze hebben vervolgens toegang tot toepassingen die meervoudige verificatie vereisen.

---

**V: Waarom krijg ik een *gebruikersnaam of wachtwoord is onjuist* bericht voor een apparaat dat ik net toegevoegd aan Azure AD?**

**A:** Veelvoorkomende redenen voor dit scenario zijn als volgt:

- Uw gebruikersreferenties zijn niet meer geldig.

- Uw computer kan niet communiceren met Azure Active Directory. Controleer of eventuele problemen met de netwerkverbinding.

- Federatieve-aanmeldingen vereisen uw federation-server voor de ondersteuning van WS-Trust-eindpunten die en die toegankelijk zijn zijn. 

- U pass-through-verificatie hebt ingeschakeld. Het tijdelijke wachtwoord moet zo worden gewijzigd wanneer u zich aanmeldt.

---

**V: Waarom zie ik de *Oeps... is een fout opgetreden!* dialoogvenster wanneer ik Azure AD wil deelnemen aan mijn PC?**

**A:** Deze fout treedt op bij het instellen van Azure Active Directory-inschrijving met Intune. Zorg ervoor dat de gebruiker die te koppelen aan Azure AD probeert de juiste Intune-licentie toegewezen heeft. Zie voor meer informatie, [instellen van inschrijving voor Windows-apparaten](https://docs.microsoft.com/intune/windows-enroll).  

---

**V: Waarom mijn poging tot het Azure AD join een PC is mislukt, maar ik heb foutgegevens niet ontvangen?**

**A:** Een waarschijnlijke oorzaak is dat u aangemeld bij het apparaat met behulp van het ingebouwde lokale beheerdersaccount. Een ander lokaal account maken voordat u Azure Active Directory join gebruiken om de installatie te voltooien. 

---

**V: wat zijn de certificaten MS-organisatie-P2P-toegang aanwezig op onze Windows 10-apparaten?**

**A:** De MS-organisatie-P2P-toegang zijn uitgegeven door Azure AD op beide Azure AD join en hybride Azure AD gekoppelde apparaten. Deze certificaten worden gebruikt om de vertrouwensrelatie tussen apparaten in dezelfde tenant voor scenario's voor extern bureaublad inschakelen. Een certificaat wordt uitgegeven aan het apparaat en een andere wordt verleend aan de gebruiker. Het apparaatcertificaat is aanwezig in `Local Computer\Personal\Certificates` en is geldig gedurende één dag. Dit certificaat wordt vernieuwd (door uitgifte van een nieuw certificaat) als het apparaat nog steeds actief zijn in Azure AD is. Het gebruikerscertificaat is aanwezig in `Current User\Personal\Certificates` en dit certificaat is één dag ook geldig, maar het op aanvraag is uitgegeven wanneer een gebruiker een extern-bureaubladsessie naar een andere Azure AD gekoppelde apparaat probeert. Het is niet vernieuwd na het verstrijken. Beide deze certificaten zijn verleend met behulp van het certificaat MS-organisatie-P2P-toegang aanwezig zijn in de `Local Computer\AAD Token Issuer\Certificates`. Dit certificaat is uitgegeven door Azure AD tijdens het registreren van apparaten. 

---

**Q:Why kan ik meerdere verlopen certificaten uitgegeven door MS-organisatie-P2P-toegang op onze Windows 10-apparaten zien? Hoe verwijder ik deze?**

**A:** Er is een probleem dat is geïdentificeerd op Windows 10 versie 1709 en lagere waar verlopen certificaten van MS-organisatie-P2P-toegang blijven bestaan in het computerarchief vanwege problemen met cryptografische. Uw gebruikers kunnen ervaart mogelijk problemen met de netwerkverbinding, als u een VPN-clients (bijvoorbeeld Cisco AnyConnect) die het grote aantal verlopen certificaten kunnen worden verwerkt. Dit probleem is opgelost in Windows 10-1803 release dergelijke verlopen MS-organisatie-P2P-toegang certificaten automatisch worden verwijderd. U kunt dit probleem oplossen door uw apparaten bij te werken naar Windows 10-1803. Als u zich niet bij te werken, kunt u deze certificaten zonder eventuele nadelige gevolgen te verwijderen.  

---


## <a name="hybrid-azure-ad-join-faq"></a>Hybride Azure AD join Veelgestelde vragen

**V: Waar vind ik het oplossen van gegevens op te sporen hybride Azure AD join fouten?**

**A:** Voor informatie over probleemoplossing, Zie de volgende artikelen:

- [Het oplossen van hybride Azure Active Directory gekoppelde Windows 10 en Windows Server 2016-apparaten](troubleshoot-hybrid-join-windows-current.md)

- [Het oplossen van hybride Azure Active Directory gekoppelde downlevel-apparaten](troubleshoot-hybrid-join-windows-legacy.md)
 
**V: Waarom zie ik een dubbele Azure AD ingeschreven record voor mijn Windows 10-hybride Azure AD toegevoegde apparaat in de lijst met Azure AD-apparaten?**

**A:** Wanneer uw gebruikers hun accounts aan apps op een apparaat domein toevoegt, wordt de gebruiker kunnen worden gevraagd met **account toevoegen aan Windows?** Als ze een **Ja** bij de prompt, het apparaat wordt geregistreerd bij Azure AD. Het type vertrouwensrelatie is gemarkeerd als geregistreerd bij Azure AD. Nadat u hybride Azure AD join in uw organisatie hebt ingeschakeld, wordt het apparaat ook toegevoegd aan hybrid Azure AD. Klik vervolgens weergegeven twee apparaatstatussen voor hetzelfde apparaat. 

Hybride Azure AD join heeft voorrang op de status geregistreerd bij Azure AD. Dus uw apparaat wordt beschouwd als hybrid Azure AD join voor elke verificatie en de evaluatie voor voorwaardelijke toegang. U kunt de record van het apparaat geregistreerd bij Azure AD veilig verwijderen van de Azure AD-portal. Meer informatie over het [voorkomen of opschonen van deze dubbele status op de computer met Windows 10](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan#review-things-you-should-know). 


---

**V: Waarom mijn gebruikers hebben problemen op Windows 10 hybride Azure AD gekoppelde apparaten na het wijzigen van de UPN?**

**A:** UPN-wijzigingen worden momenteel niet volledig ondersteund met hybride Azure AD gekoppelde apparaten. Terwijl gebruikers kunnen zich aanmelden bij het apparaat en toegang krijgen hun on-premises toepassingen tot, mislukt de verificatie met Azure AD nadat een UPN die is gewijzigd. Als gevolg hiervan hebben gebruikers eenmalige aanmelding en problemen met voorwaardelijke toegang op hun apparaten. Op dit moment dat u wilt loskoppelen van het apparaat uit Azure AD ('dsregcmd /leave' uitvoeren met verhoogde bevoegdheden) en opnieuw aanmelden (automatisch gebeurt) om het probleem te verhelpen. Er zijn momenteel gewerkt aan het aanpakken van dit probleem. Gebruikers aanmelden met Windows Hello voor bedrijven kunnen echter niet maken met dit probleem. 

---

**V: Moeten Windows 10 hybride Azure AD gekoppelde apparaten verbinding met de domeincontroller om toegang tot cloudbronnen te krijgen?**

**A:** Nee. Na het Windows 10 hybride Azure AD join is voltooid, en de gebruiker heeft ten minste één keer aangemeld, het apparaat geen verbinding met de domeincontroller voor toegang tot cloud-bronnen vereist. Windows 10 kunt krijgen eenmalige aanmelding bij Azure AD-toepassingen vanaf elke locatie met een internetverbinding, behalve wanneer een wachtwoord is gewijzigd. Als een wachtwoord is gewijzigd buiten het bedrijfsnetwerk bevinden (bijvoorbeeld met behulp van Azure AD SSPR), moet de gebruiker heeft verbinding met de domeincontroller voordat ze kunnen aanmelden op het apparaat met het nieuwe wachtwoord. Anders, ze kunnen zich alleen aanmelden met hun oude wachtwoord, die ongeldig is gemaakt door Azure AD en wordt voorkomen dat eenmalige aanmelding op. Dit probleem niet wordt echter optreden wanneer u Windows Hello voor bedrijven gebruiken. Gebruikers die Meld u aan met Windows Hello voor bedrijven gaan om op te halen één zich aanmelden bij Azure AD-toepassingen nadat een wachtwoord hebt gewijzigd, zelfs als ze geen verbinding met de domeincontroller. 

---


## <a name="azure-ad-register-faq"></a>Veelgestelde vragen over het Azure AD registreren

**V: Kan ik Android of iOS BYOD-apparaten registreren?**

**A:** Ja, maar alleen met de Azure device registratieservice en voor hybride klanten. Het wordt niet ondersteund met de on-premises device registratieservice in Active Directory Federation Services (AD FS).

**V: Hoe kan ik een macOS-apparaat registreren?**

**A:** Voer de volgende stappen uit:

1.  [Een nalevingsbeleid maken](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definieer een beleid voor voorwaardelijke toegang voor macOS-apparaten](../active-directory-conditional-access-azure-portal.md) 

**Opmerking:**

- De gebruikers die zijn opgenomen in uw beleid voor voorwaardelijke toegang moeten een [een ondersteunde versie van Office voor Mac OS](../conditional-access/technical-reference.md#client-apps-condition) voor toegang tot resources. 

- Tijdens de eerste toegang, wordt gebruikers gevraagd het apparaat te registreren met behulp van de bedrijfsportal-App.

