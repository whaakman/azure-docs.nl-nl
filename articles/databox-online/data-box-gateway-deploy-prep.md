---
title: Zelfstudie over het voorbereiden van Azure Portal voor de implementatie van Data Box Gateway | Microsoft Docs
description: De eerste zelfstudie voor het implementeren van Azure Data Box Gateway omvat het voorbereiden van Azure Portal.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: alkohli
ms.openlocfilehash: 34bc4d7cbdbb89cd9ff3f334ca32087c474735b7
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620084"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway"></a>Zelfstudie: Voorbereidingen voor het implementeren van Azure Data Box-Gateway


Dit is de eerste zelfstudie in de reeks zelfstudies voor implementatie, die noodzakelijk zijn voor het voltooien van de implementatie van uw Azure Data Box Gateway. In deze zelfstudie wordt beschreven hoe u Azure Portal voorbereidt voor de implementatie van de Data Box Gateway-resource. 

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het voorbereiden van de portal duurt minder dan 10 minuten.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe resource maken
> * De installatiekopie voor het virtuele apparaat downloaden
> * De activeringssleutel ophalen

## <a name="get-started"></a>Aan de slag

Raadpleeg de volgende zelfstudies in de voorgeschreven volgorde voor het implementeren van uw Data Box Gateway.

