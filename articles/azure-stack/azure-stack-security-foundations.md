---
title: Informatie over beveiligingsmaatregelen voor Azure Stack
description: Als servicebeheerder meer informatie over de beveiligingsmaatregelen die is toegepast op Azure Stack
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/9/2018
ms.author: patricka
ms.openlocfilehash: 8b478c1ba60df679d69d5fced660836c16079e6a
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727086"
---
# <a name="azure-stack-infrastructure-security-posture"></a>Azure-infrastructuur stack-beveiliging

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Aandachtspunten voor de beveiliging en nalevingsvoorschriften zijn een van de belangrijkste stuurprogramma's voor het gebruik van hybride clouds. Azure Stack is ontworpen voor deze scenario's. In dit artikel wordt uitgelegd dat de beveiligingsmaatregelen voor Azure Stack.

Twee beveiligingslagen houding naast elkaar bestaan in Azure Stack. De eerste laag is de Azure Stack-infrastructuur, waaronder de hardwareonderdelen die tot de Azure Resource Manager. De eerste laag omvat de beheerder en de Tenant-portals. De tweede laag bestaat uit de werkbelastingen die zijn gemaakt, geïmplementeerd en beheerd door tenants. De tweede laag bevat items zoals virtual machines en websites App Services.

## <a name="security-approach"></a>Benadering van beveiliging

De beveiligingsstatus voor Azure Stack is ontworpen om te zetten voor bescherming tegen moderne bedreigingen en is gebouwd om te voldoen aan de vereisten van de belangrijkste nalevingsstandaarden. Als gevolg hiervan is de beveiligingsstatus van de Azure Stack-infrastructuur gebaseerd op twee onderdelen:

 - **Uitgaan van inbreuk**  
Vanaf de veronderstelling dat het systeem al is geschonden, richt u op *detecteren en beperkt de gevolgen van schendingen* versus alleen probeert om aanvallen te voorkomen. 
 - **Standaard beveiligd**  
Omdat de infrastructuur wordt uitgevoerd op goed gedefinieerde hardware en software, Azure Stack *mogelijk maakt, configureert en valideert de beveiligingsfuncties* standaard.

Omdat Azure Stack wordt geleverd als een geïntegreerd systeem, wordt de beveiligingsstatus van de Azure Stack-infrastructuur wordt gedefinieerd door Microsoft. Net als in Azure zijn tenants verantwoordelijk voor het definiëren van de beveiligingsstatus van de tenant-workloads. Dit document bevat fundamentele kennis over de beveiligingsstatus van de Azure Stack-infrastructuur.

## <a name="data-at-rest-encryption"></a>Data-at-rest versleuteling
Alle gegevens in Azure Stack-infrastructuur en tenant is in rust versleuteld met Bitlocker. Deze versleuteling bescherming tegen fysiek verlies of diefstal van Azure Stack-opslagonderdelen. Zie voor meer informatie, [data-at-rest versleuteling in Azure Stack](azure-stack-security-bitlocker.md).

## <a name="data-in-transit-encryption"></a>Gegevens in transit versleuteling
De onderdelen van de Azure Stack-infrastructuur communiceren door middel van kanalen die zijn versleuteld met TLS 1.2. Certificaten voor bestandsversleuteling worden zelf beheerd door de infrastructuur. 

Alle externe infrastructuur-eindpunten, zoals de REST-eindpunten of de Azure Stack-portal, ondersteuning voor TLS 1.2 voor beveiligde communicatie. Certificaten voor bestandsversleuteling, van een derde partij of van uw enterprise-CA, moeten worden opgegeven voor deze eindpunten. 

Hoewel zelfondertekende certificaten kunnen worden gebruikt voor deze externe eindpunten, raadt Microsoft op basis van het gebruik ervan. 

## <a name="secret-management"></a>Geheimenbeheer
Azure Stack-infrastructuur maakt gebruik van een groot aantal geheimen zoals wachtwoorden, werken. De meeste van deze worden automatisch gedraaid vaak het geval is, omdat ze Group-Managed serviceaccounts, die om de 24 uur draait zijn.

De resterende geheimen die niet zijn Group-Managed Service-accounts met een script in het Privileged eindpunt handmatig kunnen worden gedraaid.

## <a name="code-integrity"></a>Code-integriteit
Azure Stack wordt gebruik gemaakt van de meest recente versie van Windows Server 2016 beveiligingsfuncties. Een van beide is Windows Defender Device Guard, dit opname in de whitelist biedt en zorgt ervoor dat alleen code wordt uitgevoerd binnen de Azure Stack-infrastructuur gemachtigde. 

