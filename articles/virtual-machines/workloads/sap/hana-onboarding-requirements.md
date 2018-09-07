---
title: Onboarding-vereisten voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Onboarding-vereisten voor SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d287f79f4161c509f96b679d4b794c2906f2e020
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028092"
---
# <a name="onboarding-requirements"></a>Voorbereidingsvereisten

Deze lijst ophaalprotocol vereisten voor het uitvoeren van SAP HANA op Azure (grotere instanties).

**Microsoft Azure**

- Een Azure-abonnement dat kan worden gekoppeld aan SAP HANA op Azure (grote instanties).
- Microsoft Premier support-contract. Zie voor specifieke informatie met betrekking tot het uitvoeren van SAP in Azure, [SAP Support Opmerking #2015553 – SAP op Microsoft Azure: vereisten voor ondersteuning](https://launchpad.support.sap.com/#/notes/2015553). Als u HANA grote instantie eenheden met 384 en meer CPU's gebruikt, moet u ook om uit te breiden de Premier-ondersteuningscontract om op te nemen van Azure Rapid Response.
- Status van de HANA grote instantie SKU's u moet na het uitvoeren van een oefening schaling met SAP.

**Verbinding met het netwerk**

- ExpressRoute tussen on-premises naar Azure: als u wilt verbinding maken met uw on-premises Datacenter naar Azure, zorg ervoor dat u de volgorde van ten minste 1 Gbps-verbinding via uw Internetprovider. 

**Besturingssysteem**

- Licenties voor SUSE Linux Enterprise Server 12 voor SAP-toepassingen.

   > [!NOTE] 
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd bij SUSE. Het is niet verbonden met een exemplaar van het beheerprogramma voor abonnement.

- SUSE Linux abonnement Management Tool geïmplementeerd in Azure op een virtuele machine. Dit hulpprogramma biedt de mogelijkheid voor SAP HANA op Azure (grote instanties) worden geregistreerd en respectievelijk bijgewerkt door SUSE. (Er is geen toegang tot internet binnen het datacenter HANA grote instantie.) 
- Licenties voor Red Hat Enterprise Linux 6.7 of 7.x voor SAP HANA.

   > [!NOTE]
   > Het besturingssysteem die is geleverd door Microsoft is niet geregistreerd met Red Hat. Het is niet verbonden met een Doelabonnementbeheerder van Red Hat-exemplaar.

- Red Hat abonnement Manager is geïmplementeerd in Azure op een virtuele machine. De Red Hat abonnement Manager biedt de mogelijkheid voor SAP HANA op Azure (grote instanties) worden geregistreerd en respectievelijk bijgewerkt door Red Hat. (Er is geen directe toegang tot internet vanaf binnen de tenant die is geïmplementeerd op de Azure Large Instance-stempel.)
- SAP vereist dat u hebt een ondersteuning contract bij uw Linux-provider. Deze vereiste wordt niet verwijderd door de oplossing van HANA grote instantie of het feit dat u Linux in Azure worden uitgevoerd. In tegenstelling tot met enkele van de Linux Azure-galerie met installatiekopieën, de kosten van de service is *niet* deel uitmaakt van de aanbieding van de oplossing van HANA grote instantie. Het is uw verantwoordelijkheid om te voldoen aan de vereisten van SAP met betrekking tot support-contract met de distributor Linux. 
   - SUSE Linux, zoekt u de vereisten van support-contract in [SAP Opmerking #1984787 - SUSE Linux Enterprise Server 12: opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787) en [SAP Opmerking #1056161 - SUSE prioriteitsondersteuning voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/1056161).
   - Voor Red Hat Linux moet u het juiste abonnement niveaus die zijn ondersteuning en service-updates voor de besturingssystemen van HANA grote instantie. Red Hat raadt aan om het abonnement op Red Hat Enterprise Linux voor SAP-oplossing. Raadpleeg https://access.redhat.com/solutions/3082481. 

Zie voor de ondersteuningsmatrix van de verschillende versies van de SAP HANA met de verschillende versies van Linux, [SAP Opmerking #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Raadpleeg voor de compatibiliteitsmatrix van het besturingssysteem en firmware-/ stuurprogrammaversies HLI [bijwerken van het besturingssysteem voor HLI](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Voor Type II eenheden alleen de SLES 12 SP2 OS-versie op dit moment ondersteund. 


**Database**

- De licenties en software-installatie van onderdelen voor SAP HANA (platform- of enterprise edition).

**Toepassingen**

- Licenties en software-installatie van onderdelen voor SAP-toepassingen die verbinding met SAP HANA en gerelateerde SAP maken ondersteuning voor opdrachten.
- Licenties en software-installatie van onderdelen voor niet-SAP-toepassingen gebruikt met betrekking tot SAP HANA op Azure (grote instanties)-omgevingen en ondersteuningscontracten die betrekking hebben.

**Vaardigheden**

- Ervaring met en kennis van Azure IaaS en de bijbehorende onderdelen.
- Ervaring met en kennis van hoe u een SAP-workloads in Azure implementeert.
- Installatie van SAP HANA-gecertificeerde personeel.
- SAP-architect vaardigheden voor het ontwerpen van hoge beschikbaarheid en herstel na noodgevallen om SAP HANA.

**SAP**

- Wordt ervan uitgegaan dat u een SAP-klant bent en een hebben contract met SAP.
- Met name voor implementaties van het Type II-klasse van HANA grote instantie SKU's, neem contact op met SAP op versies van SAP HANA en de uiteindelijke configuraties op grote schaal omhoog hardware.

**Volgende stappen**
- Raadpleeg [SAP HANA (grote instanties)-architectuur op Azure](hana-architecture.md)