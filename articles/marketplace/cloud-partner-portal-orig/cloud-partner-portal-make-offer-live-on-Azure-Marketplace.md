---
title: Uw virtuele machine live-aanbieding op Azure Marketplace
description: Uw virtuele machine live-aanbieding op Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: ad22f1944d3fe9a088b66da4cf4df7136db497f7
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075328"
---
<a name="make-your-virtual-machine-offer-live-on-azure-marketplace"></a>Uw virtuele machine live-aanbieding op Azure Marketplace
=========================================================

Nadat u alle details van de aanbieding hebt ingevuld, is het tijd om uw aanbieding publiceren en geef deze live op Azure Marketplace. Er zijn een aantal fasen doorlopen van de aanbieding. Zorg ervoor dat zowel uw marketinginhoud als uw VM-installatiekopie die voldoen aan de kwaliteitsvereisten, om te worden Azure Certified en live gaan op de website.

![Ga Live sequentie 0 bieden](./media/cloud-partner-portal-offer-go-live-azure-marketplace/makeanofferlive.png)

We gaan door dit proces in meer detail om beter te begrijpen wat er gebeurt tijdens elke stap. In dit proces moet u om te controleren of dat uw aanbieding blijft voortgang fungeren.

<a name="publishing-process"></a>Publicatieproces
------------------

Klik op 'Publiceren' onder het tabblad Editor het publicatieproces te starten.

![Ga Live reeks 1 - aanbieding publiceren](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publish.png)

Op het tabblad Status ziet u de publicatie stappen en waar uw aanbieding in het proces is.

![Bieden van Go Live reeks 2 - status](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status.png)

Op enig moment gedurende het publicatieproces, kunt u ook Meld u aan en klik op het tabblad alle biedt om de meest recente status voor het gebruik van uw aanbiedingen weer te geven. U kunt op rechtstreeks op de status van uw aanbieding en bekijk de details voor waar uw aanbieding in het publicatieproces is.

![Go Live reeks 3 - bieden alle biedt status](./media/cloud-partner-portal-offer-go-live-azure-marketplace/alloffersstatus.png)

Laten we elk van de publicatie stappen doorlopen, bespreken wat er gebeurt bij elke stap en hoe lang u moet een schatting maken dat elke stap duurt.

**Valideren van de vereisten (\<1 dag)**

Als u 'Publiceren' klikt, is een geautomatiseerde controle vindt plaats om te controleren of dat u alle vereiste velden in uw aanbieding hebt ingevuld. Als alle velden zijn niet gevuld, een waarschuwing wordt weergegeven naast het veld en u moet voor het vullen van het nauwkeurig klikt u op 'Publiceren' opnieuw.

Nadat u deze stap correct hebt voltooid, een pop-upvenster wordt u gevraagd een e-mailadres. Dit is het e-mailbericht waaraan u publishing statusmeldingen voor de rest van het publicatieproces ontvangt. Nadat u uw e-mailadres hebt ingediend, wordt deze stap is voltooid.

![Aanbieding Go Live reeks 4: uw aanbieding publiceren](./media/cloud-partner-portal-offer-go-live-azure-marketplace/publishyouroffer.png)

**Certificeringsinstantie (\<5 dagen)**

Deze stap is waar we een aantal tests om te controleren of uw VM-installatiekopie voldoet aan de vereisten voor Azure Certified uitvoeren. Alle richtlijnen die u moet ervoor zorgen geeft u de vereisten voor hardwarecertificering zijn [hier](../cloud-partner-portal/virtual-machine/cpp-prerequisites.md).

Omdat deze stap enkele dagen duren kan, kunt u zich buiten de Cloud Partner-Portal. We sturen u een e-mailmelding als er fouten optreden die u afhandelen moet. Als alles met succes is geslaagd, wordt het proces automatisch verdergaan met de Provisioning-stap.

**Ingericht (\<1 dag)**

In deze fase is we uw installatiekopieën worden gerepliceerd naar alle globale Azure-datacenters en een dag om uit te voeren kan duren.

**Pakketten en leiden generatie registratie (\<1 uur)**

