---
title: Uw klassieke waarschuwingen in Azure Monitor migreren met behulp van het hulpprogramma voor migratie op vrijwillige basis
description: Informatie over het gebruik van het hulpprogramma voor migratie op vrijwillige basis voor het migreren van de klassieke waarschuwingsregels.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 0c8aa00d069ae54584d8e828dab35c22048f1876
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295550"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Gebruik het hulpprogramma voor migratie op vrijwillige basis voor het migreren van de klassieke waarschuwingsregels

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in September 2019 (oorspronkelijk juli 2019 is). Een hulpprogramma voor migratie is beschikbaar in de Azure-portal voor klanten die gebruik van klassieke waarschuwingsregels en willen voor het activeren van de migratie zelf. In dit artikel wordt uitgelegd hoe u het hulpprogramma voor migratie gebruiken voor het migreren van de klassieke waarschuwingsregels vrijwillig voordat de automatische migratie wordt gestart in September 2019.

> [!NOTE]
> Vertraging in de uitrol van hulpprogramma voor migratie van de vervaldatum voor de migratie van de klassieke waarschuwingen is vanwege [uitgebreid tot en met 31 augustus 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) vanaf de oorspronkelijk aangekondigde datum van 30 juni 2019.

## <a name="benefits-of-new-alerts"></a>Voordelen van nieuwe waarschuwingen

Klassieke waarschuwingen worden vervangen door nieuwe, uniforme waarschuwingen in Azure Monitor. Het nieuwe waarschuwingenplatform heeft de volgende voordelen:

