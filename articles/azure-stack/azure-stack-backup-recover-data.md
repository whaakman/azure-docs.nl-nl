---
title: Herstellen van onherstelbaar gegevensverlies in met behulp van de infrastructuur Backup-Service van Azure-Stack | Microsoft Docs
description: Wanneer een onherstelbare fout zorgt dat Azure Stack mislukt, kunt uw infrastructuur-gegevens herstellen bij het hervatten van de implementatie van uw Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2ECE8580-0BDE-4D4A-9120-1F6771F2E815
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: 859d31b554fcd1936ce555f6afb0f4631a3af7aa
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="recover-from-catastrophic-data-loss"></a>Herstellen van onherstelbaar gegevensverlies

*Van toepassing op: Azure Stack geïntegreerd systemen.*

Azure Stack Azure-services worden uitgevoerd in uw datacenter. Azure Stack kunt uitvoeren op omgevingen met vier knooppunten zijn geïnstalleerd in één rack zo klein. Daarentegen Azure wordt uitgevoerd in meer dan 40 regio's in meerdere datacenters en meerdere zones in elke regio. Gebruikersbronnen kunnen meerdere servers, rekken datacenters en regio's omvatten. Met Azure-Stack hebt momenteel alleen u de keuze voor het implementeren van uw volledige in de cloud op één rack. Hiermee wordt uw cloud aan het risico van rampen bij uw datacenter of storingen als gevolg van fouten die belangrijke product. Wanneer een noodgeval biedt, wordt het exemplaar netwerkstack Azure offline gaat. Alle gegevens is niet kunnen worden hersteld.

Afhankelijk van de hoofdoorzaak van het verlies van gegevens, kunt u wellicht een service één infrastructuur repareren of herstellen van de volledige Azure-Stack-instantie. Mogelijk moet u zelfs herstellen naar andere hardware in dezelfde locatie of in een andere locatie.

Dit scenario adressen herstellen van de volledige installatie bij een storing van de apparatuur en het opnieuw distribueren van de privécloud.

| Scenario                                                           | Verlies van gegevens                            | Overwegingen                                                             |
|--------------------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------|
| Herstellen van een onherstelbare gegevensverlies als gevolg van noodherstel of product-fout | Alle infrastructuur en de gebruiker en de app-gegevens | Gebruiker toepassings- en afzonderlijk van infrastructuur-gegevens worden beveiligd |

## <a name="workflows"></a>Werkstromen

Het traject van het beschermen van Azure Start begint met het afzonderlijk back-ups van de infrastructuur- en app/tenant-gegevens. Dit document bevat informatie over het beveiligen van de infrastructuur. 

![Eerste implementatie van Azure-Stack](media\azure-stack-backup\azure-stack-backup-workflow1.png)

In slechtste case scenario's waarin alle gegevens gaan verloren, met de Azure-Stack herstellen is het proces voor het herstellen van de infrastructuur-gegevens uniek zijn voor deze implementatie van Azure-Stack en alle gegevens van de gebruiker. 

![Azure Stack implementeren](media\azure-stack-backup\azure-stack-backup-workflow2.png)

## <a name="restore"></a>Herstellen

Als onherstelbaar gegevensverlies maar de hardware kan nog steeds gebruikt worden, is opnieuw distribueren van Azure-Stack is vereist. Tijdens het opnieuw implementeren, kunt u de locatie voor de opslag en de referenties die zijn vereist voor toegang tot de back-ups. In deze modus zijn is er niet nodig om op te geven van de services die moeten worden teruggezet. Back-up infrastructuurbeheerder injects vlak status als onderdeel van de werkstroom voor de implementatie.

Als er een ramp dat de hardware onbruikbaar samenstelt, is opnieuw implementeren gaande alleen mogelijk op nieuwe hardware. Opnieuw distribueren kan duren voordat enkele weken vervanging hardware is ingedeeld en binnenkomt in het datacenter. Het herstellen van gegevens van het besturingselement vlak kan op elk gewenst moment. Terugzetten wordt niet ondersteund als de versie van het exemplaar opnieuw gedistribueerde meer dan één versie is groter dan de versie in de laatste back-up. 

| Implementatiemodus | Beginpunt | Eindpunt                                                                                                                                                                                                     |
|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Schone installatie   | Basislijn build | OEM-Stack Azure implementeert en updates voor de laatste ondersteunde versie.                                                                                                                                          |
| Modus voor herstel   | Basislijn build | OEM Azure-Stack in herstelmodus implementeert en de versie die overeenkomt met de vereisten op basis van de meest recente back-up beschikbaar worden afgehandeld. De OEM voltooit de implementatie met het bijwerken van de laatste ondersteunde versie. |

## <a name="data-in-backups"></a>Gegevens in de back-ups

Azure-Stack biedt ondersteuning voor een type implementatie cloud herstelmodus aangeroepen. Deze modus wordt alleen gebruikt als u Azure-Stack herstellen na een noodgeval of product bug de oplossing onherstelbare weergegeven. Deze implementatiemodus worden niet hersteld van de gebruikersgegevens opgeslagen in de oplossing. Het bereik van deze implementatiemodus is beperkt tot het herstellen van de volgende gegevens:

 - Implementatie van invoer
 - Interne identiteitssystemen
 - Federatieve identificeren configuration (niet-verbonden implementaties)
 - Basiscertificaten die wordt gebruikt door een interne certificeringsinstantie
 - Azure Resource Manager configuration gebruikersgegevens, zoals abonnementen, plannen, aanbiedingen en quota's voor opslag, netwerken en bronnen berekenen
 - KeyVault geheimen en kluizen
 - RBAC beleidstoewijzingen en roltoewijzingen 

Geen van de gebruiker infrastructuur als een Service (IaaS) of Platform als een Service (PaaS)-resources zijn hersteld tijdens de implementatie. Dat wil gaan IaaS VM's, opslagaccounts, blobs, tabellen, netwerkconfiguratie, enzovoort, verloren. Het doel van het herstel van de cloud is om te controleren of de operators en gebruikers kunnen zich aanmelden in de portal nadat de implementatie is voltooid. Gebruikers die zich aanmelden in ziet hun bronnen. Gebruikers hebben hun abonnementen hersteld en samen met die de oorspronkelijke plannen en biedt beleidsregels die zijn gedefinieerd door de beheerder. Gebruikers die zich aanmelden terug in het systeem wordt uitgevoerd onder de dezelfde beperkingen die zijn opgelegd door de oorspronkelijke oplossing voordat de na noodgevallen. Nadat het herstel van de cloud is voltooid, de operator handmatig kunt terugzetten waarde toevoegen en de derde partij RPs en de bijbehorende gegevens.

## <a name="next-steps"></a>Volgende stappen

 - Meer informatie over aanbevolen procedures voor het [met behulp van de infrastructuur Backup-Service](azure-stack-backup-best-practices.md).
