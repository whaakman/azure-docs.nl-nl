---
title: Snelstartgids - Een schaalset voor virtuele machines maken in Azure Portal | Microsoft Docs
description: Leer hoe u snel een schaalset voor virtuele machines kunt maken in Azure Portal
keywords: schaalsets voor virtuele machines
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: cynthn
ms.openlocfilehash: 7644940418b7322e5ec4b02b793219b44ae3aa97
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885146"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Quickstart: Een schaalset voor virtuele machines maken in Azure Portal
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. U kunt het aantal VM’s in de schaalset handmatig schalen of regels voor automatisch schalen definiëren op basis van resourcegebruik zoals CPU, vraag naar geheugen, of netwerkverkeer. Een Azure load balancer verdeelt het verkeer vervolgens naar de VM-exemplaren in de schaalset. In deze snelstartgids gaat u in Azure Portal een schaalset voor virtuele machines maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij Azure Portal op http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Schaalset voor virtuele machines maken
U kunt een schaalset implementeren met een installatiekopie van Windows Server of Linux, zoals RHEL, CentOS, Ubuntu of SLES.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar *schaalset*, kies **Virtuele-machineschaalset** en selecteer **Maken**.
3. Voer een naam in voor de schaalset, zoals *myScaleSet*.
4. Selecteer het gewenste type besturingssysteem, zoals *Windows Server 2016 Datacenter*.
5. Voer de naam van de gewenste resourcegroep in, zoals *myResourceGroup*, en de locatie, zoals *US - oost*.
6. Voer de gewenste gebruikersnaam in en selecteer het verificatietype dat u wilt gebruiken.
    - Een **wachtwoord** moet ten minste 12 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken. Zie [Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) voor meer informatie.
    - Als u een installatiekopie voor een Linux-besturingssysteem selecteert, kunt u ook **Openbare SSH-sleutel** kiezen. Geef alleen de openbare sleutel op, zoals *~/.ssh/id_rsa.pub*. U kunt vanuit de portal de Azure Cloud-Shell gebruiken om [SSH-sleutels te maken en gebruiken](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Geef een **openbaar IP-adres** op, zoals *myPublicIP*.
8. Voer een uniek **domeinnaamlabel** in, zoals *myuniquedns*. Dit DNS-label vormt de basis van de FQDN-naam voor de load balancer vóór de schaalset.
9. Selecteer **Maken** om de opties voor de schaalset te bevestigen.

    ![Een schaalset voor virtuele machines maken in Azure Portal](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbinding maken met een VM in de schaalset
Wanneer u een schaalset maakt in de portal, wordt er ook een load balancer gemaakt. Met behulp van NAT-regels (Network Address Translation) wordt verkeer voor externe verbindingen zoals RDP of SSH naar de exemplaren in de schaalset gedistribueerd.

U kunt deze NAT-regels en verbindingsinformatie als volgt weergeven voor de exemplaren in de schaalset:

1. Selecteer de resourcegroep die u in de vorige stap hebt gemaakt, zoals *myResourceGroup*.
2. Selecteer in de lijst met resources uw **load balancer**, zoals *myScaleSetLab*.
3. Kies **Binnenkomende NAT-regels** in het menu aan de linkerkant van het venster.

    ![Via binnenkomende NAT-regels kunt u verbinding maken met exemplaren in de schaalset voor virtuele machines](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

U kunt met behulp van deze NAT-regels verbinding maken met elke VM in de schaalset. Voor elk VM-exemplaar worden een doel-IP-adres en TCP-poort weergegeven. Als het doel-IP-adres bijvoorbeeld *104.42.1.19* is en de TCP-poort *50001*, maakt u als volgt verbinding met het VM-exemplaar:

- Voor een Windows-schaalset maakt u verbinding maken met het VM-exemplaar met RDP op `104.42.1.19:50001`
- Voor een Linux-schaalset maakt u verbinding maken met het VM-exemplaar met SSH op `ssh azureuser@104.42.1.19 -p 50001`

Voer desgevraagd de referenties in die u hebt opgegeven in de vorige stap bij het maken van de schaalset. De exemplaren in de schaalset zijn gewone VM's waarmee u als elke andere VM kunt werken. Zie [Toepassing implementeren in schaalsets voor virtuele machines](virtual-machine-scale-sets-deploy-app.md) voor meer informatie over het implementeren en uitvoeren van toepassingen op exemplaren in een schaalset.


## <a name="clean-up-resources"></a>Resources opschonen
De resourcegroep, de schaalset en alle gerelateerde resources kunt u verwijderen wanneer u deze niet meer nodig hebt. Om dit te doen, selecteert u de resourcegroep voor de VM en klikt u op **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u via Azure Portal een eenvoudige schaalset voor virtuele machines gemaakt. Als u meer wilt weten over schaalsets, gaat u verder met de zelfstudie voor het maken en beheren van schaalsets voor virtuele Azure-machines.

> [!div class="nextstepaction"]
> [Schaalsets voor virtuele Azure-machines maken en beheren](tutorial-create-and-manage-powershell.md)