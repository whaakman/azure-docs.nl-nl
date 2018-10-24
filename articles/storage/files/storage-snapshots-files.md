---
title: Overzicht van share-momentopnamen voor Azure Files | Microsoft Docs
description: Momentopname van een share is een alleen-lezen versie van een Azure-bestandsshare die moet worden uitgevoerd op een punt in tijd, als een manier om back-up van de share.
services: storage
author: RenaShahMSFT
ms.service: storage
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.component: files
ms.openlocfilehash: 7f03af0fc338299da9b989d46e7bbfb83f3babeb
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945702"
---
# <a name="overview-of-share-snapshots-for-azure-files"></a>Overzicht van share-momentopnamen voor Azure Files 
Azure Files biedt de mogelijkheid voor share-momentopnamen van bestandsshares. Momentopnamen vastleggen van de status van de share op dat punt in tijd delen. In dit artikel wordt beschreven welke mogelijkheden bieden voor momentopnamen van shares en hoe u kunt profiteren van deze in uw aangepaste situatie.

## <a name="when-to-use-share-snapshots"></a>Wanneer u momentopnamen van shares

### <a name="protection-against-application-error-and-data-corruption"></a>Bescherming tegen beschadiging van fout- en gegevens van toepassing
Toepassingen die gebruikmaken van bestandsshares uitvoeren bewerkingen zoals schrijven, lezen, opslag, overdracht en verwerking. Als een toepassing is onjuist geconfigureerd of als een onbedoelde bug wordt geïntroduceerd, kan per ongeluk overschrijven of schade plaatsvinden bij enkele blokken. Ter bescherming tegen deze scenario's, kunt u een share-momentopname uitvoeren voordat u nieuwe code van de toepassing implementeert. Als er een fout bug of toepassing wordt toegevoegd met de nieuwe implementatie, kunt u teruggaan naar een eerdere versie van uw gegevens op deze bestandsshare. 

### <a name="protection-against-accidental-deletions-or-unintended-changes"></a>Bescherming tegen onbedoelde verwijderingen of onbedoelde wijzigingen
Stel dat u in een tekstbestand in een bestandsshare werkt. Nadat het tekstbestand is gesloten, verliest u de mogelijkheid om uw wijzigingen ongedaan te maken. In dergelijke gevallen moet u vervolgens een vorige versie van het bestand herstellen. U kunt momentopnamen van shares gebruiken om te herstellen van eerdere versies van het bestand als deze per ongeluk is gewijzigd of verwijderd.

### <a name="general-backup-purposes"></a>Algemene back-updoeleinden
Nadat u een bestandsshare maakt, kunt u periodiek een share-momentopname van de bestandsshare te gebruiken voor de back-up maken. Een momentopname, wanneer er periodiek gemaakt share kunt onderhouden van vorige versies van gegevens die kunnen worden gebruikt voor toekomstige controle vereisten of herstel na noodgevallen.

## <a name="capabilities"></a>Functionaliteit
Momentopname van een share is een point-in-time, alleen-lezen kopie van uw gegevens. U kunt maken, verwijderen en momentopnamen beheren met behulp van de REST-API. Dezelfde mogelijkheden zijn ook beschikbaar in de client-bibliotheek, Azure CLI en Azure portal. 

