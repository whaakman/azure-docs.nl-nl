---
title: Azure Active Directory-Veelgestelde vragen over rapportage | Microsoft Docs
description: Azure Active Directory-Veelgestelde vragen over rapportage.
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
ms.openlocfilehash: 2ceaf4ecf7da78377c52135b7de5191c1dc3dd28
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232361"
---
# <a name="azure-active-directory-reporting-faq"></a>Azure Active Directory-Veelgestelde vragen over rapportage

In dit artikel bevat antwoorden op veelgestelde vragen over Azure Active Directory (Azure AD) rapportage. Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor meer informatie. 

**V: ik de https://graph.windows.net/&lt; tenant-naam&gt;/reports/ eindpunt API's voor het pull-Azure AD-controle en gebruik van de geïntegreerde toepassing programmatisch in ons systeem reporting-rapporten. Wat moet ik overschakelen naar**

**A:** opzoeken van de [API-naslagdocumentatie](https://developer.microsoft.com/graph/) om te zien hoe u de nieuwe API's kunt gebruiken voor toegang tot [activiteitsrapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal). Dit eindpunt heeft twee rapporten (Audit en aanmeldingen) waarmee alle gegevens die u hebt verkregen in de oude API-eindpunt. Dit nieuwe eindpunt heeft ook een rapport aanmeldingen met de Azure AD Premium-licentie die u gebruiken kunt om op te halen van app-gebruik, gebruik van het apparaat en aanmelden gebruikersgegevens.


--- 

**V: ik de https://graph.windows.net/&lt; tenant-naam&gt;/reports/ eindpunt API's voor het ophalen van Azure AD-beveiligingsrapporten (specifieke typen detecties, zoals gelekte aanmeldingsreferenties of aanmeldingen vanaf anonieme IP-adressen) in ons systeem reporting programmatisch. Wat moet ik overschakelen naar**

**A:** kunt u de [risicogebeurtenissen Identity Protection API](active-directory-identityprotection-graph-getting-started.md) op access security detecties via Microsoft Graph. Deze nieuwe indeling biedt meer flexibiliteit bij het opvragen van gegevens, met de geavanceerde filters, Veldselectie en meer, en risicogebeurtenissen standaardiseert in één type voor gemakkelijker integratie met siem's en andere hulpmiddelen voor het verzamelen van gegevens. Omdat de gegevens in een andere indeling is, kunt u een nieuwe query niet vervangen voor uw oude query's. Echter, [Microsoft Graph maakt gebruik van de nieuwe API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), dit is de standaard van Microsoft voor deze API's als O365 of Azure AD. U begint de overgang naar deze nieuwe standaard platform zodat het werk vereist kunt uitbreiden ofwel de huidige MS Graph-investeringen of help.

--- 

**V: Wat is het bewaren van gegevens om activiteitenlogboeken (Audit en aanmeldingen) in de Azure portal?** 

**A:** Zie [voor hoe lang de verzamelde gegevens worden opgeslagen?](active-directory-reporting-retention.md#q-for-how-long-is-the-collected-data-stored) voor een deze vraag te beantwoorden.

--- 

**V: hoe lang duurt het tot ik de activiteitsgegevens zien kunt wanneer ik mijn taak hebt voltooid?**

**A:** controlelogboeken activiteit een latentie variërend van 15 minuten tot een uur hebben. Aanmelden activiteitenlogboeken kunnen maximaal twee uur voor een aantal records van 15 minuten duren.

---

**V: moet ik een globale beheerder om te zien van de activiteit aanmeldingen bij de Azure portal of om gegevens te verkrijgen via de API?**

**A:** Nee. Moet u een **beveiliging lezer**, een **beveiliging Admin**, of een **globale beheerder** tot ophalen van gegevens in de Azure portal of via de API voor rapportage.

---

**V: kan ik Office 365 activiteit logboekgegevens via de Azure portal krijgen?**

**A:** ondanks dat Office 365-activiteit en Azure AD activiteit logboeken share veel van de directoryresources, als u wilt dat een volledig overzicht van de Office 365-activiteitenlogboeken moet gaat u naar het Office 365-beheercentrum om op te halen van de logboekgegevens van Office 365-activiteit.

---


**V: welke API's kan ik informatie krijgen over Office 365-activiteitenlogboeken gebruiken?**

**A:** de Office 365-beheer-API gebruiken voor toegang tot de [Office 365 activiteit logboeken via een API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).

---

**V: hoe veel records die ik vanuit Azure-portal kunt downloaden?**

**A:** u kunt maximaal 5000 records downloaden van de Azure-portal. De records worden gesorteerd op *meest recente* en standaard krijgt u de meest recente 5000 records. Kies uw filtervoorwaarden voor record grootte van meer dan 5000, en klik vervolgens op de *Script* knop op de Azure-portal. Hiermee wordt een PowerShell-script kan worden uitgevoerd voor het genereren van een CSV-bestand van alle records die overeenkomen met uw filtercriteria gedownload. U moet het PowerShell-script uitvoeren *beheerdersmodus*. Zie voor meer informatie [aanmelden activiteiten downloaden](active-directory-reporting-activity-sign-ins.md#download-sign-in-activities).

---

**V: hoe veel records kan ik opvragen via de API-activiteiten**

**A:** u kunt maximaal 1 miljoen records query (als u niet de operator top, sorteert u de record door de meeste recente). Als u de operator 'top' gebruikt, u kunt een query van 500 kB records. U kunt voorbeeldquery's vinden op het gebruik van de API [hier](active-directory-reporting-api-getting-started.md).

---

**V: hoe krijg ik een premium-licentie**

**A:** Zie [aan de slag met Azure Active Directory Premium](fundamentals/active-directory-get-started-premium.md) voor een deze vraag te beantwoorden.

---

**V: hoe snel moet ik activiteiten gegevens ziet nadat u een premium-licentie.**

**A:** als u al activiteiten gegevens als een beschikbare licentie hebt, wordt u dezelfde gegevens kunt zien. Als u geen gegevens, duurt het een of twee dagen.

---

**V: kan ik gegevens van vorige maand nadat u een Azure AD premium-licentie zien?**

**A:** als u hebt onlangs is overgeschakeld naar een premiumversie (met inbegrip van een evaluatieversie), kunt u zien gegevens van 7 dagen in eerste instantie. Wanneer een tijdje gegevens staan, worden er maximaal 30 dagen.

---

**V: Er is een risicogebeurtenis in Identity Protection maar ik zie niet overeenkomende aanmelden in de alle aanmeldingen. Wordt dit verwacht?**

**A:** Ja, Identity Protection risico's voor alle verificatie-overdrachten beoordeelt of interactieve of niet-interactieve. Alle aanmeldingen alleen rapport toont echter alleen de interactieve aanmeldingen.

---

**V: hoe kan ik het rapport 'Gebruikers gemarkeerd voor risico' in de Azure-portal downloaden?**

**A:** de optie voor het downloaden van *gebruikers die zijn gemarkeerd voor risico* rapport wordt binnenkort toegevoegd.

---

**V: hoe u weten waarom een aanmeldingspagina of een gebruiker is gemarkeerd in de Azure portal riskant?**

**A:** Premium edition klanten voor meer informatie over de onderliggende risicogebeurtenissen door te klikken op de gebruiker in 'Gebruikers gemarkeerd voor risico' of door te klikken op de "riskant aanmeldingen'. Gratis en Basic-editie klanten om te zien welke gebruikers en aanmeldingen zonder de onderliggende risico gebeurtenisinformatie worden opgehaald.

---

**V: hoe zijn de IP-adressen in de aanmeldingen en riskant aanmeldingen rapport berekend?**

**A:** IP-adressen worden uitgegeven zodanig dat er is geen definitieve verbinding tussen een IP-adres en waar de computer met dit adres zich fysiek bevinden. Dit wordt bemoeilijkt door factoren, zoals mobiele providers en VPN-verbindingen uitgeven van IP-adressen uit de centrale pools vaak zeer ver van waar het clientapparaat daadwerkelijk wordt gebruikt. Gezien het bovenstaande, is het IP-adres converteren naar een fysieke locatie is een zo goed mogelijke poging op basis van traceringen, registergegevens, omgekeerde uiterlijk-ups en andere informatie. 

---

**V: wat betekent dat de risicogebeurtenis 'Aanmelden met extra risico gedetecteerd' geven?**

**A:** zodat u een beter inzicht in alle riskant aanmeldingen in uw omgeving, 'aanmelden met extra risico gedetecteerd' fungeert als tijdelijke aanduiding voor aanmeldingen voor detecties die exclusief naar Azure AD Identity Protection abonnees zijn.

---
