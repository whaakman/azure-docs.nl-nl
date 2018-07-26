---
title: Azure Active Directory Reporting FAQ | Microsoft Docs
description: Azure Active Directory reporting FAQ.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d069d0e74c1bc10baa4d14cdb91c137203495ae2
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247405"
---
# <a name="azure-active-directory-reporting-faq"></a>Azure Active Directory reporting FAQ

In dit artikel bevat antwoorden op veelgestelde vragen over Azure Active Directory (Azure AD) rapportage. Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor meer informatie. 

## <a name="getting-started"></a>Aan de slag 

**V: ik ben met behulp van de https://graph.windows.net/&lt; tenant-naam&gt;/reports/ eindpunt API's voor het pull-Azure AD-controle en gebruik van de geïntegreerde toepassing via een programma in ons systeem reporting-rapporten. Wat moet ik overschakelen naar?**

**A:** opzoeken de [API-referentiedocumentatie](https://developer.microsoft.com/graph/) om te zien hoe u de nieuwe API's kunt gebruiken voor toegang tot [activiteitenrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Dit eindpunt heeft twee rapporten (Audit en -aanmeldingen) waardoor alle gegevens die u hebt verkregen in het oude API-eindpunt. Dit nieuwe eindpunt heeft ook een rapport-aanmeldingen met de Azure AD Premium-licentie die u gebruiken kunt om app-gebruik, gebruik van het apparaat en aanmelding bij gebruikersgegevens te verkrijgen.

--- 

**V: ik ben met behulp van de https://graph.windows.net/&lt; tenant-naam&gt;/reports/ eindpunt API's voor het ophalen van Azure AD-beveiligingsrapporten (specifieke typen detectie, zoals de referenties zijn gelekt of aanmeldingen vanaf anonieme IP-adressen) in onze reporting systemen via een programma. Wat moet ik overschakelen naar?**

**A:** kunt u de [risicogebeurtenissen Identity Protection API](active-directory-identityprotection-graph-getting-started.md) naar beveiligingsdetecties toegang via Microsoft Graph. Deze nieuwe indeling biedt meer flexibiliteit in hoe u van gegevens, met Geavanceerd filteren, mapselectie en meer opvragen kunt, en risicogebeurtenissen standaardiseert in één type voor eenvoudiger integratie met siem's en andere hulpmiddelen voor het verzamelen van gegevens. Omdat de gegevens zich in een andere indeling, kunt u een nieuwe query voor uw oude query's niet vervangen. Echter, [de nieuwe API maakt gebruik van Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), dit is de Microsoft-standaard voor deze API's als Office 365 of Azure AD. U begint de overgang naar deze nieuwe standard-platform, zodat het werk vereist kunt uitbreiden van uw huidige investeringen in MS Graph of de help.

--- 

**V: hoe krijg ik een premium-licentie?**

**A:** Zie [aan de slag met Azure Active Directory Premium](fundamentals/active-directory-get-started-premium.md) voor een antwoord op deze vraag.

---

**V: hoe snel moet ik activiteiten gegevens zien nadat u een premium-licentie?**

**A:** als u al gegevens hebt die activiteiten als een gratis licentie, dan kunt u dezelfde gegevens zien. Als u geen gegevens, duurt het een of twee dagen.

---

**V: kan ik zien van afgelopen maand gegevens nadat u een Azure AD premium-licentie?**

**A:** als u hebt onlangs is overgeschakeld naar een premiumversie (met inbegrip van een evaluatieversie), u kunt gegevens van 7 dagen in eerste instantie zien. Wanneer gegevens worden bij elkaar opgeteld, ziet u tot maximaal 30 dagen.

---

**V: ik moet een globale beheerder zijn om te zien van de activiteit aanmeldingen bij de Azure portal of het opvragen van gegevens via de API?**

**A:** Nee. U moet een **Beveiligingslezer**, een **beveiligingsbeheerder**, of een **globale beheerder** om aan te melden ophalen van gegevens in Azure portal of via de API.

---


## <a name="activity-logs"></a>Activiteitenlogboeken


**V: Wat is de bewaarperiode voor activiteitenlogboeken (Audit en -aanmeldingen) in Azure portal?** 

**A:** Zie [voor hoe lang is de verzamelde gegevens opgeslagen?](active-directory-reporting-retention.md#q-for-how-long-is-the-collected-data-stored) voor een antwoord op deze vraag.

--- 

**V: hoe lang duurt het tot ik de activiteitsgegevens zien kan nadat ik mijn taak hebt voltooid?**

**A:** auditlogboeken van de activiteit hebben een latentie, variërend van 15 minuten tot een uur. Aanmelden activiteitenlogboeken kunnen tot maximaal twee uur voor sommige records tussen 15 minuten duren.

---


**V: kan ik Office 365-activiteitenlogboekinformatie via Azure portal krijgen?**

**A:** Hoewel Office 365-activiteit en Azure AD-activiteit logboeken delen veel directoryresources, als u wilt dat een volledig overzicht van de Office 365-activiteitenlogboeken moet gaat u naar het Office 365-beheercentrum om op te halen van Office 365-activiteitenlogboekinformatie.

---


**Vraag: welke API's gebruik ik voor informatie over Office 365-activiteitenlogboeken?**

**A:** de Office 365 Management API's gebruiken voor toegang tot de [Office 365 activiteitenlogboeken via een API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**V: hoe veel records ik kunt downloaden vanuit Azure portal?**

**A:** u kunt maximaal 5000 records downloaden via de Azure-portal. De records worden gesorteerd op *meest recente* en standaard, krijgt u de meest recente 5000 records.

---

**V: hoe veel records kan ik opvragen via de API-activiteiten**

**A:** , kunt u tot wel 1 miljoen records opvragen (als u niet de top-operator, waarmee de record door de meeste recente). Als u de operator 'top' gebruikt, u kunt een query van 500K-records. U kunt voorbeeldquery's vinden over het gebruik van de API [hier](active-directory-reporting-api-getting-started.md).

---

## <a name="risky-sign-ins"></a>Riskante aanmeldingen

**V: Er is een risicogebeurtenis in Identity Protection, maar ik zie niet bijbehorende aanmelden in de alle aanmeldingen. Is dit verwacht?**

**A:** Ja, Identity Protection risico's voor alle verificatiestromen beoordeelt of interactieve of niet-interactieve. Alleen rapport voor alle aanmeldingen wordt echter alleen de interactieve aanmeldingen.

---

**V: hoe kan ik het rapport 'Gebruikers gemarkeerd voor risico's ' in Azure portal downloaden?**

**A:** de optie voor het downloaden *gebruikers die zijn gemarkeerd voor risico's* rapport wordt binnenkort toegevoegd.

---

**V: hoe weet ik waarom een aanmelding bij of een gebruiker is gemarkeerd riskante in Azure portal?**

**A:** Premium edition klanten meer informatie over de onderliggende risicogebeurtenissen door te klikken op de gebruiker in 'Gebruikers gemarkeerd voor risico's ' of door te klikken op de 'riskante aanmeldingen'. Gratis en Basic edities klanten krijgen om te zien welke gebruikers en aanmeldingen zonder de onderliggende gegevens van risicogebeurtenissen.

---

**V: hoe worden de IP-adressen in de aanmeldingen en het rapport riskante aanmeldingen berekend?**

**A:** IP-adressen worden uitgegeven zodanig dat er is geen definitieve verbinding tussen een IP-adres en waar de computer met dit adres zich fysiek bevindt. Dit wordt bemoeilijkt door factoren zoals mobiele providers en VPN's uitgeven van IP-adressen uit de centrale pools vaak zeer ver ligt waar het client-apparaat daadwerkelijk wordt gebruikt. Gezien het bovenstaande, is het IP-adres converteren naar een fysieke locatie is een best-effort op basis van traceringen, register, omgekeerde zoekopdrachten en andere gegevens. 

---

**V: wat de risicogebeurtenis overheen "Aanmelden met extra risico gedetecteerd" biedt?**

**A:** zodat u een beter inzicht in alle riskante aanmeldingen in uw omgeving, "aanmelden met de extra risico gedetecteerd" fungeert als tijdelijke aanduiding voor aanmeldingen voor detecties die uitsluitend tot de Azure AD Identity Protection-abonnees behoren.

---

## <a name="conditional-access"></a>Voorwaardelijke toegang

**V: Wat is er nieuw in deze functie?**

**A:** klanten kunnen nu beleidsregels voor voorwaardelijke toegang via alle aanmeldingen rapport oplossen. Klanten kunnen de status voor voorwaardelijke toegang en informatie over de details van het beleid toegepast op de aanmelding en het resultaat voor elk beleid te lezen.

**V: hoe ga ik aan de slag?**

**A:** aan de slag:
    * Navigeer naar het rapport aanmeldingen in de [Azure-portal](https://portal.azure.com). 
    * Klik op de aanmelding die u wilt oplossen.
    * Navigeer naar de **voorwaardelijke toegang** tabblad. Hier vindt u alle beleidsregels die invloed hebben de aanmelding en het resultaat voor elk beleid. 
    
**V: wat zijn alle mogelijke waarden voor de status voor voorwaardelijke toegang?**

**A:** status voor voorwaardelijke toegang kan de volgende waarden hebben:
    * **Niet toegepast**: dit betekent dat er geen CA-beleid met de gebruiker en de app binnen het bereik is. 
    * **Succes**: dit betekent dat er een CA-beleid met de gebruiker en de app binnen het bereik is en CA-beleid is voldaan. 
    * **Fout**: dit betekent dat er een CA-beleid met de gebruiker en de app binnen het bereik is en CA-beleid niet wordt voldaan. 
    
**V: wat zijn alle mogelijke waarden voor het resultaat van de beleid voor voorwaardelijke toegang?**

**A:** beleid voor voorwaardelijke toegang kan hebben de volgende resultaten:
    * **Succes**: Er is met succes is voldaan aan het beleid.
    * **Fout**: het beleid is niet voldaan aan.
    * **Niet toegepast**: dit zijn omdat het niet voldeed aan de voorwaarden van het beleid.
    * **Niet ingeschakeld**: dit wordt veroorzaakt door het beleid is uitgeschakeld. 
    
**V: de beleidsnaam in het rapport voor alle aanmelden komt niet overeen met de naam van het beleid in de CA. Waarom is dat?**

**A:** de naam van het beleid in het rapport voor alle aanmelden is gebaseerd op de naam van de CA-beleid op het moment van de aanmelding. Dit kan niet consistent met de naam van het beleid in de CA zijn, als u de beleidsnaam later, dat wil zeggen, na de aanmelding bijgewerkt.