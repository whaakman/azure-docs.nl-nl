---
title: Beveiliging voor CloudSimple-Services
description: Beschrijft de modellen van gedeelde verantwoordelijkheden voor de beveiliging van CloudSimple-Services
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/27/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ff1bd3c6e1f3cf98e92e10eecf972681ed6c7819
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816174"
---
# <a name="cloudsimple-security-overview"></a>Overzicht van CloudSimple-beveiliging

Dit artikel bevat een overzicht van de manier waarop beveiliging wordt geïmplementeerd op de Azure VMware-oplossing door CloudSimple service, infra structuur en Data Center. Meer informatie over gegevens beveiliging en beveiliging, netwerk beveiliging en hoe beveiligings problemen en patches worden beheerd.

## <a name="shared-responsibility"></a>Gedeelde verantwoordelijkheid

Azure VMware-oplossing per CloudSimple gebruikt een gedeeld verantwoordelijkheids model voor beveiliging. Vertrouwde beveiliging in de Cloud wordt bereikt door de gedeelde verantwoordelijkheden van klanten en micro soft als een service provider. Deze matrix van verantwoordelijkheid biedt betere beveiliging en elimineert individuele storings punten.

## <a name="azure-infrastructure"></a>Azure-infra structuur 

Beveiligings overwegingen voor Azure-infra structuur omvatten de data centers en de locatie van het apparaat.

### <a name="datacenter-security"></a>Beveiliging van data centers 

