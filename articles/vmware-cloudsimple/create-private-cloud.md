---
title: Azure VMware-oplossing door CloudSimple Private Cloud maken
description: Beschrijft het maken van een Privécloud van CloudSimple om uit te breiden van VMware-workloads naar de cloud met de operationele flexibiliteit en bedrijfscontinuïteit
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e5c03c1d8a865b792ce79e3e2b576a629b71e02c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333030"
---
# <a name="create-a-cloudsimple-private-cloud"></a>Een Privécloud CloudSimple maken

Het maken van een Private Cloud, kunt u een veelheid aan algemene behoeften van de netwerkinfrastructuur:

* **Groei**. Als u een hardware vernieuwen punt voor uw bestaande infrastructuur bereikt hebt, wordt er een Private Cloud kunt u om uit te breiden met geen nieuwe hardware-investeringen vereist.

* **Snelle uitbreiding**. Als een tijdelijke of niet-geplande capaciteit optreden moet, wordt er een Private Cloud kunt u de capaciteit zonder vertraging maken.

* **Beveiliging verhoogd**. Met een Privécloud met drie of meer knooppunten krijgt u automatische redundantie en hoge beschikbaarheid-beveiliging.

* **Op de lange termijn infrastructuur moet**. Als uw datacenters op capaciteit of u wilt om te herstructureren van uw kosten lager, wordt er een Private Cloud kunt u buiten gebruik stellen datacenters en migreren naar een cloud-gebaseerde oplossing en tegelijkertijd de kosten compatibel is met de activiteiten van uw onderneming.

Als u een Privécloud maakt, krijgt u een cluster met één vSphere en alle de beheer-VM's die zijn gemaakt in het cluster.

## <a name="before-you-begin"></a>Voordat u begint

Knooppunten moeten worden ingericht voordat u uw Privécloud kunt maken.  Zie voor meer informatie over het inrichten van knooppunten [knooppunten in te richten voor VMware-oplossing door CloudSimple - Azure](create-nodes.md) artikel.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Toegang tot de CloudSimple-portal

Toegang tot de [CloudSimple portal](access-cloudsimple-portal.md).

## <a name="create-a-new-private-cloud"></a>Een nieuwe Private Cloud maken

1. Op de **Resources** pagina, klikt u op **nieuwe Privécloud**.

    ![Maken van een Private Cloud - starten](media/create-pc-button.png)

2. Selecteer de locatie voor het hosten van de Privécloud-resources.

3. Kies de CS28 of CS36 knooppunt type you'ev ingericht voor de Privécloud. De laatste optie bevat de maximale capaciteit voor Computing en geheugen.

4. Selecteer het aantal knooppunten voor de Privécloud. U kunt maximaal selecteren het beschikbare aantal knooppunten dat you'ev [ingericht](create-nodes.md).

    ![Maken van een Private Cloud - basisinstellingen](media/create-private-cloud-basic-info.png)

5. Klik op **Next: Geavanceerde opties**.

6. Voer de CIDR-bereik voor vSphere/vSAN subnetten. Zorg ervoor dat de CIDR-bereik niet met een van uw on-premises of andere Azure-subnetten (virtuele netwerken) of met het gatewaysubnet overlappen.  Gebruik niet een CIDR-bereik dat is gedefinieerd in Azure virtual networks.
    
    **Opties voor CIDR-bereik:** /24, /23, /22 of /21. Een/24 CIDR-bereik ondersteunt maximaal negen knooppunten, een /23 CIDR-bereik ondersteunt maximaal 41 knooppunten, een /22 en /21 CIDR-bereik ondersteunt maximaal 64 knooppunten (het maximum aantal knooppunten in een Privécloud).

    > [!CAUTION]
    > IP-adressen in de CIDR-bereik vSphere/virtueel SAN is gereserveerd voor gebruik door Private Cloud-infrastructuur.  Gebruik het IP-adres niet in dit bereik op een virtuele machine.

7. Klik op **Next: Controleren en maken**.

8. Controleer de instellingen. Als u geen instellingen te wijzigen, klikt u op **vorige**.

9. Klik op **Create**.

Private Cloud inrichten wordt gestart nadat u op maken.  U kunt de voortgang van [taken](https://docs.azure.cloudsimple.com/activity/#tasks) pagina op CloudSimple-portal.  Inrichting kan 30 minuten tot twee uur duren.  U ontvangt een e-mail wanneer het inrichten voltooid is.

## <a name="next-steps"></a>Volgende stappen

* [Vouw privécloud](expand-private-cloud.md)