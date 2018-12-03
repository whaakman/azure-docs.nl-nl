---
title: Wat is Toegewezen HSM? | Microsoft Docs
description: Azure Toegewezen HSM biedt mogelijkheden voor sleutelopslag binnen Azure die voldoen aan de FIPS 140-2 Niveau 3-certificatie
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 92d77ec886a0f37c28f5e3031a7e14f63299c8aa
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427111"
---
# <a name="what-is-dedicated-hsm"></a>Wat is Toegewezen HSM?

Azure Toegewezen HSM biedt cryptografische sleutelopslag in Azure die voldoet aan de strengste beveiligingsvereisten. Toegewezen HSM is de ideale oplossing voor klanten die apparaten willen gebruiken die zijn gevalideerd met FIPS 140-2 Niveau 3. De HSM-apparaten zijn wereldwijd geïmplementeerd in verschillende Azure-regio's en kunnen eenvoudig worden ingericht als een paar apparaten en worden geconfigureerd voor hoge beschikbaarheid. HSM's kunnen ook worden ingericht in verschillende regio's als oplossing voor een eventuele failover op regionaal niveau. Microsoft levert de Toegewezen HSM-service met behulp van het apparaat [SafeNet Luna Network HSM 7 (Model A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) van Gemalto. Dit apparaat biedt de hoogste prestatieniveaus en de beste opties voor cryptografische integratie. Als HSM's zijn ingericht, zijn ze rechtstreeks verbonden met het virtuele netwerk van een klant. Ze zijn dan ook toegankelijk voor on-premises hulpprogramma's voor toepassingen en beheer door een punt-naar-site-verbinding of site-naar-site-verbinding te configureren voor het VPN. Klanten krijgen in de Gemalto-ondersteuningsportal software en documentatie om de HSM-apparaten te configureren en te beheren.

## <a name="why-use-azure-dedicated-hsm"></a>Waarom een Azure Toegewezen HSM gebruiken?

### <a name="fips-140-2-level-3-compliance"></a>FIPS 140-2 Niveau 3-compliance