| **#** | **In deze stap** | **Gebruikt u deze documenten** |
| --- | --- | --- | 
| 1. |**[Azure Portal voorbereiden voor Data Box Gateway](data-box-gateway-deploy-prep.md)** |Maak en configureer uw Data Box Gateway-resource voordat u een virtueel Data Box Gateway-apparaat inricht. |
| 2. |**[Data Box Gateway inrichten in Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway inrichten in VMware](data-box-gateway-deploy-provision-vmware.md)**|Voor Hyper-V moet u een virtueel Data Box Gateway-apparaat inrichten en verbinden op een hostsysteem waarop Hyper-V wordt uitgevoerd in Windows Server 2016 of Windows Server 2012 R2. <br><br><br> Voor VMware moet u een virtueel Data Box Gateway-apparaat inrichten en verbinden op een hostsysteem waarop VMware ESXi 6.0, 6.5 of 6.7 wordt uitgevoerd.<br></br> |
| 3. |**[Azure Data Box Gateway verbinden, instellen en activeren](data-box-gateway-deploy-connect-setup-activate.md)** |Maak verbinding met de lokale webinterface, voltooi het instellen van het apparaat en activeer het. Vervolgens kunt u SMB-shares inrichten.  |
| 4. |**[Gegevens overdragen met Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Shares toevoegen en verbinding maken met shares via SMB of NFS. |

U kunt nu Azure Portal gaan instellen.

## <a name="prerequisites"></a>Vereisten

Hier vindt u de configuratievereisten voor uw Data Box Gateway-resource, uw Data Box Gateway-apparaat en het datacenternetwerk.

### <a name="for-the-data-box-gateway-resource"></a>Voor de Data Box Gateway-resource

Zorg voordat u begint voor het volgende:

- Uw Microsoft Azure-abonnement moet worden ondersteund voor gegevens in het Gateway-resource. Betalen per gebruik-abonnementen worden niet ondersteund.
- U hebt een Microsoft Azure Storage-account met toegangsreferenties.

### <a name="for-the-data-box-gateway-device"></a>Voor het Data Box Gateway-apparaat

Voordat u een virtueel apparaat implementeert, controleert u of:

- U toegang hebt tot een hostsysteem met Hyper-V in Windows Server 2012 R2 of hoger, of VMware (ESXi 6.0, 6.5 of 6.7) dat kan worden gebruikt voor het inrichten van een apparaat.
- Het hostsysteem kan de volgende resources volledig toewijzen aan het inrichten van uw virtuele Data Box-apparaat:
  
  - Een minimum van 4 virtuele processors.
  - Ten minste 8 GB RAM-geheugen.
  - Eén netwerkinterface.
  - Een besturingssysteemschijf van 250 GB.
  - Een virtuele schijf van 2 TB voor systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor datacenternetwerk

Zorg voordat u begint voor het volgende:

- Het netwerk in uw datacenter is geconfigureerd volgens de netwerkvereisten voor uw Data Box Gateway-apparaat. Zie voor meer informatie de [Data Box Gateway systeemvereisten](data-box-gateway-system-requirements.md).

- Voor normale operationele voorwaarden van uw Data Box-Gateway, moet u beschikt over een:

    - Minimaal 10 Mbps bandbreedte om te controleren of het apparaat blijft bijgewerkte downloaden.
    - Een minimum van 20-Mbps toegewezen uploaden en downloaden van bandbreedte om over te dragen van bestanden.

## <a name="create-a-new-resource"></a>Een nieuwe resource maken

Als u al een Data Box Gateway-resource hebt voor het beheer van uw virtuele apparaten, kunt u deze stap overslaan en verdergaan met [Activeringscode ophalen](#get-the-activation-key).

Voor het maken van een resource in het gegevensgateway, moet u de volgende stappen uitvoeren in Azure portal.

1. Gebruik uw Microsoft Azure-referenties aanmelden bij:

    - De Azure-portal op de volgende URL: [ https://portal.azure.com ](http://portal.azure.com).
    - Of, in de Azure Government-portal op de volgende URL: [ https://portal.azure.us ](https://portal.azure.us). Voor meer informatie gaat u naar [verbinding maken met Azure Government met behulp van de portal](https://docs.microsoft.com/azure/azure-government/documentation-government-get-started-connect-with-portal).

2. Selecteer in het linkerdeelvenster **+ een resource maken**. Zoeken naar **Data Box Edge / Data Box Gateway**. Data Box rand selecteren / Data Box-Gateway. Selecteer **Maken**.
3. Kies het abonnement dat u wilt gebruiken voor gegevens in het Gateway-apparaat. Selecteer de regio waar u wilt de gegevens in het Gateway-resources implementeren. VS-Oost, Zuidoost-Azië en West-Europa zijn beschikbaar voor deze release. Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren. In de **Data Box Gateway** optie, selecteer **maken**.

    ![Data Box Gateway-service zoeken](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

4. Op de **basisbeginselen** tabblad Typ of Selecteer de volgende **projectdetails**.
    
    |Instelling  |Value  |
    |---------|---------|
    |Abonnement    |Dit wordt automatisch ingevuld op basis van de eerdere selectie. Abonnement is gekoppeld aan uw factureringsrekening. |
    |Resourcegroep  |Maak een nieuwe groep of selecteer een bestaande groep.<br>Meer informatie over [Azure-resourcegroepen](../azure-resource-manager/resource-group-overview.md).     |

5. Typ of Selecteer de volgende **exemplaar details**.

    |Instelling  |Waarde  |
    |---------|---------|
    |Name   | Een beschrijvende naam om de resource aan te duiden.<br>De naam is tussen 2 en 50 tekens lang en kan letters, cijfers en afbreekstreepjes bevatten.<br> De naam begint en eindigt met een letter of cijfer.        |   
    |Regio     |Voor deze release zijn de VS-Oost, Zuidoost-Azië en West-Europa zijn beschikbaar voor het implementeren van uw resource. Voor de Azure Government, alle government-regio's die worden vermeld in de [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/) beschikbaar zijn. <br> Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren.|
    
    ![Data Box Gateway-resource maken](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
6. Selecteer **Controleren + maken**.
 
7. Op de **bekijken + maken** tabblad de **prijsinformatie**, **gebruiksvoorwaarden**, en de details voor uw resource. Selecteer **Maken**.

    ![Data Box Gateway Resourcedetails bekijken](media/data-box-gateway-deploy-prep/data-box-gateway-resource1.png)

Het maken van de resource duurt enkele minuten. Nadat de resource is gemaakt en geïmplementeerd, krijgt u een melding. Selecteer **Ga naar resource**.

![Data Box Gateway Resourcedetails bekijken](media/data-box-gateway-deploy-prep/data-box-gateway-resource2.png)

## <a name="download-the-virtual-device-image"></a>De installatiekopie voor het virtuele apparaat downloaden

Nadat u de Data Box Gateway-resource hebt gemaakt, downloadt u de juiste installatiekopie voor het virtuele apparaat om een virtueel apparaat in te richten op uw hostsysteem. De installatiekopieën van het virtuele apparaat zijn specifiek voor een besturingssysteem.

> [!IMPORTANT]
> De software die wordt uitgevoerd op de Data Box Gateway kan alleen worden gebruikt met de Data Box Gateway-resource.

Volg deze stappen in de [Azure-portal](https://portal.azure.com/) voor het downloaden van een installatiekopie van het virtuele apparaat.

1. In de resource die u hebt gemaakt en selecteer vervolgens **overzicht**. Als u een bestaande Azure Data Box-Gateway-resource hebt, selecteert u de resource en Ga naar **overzicht**. Selecteer **Apparaatinstelling**.

    ![Nieuwe Data Box Gateway-resource](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Op de **downloaden installatiekopie** tegel, selecteert u de installatiekopie van het virtuele apparaat overeenkomt met het besturingssysteem op de hostserver die wordt gebruikt voor het inrichten van de virtuele machine. De installatiekopie van ongeveer 5,6 GB zijn.
   
   * [VHDX voor Hyper-V in Windows Server 2012 R2 en hoger](https://aka.ms/dbe-vhdx-2012).
   * [VMDK voor VMWare ESXi 6.0, 6.5 of 6.7](https://aka.ms/dbe-vmdk).

    ![Installatiekopie van het virtuele apparaat Data Box-Gateway downloaden](media/data-box-gateway-deploy-prep/data-box-gateway-download-image.png)

5. Download het bestand en pak het uit op een lokale schijf, en onthoud waar het zipbestand is uitgepakt.


## <a name="get-the-activation-key"></a>De activeringssleutel ophalen

Nadat de gegevens in het Gateway-resource actief en werkend is, hebt u de activeringscode. Deze sleutel wordt gebruikt om uw Data Box Gateway-apparaat te activeren en te verbinden met de resource. U kunt deze sleutel nu ophalen, terwijl u Azure Portal geopend hebt.

1. Selecteer de resource die u hebt gemaakt en selecteer vervolgens **Overzicht**. In de **Apparaatinstelling**, gaat u naar de **configureren en te activeren** tegel.

    ![Configureren en activeren van tegel](media/data-box-gateway-deploy-prep/data-box-gateway-configure-activate.png)

2. Selecteer **Sleutel genereren** om een activeringssleutel te maken. Selecteer het kopieerpictogram om de sleutel te kopiëren en op te slaan voor later gebruik.

    ![Activeringssleutel ophalen](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - De activeringssleutel verloopt drie dagen nadat deze is gegenereerd.
> - Als de sleutel is verlopen, genereert u een nieuwe sleutel. De oudere toegangssleutel is niet langer geldig.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Gateway, zoals:

> [!div class="checklist"]
> * Een nieuwe resource maken
> * De installatiekopie voor het virtuele apparaat downloaden
> * De activeringssleutel ophalen

Ga naar de volgende zelfstudie om te lezen hoe u een virtuele machine kunt inrichten voor uw Data Box Gateway. Raadpleeg hier de specifieke instructies voor het besturingssysteem van uw host:

> [!div class="nextstepaction"]
> [Een Azure Data Box Gateway inrichten in Hyper-V](./data-box-gateway-deploy-provision-hyperv.md)

OF

> [!div class="nextstepaction"]
> [Een Azure Data Box Gateway inrichten in VMware](./data-box-gateway-deploy-provision-vmware.md)