Micro soft heeft een volledige afdeling voor het ontwerpen, bouwen en gebruiken van de fysieke faciliteiten die ondersteuning bieden voor Azure. Dit team is geïnvesteerd in het onderhouden van geavanceerde fysieke beveiliging. Zie [Azure faciliteiten, premises en fysieke beveiliging] (https://docs.microsoft.com/azure/security/fundamentals/physical-security ) voor meer informatie over fysieke beveiliging.

### <a name="equipment-location"></a>Locatie van apparatuur

De bare-metal hardware die uw persoonlijke Clouds uitvoert, wordt gehost op Azure Data Center-locaties. Op basis van biometrische twee ledige verificatie is vereist om toegang te krijgen tot de kooien waar het apparaat zich bevindt.

## <a name="dedicated-hardware"></a>Toegewezen hardware

Als onderdeel van de CloudSimple-service krijgen alle CloudSimple-klanten toegewezen bare-metal hosts met lokale gekoppelde schijven die fysiek zijn geïsoleerd van andere Tenant-hardware. Een ESXi-Hyper Visor met vSAN wordt uitgevoerd op elk knoop punt. De knoop punten worden beheerd via een door de klant toegewezen VMware, vCenter en NSX. Het delen van hardware tussen tenants biedt geen extra laag voor isolatie en beveiliging.

## <a name="data-security"></a>Gegevensbeveiliging

Klanten houden toezicht en eigendom van hun gegevens. Gegevens opslag van klant gegevens is de verantwoordelijkheid van de klant.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Gegevens beveiliging voor gegevens in rust tijd en gegevens in beweging in interne netwerken

Voor Data-at-rest in de privécloud-omgeving kunt u vSAN-versleuteling gebruiken. vSAN-versleuteling werkt met met VMware gecertificeerde externe sleutels voor sleutel beheer servers (KMS) in uw eigen virtuele netwerk of on-premises. U beheert de gegevens versleutelings sleutels zelf. Voor gegevens in beweging in de privécloud ondersteunt vSphere versleuteling van gegevens via de kabel voor al het VMkernel-verkeer, waaronder vMotion-verkeer.

### <a name="data-protection-for-data-thats-required-to-move-through-public-networks"></a>Gegevens beveiliging voor gegevens die nodig zijn om te scha kelen via open bare netwerken

Voor het beveiligen van gegevens die via open bare netwerken worden verplaatst, kunt u IPsec-en SSL-VPN-tunnels maken voor uw privécloud. Algemene versleutelings methoden worden ondersteund, met inbegrip van 128-byte en 256-bytes AES. Gegevens in de overdracht, waaronder verificatie, beheerders toegang en klant gegevens, zijn versleuteld met standaard versleutelings mechanismen, zoals SSH, TLS 1,2 en Secure RDP. Communicatie waarbij gevoelige informatie wordt getransporteerd, maakt gebruik van de standaard versleutelings mechanismen.

### <a name="secure-disposal"></a>Beveiligde verwijdering 

Als uw CloudSimple-service verloopt of wordt beëindigd, bent u verantwoordelijk voor het verwijderen of verwijderen van uw gegevens. CloudSimple samenwerkt met u om alle klant gegevens te verwijderen of te retour neren zoals die zijn opgegeven in de klant overeenkomst, met uitzonde ring van de mate waarin CloudSimple is vereist door toepasselijk recht om enkele of alle persoons gegevens te bewaren. Als dit nodig is om persoonlijke gegevens te bewaren, archiveert CloudSimple de gegevens en implementeert hij redelijke maat regelen om te voor komen dat de klant gegevens worden verwerkt.

### <a name="data-location"></a>Locatie van gegevens

Wanneer u uw persoonlijke Clouds instelt, kiest u de Azure-regio waar ze zijn geïmplementeerd. Virtuele VMware-machine gegevens worden niet verplaatst van dat fysieke Data Center tenzij u gegevens migratie of externe gegevens back-up uitvoert. U kunt werk belastingen ook hosten en gegevens opslaan in meerdere Azure-regio's, indien van toepassing op uw behoeften.

De klant gegevens die zich in de privécloud hypergeconvergeerd-knoop punten bevinden, passeren geen locaties zonder de expliciete actie van de Tenant beheerder. Het is uw verantwoordelijkheid om uw workloads op een Maxi maal beschik bare manier te implementeren.

### <a name="data-backups"></a>Gegevens back-ups
CloudSimple maakt geen back-up van en archiveert geen klant gegevens. CloudSimple voert periodieke back-ups van vCenter-en NSX-gegevens uit om hoge Beschik baarheid van beheerser vers te bieden. Voordat de back-up wordt gemaakt, worden alle gegevens versleuteld op de vCenter-bron met behulp van VMware-Api's. De versleutelde gegevens worden getransporteerd en opgeslagen in een Azure-Blob. Versleutelings sleutels voor back-ups worden opgeslagen in een zeer veilige CloudSimple beheerde kluis die wordt uitgevoerd in het virtuele CloudSimple-netwerk in Azure.

## <a name="network-security"></a>Netwerkbeveiliging

De CloudSimple-oplossing is afhankelijk van lagen van netwerk beveiliging.

### <a name="azure-edge-security"></a>Beveiliging van Azure Edge

De CloudSimple-Services zijn gebaseerd op de basis netwerk beveiliging die wordt geboden door Azure. Azure past ingrijpende technieken toe voor detectie en tijdig antwoord op aanvallen op basis van het netwerk die zijn gekoppeld aan afwijkende ingangen of uitvoerige verkeers patronen en gedistribueerde Denial-of-service-aanvallen (DDoS). Dit beveiligings beheer is van toepassing op de omgevingen van de privécloud en de software voor het beheer vlak die is ontwikkeld door CloudSimple.

### <a name="segmentation"></a>Segmentatie

De CloudSimple-service heeft logische afzonderlijke laag 2-netwerken die de toegang tot uw eigen particuliere netwerken in uw privécloud-omgeving beperken. U kunt uw particuliere Cloud netwerken verder beveiligen door gebruik te maken van een firewall. In de CloudSimple-Portal definieert u de regels voor Oost-West en Noordoost-netwerk verkeer voor al het netwerk verkeer, waaronder intra-Private Cloud verkeer, interprivate Cloud verkeer, algemeen verkeer naar Internet en netwerk verkeer naar on-premises via IPsec VPN of Azure ExpressRoute-verbinding.

## <a name="vulnerability-and-patch-management"></a>Beveiligings problemen en patch beheer 

CloudSimple is verantwoordelijk voor periodieke beveiligings patches van beheerde VMware-software, zoals ESXi, vCenter en NSX.

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

Klanten kunnen zich verifiëren met hun Azure-account (in Azure Active Directory) door multi-factor Authentication of SSO als voor keur te gebruiken. Vanuit het Azure Portal kunt u de CloudSimple-Portal starten zonder referenties opnieuw in te voeren.

CloudSimple ondersteunt een optionele configuratie van een identiteits bron voor de privécloud. U kunt een [on-premises identiteits bron](https://docs.azure.cloudsimple.com/set-vcenter-identity), een nieuwe identiteits bron voor de privécloud of [Azure Active Directory](https://docs.azure.cloudsimple.com/azure-ad)gebruiken.

Klanten krijgen standaard de bevoegdheden die nodig zijn voor de dagelijkse bewerkingen van vCenter in de privécloud. Dit machtigings niveau bevat geen beheerders toegang tot vCenter. Als beheerders toegang tijdelijk vereist is, kunt u [uw bevoegdheden](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) voor een beperkte periode escaleren tijdens het volt ooien van de beheer taken.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [maken van een CloudSimple-service in azure](quickstart-create-cloudsimple-service.md).
* Meer informatie over het [maken van een privécloud](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Meer informatie over het [configureren van een privécloud](quickstart-create-private-cloud.md).
