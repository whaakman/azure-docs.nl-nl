---
title: De verkoper Insights Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over de functie voor verkoper inzichten van de Cloud Partner-Portal.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: ec7fc3a7877cf0bffac0043a74c34d6f0f04826b
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48810051"
---
<a name="seller-insights-faq"></a>De verkoper Insights Veelgestelde vragen
===================

In dit artikel bevat richtlijnen voor algemene gebruikersprocedures binnen en vragen over de verkoper inzichten.


<a name="find-definitions-for-the-values-in-the-downloaded-transaction-file"></a>Definities voor de waarden vinden in de gedownloade transactie-bestanden
------------------------------------------------------------------

De definities van de metrische waarden in de transactiebestand vindt u in het artikel [verkoper Insights definities](./si-insights-definitions-v4.md).


<a name="see-customer-details-of-transactions-for-which-ive-been-paid"></a>Zie de details van de klant van transacties die ik hebt betaald
-------------------------------------------------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **Payout Status**, en past u het filter om alleen de waarde 'Betaald afmelden.' weer te geven De volgende kolommen weergegeven met details over de klant: **bedrijfsnaam**, **klant e**, **klant land**, **klant status**, en **klant postcode**.


<a name="calculate-my-open-accounts-receivable"></a>Mijn open klanten berekenen
-------------------------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **Payout Status**, en past u het filter om alleen de waarde "Toekomstige Payout" en "Niet gereed voor Payout." Som van de kolom met het label vervolgens **Payout bedrag (PC)**.


<a name="calculate-revenue-by-customer-usage-period"></a>Omzet per klant gebruiksperiode berekenen
------------------------------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **transactiestatus**, en de waarde "Betaald" filteren.   Voor elke transactie vermeld, de kolom met het label **Payout bedrag (PC)** vertegenwoordigt het bedrag dat u zijn betaald.  De kolom wordt gebruikt voor een schatting van de gebruiksperiode die zijn gekoppeld aan de transactie **datum van afschrijving**, die een nauwkeurige benadering van de laatste dag van het gebruik van voor de periode dat de transactie is van toepassing is.


<a name="calculate-your-bad-debt"></a>Uw oninbare berekenen
---------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **verzameling de definitieve Status**, en het filter toepassen op alleen de weergave de waarde 'afschrijven." Som van de kolom met het label vervolgens **Payout bedrag (PC)**.


<a name="view-payout-or-customer-contact-information"></a>Payout of klant contactgegevens weergeven
-------------------------------------------

Meld u aan als een gebruiker met de rol 'eigenaar' en niet de rol 'Inzender'. Alleen de rol van eigenaar ziet betalings-en klantgegevens. U vindt meer informatie over gebruikersrollen in het artikel [gebruikers beheren](./cloud-partner-portal-manage-users.md).


<a name="calculate-my-advance-payouts"></a>Mijn betalingen vooraf berekenen
----------------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **transactietype**, en past u het filter om alleen de waarde "Kosten." weer te geven Zoek vervolgens de kolom met het label **verzameling de definitieve Status**, en past u het filter om alleen de waarde 'Wordt uitgevoerd' weer te geven. Ten slotte som van de **Payout bedrag (PC)** kolom voor het berekenen van alle vorderingen aan u betaald voor het verzamelen van de klant.


<a name="calculate-customer-refunds"></a>Klant restituties berekenen
--------------------------

Na het downloaden van uw transacties van de module Payout, zoek de kolom met het label **verzameling de definitieve Status**, en past u het filter om alleen de waarde 'Restitutie.' weer te geven Som de **gratis hoeveelheid (PC)** kolom voor het berekenen van alle restituties verwerkt voor uw klanten.


<a name="identify-which-transactions-involved-a-microsoft-channel-partner"></a>Identificeren welke transacties betrokken een Microsoft-Partner-kanaal
----------------------------------------------------------------

Alle transacties in de kolom **Azure licentietype** die zijn gefilterd om weer te geven van de waarden "Enterprise-via-wederverkoper" en "Cloud Solution Provider" betrekking hebben op een Microsoft-Partner-kanaal. Voor meer informatie over de partner, kunt u vinden hun **Reseller naam** en **Reseller e** in het downloaden van de module betalings- en het downloaden van de klant-module.


<a name="identify-trial-usage-and-trial-conversions"></a>Proefversie gebruik en de proefversie conversies identificeren
------------------------------------------

Downloads van order, gebruik en betalings-module bevat nu **proefversie einddatum** om te begrijpen wanneer de proefperiode is beëindigd voor die specifieke volgorde, indien van toepassing. Als u wilt zien proefversie gebruiks- en orders, zoek de **facturering SKU-Type** kolom in de downloads en past u het filter om alleen de waarde 'Proefperiode.' weer te geven Als u wilt zien proefversie conversies, zoek de **proefversie einddatum** kolom in de downloads en toe te passen het filter om weer te geven alleen de orders wanneer de **proefversie einddatum** valt na de datum van vandaag en **annuleren datum** kolom is leeg of later zijn dan de **proefversie einddatum**.


<a name="when-is-my-monthly-payout-calculated"></a>Wanneer is mijn maandelijkse payout berekend
------------------------------------

Uw betalingen zijn aan u verleend door de 15e van elke maand voor alle bedragen gereed voor payout door de laatste agendadag van de vorige maand. Op de derde dag van de maand, Microsoft berekenen van de vorige maand payout bedrag en bijwerken van alle van toepassing kosten in rekening gebracht transacties in de download met 'Toekomstige Payout' in de **Payout Status** kolom. Deze transacties blijft die status hebben, totdat de aanvraag voor de betaling wordt verzonden naar uw bankrekening op dat moment hun **Payout Status** wordt bijgewerkt naar 'Betaalde Out' en 'Payout datum' wordt bijgewerkt om weer te geven van de datum wordt verzonden de de aanvraag van de betaling voor uw bank.


<a name="calculate-customer-acquisition-and-loss"></a>Klantenwerving en verlies berekenen
---------------------------------------

Ziet u de datum waarop de klant eerst gekocht een van uw aanbiedingen door te zoeken op de **datum verkregen** kolom in het downloaden van de klant. Op dezelfde manier, ziet u de datum waarna had ze niet langer een aanbieding door u zijn gepubliceerd door het zoeken naar de **datum verloren** kolom in het downloaden van de klant.


<a name="finding-more-help"></a>Meer Help
-----------------

- [Definities van de verkoper Insights](./si-insights-definitions-v4.md) -definities voor metrische gegevens en gegevens vinden

- [Aan de slag met de verkoper Insights](./si-getting-started.md) -Inleiding tot de functie voor verkoper inzichten.

