---
title: Beheerhandleiding voor de Azure Data Box-portal | Microsoft Docs
description: Beschrijft hoe u de Azure-portal gebruikt om uw Azure Data Box te beheren.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 4a76d59349c37a3dcc120e64f692881b461f58fb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993458"
---
# <a name="use-the-azure-portal-to-administer-your-data-box"></a>De Azure-portal gebruiken om uw Data Box te beheren

In dit artikel worden enkele van de complexe werkstromen en beheertaken beschreven die kunnen worden uitgevoerd voor de Data Box. U kunt de Data Box beheren via de Azure-portal of via de lokale webinterface. 

Dit artikel richt zich op de taken die u kunt uitvoeren met behulp van de Azure-portal. Gebruik de Azure-portal voor het beheren van orders, het beheren van Data Box en het bijhouden van de status van de order totdat deze is voltooid.


## <a name="cancel-an-order"></a>Een order annuleren

Soms moet u een order om een bepaalde reden annuleren nadat u deze hebt geplaatst. U kunt de order alleen annuleren als deze nog niet is verwerkt. Zodra de order is verwerkt en Data Box is voorbereid, kan de order niet meer worden geannuleerd. 

Voer de volgende stappen uit om een order te annuleren.

1.  Ga naar **Overzicht > Annuleren**. 

    ![Order annuleren 1](media/data-box-portal-admin/cancel-order1.png)

2.  Geef een reden op voor het annuleren van de order.  

    ![Order annuleren 2](media/data-box-portal-admin/cancel-order2.png)

3.  Zodra de order is geannuleerd, werkt de portal de status van de order bij naar **Geannuleerd**. 

## <a name="clone-an-order"></a>Een order klonen

Klonen is handig in bepaalde situaties. Een voorbeeld: Een gebruiker heeft Data Box gebruikt om wat gegevens over te dragen. Naarmate er meer gegevens worden gegenereerd, is er nog een Data Box nodig om die gegevens naar Azure over te dragen. In dit geval kan dezelfde order gewoon worden gekloond.

Voer de volgende stappen uit om een order te klonen.

1.  Ga naar **Overzicht > Klonen**. 

    ![Order klonen 1](media/data-box-portal-admin/clone-order1.png)

2.  Alle details van de order blijven hetzelfde. De ordernaam is de oorspronkelijke ordernaam met *-Kloon* eraan toegevoegd. Schakel het selectievakje in om te bevestigen dat u de privacyinformatie hebt gelezen. Klik op **Create**.    

De kloon wordt binnen enkele minuten gemaakt en de portal wordt bijgewerkt met de nieuwe order.


## <a name="delete-order"></a>Order verwijderen

Wanneer de order is voltooid, wilt u deze wellicht verwijderen. De order bevat uw persoonlijke informatie, zoals naam, adres en contactgegevens. Deze persoonlijke informatie wordt verwijderd wanneer de order wordt verwijderd.

U kunt alleen orders verwijderen die zijn voltooid of geannuleerd. Voer de volgende stappen uit om een order te verwijderen.

1. Ga naar **Alle resources**. Zoek uw order.

2. Klik op de order die u wilt verwijderen en ga naar **Overzicht**. Klik in de opdrachtbalk op **Verwijderen**.

    ![Data Box-order verwijderen 1](media/data-box-portal-admin/delete-order1.png)

3. Voer de naam van de order in wanneer u wordt gevraagd de orderverwijdering te bevestigen. Klik op **Verwijderen**.

## <a name="download-shipping-label"></a>Verzendlabel downloaden

Als het E-ink-scherm van uw Data Box niet werkt en het verzendlabel voor retourzending niet weergeeft, kunt u het verzendlabel downloaden. 

Voer de volgende stappen uit om een verzendlabel te downloaden.
1.  Ga naar **Overzicht > Verzendlabel downloaden**. Deze optie is alleen beschikbaar nadat het apparaat is verzonden. 

    ![Verzendlabel downloaden](media/data-box-portal-admin/download-shipping-label.png)

2.  Hiermee downloadt u het volgende verzendlabel voor retourzending. Sla het label op en druk het af. Vouw het label en steek het in de doorzichtige plastic hoes op het apparaat. Zorg dat het label zichtbaar is. Verwijder eventuele stickers van de vorige verzending die op het apparaat zitten.

    ![Voorbeeld van verzendlabel](media/data-box-portal-admin/example-shipping-label.png)

## <a name="edit-shipping-address"></a>Verzendadres bewerken

