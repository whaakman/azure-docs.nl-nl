---
ms.assetid: 
title: Azure Sleutelkluis voorlopig verwijderen | Microsoft Docs
ms.service: key-vault
author: lleonard-msft
ms.author: alleonar
manager: mbaldwin
ms.date: 09/25/2017
ms.openlocfilehash: 01357e4fdb9b6f27e9baf5f5c8e4c7d6b582ad35
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-soft-delete-overview"></a>Overzicht van Azure Sleutelkluis voorlopig verwijderen

Functie van de voorlopig verwijderen van de Sleutelkluis kunt herstel van de verwijderde kluizen en objecten van de kluis, bekend als voorlopig verwijderd. In het bijzonder houden we de volgende scenario's:

- Ondersteuning voor herstelbare verwijdering van een sleutelkluis
- Ondersteuning voor herstelbare verwijderen van sleutelkluis-objecten (ex. sleutels, geheimen, certificaten)

## <a name="supporting-interfaces"></a>Interfaces ondersteunen

De functie voor voorlopig verwijderen is in eerste instantie beschikbaar via de REST, .NET / C#, PowerShell en CLI-interfaces.

Zie de naslaginformatie voor algemene informatie voor deze voor meer informatie [Key Vault Reference](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Scenario's

Azure sleutel kluizen worden bijgehouden resources, beheerd door Azure Resource Manager. Azure Resource Manager bevat ook een goed gedefinieerde gedrag voor verwijdering, die vereist dat een geslaagde verwijderbewerking moet resulteren in die resource niet meer wordt toegankelijk. De functie voor voorlopig verwijderen heeft betrekking op het herstel van het verwijderde object of de verwijdering per ongeluk of opzettelijk is.

1. In het typische scenario, een gebruiker mogelijk per ongeluk verwijderd een sleutelkluis of een sleutelkluis-object. Als die sleutel kluis of sleutel kluis object zou worden hersteld voor een vooraf bepaalde periode, de gebruiker kan de verwijdering ongedaan maken en hun gegevens herstellen.

2. Een malafide gebruiker kan in een ander scenario proberen om een sleutelkluis of een object van de sleutelkluis, zoals een sleutel in een kluis, om te leiden tot een onderbreking van de zakelijke te verwijderen. Het verwijderen van de sleutelkluis-object van de sleutelkluis of scheiden van de werkelijke verwijdering van de onderliggende gegevens kan worden gebruikt als veiligheidsmaatregel door, bijvoorbeeld rol beperken van machtigingen voor het verwijderen van de gegevens naar een andere vertrouwde. Deze benadering vergt effectief quorum voor een bewerking die anders in een onmiddellijke gegevensverlies resulteert mogelijk.

### <a name="soft-delete-behavior"></a>Gedrag voor voorlopig verwijderen

Met deze functie is de DELETE-bewerking op een sleutelkluis of sleutelkluis object een voorlopig verwijderen, de resources effectief te houden voor een bepaalde bewaartermijn tijdens het doorgeven van de weergave of het object is verwijderd. De service verder biedt een mechanisme voor het herstellen van het verwijderde object, de verwijdering in wezen ongedaan te maken. 

Voorlopig verwijderen is een optionele Sleutelkluis gedrag en is **niet standaard ingeschakeld** in deze release. 

### <a name="key-vault-recovery"></a>Sleutelkluis-herstel

Bij een sleutelkluis te verwijderen, maakt de service een middel proxy onder het abonnement toe te voegen voldoende metagegevens voor herstel. De proxy-bron is een opgeslagen object beschikbaar zijn in dezelfde locatie als de verwijderde sleutelkluis. 

### <a name="key-vault-object-recovery"></a>Sleutelkluis objectherstel

Bij het verwijderen van een object van de sleutelkluis, zoals een sleutel en plaatst de service het object in een verwijderde staat bevindt, waardoor deze niet toegankelijk voor alle bewerkingen voor het ophalen. In deze toestand is, kan de sleutelkluis-object alleen worden weergegeven, herstelde of geforceerd/permanent verwijderd. 

Sleutelkluis wordt de verwijdering van de onderliggende gegevens voor de sleutelkluis-object voor uitvoering na een vooraf vastgestelde retentie-interval van de verwijderde sleutelkluis of gepland op hetzelfde moment. De DNS-record die overeenkomt met de kluis is ook behouden voor de duur van de retentie-interval.

### <a name="soft-delete-retention-period"></a>Bewaarperiode voor voorlopig verwijderen

Voorlopig verwijderde bronnen worden bewaard gedurende een bepaalde tijd, 90 dagen. Tijdens het voorlopig verwijderen retentie-interval, de volgende van toepassing:

- U kunt alle sleutelkluizen en sleutelkluis-objecten in de voorlopig verwijderen voor uw abonnement, evenals de verwijdering en herstel informatie over deze aanbieden.
    - Alleen gebruikers met speciale machtigingen kunnen verwijderde kluizen aanbieden. U kunt het beste onze gebruikers een aangepaste beveiligingsrol maken met deze speciale machtigingen voor de verwerking van verwijderde kluizen.
- Een sleutelkluis met dezelfde naam kan niet worden gemaakt op dezelfde locatie; : een sleutelkluis-object kan niet worden gemaakt in een kluis gegeven als dat sleutelkluis bevat een object met dezelfde naam en deze bevindt zich in een verwijderde staat bevindt 
- Alleen een specifiek bevoegde gebruiker mogelijk een sleutelkluis of sleutelkluis object hersteld door een opdracht herstellen op de overeenkomende resource in de proxy.
    - De gebruiker lid is van de aangepaste rol met de bevoegdheid om een sleutelkluis onder de resourcegroep te maken kunt herstellen de kluis.
- Alleen een specifiek bevoegde gebruiker mogelijk een sleutelkluis of sleutelkluis-object door uitgifte van een verwijderopdracht op de overeenkomende resource in de proxy geforceerd verwijderen.

Tenzij een sleutelkluis of sleutelkluis-object is hersteld, aan het einde van de retentie-interval dat de service uitvoert een opschonen van de sleutelkluis voorlopig verwijderde of sleutelkluis-object en de inhoud ervan. Verwijderen van de resource kan niet opnieuw worden gepland.

### <a name="permitted-purge"></a>Toegestane opschonen

Permanent verwijderen, verwijderen, wordt een sleutelkluis is mogelijk via een POST-bewerking op de proxy-resource en speciale bevoegdheden vereist. In het algemeen is de eigenaar van het abonnement mogelijk een sleutelkluis verwijderen. De POST-bewerking activeert de onmiddellijke en onherstelbare verwijdering van deze kluis. 

Een uitzondering hierop is het geval wanneer het Azure-abonnement is gemarkeerd als *niet kan*. In dit geval wordt alleen de service voert de werkelijke verwijdering en als een geplande proces gebeurt. 

## <a name="next-steps"></a>Volgende stappen

De volgende twee handleidingen bieden de primaire gebruiksscenario's voor het gebruik van voorlopig verwijderen.

- [De Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell](key-vault-soft-delete-powershell.md) 
- [De Key Vault-functie voor voorlopig verwijderen gebruiken met CLI](key-vault-soft-delete-cli.md)