U kunt momentopnamen van een share weergeven met behulp van de REST-API en de SMB. U kunt de lijst met versies van de map of het bestand ophalen en u kunt een specifieke versie rechtstreeks als een station koppelen (alleen beschikbaar op Windows - Zie [limieten](#limits)). 

Nadat een share-momentopname is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. U kunt een hele share-momentopname niet kopiëren naar een ander opslagaccount. U moet doen dat door bestand, met behulp van AzCopy of andere mechanismen voor kopiëren.

Share-momentopname-functie aangeboden op het niveau van delen. Het ophalen van wordt geleverd op afzonderlijke bestandsniveau, om toe te staan voor het herstellen van afzonderlijke bestanden. U kunt een volledige bestandsshare herstellen met behulp van SMB, de REST-API, de portal, de client-bibliotheek of PowerShell/CLI-hulpprogramma's.

Een share-momentopname van een bestandsshare is identiek aan de basis-bestandsshare. Het enige verschil is dat een **datum-/** waarde wordt toegevoegd aan de share URI om aan te geven van de tijd waarop de share-momentopname is gemaakt. Bijvoorbeeld, als een bestandsshare-URI is http://storagesample.core.file.windows.net/myshare, de share-momentopname URI vergelijkbaar met is:
```
http://storagesample.file.core.windows.net/myshare?snapshot=2011-03-09T01:42:34.9360000Z
```

Momentopnamen van shares blijft actief totdat ze expliciet worden verwijderd. Momentopname van een share kan niet de basis-bestandsshare outlive. U kunt de momentopnamen die zijn gekoppeld aan de basis-bestandsshare om bij te houden van uw huidige momentopnamen opsommen. 

Wanneer u een share-momentopname van een bestandsshare maakt, worden de bestanden in de eigenschappen van de share gekopieerd naar de momentopname met dezelfde waarden. De bestanden en metagegevens van de bestandsshare zijn ook gekopieerd naar de momentopname, tenzij u opgeeft dat afzonderlijke metagegevens voor de share wanneer u deze maakt de momentopname.

U kunt een share met momentopnamen van shares, tenzij u eerst alle momentopnamen van shares verwijderen niet verwijderen.

## <a name="space-usage"></a>Gebruik van schijfruimte 
Momentopnamen van shares zijn incrementele van aard. Alleen de gegevens die zijn gewijzigd nadat een momentopname van de meest recente share is opgeslagen. Hierdoor minimaliseert de tijd die nodig is om de share-momentopname te maken en bespaart op kosten voor opslag. Een schrijfbewerking naar het object of eigenschap of metagegevens updatebewerking wordt geteld naar "gewijzigde inhoud" en wordt opgeslagen in een momentopname van de share. 

Om meer ruimte, kunt u de share-momentopname verwijderen voor de periode wanneer het verloop hoogste is.

Hoewel momentopnamen van shares incrementeel zijn opgeslagen, moet u ook alleen de meest recente momentopname behouden om te kunnen herstellen van de share. Wanneer u een momentopname van een share verwijdert, wordt alleen de gegevens die uniek is voor die share-momentopname verwijderd. Actieve momentopnamen bevatten alle informatie die u nodig hebt om te bladeren en uw gegevens te herstellen (vanaf het moment dat de share-momentopname is gemaakt) naar de oorspronkelijke locatie of een alternatieve locatie. U kunt herstellen op itemniveau.

Momentopnamen tellen niet mee voor uw limiet van 5 TB delen. Er is geen limiet voor hoeveel ruimte momentopnamen in totaal in beslag nemen. Opslagaccountlimieten nog steeds van toepassing.

## <a name="limits"></a>Limieten
Het maximum aantal momentopnamen van shares die Azure Files vandaag kunnen is 200. Nadat 200 momentopnamen hebt u oudere momentopnamen van shares verwijderen om het nieuwe te maken. 

Er is geen limiet voor de gelijktijdige aanroepen voor het maken van momentopnamen van shares. Er is geen limiet voor de hoeveelheid ruimte die share momentopnamen van een bepaalde bestandsshare kunnen gebruiken. 

Het is vandaag de dag niet mogelijk om te koppelen van momentopnamen van shares op Linux. Dit komt doordat de Linux SMB-client biedt geen ondersteuning voor momentopnamen koppelen, zoals Windows biedt.

## <a name="copying-data-back-to-a-share-from-share-snapshot"></a>Kopiëren van gegevens naar een share vanuit een momentopname van bestandsshare
Kopieerbewerkingen die betrekking hebben op bestanden en momentopnamen van bestandsshares volgt u deze regels:

U kunt afzonderlijke bestanden in een momentopname van bestandsshare via kopiëren naar de base share of een andere locatie. U kunt herstellen van een eerdere versie van een bestand of de volledige bestandsshare herstellen door het bestand kopiëren vanuit een momentopname van de share. De momentopname is niet worden gepromoveerd naar base delen. 

Een momentopname van de share blijft behouden nadat u hebt gekopieerd, maar de basis-bestandsshare wordt overschreven met een kopie van de gegevens die beschikbaar in de momentopname was. Alle herstelde bestanden tellen mee voor 'gewijzigde inhoud."

U kunt een bestand kopiëren in een momentopname van een share naar een bestemming met een andere naam. Het resulterende doelbestand is een beschrijfbare bestands- en niet de momentopname van een share.

Wanneer een doelbestand met behulp van een kopie overschreven, zijn alle momentopnamen die zijn gekoppeld aan het oorspronkelijke doelbestand blijven intact.

## <a name="general-best-practices"></a>Algemene aanbevolen procedures 
Wanneer u infrastructuur op Azure uitvoert, automatiseren, back-ups voor gegevensherstel indien mogelijk. Automatische acties zijn betrouwbaarder dan handmatige processen, helpt de beveiliging van gegevens en herstelmogelijkheden te verbeteren. U kunt de REST-API, SDK van de Client of het uitvoeren van scripts voor automatisering.

Voordat u de share-momentopname scheduler implementeert, moet u zorgvuldig overwegen de frequentie van de share-momentopname en instellingen voor het bewaren om onnodige kosten te besparen.

Momentopnamen van shares bieden alleen op bestandsniveau beveiliging. Momentopnamen van shares voorkomen niet fat-vinger verwijderingen op een share of storage account. Om u te helpen een storage-account te beveiligen tegen onbedoelde verwijderingen, vergrendelt u het opslagaccount of de resourcegroep.

## <a name="next-steps"></a>Volgende stappen
- Werken met momentopnamen van shares in:
    - [PowerShell](storage-how-to-use-files-powershell.md)
    - [CLI](storage-how-to-use-files-cli.md)
    - [Windows](storage-how-to-use-files-windows.md#accessing-share-snapshots-from-windows)
- [Veelgestelde vragen over momentopname delen](storage-files-faq.md#share-snapshots)
