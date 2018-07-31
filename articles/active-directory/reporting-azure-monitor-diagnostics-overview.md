---
title: Azure Active Directory-activiteitenlogboeken in Azure Monitor (preview) | Microsoft Docs
description: Overzicht van Azure Active Directory-activiteitenlogboeken in Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 9b594360bc760fa9eec8ab9900c3aadcb10e9db6
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240101"
---
# <a name="azure-active-directory-activity-logs-in-azure-monitor-preview"></a>Azure Active Directory-activiteitenlogboeken in Azure Monitor (preview)

U kunt nu met Azure Monitor uw Azure AD-activiteitenlogboeken doorsturen naar uw eigen opslagaccount of Event Hub. In de openbare preview van Azure Active Directory-logboeken in Azure Monitor kunt u:

* Uw auditlogboeken archiveren voor een Azure-opslagaccount, zodat u de gegevens voor een langere tijd kunt bewaren
* Uw auditlogboeken naar een Azure Event Hub streamen voor analyse met populaire SIEM-hulpprogramma's als Splunk en QRadar
* Uw auditlogboeken integreren met uw eigen aangepaste logboekoplossingen door deze naar een Event Hub te streamen

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Ondersteunde rapporten

Met deze functie kunt u uw audit- en aanmeldingslogboeken doorsturen naar uw Azure-opslagaccount, Event Hub of aangepaste oplossing. 

* **Auditlogboeken**: het [activiteitenrapport voor auditlogboeken](active-directory-reporting-activity-audit-logs.md) biedt u toegang tot de geschiedenis van elke taak die in uw tenant is uitgevoerd.
* **Aanmeldingen**: met het [activiteitenrapport voor aanmeldingen](active-directory-reporting-activity-sign-ins.md) kunt u bepalen wie de taken heeft uitgevoerd die in het auditlogboek zijn gerapporteerd.

