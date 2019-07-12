---
title: Beveiliging voor CloudSimple services
description: Beschrijving van de gedeelde verantwoordelijkheid modellen voor beveiliging van de CloudSimple services
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/27/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 325915aae43c905236910382f650730a6daa127a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595328"
---
# <a name="cloudsimple-security-overview"></a>Overzicht van de beveiliging van CloudSimple

Dit artikel bevat een overzicht van hoe beveiliging op de VMware-oplossing van Azure is geïmplementeerd door de service, de infrastructuur en het datacenter CloudSimple. U meer informatie over de beveiliging van gegevens en beveiliging, netwerkbeveiliging en hoe beveiligingsproblemen en patches worden beheerd.

## <a name="shared-responsibility"></a>Gedeelde verantwoordelijkheid

Azure VMware-oplossing door CloudSimple maakt gebruik van een model met gedeelde verantwoordelijkheid voor beveiliging. Vertrouwde beveiliging in de cloud wordt bereikt door middel van de gedeelde verantwoordelijkheden van klanten en Microsoft als een serviceprovider. Deze matrix verantwoordelijkheid biedt betere beveiliging en elimineert single point of failure.

## <a name="azure-infrastructure"></a>Azure-infrastructuur 

Beveiligingsoverwegingen voor Azure-infrastructuur bevatten de locatie van de datacenters en apparatuur.

### <a name="datacenter-security"></a>Datacenterbeveiliging 