U moet het verzendadres wellicht bewerken nadat de order is geplaatst. Dit kan alleen als het apparaat nog niet is verzonden. Zodra het apparaat is verzonden, is deze optie niet meer beschikbaar.

Voer de volgende stappen uit om de order te bewerken.

1. Ga naar **Ordergegevens > Verzendadres bewerken**.

    ![Verzendadres bewerken 1](media/data-box-portal-admin/edit-shipping-address1.png)

2. Bewerk en valideer het verzendadres en sla de wijzigingen vervolgens op.

    ![Verzendadres bewerken 2](media/data-box-portal-admin/edit-shipping-address2.png)

## <a name="edit-notification-details"></a>Meldingsdetails bewerken

U moet wellicht wijzigen welke gebruikers de e-mails met de orderstatus ontvangen. Een voorbeeld: Een bepaalde gebruiker moet worden geïnformeerd wanneer het apparaat wordt afgeleverd of opgehaald. Een andere gebruiker moet worden geïnformeerd wanneer het kopiëren van de gegevens is voltooid, zodat hij kan verifiëren dat de gegevens zich in het Azure-opslagaccount bevinden voordat hij ze uit de bron verwijdert. In deze gevallen kunt u de meldingsdetails bewerken.

Voer de volgende stappen uit om meldingsdetails te bewerken.

1. Ga naar **Ordergegevens > Meldingsdetails bewerken**.

    ![Meldingsdetails bewerken 1](media/data-box-portal-admin/edit-notification-details1.png)

2. U kunt de meldingsdetails nu bewerken en de wijzigingen vervolgens opslaan.
 
    ![Meldingsdetails bewerken 2](media/data-box-portal-admin/edit-notification-details2.png)


## <a name="view-order-status"></a>Orderstatus bekijken

Wanneer de apparaatstatus in de portal verandert, wordt u per e-mail op de hoogte gesteld.

|Orderstatus |Beschrijving |
|---------|---------|
|Besteld     | De order is geplaatst. <br>Als het apparaat beschikbaar is, identificeert Microsoft het apparaat dat moet worden verzonden en bereidt Microsoft het apparaat voor. <br> Als het apparaat niet onmiddellijk beschikbaar is, wordt de order verwerkt zodra het apparaat beschikbaar is. Het kan enkele dagen tot een paar maanden duren voordat de order is verwerkt. Als de order niet binnen 90 dagen kan worden voltooid, wordt deze geannuleerd en wordt u op de hoogte gesteld.         |
|Verwerkt     | De order is verwerkt. In overeenstemming met uw order wordt het apparaat in het datacenter voorbereid voor verzending.         |
|Verzonden     | De order is verzonden. Gebruik het volgnummer dat in uw order in de portal wordt weergegeven om de verzending bij te houden.        |
|Afgeleverd     | De verzending is afgeleverd op het adres dat in de order is opgegeven.        |
|Opgehaald     |Uw retourzending is opgehaald en gescand door de vervoerder.         |
|Ontvangen     | Uw apparaat is ontvangen en gescand in het Azure-datacenter. <br> Zodra de verzending is geïnspecteerd, wordt de apparaatupload gestart.      |
|Gegevens kopiëren     | Het kopiëren van gegevens is in voortgang. Houd de kopieervoortgang voor uw order bij in de Azure-portal. <br> Wacht totdat het kopiëren van gegevens is voltooid. |
|Voltooid       |De order is voltooid.<br> Verifieer dat uw gegevens zich in Azure bevinden voordat u de on-premises gegevens van servers verwijdert.         |
|Voltooid met fouten| Het kopiëren van gegevens is voltooid, maar er zijn fouten opgetreden tijdens het kopiëren. <br> Bekijk de logboeken met kopieerbewerkingen via het pad in de Azure-portal.   |
|Geannuleerd            |De order is geannuleerd. <br> U hebt de order zelf geannuleerd, of er is een fout opgetreden waardoor de service de order heeft geannuleerd. Als de order niet binnen 90 dagen kan worden voltooid, wordt deze ook geannuleerd en wordt u op de hoogte gesteld.     |
|Opruimen | De gegevens op het apparaat worden gewist. Het opruimen van het apparaat wordt als voltooid beschouwd zodra het orderlogboekrapport beschikbaar is in de Azure-portal.|



## <a name="next-steps"></a>Volgende stappen

- Leren hoe u [problemen met Data Box oplost](data-box-faq.md).
