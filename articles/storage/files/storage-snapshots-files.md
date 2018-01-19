---
title: Overzicht van momentopnamen van de share voor Azure-bestanden (preview) | Microsoft Docs
description: Een momentopname van de share is een alleen-lezen-versie van een Azure-bestanden-share die wordt uitgevoerd op een punt in tijd, als een manier om back-up van de share.
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: c309804f33fc0e5b2091e18dfe5fe3c9849a2709
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="overview-of-share-snapshots-for-azure-files-preview"></a>Overzicht van momentopnamen van de share voor Azure-bestanden (preview)
Azure Files biedt de mogelijkheid om te delen momentopnamen van bestandsshares. Share-momentopnamen (preview) leggen de status van de share op dat moment. In dit artikel wordt beschreven welke mogelijkheden share momentopnamen bieden en hoe kunt u profiteren van deze in uw aangepaste gebruiksvoorbeeld.


## <a name="when-to-use-share-snapshots"></a>Het gebruik van de share momentopnamen

### <a name="protection-against-application-error-and-data-corruption"></a>Bescherming tegen beschadiging van de toepassing fout en gegevens

Toepassingen die gebruikmaken van bestandsshares uitvoeren bewerkingen zoals schrijven, lezen, opslag, verzending en verwerking. Als een toepassing is onjuist geconfigureerd of als een onbedoelde bug wordt geïntroduceerd, kan per ongeluk overschrijven of schade aan enkele blokken gebeuren. Om te beschermen tegen deze scenario's, kunt u een momentopname van een share maken voordat u nieuwe toepassingscode voor de implementeert. Als er een fout of een toepassing opgetreden met de nieuwe implementatie wordt toegevoegd, kunt u teruggaan naar een eerdere versie van uw gegevens op of de bestandsshare. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Bescherming tegen onbedoeld verwijderen of onbedoelde wijzigingen

Stel dat u op een tekstbestand in een bestandsshare werkt. Nadat het tekstbestand is gesloten, verliest u de mogelijkheid om uw wijzigingen ongedaan te maken. In dergelijke gevallen moet u vervolgens een eerdere versie van het bestand te herstellen. Share momentopnamen kunt u eerdere versies van het bestand te herstellen als deze per ongeluk is hernoemd of verwijderd.

### <a name="general-backup-purposes"></a>Algemene back-updoeleinden

Nadat u een bestandsshare maakt, kunt u regelmatig een momentopname van een share van de bestandsshare te gebruiken voor de back-up maken. Een momentopname, wanneer er regelmatig gemaakt share kunt onderhouden van vorige versies van gegevens die kunnen worden gebruikt voor toekomstige audit vereisten of herstel na noodgevallen.

## <a name="capabilities"></a>Functionaliteit

Een momentopname van een share is een punt in tijd, alleen-lezen kopie van uw gegevens. U kunt maken, verwijderen en momentopnamen beheren met behulp van de REST-API. Dezelfde mogelijkheden zijn ook beschikbaar in de clientbibliotheek, Azure CLI en Azure-portal. 

U kunt momentopnamen van een share weergeven met behulp van de REST-API en de SMB. U kunt de lijst met versies van de map of bestand ophalen en kunt u een specifieke versie rechtstreeks als een station koppelen. 

Nadat een momentopname van een share is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. U kunt een momentopname van de hele share niet kopiëren naar een ander opslagaccount. U moet doen dat door bestand, met behulp van AzCopy of andere wijze kopiëren.

Share Snapshots wordt op niveau van de bestandsshare aangeboden. Ophalen is op afzonderlijke bestandsniveau, om toe te staan voor het herstellen van afzonderlijke bestanden opgegeven. U kunt een volledige bestandsshare via SMB, de REST-API, de portal, de clientbibliotheek of PowerShell/CLI tooling herstellen.

