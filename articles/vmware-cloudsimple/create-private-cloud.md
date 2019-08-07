---
title: Azure VMware-oplossing maken op basis van CloudSimple-Privécloud
description: Hierin wordt beschreven hoe u een CloudSimple-Privécloud maakt om VMware-workloads uit te breiden naar de Cloud met operationele flexibiliteit en continuïteit
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 02a2bd311ea1e89a49eb12ef57a167a08eea5f98
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812248"
---
# <a name="create-a-cloudsimple-private-cloud"></a>Een CloudSimple-Privécloud maken

Het maken van een Privécloud helpt u bij het oplossen van een aantal gemeen schappelijke behoeften voor de netwerk infrastructuur:

* **Groei**. Als u een hardware-vernieuwings punt hebt bereikt voor uw bestaande infra structuur, kunt u een Privécloud uitbreiden zonder dat er nieuwe hardware-investering vereist is.

* **Snelle uitbrei ding**. Als er sprake is van tijdelijke of niet-geplande capaciteit, kunt u met een Privécloud de extra capaciteit zonder vertraging maken.

* **Betere beveiliging**. Met een Privécloud van drie of meer knoop punten krijgt u automatische redundantie en bescherming met hoge Beschik baarheid.

* **Vereisten voor de infra structuur op lange termijn**. Als uw data centers een capaciteit hebben of u zich wilt herstructureren om uw kosten te verlagen, kunt u met een Privécloud data centers buiten gebruik stellen en migreren naar een Cloud oplossing terwijl deze compatibel zijn met uw bedrijfs activiteiten.

Wanneer u een Privécloud maakt, krijgt u één vSphere-cluster en alle beheer-Vm's die in het cluster zijn gemaakt.

## <a name="before-you-begin"></a>Voordat u begint

Knoop punten moeten worden ingericht voordat u de Privécloud kunt maken.  Zie voor meer informatie over het inrichten van knoop punten [richt knooppunten inrichten voor VMware-oplossing door CloudSimple-Azure-](create-nodes.md) artikel.

Wijs een CIDR-bereik toe voor vSphere/vSAN-subnetten voor de Privécloud. Een Privécloud wordt gemaakt als een geïsoleerde VMware-stack (ESXi hosts, vCenter, vSAN en NSX) die worden beheerd door een vCenter-Server. Beheer onderdelen worden geïmplementeerd in het netwerk dat is geselecteerd voor vSphere/vSAN-subnets CIDR. Het netwerk-CIDR-bereik is tijdens de implementatie onderverdeeld in verschillende subnetten.  De adres ruimte van het vSphere/vSAN-subnet moet uniek zijn. Het mag niet overlappen met een netwerk dat communiceert met de CloudSimple-omgeving.  De netwerken die met CloudSimple communiceren, zijn onder andere on-premises netwerken en Azure Virtual Networks.  Zie voor meer informatie over vSphere-en vSAN-subnetten [vlan's en subnetten-overzicht](cloudsimple-vlans-subnets.md).

* Minimum aantal vSphere/vSAN-subnetten CIDR-bereik voor voegsel:/24 
* Maximum aantal vSphere/vSAN-subnetten voor voegsel van CIDR-bereik:/21

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Toegang tot de CloudSimple-portal

Toegang tot de [CloudSimple-Portal](access-cloudsimple-portal.md).

## <a name="create-a-new-private-cloud"></a>Een nieuwe Privécloud maken

1. Klik op de pagina **resources** op **nieuwe privécloud**.

    ![Een Privécloud maken-How to start](media/create-pc-button.png)

2. Selecteer de locatie waar u de persoonlijke cloud resources wilt hosten.

3. Kies het CS28-of CS36-knooppunt type you'ev ingericht voor de Privécloud. De laatste optie omvat de maximale reken-en geheugen capaciteit.

4. Selecteer het aantal knoop punten voor de Privécloud. U kunt Maxi maal het beschik bare aantal knoop punten selecteren dat you'ev [ingericht](create-nodes.md).

    ![Een persoonlijke Cloud-basis instellingen maken](media/create-private-cloud-basic-info.png)

5. Klik op **Next: Geavanceerde opties**.

6. Voer het CIDR-bereik in voor vSphere/vSAN-subnetten. Zorg ervoor dat het CIDR-bereik niet overlapt met een van uw on-premises of andere Azure-subnetten (virtuele netwerken) of met het gateway-subnet.  Gebruik geen CIDR-bereik dat is gedefinieerd in azure Virtual Networks.
    
    **Opties voor het CIDR-bereik:** /24,/23,/22 of/21. Een/24 CIDR-bereik ondersteunt Maxi maal negen knoop punten, een/23 CIDR-bereik ondersteunt Maxi maal 41 knoop punten en een/22-en/21 CIDR-bereik ondersteunt Maxi maal 64 knoop punten (het maximum aantal knoop punten in een Privécloud).

    > [!CAUTION]
    > IP-adressen in vSphere/vSAN CIDR-bereik zijn gereserveerd voor gebruik door een particuliere cloud infrastructuur.  Gebruik het IP-adres in dit bereik op geen enkele virtuele machine.

7. Klik op **Next: Controleren en maken**.

8. Controleer de instellingen. Als u instellingen wilt wijzigen, klikt u op **vorige**.

9. Klik op **Create**.

Het inrichten van de privécloud wordt gestart zodra u op maken klikt.  U kunt de voortgang van de pagina [taken](https://docs.azure.cloudsimple.com/activity/#tasks) in de CloudSimple-Portal bewaken.  Het inrichten kan 30 minuten tot twee uur duren.  U ontvangt een e-mail bericht zodra het inrichten is voltooid.

## <a name="next-steps"></a>Volgende stappen

* [Open de privécloud](expand-private-cloud.md)