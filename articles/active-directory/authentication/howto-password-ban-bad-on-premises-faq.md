---
title: On-premises bescherming van Azure AD-wachtwoord Veelgestelde vragen
description: On-premises bescherming van Azure AD-wachtwoord Veelgestelde vragen
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b29e4f6e6e877a3d03ded9edd9e282e0960e177
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414626"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Preview: Azure AD wachtwoordbeveiliging on-premises - Veelgestelde vragen over

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Algemene vragen

**V: Wanneer wordt Azure AD-wachtwoord beveiliging algemene beschikbaarheid (GA) bereikt?**

Algemene beschikbaarheid is gepland voor Q1 CY2019 (vóór het einde van maart 2019). Hartelijk dank aan iedereen die is opgegeven feedback over de functie op datum - we stellen het op prijs!

**V: Welke richtlijnen moeten gebruikers worden gegeven voor het selecteren van een veilig wachtwoord?**

De huidige richtlijnen van Microsoft over dit onderwerp kunt u vinden op de volgende koppeling:

[Wachtwoord voor Microsoft-richtlijnen](https://www.microsoft.com/en-us/research/publication/password-guidance)

**V: Kan on-premises bescherming van Azure AD-wachtwoord in niet-openbare clouds worden ondersteund?**

Nee, on-premises Azure AD-wachtwoord beveiliging wordt alleen ondersteund in de openbare cloud. Geen datum is aangekondigd voor beschikbaarheid van niet-openbare cloud.

**V: Hoe kan ik voordelen van Azure AD-wachtwoord beveiliging toepassen op een subset van mijn on-premises gebruikers?**

Wordt niet ondersteund. Zodra de geïmplementeerd en ingeschakeld, beveiliging van Azure AD-wachtwoord geen onderscheid - ontvangen gelijk beveiligingsvoordelen van alle gebruikers.

**V: Is dit dan ondersteund voor het installeren van Azure AD-wachtwoord bescherming naast andere producten op basis van wachtwoord-filter?**

Ja. Ondersteuning voor meerdere geregistreerde wachtwoord filter-dll's een functie van de Windows core is en niet specifiek voor Azure AD-wachtwoord beveiliging. Alle geregistreerde wachtwoord filter dll-bestanden moet accepteren voordat een wachtwoord wordt geaccepteerd.

**V: Hoe kan ik implementeren en Azure AD-wachtwoord beveiliging configureren in de Active Directory-omgeving zonder het gebruik van Azure?**

Wordt niet ondersteund. Azure AD-wachtwoord beveiliging is een functie van Azure die ondersteuning biedt voor wordt uitgebreid naar een on-premises Active Directory-omgeving.

**V: Hoe kan ik de inhoud van het beleid op het niveau van de Active Directory wijzigen?**

Wordt niet ondersteund. Het beleid kan alleen worden beheerd met behulp van de Azure AD-beheerportal. Zie ook de vorige vraag.

**V: Waarom is de DFS-replicatie voor sysvol-replicatie vereist?**

FRS (de voorafgaande-technologie DFSR) heeft tal van bekende problemen en wordt volledig ondersteund in nieuwere versies van Windows Server Active Directory. Nul testen van de beveiliging van Azure AD-wachtwoord wordt uitgevoerd op domeinen FRS geconfigureerd.

Zie de volgende artikelen voor meer informatie:

[De aanvraag voor sysvol-replicatie migreren naar DFS-replicatie](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Het einde is Nigh voor FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**V: Hoeveel schijfruimte vereist de functie op de sysvol-share van het domein?**

Het gebruik van de precieze schijfruimte kan niet correct worden opgegeven omdat deze is afhankelijk van factoren zoals het aantal en de lengte van de tokens uitgesloten wachtwoorden in de lijst met uitgesloten wachtwoorden globale en de aangepaste per tenant lijst met wachtwoorden, plus de overhead van versleuteling verboden. De inhoud van deze lijsten zijn waarschijnlijk in de toekomst vergroten. Met die in rekening met is een redelijke huidige vuistregel dat de functie ten minste vijf (5) MB aan ruimte op de sysvol-share van het domein moet.

**V: Waarom is opnieuw opstarten vereist om te installeren of upgraden van de DC-agentsoftware?**

Deze vereiste wordt veroorzaakt door core Windows gedrag.

**V: Is er manier om een DC-agent voor het gebruik van een bepaalde proxyserver te configureren?**

Nee. Houd er rekening mee dat omdat de proxyserver stateless, is het niet belangrijk welke specifieke proxy-server wordt gebruikt.

**V: Is het goed dat het implementeren van de Azure AD-wachtwoord beveiliging Proxy service side-by-side op een computer met andere services zoals Azure AD Connect?**

Ja. De Azure AD-wachtwoord beveiliging Proxy-service en Azure AD Connect moet nooit rechtstreeks met elkaar conflicteren.

**V: Ik moet over de prestaties op de domeincontrollers van het implementeren van deze functie is bereikt?**

De Azure AD-wachtwoord DC Protection Agent-service is ontworpen om te worden zo efficiënt mogelijke en moet niet significant van invloed op prestaties van domeincontrollers in een bestaande Active Directory-implementatie die al voldoende resources wordt voorzien.

Dit is handig om te weten dat de wijzigingsbewerkingen voor voor de meeste implementaties van Active Directory-wachtwoord een klein deel van de algemene workload op een bepaalde domeincontroller zijn. Stel bijvoorbeeld dat Active Directory-domein met 10000 gebruikersaccounts en een MaxPasswordAge beleid ingesteld op 30 dagen. Gemiddeld, ziet dit domein 10000/30 = ~ 333 wachtwoordswijzigingen elke dag een klein aantal bewerkingen voor zelfs één domeincontroller is. Het voorbeeld in op een mogelijke ergste geval, pushen, stel deze ~-333 wachtwoordwijzigingen op een enkele DC via één uur zijn uitgevoerd (bijvoorbeeld: in dit scenario treedt op wanneer een groot aantal van alle werknemers worden geleverd om te werken op een maandagochtend); zelfs in dit geval we proberen nog steeds ~333/60 minuten = zes wachtwoordwijzigingen per minuut, zodat het is niet een aanzienlijke belasting.

Basis hiervan dient als uw huidige domeincontrollers zijn al wordt uitgevoerd op het niveau van prestaties beperkt (bijvoorbeeld maximum is overschreden met betrekking tot CPU, schijfruimte, schijf-i/o, enzovoort), is het een goed idee om extra domeincontrollers implementeren en/of beschikbare schijf uitbreiden ruimte vóór de implementatie van Azure AD-wachtwoord beveiliging. Zie ook de vraag hierboven over sysvol schijfruimtegebruik hierboven.

**V: Ik wil Azure AD-wachtwoord beveiliging op een paar DC's in Mijn domein testen. Is het mogelijk om af te dwingen de wachtwoordwijzigingen gebruiker deze specifieke DC's gebruiken?**

Nee. Wanneer een gebruiker het wachtwoord wijzigt, wordt de domeincontroller die wordt gebruikt door het besturingssysteem van de Windows-client op basis van factoren, zoals Active Directory-site en elk subnet toewijzingen, omgevingsspecifieke netwerkconfiguratie, enzovoort geselecteerd. Beveiliging van Azure AD-wachtwoord niet van invloed zijn op of beheren van deze factoren en daarom kan niet van invloed zijn op welke domeincontroller voor een bepaalde gebruiker wachtwoordwijziging is geselecteerd.

Basis hiervan dient is een benadering die wordt geleverd dicht bij het simuleren van dit doel Azure AD-wachtwoord beveiliging op alle domeincontrollers in een bepaalde Active Directory-site implementeren. Deze benadering biedt redelijkerwijs alomtegenwoordige dekking voor de Windows-clients die zijn toegewezen aan die site, en dus ook voor de gebruikers die zijn aangemeld bij deze clients en hun wachtwoorden wijzigen.

**V: Als ik de Azure AD-wachtwoord DC Protection Agent-service op alleen de primaire domeincontroller (PDC) installeert, worden alle andere domeincontrollers in het domein ook worden beveiligd?**

Nee. Als het wachtwoord van een gebruiker wordt gewijzigd op een domeincontroller voor bepaalde niet-primaire domeincontroller, wordt het wachtwoord niet-versleutelde tekst nooit naar de primaire domeincontroller (dit idee is een algemene profieltoewijzingen perceptie) verzonden. In plaats daarvan wanneer een nieuw wachtwoord wordt geaccepteerd op een domeincontroller die is opgegeven, die DC dat wachtwoord gebruikt voor het maken van de verschillende verificatie-protocol-specifieke-hashes van wachtwoord en klikt u vervolgens blijft bestaan die hashes in de map. De bijgewerkt hashes worden vervolgens gerepliceerd naar de PDC - maar Protection voor Azure AD-wachtwoord is niet betrokken bij dit proces. Wachtwoorden van gebruikers kunnen in sommige gevallen rechtstreeks op de PDC en wordt opnieuw, afhankelijk van verschillende factoren, zoals netwerktopologie en ontwerp voor Active Directory-site worden gewijzigd. (Zie de vorige vraag.)

Om samen te vatten: implementatie van de service Azure AD wachtwoord-DC-beveiligingsagent op de PDC tot 100% dekking voor de beveiliging van de functie is vereist, maar dat op zichzelf biedt geen beveiliging voor Azure AD-wachtwoord de voordelen van beveiliging voor alle andere domeincontrollers in het domein.

**V: Een managementpack van System Center Operations Manager (SCOM) is beschikbaar voor Azure AD-wachtwoord beveiliging?**

Nee.

## <a name="additional-content"></a>Aanvullende inhoud

De volgende koppelingen kunnen maken geen deel uit van de belangrijkste documentatie voor Azure AD-wachtwoord Protection maar een handige bron van meer informatie over de functie.

[E-Phishing Protection handleiding – deel 15: Implementeren van de beveiligingsservice voor Microsoft Azure AD-wachtwoord (voor On-Premises te!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD-wachtwoordbeveiliging en Smart Lockout bent nu in openbare Preview.](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="next-steps"></a>Volgende stappen

Als u een on-premises bescherming van Azure AD-wachtwoord-vraag die hier niet wordt beantwoord hebt, dienen een feedbackitem hieronder - Hartelijk dank!

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)
