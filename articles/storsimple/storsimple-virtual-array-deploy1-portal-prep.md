---
title: Portal prep voor virtuele StorSimple-matrix | Microsoft Docs
description: Eerste zelfstudie voor het implementeren van virtuele StorSimple-matrix bestaat uit het voorbereiden van de Azure-portal
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 68a4cfd3-94c9-46cb-805c-46217290ce02
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d0801053721f98ce7a2b0fcbe3c65da8dbdd8d3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-storsimple-virtual-array---prepare-the-azure-portal"></a>StorSimple virtuele matrix implementeren: voorbereiden van de Azure-portal

![](./media/storsimple-virtual-array-deploy1-portal-prep/getstarted4.png)
## <a name="overview"></a>Overzicht

Dit is het eerste artikel in de reeks van zelfstudies voor implementatie vereist voor het implementeren van uw virtuele matrix volledig als een bestandsserver of een iSCSI-server met het Resource Manager-model. In dit artikel beschrijft de voorbereiding is vereist voor het maken en configureren van uw StorSimple-apparaat Manager-service voorafgaand aan het inrichten van een virtuele-matrix. In dit artikel bevat ook koppelingen uit naar een Configuratiecontrolelijst voor implementatie en configuratie vereisten.

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het is raadzaam om de configuratiecontrolelijst voor implementatie te controleren voordat u begint. De portal voorbereiding duurt minder dan 10 minuten.

De informatie die is gepubliceerd in dit artikel geldt voor de implementatie van het virtuele StorSimple-matrices in de Azure-portal en Microsoft Azure Government Cloud.

### <a name="get-started"></a>Aan de slag
De implementatiewerkstroom bestaat uit het voorbereiden van de portal, het inrichten van een virtuele-matrix in uw gevirtualiseerde omgeving en de installatie voltooien. Om te beginnen met de implementatie van het virtuele StorSimple-matrix als een bestandsserver of een server met iSCSI-, moet u verwijzen naar de volgende gebruikmaking bronnen.

#### <a name="deployment-articles"></a>Artikelen over implementatie

Raadpleeg de volgende artikelen in de voorgeschreven volgorde voor het implementeren van uw virtuele StorSimple-matrix.

| **#** | **In deze stap** | **U dit wilt doen...** | **En gebruik van deze documenten.** |
| --- | --- | --- | --- |
| 1. |**De Azure portal instellen** |Maak en configureer uw StorSimple-apparaat Manager-service vóór het inrichten van een virtueel StorSimple-matrix. |[Voorbereiden van de portal](storsimple-virtual-array-deploy1-portal-prep.md) |
| 2. |**De virtuele matrix inrichten** |Voor Hyper-V, inrichten en verbinding maken met een virtueel StorSimple-matrix op een hostsysteem met Hyper-V op Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2. <br></br> <br></br> Voor VMware, inrichten en verbinding maken met een virtueel StorSimple-matrix op een hostsysteem met VMware ESXi 5.5 en hoger.<br></br> |[Inrichten van een virtuele-matrix in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) <br></br> <br></br> [Een virtuele-matrix in VMware inrichten](storsimple-virtual-array-deploy2-provision-vmware.md) |
| 3. |**Instellen van de virtuele matrix** |Eerste installatie uitvoeren, registreren van uw StorSimple-bestandsserver en de Apparaatinstelling voor uw bestandsserver. Vervolgens kunt u SMB-shares inrichten. <br></br> <br></br> Eerste installatie uitvoeren voor uw server met iSCSI-, uw StorSimple iSCSI-server registreren en voltooit de installatie van het apparaat. Vervolgens kunt u iSCSI-volumes inrichten. |[Virtuele matrix instellen als bestandsserver](storsimple-virtual-array-deploy3-fs-setup.md)<br></br> <br></br>[Virtuele matrix ingesteld als iSCSI-server](storsimple-virtual-array-deploy3-iscsi-setup.md) |

U kunt nu beginnen met het instellen van de Azure-portal.

## <a name="configuration-checklist"></a>Configuratiecontrolelijst

