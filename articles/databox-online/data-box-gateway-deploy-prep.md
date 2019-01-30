---
title: Zelfstudie over het voorbereiden van Azure Portal voor de implementatie van Data Box Gateway | Microsoft Docs
description: De eerste zelfstudie voor het implementeren van Azure Data Box Gateway omvat het voorbereiden van Azure Portal.
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6db713984b62ce3db48b2e72a4b117696bdd6add
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452969"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>Zelfstudie: Implementatie van Azure Data Box Gateway (preview) voorbereiden


Dit is de eerste zelfstudie in de reeks zelfstudies voor implementatie, die noodzakelijk zijn voor het voltooien van de implementatie van uw Azure Data Box Gateway. In deze zelfstudie wordt beschreven hoe u Azure Portal voorbereidt voor de implementatie van de Data Box Gateway-resource. 

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. Het voorbereiden van de portal duurt minder dan 10 minuten.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe resource maken
> * De installatiekopie voor het virtuele apparaat downloaden
> * De activeringssleutel ophalen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


> [!IMPORTANT]
> - Data Box Gateway verkeert in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 

### <a name="get-started"></a>Aan de slag

Raadpleeg de volgende zelfstudies in de voorgeschreven volgorde voor het implementeren van uw Data Box Gateway.

| **#** | **In deze stap** | **Gebruikt u deze documenten** |
| --- | --- | --- | 
| 1. |**[Azure Portal voorbereiden voor Data Box Gateway](data-box-gateway-deploy-prep.md)** |Maak en configureer uw Data Box Gateway-resource voordat u een virtueel Data Box Gateway-apparaat inricht. |
| 2. |**[Data Box Gateway inrichten in Hyper-V](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[Data Box Gateway inrichten in VMware](data-box-gateway-deploy-provision-vmware.md)**|Voor Hyper-V moet u een virtueel Data Box Gateway-apparaat inrichten en verbinden op een hostsysteem waarop Hyper-V wordt uitgevoerd in Windows Server 2016 of Windows Server 2012 R2. <br><br><br> Voor VMware moet u een virtueel Data Box Gateway-apparaat inrichten en verbinden op een hostsysteem waarop VMware ESXi 6.0 of 6.5 wordt uitgevoerd.<br></br> |
| 3. |**[Azure Data Box Gateway verbinden, instellen en activeren](data-box-gateway-deploy-connect-setup-activate.md)** |Maak verbinding met de lokale webinterface, voltooi het instellen van het apparaat en activeer het. Vervolgens kunt u SMB-shares inrichten.  |
| 4. |**[Gegevens overdragen met Data Box Gateway](data-box-gateway-deploy-add-shares.md)** |Shares toevoegen en verbinding maken met shares via SMB of NFS. |

U kunt nu Azure Portal gaan instellen.

## <a name="prerequisites"></a>Vereisten

Hier vindt u de configuratievereisten voor uw Data Box Gateway-resource, uw Data Box Gateway-apparaat en het datacenternetwerk.

### <a name="for-the-data-box-gateway-resource"></a>Voor de Data Box Gateway-resource

Zorg voordat u begint voor het volgende:

* Uw Microsoft Azure-abonnement moet worden ingeschakeld voor de Data Box Gateway-resource.
* U hebt een Microsoft Azure Storage-account met toegangsreferenties.

### <a name="for-the-data-box-gateway-device"></a>Voor het Data Box Gateway-apparaat

Voordat u een virtueel apparaat implementeert, controleert u of:

* U toegang hebt tot een hostsysteem met Hyper-V in Windows Server 2012 R2 of hoger, of VMware (ESXi 6.0 of 6.5) dat kan worden gebruikt voor het inrichten van een apparaat.
* Het hostsysteem kan de volgende resources volledig toewijzen aan het inrichten van uw virtuele Data Box-apparaat:
  
  * Minimaal 4 processorkernen.
  * Ten minste 8 GB RAM-geheugen. 
  * Eén netwerkinterface.
  * Een besturingssysteemschijf van 250 GB.
  * Een virtuele schijf van 2 TB voor systeemgegevens.

### <a name="for-the-datacenter-network"></a>Voor datacenternetwerk

Zorg voordat u begint voor het volgende:

* Het netwerk in uw datacenter is geconfigureerd volgens de netwerkvereisten voor uw Data Box Gateway-apparaat. Zie [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md) voor meer informatie.

* Uw Data Box Gateway te allen tijde beschikt over een toegewezen internetbandbreedte van 20 Mbps (of meer). Deze bandbreedte mag niet worden gedeeld met andere toepassingen. Als u gebruikmaakt van bandbreedtebeperking, adviseren we om een bandbreedte van 32 Mbps of meer toe te wijzen om de beperking te laten werken.

## <a name="create-a-new-resource"></a>Een nieuwe resource maken

Voer de volgende stappen uit om een nieuwe Data Box Gateway-resource te maken. 

Als u al een Data Box Gateway-resource hebt voor het beheer van uw virtuele apparaten, kunt u deze stap overslaan en verdergaan met [Activeringscode ophalen](#get-the-activation-key).

Voer de volgende stappen uit in Azure Portal om een Data Box-resource te maken.
1. Gebruik uw Microsoft Azure-referenties om u aan te melden bij Azure Preview Portal op de volgende URL: [https://aka.ms/databox-edge](https://aka.ms/databox-edge). 

2. Selecteer het abonnement dat u wilt gebruiken voor de preview van Data Box Edge. Selecteer de regio waar u de Data Box Edge-resource wilt implementeren. Klik in de optie **Data Box Gateway** op **Maken**.

    ![Data Box Gateway-service zoeken](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. Voer de volgende informatie in of selecteer deze voor de nieuwe resource.
    
    |Instelling  |Waarde  |
    |---------|---------|
    |Resourcenaam   | Een beschrijvende naam om de resource aan te duiden.<br>De naam is tussen 2 en 50 tekens lang en kan letters, cijfers en afbreekstreepjes bevatten.<br> De naam begint en eindigt met een letter of cijfer.        |
    |Abonnement    |Abonnement is gekoppeld aan uw factureringsrekening. |
    |Resourcegroep  |Maak een nieuwe groep of selecteer een bestaande groep.<br>Meer informatie over [Azure-resourcegroepen](../azure-resource-manager/resource-group-overview.md).     |
    |Locatie     |Voor deze release zijn US - oost, US - west 2, Azië - zuidoost en Europa - west beschikbaar. <br> Kies een locatie die het dichtst bij de geografische regio ligt waar u uw apparaat wilt implementeren.|
    
    ![Data Box Gateway-resource maken](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. Klik op **OK**.
 
Het maken van de resource duurt enkele minuten. Nadat de resource is gemaakt, wordt u daarvan op de hoogte gebracht.


## <a name="download-the-virtual-device-image"></a>De installatiekopie voor het virtuele apparaat downloaden

Nadat u de Data Box Gateway-resource hebt gemaakt, downloadt u de juiste installatiekopie voor het virtuele apparaat om een virtueel apparaat in te richten op uw hostsysteem. Elk besturingssysteem heeft specifieke installatiekopieën voor virtuele apparaten, die kunnen worden gedownload vanaf de blade **Snelstart** van uw resource in Azure Portal.

> [!IMPORTANT]
> De software die wordt uitgevoerd op de Data Box Gateway kan alleen worden gebruikt met de Data Box Gateway-resource.


Voer de volgende stappen uit in [Azure Portal](https://portal.azure.com/).

1. Klik op de resource die u hebt gemaakt en klik vervolgens op **Overzicht**. Als u een bestaande Azure Data Box Gateway-resource hebt, klikt u op de resource en gaat u naar **Overzicht**.

    ![Nieuwe Data Box Gateway-resource](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. Klik in de snelstart in het rechterdeelvenster op de koppeling voor de installatiekopie die u wilt downloaden. De installatiekopieën zijn ongeveer 4,8 GB.
   
   * [VHDX voor Hyper-V in Windows Server 2012 R2 en hoger](https://aka.ms/dbe-vhdx-2012).
   * [VMDK voor VMWare ESXi 6.0 of 6.5](https://aka.ms/dbe-vmdk).

5. Download het bestand en pak het uit op een lokale schijf, en onthoud waar het zipbestand is uitgepakt.


## <a name="get-the-activation-key"></a>De activeringssleutel ophalen

Nadat de Azure Data Box Gateway-resource is geactiveerd, hebt u de activeringssleutel nodig. Deze sleutel wordt gebruikt om uw Data Box Gateway-apparaat te activeren en te verbinden met de resource.

De activeringssleutel wordt gebruikt voor het registreren van alle Data Box Gateway-apparaten die moeten worden geactiveerd met uw Azure Data Box Gateway-resource. U kunt deze sleutel nu ophalen, terwijl u Azure Portal geopend hebt.

1. Klik op de resource die u hebt gemaakt en klik vervolgens op **Overzicht**.

    ![Nieuwe Data Box Gateway-resource](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. Klik op **Sleutel genereren** om een activeringssleutel te maken. Klik op het kopieerpictogram om de sleutel te kopiëren en op te slaan voor later gebruik.

    ![Activeringssleutel ophalen](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - De activeringssleutel verloopt 3 dagen nadat deze is gegenereerd. 
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


