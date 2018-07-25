---
title: Herstel na onherstelbare gegevensverlies in Azure Stack met behulp van de infrastructuur voor Backup-Service | Microsoft Docs
description: Wanneer een onherstelbare fout zorgt ervoor dat Azure Stack mislukken, u kunt uw infrastructuur-gegevens herstellen als uw Azure Stack-implementatie hervatten.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/20/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: f1582efa7d357f6f535c562a656ec17024357320
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242866"
---
# <a name="recover-from-catastrophic-data-loss"></a>Herstel na onherstelbare gegevensverlies

*Is van toepassing op: Azure Stack-geïntegreerde systemen.*

Azure Stack Azure-services worden uitgevoerd in uw datacenter. Azure Stack kunt uitvoeren op omgevingen zo klein is als vier knooppunten in één rack zijn geïnstalleerd. Daarentegen Azure wordt uitgevoerd in meer dan 40 regio's in meerdere datacenters en meerdere zones in elke regio. Gebruikersbronnen kunnen meerdere servers, rekken, datacenters en regio's omvatten. Met Azure Stack nog u alleen kiezen waarmee u uw volledige in de cloud implementeren met een enkele rack. Hiermee wordt aangegeven dat de cloud om het risico van catastrofale gebeurtenissen in uw datacenter of storingen als gevolg van belangrijke productfouten. Wanneer zich een noodsituatie voordoet, wordt het Azure Stack-exemplaar offline gaat. Alle gegevens is mogelijk niet kan worden hersteld.

Afhankelijk van de hoofdoorzaak van het verlies van gegevens moet u een service één infrastructuur repareren of herstellen van de volledige Azure Stack-exemplaar. Mogelijk moet u ook naar andere hardware op dezelfde locatie of in een andere locatie herstellen.

In dit scenario adressen herstellen van de volledige installatie in het geval van een storing van de apparatuur en het opnieuw distribueren van de privécloud.

| Scenario                                                           | Verlies van gegevens                            | Overwegingen                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Herstel na onherstelbare gegevensverlies vanwege een natuurramp of product-fout | Alle infrastructuur en de gebruiker en de app-gegevens | Gebruikerstoepassing en de gegevens zijn afzonderlijk uit infrastructuur-gegevens beveiligd |

## <a name="workflows"></a>Werkstromen

De reis van het beveiligen van Azure starten begint met het afzonderlijk back-ups van de infrastructuur en -app/tenant-gegevens. In dit document bevat informatie over het beveiligen van de infrastructuur. 

![Eerste implementatie van Azure Stack](media\azure-stack-backup\azure-stack-backup-workflow1.png)

In slechtste geval scenario's waarin alle gegevens verloren gaan, is herstellen van Azure Stack het proces van het terugzetten van de infrastructuur van de unieke gegevens naar deze implementatie van Azure Stack en alle gebruikersgegevens. 

![Azure Stack opnieuw implementeren](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Herstellen

Als er onherstelbaar gegevensverlies is, maar de hardware kan nog steeds worden gebruikt, is opnieuw implementeren van Azure Stack is vereist. U kunt de locatie voor de opslag en de referenties die zijn vereist voor toegang tot back-ups opgeven tijdens het opnieuw te implementeren. In deze modus is er niet nodig om op te geven van de services die moeten worden hersteld. Infrastructuur voor back-up Controller injects besturingselement vlak status als onderdeel van de implementatiewerkstroom.

Als er een ramp die de hardware onbruikbaar wordt weergegeven, is opnieuw implementeren alleen mogelijk op nieuwe hardware. Opnieuw implementeren kan enkele weken duren terwijl vervanging hardware is ingedeeld en in het datacenter wordt ontvangen. Terugzetten van besturingselement vlak van gegevens is mogelijk op elk gewenst moment. Terugzetten wordt niet ondersteund als de versie van het opnieuw geïmplementeerde exemplaar groter is dan de versie die wordt gebruikt in de laatste back-up van meer dan één versie is. 

| Implementatiemodus | Beginpunt | Eindpunt                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Schone installatie   | Basislijn maken | OEM implementeert Azure Stack en updates voor de nieuwste ondersteunde versie.                                                                                                                                          |
| Modus voor herstel   | Basislijn maken | OEM Azure Stack in de herstelmodus implementeert en de versie die overeenkomt met de vereisten op basis van de meest recente back-up beschikbaar worden verwerkt. De OEM is de implementatie voltooid door te werken naar de nieuwste ondersteunde versie. |

## <a name="data-in-backups"></a>Gegevens in de back-ups

Azure Stack biedt ondersteuning voor een type implementatie met de naam cloud-herstelmodus. In deze modus wordt alleen gebruikt als u Azure Stack herstellen na een noodgeval of productfout de oplossing niet kan worden hersteld weergegeven. In deze implementatiemodus niet binnen een van de gegevens die zijn opgeslagen in de oplossing. Het bereik van deze implementatiemodus is beperkt tot het herstellen van de volgende gegevens:

 - Implementatie-invoer
 - Interne identiteitssystemen
 - Federatieve identificatie configuratie (niet-verbonden implementaties)
 - Basiscertificaten die worden gebruikt door een interne certificeringsinstantie
 - Azure Resource Manager configuratie gebruikersgegevens, zoals abonnementen, plannen, aanbiedingen en quota's voor opslag, netwerk en compute-resources
 - KeyVault-geheimen en kluizen
 - Toewijzingen van beleid voor RBAC en roltoewijzingen 

Geen van de gebruiker infrastructuur als een Service (IaaS) of Platform als een Service (PaaS)-resources worden hersteld tijdens de implementatie. Dat wil zeggen gaan IaaS-VM's, opslagaccounts, blobs, tabellen, netwerkconfiguratie en enzovoort, verloren. Het doel van cloudherstel is om te controleren of uw operators en gebruikers kunnen zich aanmelden weer in de portal nadat de implementatie is voltooid. Weer aanmelden niet zichtbaar voor gebruikers van hun resources. Gebruikers hebben hun abonnementen hersteld en samen met die de oorspronkelijke plannen en biedt beleidsregels die zijn gedefinieerd door de beheerder. Gebruikers die zich aanmelden terug in het systeem werkt onder de dezelfde beperkingen die zijn opgelegd door de oorspronkelijke oplossing voordat de na noodgevallen. Nadat de cloudherstel is voltooid, de operator handmatig kunt herstellen toegevoegde waarde en de derde partij RPs en de bijbehorende gegevens.

## <a name="next-steps"></a>Volgende stappen

 - Meer informatie over de best practices voor [met behulp van de infrastructuur voor Backup-Service](azure-stack-backup-best-practices.md).