Geautoriseerde code is ondertekend door Microsoft of de OEM-partner. De ondertekende geautoriseerde code is opgenomen in de lijst met toegestane software die zijn opgegeven in een beleid is gedefinieerd door Microsoft. Met andere woorden, kan alleen de software die is goedgekeurd om te worden uitgevoerd in de Azure Stack-infrastructuur worden uitgevoerd. Elke poging voor het uitvoeren van niet-geautoriseerde code geblokkeerd en wordt een controle wordt gegenereerd.

Het Device Guard-beleid voorkomt ook dat software of agenten van derden die worden uitgevoerd in de Azure Stack-infrastructuur.

## <a name="credential-guard"></a>Credential Guard
Een andere Windows Server 2016 security-functie in Azure Stack is Windows Defender Credential Guard, die wordt gebruikt voor het beveiligen van referenties voor Azure Stack-infrastructuur van Pass-the-Hash en Pass-the-Ticket-aanvallen.

## <a name="antimalware"></a>Antimalware
Elk onderdeel in Azure Stack (Hyper-V-hosts en virtuele Machines) wordt beschermd met Windows Defender Antivirus.

Antivirus-definitie en engine-updates worden meerdere keren per dag toegepast in verbonden scenario's. In niet-verbonden scenario's worden anti-malware-updates toegepast als onderdeel van de maandelijkse updates van Azure Stack. Zie voor meer informatie, [bijwerken van Windows Defender Antivirus op Azure Stack](azure-stack-security-av.md).

## <a name="constrained-administration-model"></a>Beperkte beheermodel
Beheer in Azure Stack wordt beheerd door het gebruik van drie toegangspunten, elk met een specifiek doel: 
1. De [Beheerdersportal](azure-stack-manage-portals.md) biedt een aanwijzen en klikken voor dagelijkse beheertaken uit te voeren.
2. Azure Resource Manager wordt aangegeven dat alle beheerbewerkingen van de Beheerdersportal via een REST-API, die worden gebruikt door PowerShell en Azure CLI. 
3. Voor bepaalde bewerkingen op laag niveau, bijvoorbeeld data center-integratie of ondersteuning van scenario's, Azure Stack wordt aangegeven dat een PowerShell-eindpunt met de naam [bevoegde eindpunt](azure-stack-privileged-endpoint.md). Dit eindpunt wordt alleen een goedgekeurde set cmdlets en het is sterk gecontroleerd.

## <a name="network-controls"></a>Netwerkbesturingselementen
Azure Stack-infrastructuur wordt geleverd met meerdere lagen van netwerk lijst met ACL (Access Control). De ACL's voorkomt ongeoorloofde toegang tot de onderdelen van de infrastructuur en infrastructuur voor communicatie met alleen de paden die vereist voor de werking ervan zijn te beperken. 

Netwerk-ACL's worden afgedwongen in drie lagen:
1.  Top van Rack-switches
2.  Software Defined Network
3.  Host en virtuele machine besturingssysteem-firewalls

## <a name="regulatory-compliance"></a>Naleving van regelgeving

Azure Stack is geworden door een formele evaluatie van een derde partij-onafhankelijke controle onderneming. Als gevolg hiervan is de documentatie over hoe de Azure Stack-infrastructuur voldoet aan de toepasselijke besturingselementen uit diverse belangrijke nalevingsstandaarden beschikbaar. De documentatie is niet een certificering van Azure Stack vanwege de standaarden met inbegrip van verschillende besturingselementen voor personeel met betrekking tot en met betrekking tot proces. Klanten kunnen in plaats daarvan deze documentatie gebruiken voor een vliegende start geven hun certificeringsproces.

De beoordelingen zijn onder andere de volgende standaarden:

- [PCI-DSS](https://www.pcisecuritystandards.org/pci_security/) payment card industry-adressen.
- [CSA Cloud Control Matrix](https://cloudsecurityalliance.org/group/cloud-controls-matrix/#_overview) is een uitgebreide toewijzing voor meerdere standaarden, inclusief FedRAMP gemiddeld, ISO27001, HIPAA, HITRUST, ITAR, NIST SP800-53 en anderen.
- [FedRAMP High](https://www.fedramp.gov/fedramp-releases-high-baseline/) voor government-klanten.

De naleving-documentatie te vinden in de [Microsoft Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/Blueprint). De handleidingen naleving zijn van een beveiligde bron en moeten u zich aanmelden met de referenties van uw Azure-cloud-service.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over het draaien van uw geheimen in Azure Stack](azure-stack-rotate-secrets.md)
- [PCI-DSS en CSA-CCM documenten voor Azure Stack](https://servicetrust.microsoft.com/ViewPage/TrustDocuments)
- [Amerikaanse ministerie van defensie en NIST-documenten voor Azure Stack](https://servicetrust.microsoft.com/ViewPage/Blueprint)