Een momentopname van het delen van een bestandsshare is identiek aan de basis-bestandsshare. Het enige verschil is dat een **DateTime** waarde wordt toegevoegd aan de share URI om aan te geven van de tijd waarop de momentopname van de share. Bijvoorbeeld, als een bestandsshare URI http://storagesample.core.file.windows.net/myshare, de share momentopname URI is vergelijkbaar met:
```
http://storagesample.core.file.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Share momentopnamen bewaard totdat ze expliciet worden verwijderd. Een momentopname van een bestandsshare kan niet de base bestandsshare outlive. U kunt de momentopnamen die zijn gekoppeld aan de basis bestandsshare voor het bijhouden van uw huidige momentopnamen opsommen. 

Wanneer u een share momentopname van een bestandsshare maakt, worden de bestanden in de eigenschappen van de share gekopieerd naar de momentopname delen dezelfde waarden. De bestanden en metagegevens van de bestandsshare ook gekopieerd naar de share momentopname, tenzij u afzonderlijke metagegevens voor de share wanneer u deze maakt een momentopname opgeeft.

U kunt een share met de share momentopnamen bevat, tenzij u eerst verwijdert u alle momentopnamen van de bestandsshare niet verwijderen.


## <a name="space-usage"></a>Gebruik van schijfruimte 

Share momentopnamen zijn incrementele aard. Alleen de gegevens die zijn gewijzigd nadat de momentopname van de meest recente share is opgeslagen. Hierdoor minimaliseert de benodigde tijd voor het maken van de momentopname van de share en wordt opgeslagen op de opslagkosten. Een schrijfbewerking voor het object of eigenschap of metagegevens updatebewerking naar 'gewijzigde inhoud' wordt beschouwd en wordt opgeslagen in de momentopname van de share. 

Om te besparen, kunt u de momentopname van de share voor de periode wanneer het verloop was het hoogst verwijderen.

Hoewel share momentopnamen incrementeel worden opgeslagen, moet u de meest recente share momentopname behouden om de share te herstellen. Wanneer u een momentopname van de bestandsshare verwijdert, wordt alleen de gegevens die uniek is voor het die momentopname share verwijderd. Actieve momentopnamen bevatten alle gegevens die u nodig hebt om te zoeken en herstellen van uw gegevens (vanaf het moment dat de share momentopname werd gemaakt) naar de oorspronkelijke locatie of een alternatieve locatie. U kunt herstellen op itemniveau.

Momentopnamen meetellen niet voor de limiet voor het delen van 5 TB. Er is geen limiet hoeveel ruimte share momentopnamen innemen in totaal. Limieten voor opslagaccounts nog steeds toepassen.

## <a name="limits"></a>Limieten

Het maximum aantal momentopnamen share waarmee Azure Files vandaag is 200. U moet oudere share momentopnamen verwijderen om te maken van nieuwe na 200 share momentopnamen. 

Er is geen limiet voor de gelijktijdige aanroepen voor het maken van momentopnamen van de share. Er is geen limiet voor de hoeveelheid ruimte die share momentopnamen van een bepaalde bestandsshare kunnen gebruiken. 

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Kopiëren van gegevens naar een share vanuit een momentopname van de share

Kopieerbewerkingen die betrekking hebben op bestanden en momentopnamen delen voldoen deze regels:

U kunt afzonderlijke bestanden in een momentopname van een share via kopiëren naar de share base of een andere locatie. U kunt herstellen van een eerdere versie van een bestand of de volledige bestandsshare herstellen door het bestand kopiëren van de momentopname delen. De momentopname van de share is niet worden gepromoveerd naar base share. 

De momentopname van de share blijft intact nadat u hebt gekopieerd, maar de basis van de bestandsshare wordt overschreven met een kopie van de gegevens die beschikbaar in de momentopname van de share is. De herstelde bestanden telling naar 'gewijzigde inhoud."

Kopieer een bestand in een momentopname van een share naar een doel met een andere naam. Het resulterende doelbestand is een beschrijfbare bestands- en niet een momentopname van de share.

Wanneer een doel-bestand wordt overschreven met een kopie, blijven de share momentopnamen die zijn gekoppeld aan het oorspronkelijke doelbestand intact.

## <a name="general-best-practices"></a>Aanbevolen beveiligingsprocedures 

Wanneer u infrastructuur op Azure uitvoert, automatiseren back-ups voor gegevensherstel indien mogelijk. Acties die automatisch zijn betrouwbaarder dan handmatige processen, helpt gegevensbescherming en herstelmogelijkheden te verbeteren. U kunt de REST-API, de Client SDK of scripts voor automatisering.

Voordat u de share momentopname scheduler implementeert, moet u zorgvuldig overwegen uw share de frequentie van momentopnamen en de retentie-instellingen om te vermijden onnodige kosten.

Momentopnamen van de share bieden alleen bestandsniveau beveiliging. Momentopnamen van de share niet voorkomen dat fat vinger verwijderingen op een bestand of share storage-account. Ter bescherming van een opslagaccount van per ongeluk zijn verwijderd, kunt u het opslagaccount of de resourcegroep vergrendelen.

## <a name="next-steps"></a>Volgende stappen
* [Werken met momentopnamen van de share](storage-how-to-use-files-snapshots.md)
* [Veelgestelde vragen over momentopname delen](storage-files-faq.md)