- U kunt waarschuwingen op tal van multidimensionale metrische gegevens voor [veel meer Azure-services](alerts-metric-near-real-time.md#metrics-and-dimensions-supported).
- De nieuwe metrische gegevens van waarschuwingen ondersteuning [meerdere resource-waarschuwingsregels](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) die de overhead van het beheer van veel regels aanzienlijk verminderen.
- De geïntegreerde meldingsmechanisme, die ondersteuning biedt voor:
  - [Actiegroepen](action-groups.md), een modulaire meldingsmechanisme die geschikt is voor alle nieuwe Waarschuwingstypen (metrische gegevens, logboeken en het activiteitenlogboek).
  - Nieuwe meldingsmechanismen zoals SMS, spraak- en ITSM-Connector.
- De [uniforme wijze van werken waarschuwing](alerts-overview.md) voorziet in alle waarschuwingen op andere signalen (metrische gegevens, logboeken en het activiteitenlogboek) in één locatie.

## <a name="before-you-migrate"></a>Voordat u migreert

Het migratieproces klassieke waarschuwingsregels converteert naar nieuwe, gelijkwaardige regels voor waarschuwingen en actiegroepen gemaakt. In voorbereiding, worden op de hoogte van de volgende punten:

- Zowel de indeling van de melding nettolading en de API's maken en beheren van nieuwe regels voor waarschuwingen zijn anders dan die van de klassieke waarschuwingsregels omdat ze ondersteuning meer functies bieden. [Informatie over het voorbereiden van de migratie](alerts-prepare-migration.md).

- Sommige regels voor klassieke waarschuwingen kunnen niet worden gemigreerd met behulp van het hulpprogramma. [Meer informatie over welke regels kunnen niet worden gemigreerd en wat er moet gebeuren met hen](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated).

    > [!NOTE]
    > Het migratieproces heeft geen gevolgen voor de evaluatie van de klassieke waarschuwingsregels. Ze blijven uitvoeren en waarschuwingen verzenden totdat ze zijn gemigreerd en de nieuwe regels voor waarschuwingen van kracht.

## <a name="how-to-use-the-migration-tool"></a>Het gebruik van het hulpprogramma voor migratie

Volg deze stappen voor het activeren van de migratie van de klassieke waarschuwingsregels in Azure portal:

1. In [Azure-portal](https://portal.azure.com), selecteer **Monitor**.

1. Selecteer **waarschuwingen**, en selecteer vervolgens **Waarschuwingsregels beheren** of **klassieke waarschuwingen weergeven**.

1. Selecteer **migreren naar nieuwe regels** om naar de startpagina van de migratie te gaan. Deze pagina bevat een overzicht van al uw abonnementen en hun migratiestatus:

    ![migratie-landingspagina](media/alerts-migration/migration-landing.png "regels migreren")

    Alle abonnementen die kunnen worden gemigreerd met behulp van het hulpprogramma zijn gemarkeerd als **klaar om te migreren**.

    > [!NOTE]
    > Het Migratiehulpmiddel is beschikbaar in fasen voor alle abonnementen die gebruikmaken van klassieke waarschuwingsregels. In de eerste fasen van de implementatie ziet u mogelijk sommige abonnementen die zijn gemarkeerd als niet gereed voor migratie.

1. Selecteer een of meer abonnementen, en selecteer vervolgens **migratie Preview**.

    De resulterende pagina worden de details van klassieke waarschuwingsregels die voor een abonnement op een moment worden gemigreerd. U kunt ook selecteren **downloaden van de details van de migratie voor dit abonnement** om de details in een CSV-indeling.

    ![migratie-preview](media/alerts-migration/migration-preview.png "Preview-migratie")

1. Geef een of meer e-mailadressen om te worden geïnformeerd over de migratiestatus van de. U ontvangt e-mailbericht wanneer de migratie voltooid is of als u geen actie te ondernemen.

1. Selecteer **migratie Start**. Lees de informatie die wordt weergegeven in het bevestigingsdialoogvenster en Bevestig dat u bent klaar om te beginnen het migratieproces.

    > [!IMPORTANT]
    > Nadat u de migratie voor een abonnement hebt gestart, kunt u zich niet bewerken of maken van regels voor klassieke waarschuwingen voor dat abonnement. Deze beperking zorgt ervoor dat er geen wijzigingen aangebracht in de klassieke waarschuwingsregels verloren tijdens de migratie naar de nieuwe regels zijn. Hoewel het niet mogelijk om te wijzigen van de klassieke waarschuwingsregels, blijven deze gewoon nog steeds om uit te voeren en om te geven van waarschuwingen totdat ze zijn gemigreerd. U geen klassieke waarschuwingsregels nadat de migratie voltooid voor uw abonnement is, niet meer gebruiken.

    ![migratie-confirm](media/alerts-migration/migration-confirm.png "bevestigen migratie starten")

1. Wanneer de migratie is voltooid, of als u niets, ontvangt u een e-mailbericht op de adressen die u eerder hebt opgegeven. U kunt ook periodiek de status op de startpagina van de migratie in de portal controleren.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Waarom wordt mijn abonnement vermeld als niet gereed voor migratie?

Het Migratiehulpmiddel is beschikbaar voor klanten in fasen. In de eerste fasen, meeste of alle van uw abonnementen kan worden gemarkeerd als **niet gereed voor migratie**. 

Wanneer een abonnement gereed voor migratie is, moet u eigenaar van het abonnement ontvangt een e-mailbericht dat aangeeft dat het hulpprogramma beschikbaar is. Houd gaten af voor dit bericht.

### <a name="who-can-trigger-the-migration"></a>Die de migratie kunt activeren?

Gebruikers die de rol van inzender voor bewaking aan hen toegewezen op het abonnementsniveau kunnen voor het activeren van de migratie. [Meer informatie over toegangsbeheer op basis van rollen voor het migratieproces](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>Hoe lang duurt de migratie?

Migratie is voltooid voor de meeste abonnementen in minder dan een uur. U kunt bijhouden van de voortgang van de migratie op de startpagina van de migratie. Tijdens de migratie, worden er gerust op zijn dat uw waarschuwingen nog steeds worden uitgevoerd in het systeem klassieke waarschuwingen of in het nieuwe certificaat.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Wat moet ik doen als ik een probleem tijdens de migratie ondervindt?

Zie de [problemen oplossen met](alerts-understand-migration.md#common-problems-and-remedies) voor hulp bij problemen die u tijdens de migratie kan tegenkomen. Als u geen actie te ondernemen om de migratie te voltooien, krijgt u een bericht op de e-mailadressen die u hebt opgegeven bij het instellen van het hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

- [Voorbereiden voor de migratie](alerts-prepare-migration.md)
- [Informatie over de werking van het hulpprogramma voor migratie](alerts-understand-migration.md)
