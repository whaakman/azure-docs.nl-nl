---
ms.assetid: ''
title: Azure Key Vault-functie voor voorlopig verwijderen | Microsoft Docs
ms.service: key-vault
ms.topic: conceptual
author: bryanla
ms.author: bryanla
manager: mbaldwin
ms.date: 09/25/2017
ms.openlocfilehash: c8bde99e0247871212766a9915b9d07b7f392201
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465585"
---
# <a name="azure-key-vault-soft-delete-overview"></a>Overzicht van Azure Key Vault-functie voor voorlopig verwijderen

Functie voor voorlopig verwijderen van de Sleutelkluis kan herstel van de verwijderde kluizen en kluis objecten, functie voor voorlopig verwijderen genoemd. Specifiek, behandelen we de volgende scenario's:

- Ondersteuning voor het herstelbare verwijderen van een key vault
- Ondersteuning voor herstelbare verwijdering van key vault-objecten (ex.) sleutels, geheimen, certificaten)

## <a name="supporting-interfaces"></a>Interfaces ondersteunen

De functie voor voorlopig verwijderen is in eerste instantie beschikbaar zijn via de REST, .NET / C#, PowerShell en CLI-interfaces.

Zie de naslaginformatie voor algemene informatie voor deze voor meer informatie [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Scenario's

Sleutelkluizen van Azure worden bijgehouden bronnen, die worden beheerd door Azure Resource Manager. Azure Resource Manager geeft ook een goed gedefinieerde gedrag voor verwijdering, waarvoor is vereist dat een geslaagde bewerking moet resulteren in die resource niet meer wordt toegankelijk is. De functie voor voorlopig verwijderen wordt het herstel van het verwijderde object of de verwijdering per ongeluk of opzettelijk is.

1. In de typische scenario wordt een gebruiker mogelijk per ongeluk verwijderd een key vault of een key vault-object. Als die key vault of key vault-object zijn om te worden hersteld voor een vooraf vastgestelde periode, de gebruiker kan de verwijdering ongedaan maken en herstellen van hun gegevens.

2. In een ander scenario kan een rogue-gebruiker probeert te verwijderen van een key vault of een key vault-object, zoals een sleutel in een kluis, een bedrijfsonderbreking veroorzaken. Het verwijderen van de key vault of key vault-object te scheiden van de werkelijke verwijdering van de onderliggende gegevens kan worden gebruikt als een veiligheidsmaatregel door, bijvoorbeeld rol beperken van machtigingen voor het verwijderen van gegevens naar een andere vertrouwd. Deze aanpak vereist effectief quorum voor een bewerking die anders tot een directe gegevens verloren gaan leiden kan.

### <a name="soft-delete-behavior"></a>Gedrag van de functie voor voorlopig verwijderen

Met deze functie is de DELETE-bewerking op een key vault of key vault-object een voorlopig verwijderen, effectief met de resources voor een bepaalde bewaartermijn, terwijl het uiterlijk dat het object is verwijderd. De service meer biedt een mechanisme voor het herstellen van het verwijderde object, in feite de verwijdering ongedaan te maken. 

Voorlopig verwijderen is een optionele Key Vault-gedrag en is **niet standaard ingeschakeld** in deze release. 

### <a name="key-vault-recovery"></a>Herstel van de sleutelkluis

Bij het verwijderen van een key vault, maakt de service een resource proxy onder het abonnement, voldoende metagegevens voor herstel. De proxy-bron is een opgeslagen-object, beschikbaar in dezelfde locatie als de verwijderde key vault. 

### <a name="key-vault-object-recovery"></a>Sleutelkluis objectherstel

Bij het verwijderen van een key vault-object, zoals een sleutel, plaatst de service het object in een verwijderde status, waardoor deze niet toegankelijk voor alle bewerkingen voor het ophalen. In deze toestand kan de sleutelkluis-object alleen worden weergegeven, hersteld of geforceerd/definitief verwijderd. 

Key Vault wordt de verwijdering van de onderliggende gegevens die overeenkomt met de verwijderde sleutelkluis of het object van de sleutelkluis voor uitgevoerd na een vooraf vastgestelde bewaarinterval plannen op hetzelfde moment. De DNS-record die overeenkomt met de kluis is ook behouden voor de duur van de retentie-interval.

### <a name="soft-delete-retention-period"></a>Bewaarperiode voor voorlopig verwijderen

Voorlopig verwijderde bronnen worden gedurende een bepaalde periode, kunt u 90 dagen bewaard. Tijdens het voorlopig verwijderen retentie-interval, de volgende van toepassing:

- U kunt alle sleutelkluizen en key vault-objecten in de status van de functie voor voorlopig verwijderen voor uw abonnement, evenals de toegang verwijderen en herstel informatie over deze aanbieden.
    - Alleen gebruikers met speciale machtigingen kunnen aanbieden, verwijderde kluizen. Het wordt aangeraden om onze gebruikers een aangepaste rol maken met speciale machtigingen voor de verwerking van verwijderde kluizen.
- Een sleutelkluis met dezelfde naam kan niet worden gemaakt op dezelfde locatie; dienovereenkomstig, een key vault-object kan niet worden gemaakt in een bepaalde kluis als die key vault bevat een object met dezelfde naam en deze bevindt zich in een verwijderde status 
- Alleen een specifiek bevoegde gebruiker mogelijk een key vault of key vault-object herstellen door een opdracht voor herstellen op de bijbehorende proxy-resource.
    - De gebruiker lid is van de aangepaste rol met de bevoegdheid om een sleutelkluis onder de resourcegroep te maken kunt de kluis herstellen.
- Alleen een specifiek bevoegde gebruiker mogelijk een key vault of key vault-object met behulp van een opdracht tot verwijderen van de bijbehorende proxy-bron geforceerd verwijderen.

Tenzij een key vault of key vault-object is hersteld, aan het einde van de retentie-interval dat de service wordt uitgevoerd een opschonen van de voorlopig verwijderde key vault of key vault-object en de bijbehorende inhoud. Verwijderen van de resource kan niet opnieuw worden gepland.

### <a name="permitted-purge"></a>Toegestane opschonen

Definitief verwijderen, verwijderen van gegevens, wordt een key vault is het mogelijk is via een POST-bewerking op de proxy-resource en speciale bevoegdheden vereist. Over het algemeen is alleen de eigenaar van het abonnement mogelijk een sleutelkluis leegmaken. De POST-bewerking wordt geactiveerd voor de onmiddellijke en niet-herstelbare verwijdering van deze kluis. 

Een uitzondering hierop is het geval wanneer het Azure-abonnement is gemarkeerd als *niet kan*. In dit geval alleen de service voert de werkelijke verwijdering en wordt als een geplande proces. 

### <a name="billing-implications"></a>Gevolgen van facturering

In het algemeen, wanneer een object (een key vault of een sleutel of geheim) verwijderde status heeft, er zijn slechts twee bewerkingen mogelijk: 'wissen' en 'herstellen'. Alle andere bewerkingen mislukken. Hoewel het object bestaat, daarom geen bewerkingen kunnen worden uitgevoerd en kan daarom geen gebruik wordt uitgevoerd, dus geen factuur. Maar er zijn de volgende uitzonderingen:

- 'wissen' en 'herstellen' Acties tellen mee voor normale key vault-bewerkingen en wordt in rekening gebracht.
- Als het object een HSM-sleutel is, geldt de 'HSM beveiligde sleutel' kosten in rekening gebracht per sleutelversie per maand kosten in rekening gebracht als een sleutelversie is gebruikt in de afgelopen 30 dagen. Hierna, omdat het object verwijderde status heeft die geen bewerkingen kunnen worden uitgevoerd op basis van deze, dus geen kosten in rekening gebracht is van toepassing.

## <a name="next-steps"></a>Volgende stappen

De volgende twee handleidingen bieden de primaire gebruiksscenario's voor voorlopig verwijderen gebruiken.

- [De Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell](key-vault-soft-delete-powershell.md) 
- [De Key Vault-functie voor voorlopig verwijderen gebruiken met CLI](key-vault-soft-delete-cli.md)