Microsoft heeft een gehele afdeling besteed aan het ontwerpen, bouwen en de fysieke faciliteiten die ondersteuning bieden voor Azure. Dit team wordt geïnvesteerd in het onderhouden van fysieke beveiliging van status-of-the-art. Zie voor meer informatie over de fysieke beveiliging [Azure faciliteiten, lokale en fysieke beveiliging](https://docs.microsoft.com/azure/security/azure-physical-security).

### <a name="equipment-location"></a>Locatie van de apparatuur

De bare-metal hardware-apparatuur die wordt uitgevoerd van uw privéclouds wordt gehost in Azure datacenter-locaties. Op basis van biometrische verificatie met twee factoren is vereist voor toegang tot de kooien waarin die apparatuur zich bevindt.

## <a name="dedicated-hardware"></a>Toegewezen hardware

Als onderdeel van de service CloudSimple krijgen alle CloudSimple klanten specifieke hosts van bare-metal lokaal gekoppelde schijven die fysiek geïsoleerd van de hardware van andere tenants zijn. Een ESXi-hypervisor met een virtueel SAN wordt uitgevoerd op elk knooppunt. De knooppunten worden beheerd via de klant toegewezen VMware vCenter en NSX. Geen hardware tussen tenants delen biedt een extra laag van isolatie en beveiliging.

## <a name="data-security"></a>Gegevensbeveiliging

Klanten behouden de controle en het eigendom van hun gegevens. Gegevens kas klantgegevens is de verantwoordelijkheid van de klant.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Gegevensbeveiliging voor gegevens in rust en gegevens in beweging binnen de interne netwerken

Voor gegevens in rust in de privécloud-omgeving, kunt u vSAN-versleuteling. virtueel SAN versleuteling werkt met externe Sleutelbeheer VMware-gecertificeerde-servers (KMS) in uw eigen on-premises of virtueel netwerk. U bepaalt de versleutelingssleutels voor de gegevens zelf. Voor gegevens in beweging binnen de privécloud ondersteunt vSphere-versleuteling van gegevens via de kabel voor alle VMkernel verkeer, waaronder vMotion-verkeer.

### <a name="data-protection-for-data-thats-required-to-move-through-public-networks"></a>Gegevensbeveiliging voor gegevens die vereist zijn om te gaan via openbare netwerken

Ter bescherming van gegevens die worden verplaatst via openbare netwerken, kunt u IPsec en SSL VPN-tunnels voor uw persoonlijke clouds. Algemene versleutelingsmethoden worden ondersteund, met inbegrip van AES 128-bytes en 256 bytes. Gegevens die onderweg zijn, waaronder verificatie, toegang op beheerdersniveau en gegevens van de klant, is versleuteld met standard versleutelingsmechanismen, zoals SSH, TLS 1.2 en veilige RDP. Communicatie die gevoelige informatie transporten maakt gebruik van de standaard codering mechanismen.

### <a name="secure-disposal"></a>Beveiligen van verwijdering 

Als uw service CloudSimple is verlopen of is beëindigd, bent u verantwoordelijk voor het verwijderen of het verwijderen van uw gegevens. CloudSimple werkt samen met u te verwijderen of te retourneren van alle gegevens van de klant, zoals beschreven in de KLANTOVEREENKOMST, behalve voor zover CloudSimple is vereist door de toepasselijke wetgeving sommige of alle van de persoonlijke gegevens behouden. Indien nodig een persoonlijke gegevens wilt behouden, worden de CloudSimple archieven van de gegevens en implementeert redelijke maatregelen om te voorkomen dat de gegevens van de klant verdere verwerking.

### <a name="data-location"></a>Locatie van gegevens

Bij het instellen van uw privéclouds, kiest u de Azure-regio waar deze zijn geïmplementeerd. Gegevens van virtuele machines van VMware niet verplaatst van dit fysieke datacenter, tenzij u de gegevensmigratie of externe gegevens back-ups uitvoeren. U kunt ook werkbelastingen hosten en opslaan van gegevens in meerdere Azure-regio's, indien van toepassing zijn voor uw behoeften.

De gegevens van de klant die in de privécloud hypergeconvergeerd knooppunten woont passeren niet locaties zonder de expliciete actie van de tenantbeheerder. Het is uw verantwoordelijkheid voor het implementeren van uw workloads in een maximaal beschikbare manier.

### <a name="data-backups"></a>Gegevensback-ups
CloudSimple geen back-up maken of archiveren van gegevens van de klant. CloudSimple voert periodieke back-up van de vCenter- en NSX gegevens voor een hoge beschikbaarheid van beheerservers. Voordat u back-up, alle gegevens versleuteld op de vCenter-bron met VMware APIs. De versleutelde gegevens worden getransporteerd en opgeslagen in een Azure-blob. Versleutelingssleutels voor back-ups worden opgeslagen in een zeer veilige CloudSimple beheerde-kluis die wordt uitgevoerd in het virtuele netwerk CloudSimple in Azure.

## <a name="network-security"></a>Netwerkbeveiliging

De oplossing CloudSimple, is afhankelijk van lagen van netwerkbeveiliging.

### <a name="azure-edge-security"></a>Beveiliging van Azure edge

De services CloudSimple zijn gebaseerd op de netwerkbeveiliging van de basis van Azure. Azure is van toepassing in ingrijpende technieken voor detectie en tijdige reactie op netwerk gebaseerde aanvallen die zijn gekoppeld aan de afwijkende inkomend of uitgaand verkeerspatronen en gedistribueerde denial-of-service (DDoS)-aanvallen. Deze beveiligingscontrole geldt voor privécloudomgevingen en de software van het besturingselement vlak ontwikkeld door CloudSimple.

### <a name="segmentation"></a>Segmentering

De CloudSimple-service heeft een logische manier apart Layer 2-netwerken die toegang tot uw eigen particuliere netwerken in uw privécloud-omgeving beperken. Verder kunt u uw persoonlijke cloudnetwerken beveiligen met behulp van een firewall. In de portal CloudSimple definieert u Oost-west en Noord-Zuid-besturingselement regels voor netwerkverkeer voor al het netwerkverkeer, waaronder intra privécloud verkeer, verkeer tussen privécloud, algemene verkeer naar Internet en netwerkverkeer naar on-premises via IPsec VPN of Azure ExpressRoute-verbinding.

## <a name="vulnerability-and-patch-management"></a>Door een beveiligingslek en patch management 

CloudSimple is verantwoordelijk voor het periodiek beveiligingspatches van beheerde VMware-software, zoals ESXi en vCenter NSX.

## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

Klanten kunnen worden geverifieerd bij hun Azure-account (in Azure Active Directory) met behulp van multi-factor authentication of eenmalige aanmelding als voorkeur. Vanuit de Azure-portal, kunt u de portal CloudSimple zonder referenties opnieuw te starten.

CloudSimple biedt ondersteuning voor optionele configuratie van een bron-id voor de vCenter-privécloud. U kunt een [on-premises identiteitsbron](https://docs.azure.cloudsimple.com/set-vcenter-identity), een nieuwe identiteitsbron voor de privécloud of [Azure Active Directory](https://docs.azure.cloudsimple.com/azure-ad).

Standaard krijgen klanten de bevoegdheden die nodig voor de alledaagse bewerkingen van vCenter binnen de privécloud zijn. Dit machtigingsniveau bevat geen beheerderstoegang tot vCenter. Als met beheerdersrechten toegang tijdelijk vereist is, kunt u [escaleren van uw bevoegdheden](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) gedurende een beperkte periode tijdens het voltooien van de administratieve taken.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een CloudSimple-service in Azure maken](quickstart-create-cloudsimple-service.md).
* Meer informatie over het [maken van een privécloud](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Meer informatie over het [configureren van een persoonlijke cloudomgeving](quickstart-create-private-cloud.md).
