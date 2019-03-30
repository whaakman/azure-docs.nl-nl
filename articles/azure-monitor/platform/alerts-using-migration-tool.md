---
title: Migreren van uw klassieke waarschuwingen in Azure Monitor met behulp van het hulpprogramma voor migratie op vrijwillige basis
description: Informatie over het gebruik van hulpprogramma voor migratie op vrijwillige basis voor het migreren van de klassieke waarschuwingsregels.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 58c664beee942fe7115c7fff38a039c23bed6ac3
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632105"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Gebruik het hulpprogramma voor migratie op vrijwillige basis voor het migreren van de klassieke waarschuwingsregels

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in juli 2019. Het hulpprogramma voor migratie voor het activeren van migratie vrijwillig is beschikbaar in Azure portal en is beschikbaar voor klanten die gebruikmaken van klassieke waarschuwingsregels. In dit artikel begeleidt u stapsgewijs door over het gebruik van het hulpprogramma voor migratie voor het migreren van de klassieke waarschuwingsregels vrijwillig voordat de automatische migratie wordt gestart in juli 2019.

## <a name="benefits-of-new-alerts"></a>Voordelen van nieuwe waarschuwingen

Klassieke waarschuwingen worden vervangen door nieuwe geïntegreerde waarschuwingen in Azure Monitor. Het nieuwe waarschuwingenplatform heeft de volgende voordelen:

- Waarschuwen bij tal van multi-dimensionale metrische gegevens voor [veel meer Azure-services](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)
- Ondersteuning voor de nieuwe metrische gegevens van de waarschuwingen [meerdere resource-waarschuwingsregels](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) de overhead van het beheer van veel regels aanzienlijk verminderen.
- Geïntegreerde meldingsmechanisme
  - [Actiegroepen](action-groups.md) is een modulaire meldingsmechanisme die geschikt is voor alle nieuwe Waarschuwingstypen (metrische gegevens, logboeken en het activiteitenlogboek)
  - Kunt u zich ook profiteren van nieuwe meldingsmechanismen zoals SMS, spraak- en ITSM-Connector
- De [uniforme wijze van werken waarschuwing](alerts-overview.md) brengt alle waarschuwingen op andere signalen (metrische gegevens, activiteit logboek- en logboekbestanden) op één centrale plaats

## <a name="before-you-migrate"></a>Voordat u migreert

Als onderdeel van de migratie, klassieke waarschuwingsregels worden geconverteerd naar de equivalente nieuwe regels voor waarschuwingen en actiegroepen worden gemaakt.

- De indeling van de melding nettolading, evenals de API's maken en beheren van nieuwe waarschuwingsregels wijkt af van die van de klassieke waarschuwingsregels omdat meer functies wordt ondersteund. Informatie over [voorbereiden voor de migratie](alerts-prepare-migration.md).

