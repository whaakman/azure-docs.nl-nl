---
title: Azure Stack-logboek- en verwerking van gegevens | Microsoft Docs
description: Meer informatie over hoe Azure Stack wordt informatie verzameld.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2019
ms.author: PatAltimore
ms.reviewer: chengwei
ms.lastreviewed: 02/14/2019
ms.openlocfilehash: 7beb9dc31e8987d69c2cf9cf64be36caf0e0e308
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56318923"
---
# <a name="azure-stack-log-and-customer-data-handling"></a>Azure Stack logboek- en verwerking van gegevens 
*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*  

Voor zover is Microsoft een processor- of subprocessor van persoonlijke gegevens in verband met Azure Stack, Microsoft maakt voor alle klanten, effectieve 25 mei 2018 worden de toezeggingen (a) in de '-verwerking van persoonlijke gegevens. Avg"inrichten van de sectie 'Voorwaarden voor gegevensbescherming' van de [voorwaarden voor onlineservices](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551078818&sdata=cpWsfZTBHpqEFr50DWQOryq342U8shgeFgMXVPQz5ug%3D&reserved=0) en (b) in de Europese Unie algemene verordening voorwaarden voor gegevensbescherming in bijlage 4 van de [voorwaarden voor onlineservices](https://nam06.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.microsoftvolumelicensing.com%2FDocumentSearch.aspx%3FMode%3D3%26DocumentTypeId%3D31&data=02%7C01%7Ccomartin%40microsoft.com%7Ce2ce478261764c79c3f308d68df01136%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636852459551088813&sdata=bv1CBiaCnYmjiv6S0dFCbWEd4fNCkPBjBwgylNa%2FNt0%3D&reserved=0). 

Als Azure Stack bevindt zich in hun eigen Datacenter, Microsoft is de Gegevensbeheerder van de gegevens die worden gedeeld met Microsoft via uitsluitend [Diagnostics](azure-stack-diagnostics.md), [telemetrie](azure-stack-telemetry.md), en [Facturering](azure-stack-usage-reporting.md).  

## <a name="data-access-controls"></a>Besturingselementen voor toegang van gegevens 
Microsoft-medewerkers die zijn toegewezen aan een specifieke ondersteuningsaanvraag onderzoeken, krijgt alleen-lezen toegang tot de versleutelde gegevens. Microsoft-medewerkers hebben ook toegang tot hulpprogramma's die kunnen worden gebruikt voor het verwijderen van de gegevens indien nodig. Alle toegang tot de gegevens van de klant wordt gecontroleerd en vastgelegd.  

Data access-besturingselementen:
1.  Gegevens worden alleen bewaard voor een maximum van negentig dagen na het sluiten van de aanvraag.
2.  De klant heeft altijd de keuze om de gegevens op elk gewenst moment in die periode van 90 dagen wordt verwijderd.
3.  Microsoft-werknemers toegang krijgen tot de gegevens op een per geval beoordeeld en alleen indien nodig om u te helpen bij het oplossen van de ondersteuning. 
4.  In de gebeurtenis is waarbij Microsoft gegevens van de klant moet delen met OEM-partners, klanten toestemming verplicht.  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>Welke gegevens onderwerp aanvragen (DSR) besturingselementen komen klanten hebben?
Zoals eerder vermeld, ondersteunt Microsoft het verwijderen van de gegevens op aanvraag per klantaanvraag. Klanten kunnen aanvragen dat onze ondersteuningstechnicus van hun logboeken voor een bepaalde aanvraag verwijderen op elk gewenst moment van de klant te kiezen, voordat de gegevens permanent worden gewist.  

### <a name="does-microsoft-notify-customers-when-the-data-is-deleted"></a>Microsoft klanten melden wanneer de gegevens worden verwijderd?
Voor de actie geautomatiseerde verwijdering (negentig dagen na het sluiten van de aanvraag), we geen proactief contact opnemen met klanten en ze hierover te informeren over de verwijdering. 

Voor de actie op aanvraag gegevens verwijderen heeft ondersteuningstechnicus van Microsoft toegang tot het hulpprogramma waar ze kunnen beginnen met het verwijderen van gegevens op aanvraag en ze kunnen bevestiging geven op de telefoon met de klant wanneer dit voltooid.

## <a name="diagnostic-data"></a>Diagnostische gegevens
Als onderdeel van het ondersteuningsproces, Azure Stack-Operators kunt [diagnostische logboeken delen](azure-stack-diagnostics.md) met Azure Stack-ondersteuning en -engineering-teams om te kunnen oplossen.

Microsoft voorziet in een hulpprogramma en scripts voor klanten om te verzamelen en uploaden aangevraagd diagnostische logboekbestanden. Zodra de verzameld, de logboekbestanden worden overgedragen via een HTTPS beveiligd versleutelde verbinding met Microsoft. Omdat de versleuteling HTTPS via de kabel biedt, is er geen wachtwoord nodig voor de versleuteling tijdens overdracht. Nadat deze zijn ontvangen, worden logboeken versleuteld en opgeslagen totdat ze worden automatisch verwijderd negentig dagen na de ondersteuningskwestie is afgesloten.

## <a name="telemetry-data"></a>Telemetriegegevens
[Azure Stack-telemetrie](azure-stack-telemetry.md) uploadt systeemgegevens automatisch naar Microsoft via de gebruikerservaring die zijn verbonden. Azure Stack-Operators hebben besturingselementen voor het aanpassen van de telemetriefuncties- en privacy-instellingen op elk gewenst moment.

Microsoft niet van plan bent voor het verzamelen van gevoelige gegevens, zoals creditcardnummers, gebruikersnamen en wachtwoorden, e-mailadressen of vergelijkbare gevoelige informatie. Als we ontdekken dat gevoelige informatie per ongeluk is ontvangen, wordt het verwijderen. 

## <a name="billing-data"></a>Factureringsgegevens
[Facturering van Azure Stack](azure-stack-usage-reporting.md) maakt gebruik van de facturering en het gebruik van de globale Azure pijplijn en kan daarom in overeenstemming met richtlijnen voor de naleving van Microsoft.

Azure Stack-Operators kunt configureren voor Azure Stack voor het doorsturen van gebruiksgegevens naar Azure voor facturering. Dit is vereist voor meerdere knooppunten Azure Stack-klanten die betalen als u-gebruik factureringsmodel. Rapportage over het gebruik van de telemetriegegevens onafhankelijk van elkaar wordt beheerd en is niet vereist zijn voor klanten met meerdere knooppunten die het capaciteitsmodel of voor gebruikers van Azure Stack Development Kit. Voor deze scenario's, rapportage over het gebruik kan worden uitgeschakeld met behulp van [het script voor de registratie](azure-stack-usage-reporting.md).


## <a name="next-steps"></a>Volgende stappen 
[Meer informatie over Azure Stack-beveiliging](azure-stack-security-foundations.md) 