> [!NOTE]
> Auditlogboeken en aanmeldingslogboeken met betrekking tot B2C worden momenteel niet ondersteund.
>

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een [licentie](https://azure.microsoft.com/pricing/details/active-directory/) voor Azure AD Free, Basic, Premium 1 of Premium 2 voor toegang tot de Azure AD-logboeken in Azure Portal. 

Afhankelijk van waarnaar u uw auditlogboekgegevens wilt doorsturen, hebt u het volgende mogelijk nodig:

* Een Azure-opslagaccount waarop u *ListKeys*-machtigingen hebt. We raden u aan om een algemeen opslagaccount te gebruiken en geen Blob Storage-account. Raadpleeg de [Azure Storage-prijscalculator](https://azure.microsoft.com/pricing/calculator/?service=storage) voor opslagprijzen. 
* Een Event Hubs-naamruimte om te integreren met oplossingen van derden.

> [!NOTE]
> U moet *Algemeen beheerder* of *Beveiligingsbeheerder* zijn in de Azure AD-tenant om toegang te krijgen tot de Azure AD-activiteitenlogboeken.
>

## <a name="cost-considerations"></a>Kostenoverwegingen

Als u al een Azure AD-licentie hebt, moet u een Azure-abonnement hebben om het opslagaccount en de Event Hub op te zetten. Het Azure-abonnement is gratis, maar u moet betalen om Azure-resources te gebruiken, waaronder het opslagaccount dat u gebruikt om te archiveren en de Event Hub die u gebruikt om te streamen. De hoeveelheid gegevens, en dus de in rekening gebrachte kosten, variëren sterk, afhankelijk van de grootte van de tenant. 

### <a name="storage-size-for-activity-logs"></a>Opslaggrootte voor activiteitenlogboeken

Elke auditlogboekgebeurtenis neemt ongeveer 2 KB aan opslagruimte in beslag. Zo hebt u voor een tenant met 100.000 gebruikers, wat neerkomt op ongeveer 1,5 miljoen gebeurtenissen per dag, ongeveer 3 GB aan gegevensopslag per dag nodig. Aangezien schrijfbewerkingen in batches van ongeveer 5 minuten voorkomen, kunt u om en nabij 9000 schrijfbewerkingen per maand verwachten. De volgende tabel bevat een kostenraming, afhankelijk van de grootte van de tenant, voor een opslagaccount voor algemeen gebruik v2 in US - west met een bewaringstermijn van ten minste één jaar. Gebruik de [Azure Storage-prijscalculator](https://azure.microsoft.com/pricing/details/storage/blobs/) om een meer nauwkeurige inschatting te maken van het gegevensvolume dat u denkt te gebruiken voor uw toepassing. Elke auditlogboekgebeurtenis neemt ongeveer 2 KB aan opslagruimte in beslag. Zo hebt u voor een tenant met 100.000 gebruikers, wat neerkomt op ongeveer 1,5 miljoen gebeurtenissen per dag, ongeveer 3 GB aan gegevensopslag per dag nodig. Aangezien schrijfbewerkingen in batches van ongeveer 5 minuten voorkomen, kunt u om en nabij 9000 schrijfbewerkingen per maand verwachten. De volgende tabel bevat een kostenraming, afhankelijk van de grootte van de tenant, voor een opslagaccount voor algemeen gebruik v2 in US - west met een bewaringstermijn van ten minste één jaar. Gebruik de [Azure Storage-prijscalculator](https://azure.microsoft.com/pricing/details/storage/blobs/) om een meer nauwkeurige inschatting te maken van het gegevensvolume dat u denkt te gebruiken voor uw toepassing. 

| Logboekcategorie | Aantal gebruikers | Aantal gebeurtenissen/dag | Geschat gegevensvolume per maand | Geschatte kosten per maand | Geschatte kosten per jaar |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Controleren | 100.000 | 1,5 miljoen | 90 GB | $ 1,93 | $ 23,12 |
| Controleren | 1000 | 15.000 | 900 MB | $ 0,02 | $ 0,24 |
| Aanmeldingen | 1000 | 34.800 | 4 GB | $ 0,13 | $ 1,56 |
| Aanmeldingen | 100.000 | 15 miljoen | 1,7 TB | $ 35,41 | $ 424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Event Hub-berichten voor activiteitenlogboeken

Gebeurtenissen worden ongeveer vijf minuten lang gebatched en vervolgens als één bericht met alle gebeurtenissen in die vijf minuten verstuurd. Een bericht in de Event Hub heeft een maximale grootte van 256 kB en als de grootte van alle berichten binnen de vijf minuten die grootte overstijgt, worden er meerdere berichten gestuurd. 

Voor bijvoorbeeld een grote tenant van meer dan 100.000 gebruikers zijn er doorgaans ongeveer 18 gebeurtenissen per seconde, wat neerkomt op 5400 gebeurtenissen in 5 minuten. Aangezien auditlogboeken ongeveer 2 KB per gebeurtenis bevatten, komt dit neer op 10,8 MB aan gegevens en daarom op 43 berichten naar de Event Hub in 5 minuten. De volgende tabel bevat een kostenraming van een eenvoudige Event Hub in US - west, afhankelijk van het volume van de gebeurtenisgegevens. Gebruik de [Event Hub-prijscalculator](https://azure.microsoft.com/pricing/details/event-hubs/) om een nauwkeurige inschatting te berekenen voor het gegevensvolume dat u voor uw toepassing verwacht.

| Logboekcategorie | Aantal gebruikers | Aantal gebeurtenissen/seconde | Aantal gebeurtenissen per interval van 5 minuten | Volume per interval | Aantal berichten per interval | Aantal berichten per maand | Geschatte kosten per maand |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Controleren | 100.000 | 18 | 5400 | 10,8 MB | 43 | 371.520 | $ 10,83 |
| Controleren | 1000 | 0.1 | 52 | 104 kB | 1 | 8640 | $ 10,8 |
| Aanmeldingen | 1000 | 178 | 53.400 | 106,8 MB | 418 | 3.611.520 | $ 11,06 |  


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Deze sectie bevat de veelgestelde vragen over en bekende problemen met Azure Active Directory-logboeken in Azure Monitor.

**V: waar moet ik beginnen?** 

**A:** begin met het [Overzicht](reporting-azure-monitor-diagnostics-overview.md) om een beter beeld te krijgen van wat u nodig hebt om deze functie te implementeren. Nadat u bekend bent met de vereisten, kunt u de zelfstudies bekijken om u te helpen om uw logboeken te configureren en door te sturen naar Event Hubs.

---

**V: welke logboeken zijn opgenomen?**

**A:** in deze functie zijn zowel aanmeldings- als auditlogboeken beschikbaar om doorgestuurd te worden. Gebeurtenissen met betrekking tot B2C worden momenteel niet ondersteund. Lees het [Auditlogboekschema](reporting-azure-monitor-diagnostics-audit-log-schema.md) en het [Aanmeldingslogboekschema](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) om uit te vinden welke typen logboeken en welke op functie gebaseerde logboeken momenteel worden ondersteund. 

---

**V: hoe snel na een actie worden de bijbehorende logboeken weergegeven in Event Hubs?**

**A:** de logboeken horen tussen twee en vijf minuten na het uitvoeren van een actie te verschijnen in Event Hubs. Raadpleeg [Wat is Event Hubs?](/azure/event-hubs/event-hubs-what-is-event-hubs.md) voor meer informatie over Event Hubs.

---

**V: hoe snel na een actie worden de bijbehorende logboeken weergegeven in opslagaccounts?**

**A:** voor Azure-opslagaccounts ligt de latentie tussen de 5 en 15 minuten na het uitvoeren van een actie.

---

**V: hoeveel kost het om mijn gegevens op te slaan?**

**A:** de opslagkosten zijn afhankelijk van de grootte van uw logboeken en de door u gekozen retentieperiode. Raadpleeg het [Overzicht](reporting-azure-monitor-diagnostics-overview.md) voor een kostenraming voor tenants, afhankelijk van het aantal logboeken dat wordt gegenereerd.

---

**V: hoeveel kost het om mijn gegevens naar Event Hubs te streamen?**

**A:** de kosten voor het stream zijn afhankelijk van het aantal berichten dat u per minuut ontvangt. Lees het [Overzicht](reporting-azure-monitor-diagnostics-overview.md) voor meer informatie over hoe de kosten worden berekend, evenals een inschatting op basis van het aantal berichten. 

---

**V: welke SIEM-hulpprogramma's worden momenteel ondersteund?** 

**A:** Azure Monitor wordt momenteel ondersteund door Splunk, QRadar en Sumologic. Splunk is echter het enige SIEM-hulpprogramma dat is ondersteund voor Azure Active Directory-logboeken. Raadpleeg [Azure-bewakingsgegevens streamen naar een Event Hub voor gebruik door een extern hulpprogramma](/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs) voor meer informatie over hoe de connectors werken.

---

**V: heb ik toegang tot de gegevens van Event Hub zonder een extern SIEM-hulpprogramma?** 

**A:** ja, u kunt de [Event Hub-API](/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph) gebruiken om de logboeken vanuit uw eigen aangepaste toepassing te bekijken. 

---


## <a name="next-steps"></a>Volgende stappen

* [Activiteitenlogboeken in een opslagaccount archiveren](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Activiteitenlogboeken doorsturen naar Event Hub](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Meer informatie over Azure-logboeken met diagnostische gegevens](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)