De Configuratiecontrolelijst bevat de informatie die u verzamelen moet voordat u de software op uw virtuele StorSimple-matrix configureren. Deze informatie tevoren voorbereid helpt bij het stroomlijnen van de implementatie van het StorSimple-apparaat in uw omgeving. Afhankelijk van het feit of uw virtuele StorSimple-matrix is geïmplementeerd als een bestandsserver of een iSCSI-server, moet u een van de volgende controlelijsten.

* Download de [Server Configuratiecontrolelijst voor StorSimple virtuele matrix bestand](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).
* Download de [virtuele StorSimple-matrix iSCSI Server Configuratiecontrolelijst](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Vereisten

Hier vindt u de configuratievereisten voor uw StorSimple-apparaat Manager-service, uw virtuele StorSimple-matrix en het datacenternetwerk.

### <a name="for-the-storsimple-device-manager-service"></a>Voor de StorSimple-apparaatbeheerfunctie

Zorg voordat u begint voor het volgende:

* U hebt een Microsoft-account met toegangsreferenties.
* U hebt een Microsoft Azure Storage-account met toegangsreferenties.
* Uw Microsoft Azure-abonnement moet worden ingeschakeld voor Apparaatbeheer van StorSimple-service.

### <a name="for-the-storsimple-virtual-array"></a>Voor de virtuele StorSimple-matrix

Voordat u een virtuele-matrix implementeert, zorg ervoor dat:

* U hebt toegang tot een hostsysteem met Hyper-V in Windows Server 2008 R2 of later of VMware (ESXi 5.5 of hoger) die kan worden gebruikt voor een voorziening van een apparaat.
* Het hostsysteem kan toe te wijzen aan de volgende bronnen voor het inrichten van uw virtuele matrix:
  
  * Een minimum van 4 kernen.
  * Ten minste 8 GB aan RAM-geheugen. Als u van plan bent de virtuele matrix als bestandsserver te configureren, ondersteunt 8 GB 2 miljoen bestanden. U moet 16 GB RAM-geheugen voor de ondersteuning van 2-4 miljoen bestanden.
  * Één netwerkinterface.
  * 500 GB virtuele schijf voor systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor het datacenternetwerk

Zorg voordat u begint voor het volgende:

* Het netwerk in uw datacenter is geconfigureerd volgens de vereisten voor uw StorSimple-apparaat. Zie voor meer informatie de [StorSimple virtuele matrix systeemvereisten](storsimple-ova-system-requirements.md).
* Uw virtuele StorSimple-matrix heeft een speciale 5 Mbps internetbandbreedte (of meer) beschikbaar te allen tijde. Deze bandbreedte mag niet worden gedeeld met andere toepassingen.

## <a name="step-by-step-preparation"></a>Stapsgewijze voorbereiding

Gebruik de volgende stapsgewijze instructies voor het voorbereiden van de portal voor de service Manager voor StorSimple-apparaat.

## <a name="step-1-create-a-new-service"></a>Stap 1: een nieuwe service maken

Slechts één exemplaar van de service Manager voor StorSimple-apparaat kunt beheren meerdere virtuele StorSimple-matrices. Voer de volgende stappen uit om een exemplaar van de StorSimple-apparaatbeheerfunctie uit te voeren. Als u een bestaande service Manager voor StorSimple-apparaat voor het beheren van uw virtuele matrices hebt, deze stap overslaan en gaat u naar [stap 2: de serviceregistratiesleutel ophalen](#step-2-get-the-service-registration-key).

[!INCLUDE [storsimple-virtual-array-create-new-service](../../includes/storsimple-virtual-array-create-new-service.md)]

> [!IMPORTANT]
> Als u de service niet hebt ingeschakeld om automatisch een opslagaccount te maken, moet u minimaal één opslagaccount maken nadat u een service hebt gemaakt.
> 
> * Als u niet automatisch een opslagaccount hebt gemaakt, gaat u naar [Een nieuw opslagaccount voor de service configureren](#optional-step-configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
> * Als u het automatisch maken van een opslagaccount hebt ingeschakeld, gaat u naar [Stap 2: de serviceregistratiesleutel ophalen](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: de serviceregistratiesleutel ophalen

Wanneer de StorSimple-apparaatbeheerfunctie bedrijfsklaar is, moet u de serviceregistratiesleutel ophalen. Deze sleutel wordt gebruikt om het StorSimple-apparaat te registreren en te verbinden met de service.

Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com/).

[!INCLUDE [storsimple-virtual-array-get-service-registration-key](../../includes/storsimple-virtual-array-get-service-registration-key.md)]

> [!NOTE]
> De serviceregistratiesleutel wordt gebruikt voor het registreren van alle Apparaatbeheer StorSimple-apparaten die moeten worden geregistreerd bij uw StorSimple-apparaat Manager-service.
> 
> 

## <a name="step-3-download-the-virtual-array-image"></a>Stap 3: Download de installatiekopie van het virtuele matrix

Nadat u de serviceregistratiesleutel hebt, moet u de installatiekopie van het juiste virtuele matrix voor het inrichten van een virtuele-matrix op uw hostsysteem downloaden. Installatiekopieën van het virtuele matrix zijn specifiek besturingssysteem en kunnen worden gedownload vanaf de pagina snel starten in de Azure portal.

> [!IMPORTANT]
> De software die wordt uitgevoerd op de virtuele StorSimple-matrix kan alleen worden gebruikt met de service Manager voor StorSimple-apparaat.
> 
> 

Voer de volgende stappen uit in de [Azure-portal](https://portal.azure.com/).

#### <a name="to-get-the-virtual-array-image"></a>De installatiekopie van het virtuele matrix ophalen

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/). 
2. Klik in de Azure-portal op **Bladeren > Managers voor apparaatregistratie StorSimple**.
3. Selecteer een bestaande Apparaatbeheer StorSimple-service. In de **StorSimple Apparaatbeheer** blade, klikt u op **Quick Start**. 
4. Klik op de koppeling die overeenkomt met de installatiekopie die u wilt downloaden uit het Microsoft Download Center. De afbeeldingsbestanden zijn ongeveer 4,8 GB.
   
   * VHDX voor Hyper-V op Windows Server 2012 en hoger
   * VHD voor Hyper-V in Windows Server 2008 R2 en hoger
   * VMDK voor VMWare ESXi 5.5 en hoger
5. Download en pak het bestand naar een lokaal station, waardoor een notitie van waarin de uitgepakte bestand zich bevindt.

## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Optionele stap: een nieuw opslagaccount voor de service configureren

Deze stap is optioneel en moet worden uitgevoerd alleen als u het automatisch maken van een opslagaccount met uw service niet hebt ingeschakeld.

Als u maken van een Azure storage-account in een andere regio wilt, Zie [het maken van een opslagaccount](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor stapsgewijze instructies.

Voer de volgende stappen uit in de [Azure-portal](https://ms.portal.azure.com/) op de pagina van de service Manager voor StorSimple-apparaat toevoegen van een bestaand Microsoft Azure-opslagaccount.

#### <a name="to-add-a-storage-account-credential-that-has-the-same-azure-subscription-as-the-device-manager-service"></a>Toevoegen van een referentie storage-account met de dezelfde Azure-abonnement als de service voor Apparaatbeheer

1. Navigeer naar uw service Apparaatbeheer, selecteert en dubbelklik erop. Hiermee opent u de **overzicht** blade.
2. Selecteer **opslagaccountreferenties** binnen de **configuratie** sectie.
3. Klik op **Add**.
4. In de **een opslagaccount toevoegen** blade het volgende doen:
   
    1. Voor **abonnement**, selecteer **huidige**.
   
    2. Geef de naam van uw Azure storage-account.
   
    3. Selecteer **inschakelen** voor het maken van een beveiligd kanaal voor netwerkcommunicatie tussen uw StorSimple-apparaat en de cloud. Selecteer **uitschakelen** alleen als u in een privécloud werkt.
   
    4. Klik op **Add**. U wordt gewaarschuwd nadat het opslagaccount is gemaakt.<br></br>
   
     ![Een bestaande referentie voor de storage-account toevoegen](./media/storsimple-virtual-array-manage-storage-accounts/ova-add-storageacct.png)

## <a name="next-step"></a>Volgende stap

De volgende stap is het inrichten van een virtuele machine voor uw virtuele StorSimple-matrix. Raadpleeg de gedetailleerde instructies in, afhankelijk van uw hostbesturingssysteem:

* [Inrichten van een virtueel StorSimple-matrix in Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md)
* [Een virtueel StorSimple-matrix in VMware inrichten](storsimple-virtual-array-deploy2-provision-vmware.md)

