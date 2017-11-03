---
title: Azure-Stack beveiligingsmechanismen begrijpen | Microsoft Docs
description: Als servicebeheerder meer informatie over de beveiligingsbesturingselementen toegepast op Azure-Stack
services: azure-stack
documentationcenter: 
author: Heathl17
manager: byronr
editor: 
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 106fcf7b0edc095a52e82d58ad48a73084b65d1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-infrastructure-security-posture"></a>Azure beveiligingspostuur van Stack-infrastructuur

*Van toepassing op: Azure Stack geïntegreerd systemen*

Beveiligingsoverwegingen en nalevingsreglementen zijn een van de belangrijkste stuurprogramma's voor het gebruik van hybride clouds. Azure-Stack is ontworpen voor deze scenario's en het is belangrijk te begrijpen van de controlemechanismen al zijn geïmplementeerd als overstap op Azure-Stack.

In Azure-Stack zijn er twee houding beveiligingslagen die naast elkaar bestaan. De eerste laag bestaat uit de Stack van Azure-infrastructuur, die u gaat van de hardwareonderdelen helemaal tot aan de Azure Resource Manager en bevat de beheerder en het Tenant-portals. De tweede laag bestaat uit de werkbelastingen die tenants maken, implementeren en beheren en bevat items zoals virtuele machines of Services van de App-websites.  

## <a name="security-approach"></a>Beveiliging benadering
Azure-Stack is ontworpen met een beveiligingspostuur als bescherming tegen actieve bedreigingen en is ontworpen om te voldoen aan de vereisten van de belangrijkste nalevingsstandaards. Als gevolg hiervan is de beveiligingsstatus van de Azure-Stack-infrastructuur gebaseerd op twee stijlen:

 - **Stel dat inbreuk.** Vanaf de veronderstelling dat het systeem al is geschonden, we richten op *detecteren en het beperken van de gevolgen van inbreuk* versus alleen probeert om aanvallen te voorkomen. 
 - **Standaard gehard.**  Aangezien de infrastructuur worden uitgevoerd op een goed gedefinieerde hardware en software, we *inschakelen, configureren en valideren van beveiligingsfuncties* die meestal naar links om klanten te implementeren.

Omdat Azure-Stack is geleverd als een geïntegreerde systeem, wordt de beveiligingsstatus van de Azure-Stack-infrastructuur wordt gedefinieerd door Microsoft.  Net als in Azure zijn tenants verantwoordelijk voor het definiëren van de beveiligingsstatus van de tenantwerkbelastingen. Dit document bevat fundamentele kennis over de beveiligingsstatus van de Azure-Stack-infrastructuur.

## <a name="data-at-rest-encryption"></a>Data-at-rest versleuteling
Alle Azure-Stack-infrastructuur en tenant-gegevens in rust met Bitlocker is versleuteld. Deze versleuteling wordt beschermd tegen fysieke verlies of diefstal van Azure-Stack opslagonderdelen. 

## <a name="data-in-transit-encryption"></a>Gegevens in de doorvoer-versleuteling
De onderdelen van de Azure-Stack-infrastructuur communiceren met kanalen die zijn versleuteld met TLS 1.2. Certificaten voor bestandsversleuteling worden automatisch beheerd door de infrastructuur. 

Alle eindpunten van de externe infrastructuur, zoals de REST-eindpunten of de Stack van Azure-portal, TLS 1.2-ondersteuning voor beveiligde communicatie. Certificaten voor bestandsversleuteling, van een derde partij of van de certificeringsinstantie van uw bedrijf moeten worden opgegeven voor deze eindpunten. 

Terwijl de zelfondertekende certificaten kunnen worden gebruikt voor deze externe eindpunten, raadt Microsoft tegen het gebruik ervan. 

## <a name="secret-management"></a>Geheime management
Een groot aantal geheimen zoals wachtwoorden, Azure Stack-infrastructuur gebruikt om te functioneren. De meeste van deze worden automatisch gedraaid vaak, omdat ze groep beheerde serviceaccounts, die om de 24 uur draait.

De resterende geheimen die niet zijn voorzien van een groep beheerde serviceaccounts kunnen handmatig worden gedraaid met een script in de beschermde eindpunt.

## <a name="code-integrity"></a>Code-integriteit
Azure-Stack wordt gebruikgemaakt van de meest recente versie van Windows Server 2016 beveiligingsfuncties. Een van beide is Windows Defender Device Guard, die toepassing whitelisting biedt en zorgt ervoor dat alleen code wordt uitgevoerd binnen de Azure-Stack-infrastructuur gemachtigde. 

Geautoriseerde code is ondertekend door Microsoft of de OEM-partner en het is opgenomen in de lijst met toegestane software die is opgegeven in een beleid is gedefinieerd door Microsoft. Met andere woorden, kan alleen de software die is goedgekeurd voor uitvoering in de Azure-Stack-infrastructuur worden uitgevoerd. Elke poging om uit te voeren niet-geautoriseerde code worden geblokkeerd en wordt een controlestatusbericht gegenereerd.

Het beleid Device Guard voorkomt ook dat software of agenten van derden uitgevoerd in de Azure-Stack-infrastructuur.

## <a name="credential-guard"></a>Referentie Guard
Een andere Windows Server 2016 beveiligingsfunctie in Azure-Stack is Windows Defender referentie Guard, die wordt gebruikt voor het beveiligen van Azure-Stack infrastructuur referenties Pass-the-Hash en Pass-the-Ticket-aanvallen.

## <a name="antimalware"></a>Antimalware
Elk onderdeel in Azure-Stack (Hyper-V-hosts en virtuele Machines) is met Windows Defender Antivirus beveiligd.

## <a name="constrained-administration-model"></a>Beperkte beheermodel
Beheer in Azure-Stack wordt beheerd door het gebruik van drie toegangspunten, elk met een specifiek doel: 
1. De [Beheerdersportal](azure-stack-manage-portals.md) biedt een point-and-click-ervaring voor dagelijkse beheertaken uit te voeren.
2. Azure Resource Manager beschrijft de beheerbewerkingen van de Beheerdersportal via een REST-API die wordt gebruikt door PowerShell en Azure CLI. 
3. Voor specifieke bewerkingen op laag niveau, bijvoorbeeld data center-integratie of ondersteuning voor scenario's, Azure Stack beschrijft een PowerShell-eindpunt aangeroepen [bevoegde eindpunt](azure-stack-privileged-endpoint.md). Dit eindpunt wordt alleen een goedgekeurde lijst set cmdlets en het wordt sterk gecontroleerd.

## <a name="network-controls"></a>Netwerk-besturingselementen
Azure Stack-infrastructuur wordt geleverd met meerdere lagen van network Access Control List(ACL).  De ACL's te voorkomen dat onbevoegde toegang tot de infrastructuuronderdelen en infrastructuur voor communicatie met alleen de paden die vereist voor de werking ervan zijn te beperken. 

Netwerk-ACL's worden afgedwongen in drie lagen:
1.  Bovenaan Rack-switches
2.  Software-gedefinieerde netwerk
3.  Host en de VM besturingssysteem firewalls 


