---
title: Portal prep voor StorSimple Virtual Array | Microsoft Docs
description: Eerste zelfstudie voor het implementeren van de StorSimple virtual array omvat het voorbereiden van de Azure-portal
services: storsimple
documentationcenter: NA
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/11/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c6f0a6371b38f0271237db0f7d80b831ecc145c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58088703"
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>StorSimple Virtual Array implementeren - Azure portal voorbereiden

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Overzicht

Dit is het eerste artikel in de reeks van zelfstudies voor implementatie vereist voor het implementeren van uw virtuele array volledig als een bestandsserver of een iSCSI-server met behulp van de Resource Manager-model. Dit artikel beschrijft de voorbereiding is vereist voor het maken en configureren uw StorSimple Device Manager-service voordat u een virtuele matrix inrichten. In dit artikel bevat ook koppelingen om naar een Configuratiecontrolelijst voor implementatie en configuratie van vereisten.

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het is raadzaam om de configuratiecontrolelijst voor implementatie te controleren voordat u begint. Het voorbereiden van de portal duurt minder dan 10 minuten.

De informatie die is gepubliceerd in dit artikel is van toepassing op de implementatie van virtuele StorSimple-matrices in Azure portal en Microsoft Azure Government-Cloud.

### <a name="get-started"></a>Aan de slag
De implementatiewerkstroom bestaat uit het voorbereiden van de portal, het inrichten van een virtuele matrix in uw gevirtualiseerde omgeving en de setup is voltooid. Om te beginnen met de StorSimple Virtual Array implementatie als een bestandsserver of een iSCSI-server, moet u verwijzen naar de volgende gebruikmaking bronnen.

#### <a name="deployment-articles"></a>Artikelen over implementatie

Raadpleeg de volgende artikelen in de voorgeschreven volgorde voor het implementeren van uw StorSimple Virtual Array.