- Sommige regels voor klassieke waarschuwingen kunnen niet worden gemigreerd met behulp van het hulpprogramma. [Meer informatie over welke regels zijn niet migreerbare en informatie over het migreren van de accounts](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > Migratieproces heeft geen gevolgen voor de evaluatie van de klassieke waarschuwingsregels. Ze blijven uitvoeren en waarschuwingen verzenden totdat ze worden gemigreerd en nieuwe regels voor waarschuwingen evaluatie start.


## <a name="how-to-use-the-migration-tool"></a>Het gebruik van het hulpprogramma voor migratie

De volgende procedure wordt beschreven hoe u de migratie van de klassieke waarschuwingsregels in Azure-portal te activeren:

1. In [Azure-portal](https://portal.azure.com), klikt u op **Monitor**.

2. Klik op **waarschuwingen** klikt u op **Waarschuwingsregels beheren** of **klassieke waarschuwingen weergeven**.

3. Klik op **migreren naar nieuwe regels** om naar de startpagina van de migratie te gaan. Deze pagina bevat een overzicht van al uw abonnementen en de status van de migratie voor hen.

    ![migratie-landingspagina](media/alerts-migration/migration-landing.png "regels migreren")

4. Alle abonnementen die kunnen worden gemigreerd met behulp van het hulpprogramma wordt gemarkeerd als **klaar om te migreren**.

    > [!NOTE]
    > Het Migratiehulpmiddel is beschikbaar in fasen voor alle abonnementen die gebruikmaken van klassieke waarschuwingsregels. In de eerste fasen van de uitrol ziet u mogelijk sommige abonnementen als niet gereed voor migratie.

5. Selecteer een of meer abonnementen en klik op **Preview-migratie**

6. Op deze pagina ziet u de details van klassieke waarschuwingsregels die voor een abonnement op een moment worden gemigreerd. U kunt ook **downloaden van de details van de migratie voor dit abonnement** in een CSV-indeling.

    ![migratie-preview](media/alerts-migration/migration-preview.png "Preview-migratie")

7. Geef een of meer **e-mailadressen** om te worden geïnformeerd over de migratiestatus van de. We sturen een e-mailbericht wanneer de migratie is voltooid of een actie te ondernemen.

8. Klik op **migratie Start**. Lees de informatie die wordt weergegeven in het bevestigingsvenster en controleer of als u klaar bent voor de migratie start.

    >[!IMPORTANT]
    > Nadat u het migratieproces voor een abonnement hebt gestart, kunt u zich niet bewerken/maken-klassieke waarschuwingsregels voor het abonnement. De klassieke waarschuwingsregels blijven echter uitgevoerd en die u waarschuwingen geven totdat ze zijn gemigreerd. Dit is om te controleren of de betrouwbaarheid tussen klassieke waarschuwingsregels en de nieuwe regels die zijn gemaakt tijdens de migratie. Zodra de migratie voltooid voor uw abonnement is, kunt u regels voor klassieke waarschuwingen niet meer gebruiken.

    ![migratie-confirm](media/alerts-migration/migration-confirm.png "bevestigen migratie starten")

9. Als we migratie voltooien of een actie van u nodig hebt, ontvangt u een e-mailbericht op de e-mailadressen die is opgegeven in de stap 8. U kunt ook periodiek de status van de startpagina van de migratie in de portal controleren.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-is-my-subscriptions-listed-as-not-ready-for-migration"></a>**Waarom wordt Mijn abonnementen vermeld als niet gereed voor migratie?**

Het Migratiehulpmiddel is beschikbaar in fasen voor alle klanten. In de eerste fasen, de meeste of alle uw abonnementen kunnen worden gemarkeerd als **niet gereed voor migratie**. Mid-april moeten alle abonnementen zich echter klaar om te migreren.

Wanneer een abonnement gereed voor migratie is, ontvangt de eigenaars van abonnementen een e-mail om de beschikbaarheid van het hulpprogramma te informeren. Houd gaten uit voor deze melding.

### <a name="who-can-trigger-the-migration"></a>**Die de migratie kunt activeren?**

Gebruikers die de rol van inzender voor bewaking aan hen toegewezen op abonnementsniveau zich voor het activeren van de migratie. Meer informatie over [rollen gebaseerd toegangsbeheer voor migratieproces](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-is-the-migration-going-to-take"></a>**Hoe lang de migratie gaat uitvoeren?**

Voor de meeste abonnementen voltooit de migratie meestal minder dan een uur. U kunt bijhouden van de voortgang van de migratie van de startpagina van de migratie.  Gedurende deze tijd voor worden gezorgd dat uw waarschuwingen nog steeds worden uitgevoerd in het systeem klassieke waarschuwingen of het nieuwe certificaat.

### <a name="what-can-i-do-if-i-run-into-an-issue-during-migration"></a>**Wat moet ik doen als ik een probleem tijdens de migratie ondervindt?**

Volg de [problemen oplossen als u wilt zien welke stappen voor problemen die u tijdens de migratie kan tegenkomen met](alerts-understand-migration.md#common-issues-and-remediations). Als geen actie te ondernemen om de migratie te voltooien, wordt u een melding op het e-mailadressen verstrekt tijdens de migratie.

## <a name="next-steps"></a>Volgende stappen

- [Voorbereiden voor migratie](alerts-prepare-migration.md)
- [Informatie over de werking van het hulpprogramma voor migratie](alerts-understand-migration.md)