Veel organisaties hebben strenge brancheregels die voorschrijven dat cryptografische sleutelopslag voldoet aan de [FIPS 140-2 Niveau 3](https://csrc.nist.gov/publications/detail/fips/140/2/final)-vereisten. De Azure Key Vault-service met meerdere tenants van Microsoft biedt momenteel alleen ondersteuning voor FIPS 140-2 Niveau 2-certificatie. Azure Toegewezen HSM vervult de behoefte van de financiële branche, overheidsinstellingen, en anderen die moeten voldoen aan FIPS 140-2 Niveau 3-vereisten.

### <a name="single-tenant-devices"></a>Apparaten met een enkele tenant

Veel van onze klanten hebben behoefte aan een cryptografisch opslagapparaat met een enkele tenant. Met de Azure Toegewezen HSM-service kunt u een fysiek apparaat inrichten vanuit een van de wereldwijd gedistribueerde datacenters van Microsoft. Zodra dit apparaat is ingericht voor een klant, heeft alleen deze klant toegang tot het apparaat.

### <a name="full-administrative-control"></a>Volledig beheer

Naast apparaten met een enkele tenant hebben veel klanten ook behoefte aan volledig beheer, en toegang die exclusief is bedoeld voor beheer. Zodra dit is ingericht, heeft alleen deze klant op beheer- of toepassingsniveau toegang tot het apparaat. Als de klant de eerste keer toegang krijgt, moet het wachtwoord worden gewijzigd, waarna Microsoft geen beheertoegang meer heeft. Vanaf dit moment beschikt de klant over een enkele tenant met mogelijkheden voor volledig beheer en toepassingsbeheer. Microsoft behoudt wel toegang op bewakingsniveau (dit is geen beheerdersrol) voor telemetrie via een seriële poortverbinding, die geldt voor hardwaremonitors zoals temperatuur, en de status van de voeding en van de ventilator. Het staat de klant vrij om dit uit te schakelen, indien nodig. De klant ontvangt dan echter geen proactieve statusmeldingen meer van Microsoft.

### <a name="high-performance"></a>Hoge prestaties

Het Gemalto-apparaat is geselecteerd voor deze service vanwege de brede ondersteuning voor cryptografische algoritmes, de verscheidenheid aan ondersteunde besturingssystemen en de brede API-ondersteuning. Het specifieke model biedt uitstekende prestaties met 10.000 bewerkingen per seconde voor RSA-2048. Het biedt ondersteuning voor 10 partities die kunnen worden gebruikt voor unieke toepassingsexemplaren. Dit is een apparaat met een lage latentie, hoge capaciteit en hoge doorvoer.

### <a name="unique-cloud-based-offering"></a>Unieke cloudaanbieding

Microsoft heeft een specifieke behoefte herkend bij een unieke groep klanten en is de enige cloudprovider die nieuwe klanten een Toegewezen HSM-service biedt die is gevalideerd met FIPS 140-2 Niveau 3, en de enige die een dergelijke uitgebreide toepassingsintegratie biedt, zowel in de cloud als on-premises.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Is Azure Toegewezen HSM geschikt voor u?

Azure Toegewezen HSM is een gespecialiseerde service die zich richt op de unieke vereisten van een specifiek type grootschalige organisaties. Het gevolg hiervan is dat de meeste Azure-klanten daarom waarschijnlijk niet beantwoorden aan het profiel voor het gebruik van deze service. Velen zullen de Azure Key Vault-service passender en kosteneffectiever vinden. We hebben de volgende criteria geïdentificeerd om u te helpen te bepalen of u voldoet aan de vereisten.

### <a name="best-fit"></a>Geschikt voor

Meest geschikt voor lift-and-shift-scenario's waarvoor directe en exclusieve toegang tot HSM-apparaten is vereist. Voorbeelden zijn:

- Toepassingen migreren van on-premises naar Azure Virtual Machines
- Het migreren van toepassingen vanuit Amazon AWS EC2 naar Azure Virtual Machines die gebruikmaken van de service AWS Cloud HSM Classic (Amazon biedt deze aanbieding niet aan nieuwe klanten)
- Het uitvoeren van shrink-wrapped software in Azure Virtual Machines, zoals Apache/Ngnix SSL Offload, Oracle TDE en ADCS

### <a name="not-a-fit"></a>Niet geschikt voor

Microsoft-cloudservices die ondersteuning bieden voor versleuteling met door de klant beheerde sleutels (zoals Azure Information Protection, Azure Disk Encryption, Azure Data Lake Store, Azure Storage, Azure SQL, Office 365 Customer Key), zijn niet geïntegreerd met Azure Toegewezen HSM.

### <a name="it-depends"></a>Geschikt onder bepaalde omstandigheden

In veel scenario's is de geschiktheid afhankelijk van een complexe mengeling van vereisten en van de compromissen die wel of niet kunnen worden gemaakt. Een voorbeeld is de FIPS 140-2 Niveau 3-vereiste, die vaak verplicht is, en daarom is Toegewezen HSM momenteel de enige optie.  Als deze verplichte vereisten niet relevant zijn, is de keuze tussen Azure Key Vault en Toegewezen HSM gebaseerd op het beoordelen van een mengeling van vereisten. Voorbeelden zijn:

- Nieuwe code die wordt uitgevoerd op de virtuele Azure-machine van een klant
- SQL Server TDE in een virtuele Azure-machine
- Azure Storage-versleuteling aan de clientzijde
- SQL Server en Azure SQL DB Always Encrypted

## <a name="next-steps"></a>Volgende stappen

Gezien de zeer gespecialiseerde aard van deze service wordt aangeraden om een goed begrip te hebben van enkele van de belangrijkste concepten in deze documentatieset, en om volledig op de hoogte te zijn van de prijzen, ondersteuning en serviceovereenkomsten. Vervolgens is er een zelfstudie beschikbaar om het inrichten van de HSM's in een bestaand virtueel netwerk te vergemakkelijken. [Gemalto-integratiehandleidingen](https://safenet.gemalto.com/partners/microsoft/) en instructiegidsen voor het bepalen van de implementatiearchitectuur vormen ook een geweldige resource.

* [Hoge beschikbaarheid](high-availability.md)
* [Fysieke beveiliging](physical-security.md)
* [Netwerken](networking.md)
* [Ondersteuning](supportability.md)
* [Controle](monitoring.md)