| **#** | **In deze stap** | **U doet dit...** | **En gebruik deze documenten.** |
| --- | --- | --- | --- |
| 1. |**Instellen van de Azure-portal** |Maken en configureren uw StorSimple Device Manager-service voordat u een StorSimple Virtual Array wordt ingericht. |[De portal voorbereiden](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**De virtuele matrix inrichten** |Voor Hyper-V, inrichten en maak verbinding met een StorSimple Virtual Array op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. <br></br> <br></br> Voor VMware, inrichten en maak verbinding met een StorSimple Virtual Array op een hostsysteem met VMware ESXi 5.0, 5.5, 6.0 of 6.5.<br></br> |[Een virtuele matrix in Hyper-V inrichten](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Een virtuele matrix in VMware inrichten](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Instellen van de virtuele matrix** |Voor uw bestandsserver uitvoeren van de eerste configuratie, registreren van uw StorSimple-bestandsserver en de apparaatconfiguratie voltooien. Vervolgens kunt u SMB-shares inrichten. <br></br> <br></br> Voor de iSCSI-server uitvoeren van de eerste configuratie, uw StorSimple-iSCSI-server te registreren en de apparaatconfiguratie voltooien. Vervolgens kunt u iSCSI-volumes inrichten. |[Virtuele matrix instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Virtuele matrix als iSCSI-server instellen](storsimple-virtual-array-deploy3-iscsi-setup.md) |

U kunt nu Azure Portal gaan instellen.

## <a name="configuration-checklist"></a>Configuratiecontrolelijst voor

De Configuratiecontrolelijst bevat de informatie die u verzamelen moet voordat u de software op uw StorSimple Virtual Array configureren. Deze informatie tevoren voorbereid helpt bij het stroomlijnen van de implementatie van het StorSimple-apparaat in uw omgeving. Afhankelijk van of uw StorSimple Virtual Array wordt geïmplementeerd als een bestandsserver of een iSCSI-server, moet u een van de volgende controlelijsten.

* Download de [Configuratiecontrolelijst voor StorSimple Virtual Array bestand Server](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Download de [StorSimple Virtual Array iSCSI-Server Configuratiecontrolelijst](https://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Vereisten

Hier vindt u de configuratievereisten voor uw StorSimple Device Manager-service, de StorSimple Virtual Array en het netwerk van datacenters.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie

Zorg voordat u begint voor het volgende:

* U hebt een Microsoft-account met toegangsreferenties.
* U hebt een Microsoft Azure Storage-account met toegangsreferenties.
* Uw Microsoft Azure-abonnement moet worden ingeschakeld voor StorSimple Device Manager-service.

### <a name="for-the-storsimple-virtual-array"></a>Voor de StorSimple Virtual Array

Voordat u een virtuele matrix implementeert, controleert u of:

* U hebt toegang tot een hostsysteem met Hyper-V op Windows Server 2008 R2 of later of VMware (ESXi 5.0, 5.5, 6.0 of 6.5) die kan worden gebruikt voor het inrichten van een een-apparaat.
* Het hostsysteem kan besteden aan de volgende bronnen voor het inrichten van uw virtuele matrix:
  
  * Minimaal 4 kerngeheugens.
  * Ten minste 8 GB RAM-geheugen. Als u van plan bent de virtuele matrix configureren als bestandsserver, ondersteunt 8 GB 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Eén netwerkinterface.
  * Een 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor datacenternetwerk

Zorg voordat u begint voor het volgende:

* Het netwerk in uw datacenter is geconfigureerd volgens de netwerkvereisten voor uw StorSimple-apparaat. Zie voor meer informatie de [StorSimple Virtual Array System Requirements](storsimple-ova-system-requirements.md).
* Uw StorSimple Virtual Array is een speciale 5 Mbps internetbandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld met andere toepassingen.

## <a name="step-by-step-preparation"></a>Stapsgewijze voorbereiden

Gebruik de volgende stapsgewijze instructies om voor te bereiden uw portal voor de StorSimple Device Manager-service.

## <a name="step-1-create-a-new-service"></a>Stap 1: Een nieuwe service maken

Één exemplaar van de service StorSimple Device Manager kunt u meerdere virtuele StorSimple-matrices beheren. Voer de volgende stappen uit om een exemplaar van de StorSimple-apparaatbeheerfunctie uit te voeren. Hebt u een bestaande service StorSimple Device Manager voor het beheren van uw virtuele matrices, sla deze stap over en gaat u naar [stap 2: De serviceregistratiesleutel ophalen](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Als u de service niet hebt ingeschakeld om automatisch een opslagaccount te maken, moet u minimaal één opslagaccount maken nadat u een service hebt gemaakt.
> 
> * Als u niet automatisch een opslagaccount hebt gemaakt, gaat u naar [Een nieuw opslagaccount voor de service configureren](#optional-step-configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
> * Als u het automatisch maken van een storage-account hebt ingeschakeld, gaat u naar [stap 2: De serviceregistratiesleutel ophalen](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: De serviceregistratiesleutel ophalen

Wanneer de StorSimple-apparaatbeheerfunctie bedrijfsklaar is, moet u de serviceregistratiesleutel ophalen. Deze sleutel wordt gebruikt om het StorSimple-apparaat te registreren en te verbinden met de service.

Voer de volgende stappen uit in [Azure Portal](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> De serviceregistratiesleutel wordt gebruikt om alle StorSimple Device Manager-apparaten die moeten worden geregistreerd met uw StorSimple Device Manager-service te registreren.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>Stap 3: De installatiekopie voor de virtuele matrix downloaden

Nadat u de serviceregistratiesleutel hebt, moet u de installatiekopie van het juiste virtuele matrix voor het inrichten van een virtuele matrix op uw hostsysteem downloaden. De virtuele matrix-installatiekopieën worden specifieke besturingssysteem en kunnen worden gedownload vanaf de pagina snel starten in Azure portal.

> [!IMPORTANT]
> De software die wordt uitgevoerd op de StorSimple Virtual Array kan alleen worden gebruikt met de StorSimple Device Manager-service.
> 
> 

Voer de volgende stappen uit in [Azure Portal](https://portal.azure.com/).

#### <a name="to-get-the-virtual-array-image"></a>Om op te halen van de installatiekopie van het virtuele matrix

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/). 
2. Klik in de Azure-portal op **Bladeren > StorSimple-Apparaatbeheerfuncties**.
3. Selecteer een bestaande StorSimple Device Manager-service. In de **StorSimple Device Manager** blade, klikt u op **Quick Start**. 
4. Klik op de koppeling die overeenkomt met de installatiekopie die u wilt downloaden uit het Microsoft Download Center. De installatiekopieën zijn ongeveer 4,8 GB.
   
   * VHDX voor Hyper-V op Windows Server 2012 en hoger
   * VHD voor Hyper-V op Windows Server 2008 R2 en hoger
   * VMDK voor VMWare ESXi 5.0, 5.5, 6.0 of 6.5
5. Download het bestand en pak het uit op een lokale schijf, en onthoud waar het zipbestand is uitgepakt.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionele stap: Een nieuw opslagaccount voor de service configureren

Deze stap is optioneel en alleen als u het automatisch maken van een storage-account niet hebt ingeschakeld met uw service moet worden uitgevoerd.

Als u maken van een Azure storage-account in een andere regio wilt, Zie [over het maken van een storage-account](../storage/common/storage-quickstart-create-account.md) voor stapsgewijze instructies.

Voer de volgende stappen uit in de [Azure-portal](https://ms.portal.azure.com/) op de pagina van de service StorSimple Device Manager voor het toevoegen van een bestaande Microsoft Azure storage-account.

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Als u wilt een opslagaccountreferentie toevoegen waarvoor hetzelfde Azure-abonnement als de Device Manager-service

1. Navigeer naar uw Device Manager-service en selecteer en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie.
3. Klik op **Add**.
4. In de **een storage-account toevoegen** blade, doet u het volgende:
   
   1. Voor **abonnement**, selecteer **huidige**.
   
   2. Geef de naam van uw Azure storage-account.
   
   3. Selecteer **inschakelen** te maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud. Selecteer **uitschakelen** alleen als u binnen een privécloud werkt.
   
   4. Klik op **Add**. U krijgt een melding nadat het opslagaccount is gemaakt.<br></br>
   
      ![Een bestaande opslagaccountreferentie toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Volgende stap

De volgende stap is het inrichten van een virtuele machine voor uw StorSimple Virtual Array. Raadpleeg hier de specifieke instructies voor het besturingssysteem van uw host:

* [Een virtuele StorSimple-matrix in Hyper-V inrichten](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Een virtuele StorSimple-matrix in VMware inrichten](storsimple-virtual-array-deploy2-provision-vmware.md)