Tijdens deze fase, zijn we de inhoud van de technische en marketingmateriaal combineren in wat de productpagina van op de website zal zijn.

Bovendien, als u de functie leiden generatie hebt geconfigureerd, zullen we dit valideren of uw CRM-integratie werkt door een lead test verzenden naar uw CRM. Hier ziet u een record met de valse gegevens in uw CRM- of Azure Table vullen nadat deze stap voltooid is. Alle documentatie voor het genereren van leiden bevindt [hier](./cloud-partner-portal-get-customer-leads.md).

**De aanbieding is beschikbaar in Preview**

U ontvangt een e-mailmelding die uw aanbieding met succes is voltooid voor de stappen die nodig zijn voor toegang tot de aanbieding in preview. Tijdens deze stap moet u voorbeeld van uw aanbieding en zorg ervoor dat alles ziet er als het zijn moet en waarmee uw virtuele machine correct wordt geïmplementeerd in de faseringsomgeving.

*Alleen in de whitelist opgenomen abonnementen kunnen doen deze verificatie.*

**Aftekening van de uitgever**

Als u controleren of alles er goed uitziet en goed werkt in de Preview-versie, bent u klaar om live te gaan. Klik op **Go Live** onder de **Status** tabblad en we zullen stappen voor het maken van uw aanbieding live in productie en op de website. Meestal duurt het enkele uren vanaf het moment dat u op Go Live en wanneer de aanbieding beschikbaar via de website is. We sturen u een e-mailmelding wanneer uw aanbieding officieel live op de website is.

![Bieden van Go Live reeks 5: live gaan](./media/cloud-partner-portal-offer-go-live-azure-marketplace/golive.png)

**Live**

Uw aanbieding is nu Live op Azure Marketplace en Azure portal en klanten kunnen weergeven en uw virtuele machine implementeren in hun Azure-abonnementen. Op elk gewenst moment kunt u klikt u op het tabblad alle aanbiedingen en de status bekijken voor uw aanbiedingen die zijn vermeld in de rechterkolom. U kunt klikken op de status om te zien van de publicatiestatus van de stroom in de details voor uw aanbieding.

<a name="error-handling"></a>Foutafhandeling
--------------

Tijdens het publicatieproces, kan een fout optreden. Als er een fout is opgetreden, ontvangt u een e-mailmelding waarin wordt gemeld dat er een fout met instructies voor de volgende stappen opgetreden. U ziet ook fouten op elk gewenst moment tijdens dit proces door te klikken op het tabblad Status. U ziet dat punt in het proces dat de fout is opgetreden, samen met een foutbericht geeft een overzicht van wat er moet worden omgezet.

![Bieden van Go Live reeks 6 - foutbericht](./media/cloud-partner-portal-offer-go-live-azure-marketplace/errormessage.png)

Als er fouten tijdens het publicatieproces optreden, bent u vereist voor het oplossen van deze problemen, en klik vervolgens op 'Publiceren' om het proces opnieuw te starten. U moet beginnen aan het begin van de publicatie stappen aan de vereisten valideren bij het opnieuw na elke fout-oplossing te publiceren.

Als u een fout bij het oplossen van problemen, moet u een ondersteuningsaanvraag om hulp te krijgen van onze ondersteuningsmedewerkers openen.

![Bieden van Go Live reeks 7 - ondersteuning krijgen](./media/cloud-partner-portal-offer-go-live-azure-marketplace/getsupport.png)

<a name="canceling-the-publishing-request"></a>Het publicatieverzoek annuleren
--------------------------------

U kunt start het proces van de publicatie en een nodig voor het annuleren van uw aanvraag. Zodra de aanvraag publiceren de stap Publisher Sign-off bereikt, kunt u alleen een aanvraag voor publicatie annuleren. Als u wilt annuleren, klikt u op Annuleren publiceren. De status van de publicatie wordt opnieuw ingesteld naar stap 1, en als u wilt opnieuw publiceert, moet u publiceren op en volg de stappen in de status.

![Bieden van Go Live reeks 8 - status](./media/cloud-partner-portal-offer-go-live-azure-marketplace/status5.png)

