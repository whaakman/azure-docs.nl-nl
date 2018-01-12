---
title: Een virtuele-Machineschaalset maken in de Azure portal | Microsoft Docs
description: Informatie over het snel een virtuele-machineschaalset maken in de Azure portal
keywords: Virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a501a852a317ec7d087904c3a675ebefce1bece0
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Een virtuele-Machineschaalset maken in de Azure portal
Met een virtuele-machineschaalset kunt u een reeks identieke virtuele machines met automatisch schalen implementeren en beheren. U kunt het aantal VM’s in de schaalset handmatig schalen of regels voor automatisch schalen definiëren op basis van resourcegebruik zoals CPU, vraag naar geheugen, of netwerkverkeer. In dit ophalen gestart artikel maakt u een virtuele-machineschaalset ingesteld in de Azure-portal. U kunt ook maken met een schaal ingesteld met de [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) of [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u via http://portal.azure.com aan bij Azure Portal.


## <a name="create-virtual-machine-scale-set"></a>Virtuele-machineschaalset maken
U kunt een schaal ingesteld met een installatiekopie van Windows Server of Linux-installatiekopie zoals RHEL, CentOS, Ubuntu of SLES implementeren.

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Zoeken naar *schaalset*, kies **virtuele-machineschaalset**, selecteer daarna **maken**.
3. Voer een naam voor de schaal is ingesteld, zoals *myScaleSet*.
4. Selecteer het gewenste OS-type, zoals *Windows Server 2016 Datacenter*.
5. Voer de naam van uw gewenste resource, zoals *myResourceGroup*, en de locatie, zoals *VS-Oost*.
6. De gewenste gebruikersnaam invoeren en selecteer het verificatietype u liever.
    - Een **wachtwoord** moet ten minste 12 tekens lang zijn en drie van de volgende vier complexiteitsvereisten voldoen aan: één kleine letter, één hoofdletter, één cijfer en één speciaal teken. Zie voor meer informatie [gebruikersnaam en wachtwoord vereisten](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Als u de installatiekopie van een Linux-besturingssysteem-schijf selecteert, kunt u in plaats daarvan kiezen **openbare SSH-sleutel**. Geef alleen de openbare sleutel, zoals *~/.ssh/id_rsa.pub*. U kunt de Azure-Cloud-Shell vanuit de portal te gebruiken [maken en gebruiken van SSH-sleutels](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Voer een **naam openbare IP-adres**, zoals *myPublicIP*.
8. Voer een unieke **domeinnaamlabel**, zoals *myuniquedns*. Deze DNS-label vormt de basis van de FQDN-naam voor de load balancer voor de scale-set.
9. Selecteer om te bevestigen de schaal opties instellen, **maken**.

    ![Maken van een virtuele-machineschaalset ingesteld in de Azure portal](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbinding maken met een virtuele machine in de schaalset
Wanneer u een schaal ingesteld in de portal maakt, wordt een load balancer gemaakt. Network Address Translation (NAT) regels worden gebruikt om verkeer naar de scale set exemplaren voor externe verbindingen zoals RDP of SSH kunt distribueren.

Instellen exemplaren om weer te geven deze NAT-regels en de verbindingsinformatie in voor uw scale:

1. Selecteer de resourcegroep die u in de vorige stap hebt gemaakt, zoals *myResourceGroup*.
2. Selecteer in de lijst met resources uw **Load balancer**, zoals *myScaleSetLab*.
3. Kies **binnenkomende NAT-regels** in het menu aan de linkerkant van het venster.

    ![Binnenkomende NAT-regels kunnen u verbinding maken met de VM-scale set instanties](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

U kunt verbinding maken met elke virtuele machine in de schaal instelt met behulp van deze NAT-regels. Elk VM-instantie bevat een doel-IP-adres en de waarde van de TCP-poort. Bijvoorbeeld, als het IP-doeladres *104.42.1.19* en de TCP-poort is *50001*, u verbinding maken met het VM-exemplaar als volgt:

- Voor een Windows-schaalset verbinding maken met de VM-instantie met RDP op`104.42.1.19:50001`
- Voor een Linux-schaalset verbinding maken met de VM-instantie met SSH op`ssh azureuser@104.42.1.19 -p 50001`

Voer desgevraagd de referenties die u hebt opgegeven van de vorige stap bij het maken van de scale-set. De schaal set-exemplaren zijn reguliere virtuele machines die u kunt met werken normaal. Zie voor meer informatie over het implementeren en uitvoeren van toepassingen op uw scale set instanties [implementeren van uw toepassing op virtuele-machineschaalsets](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Resources opschonen
Wanneer deze niet meer nodig, de resourcegroep verwijderen, schaalset en alle bijbehorende resources. Om dit te doen, selecteert u de resourcegroep voor de VM en klikt u op **Verwijderen**.


## <a name="next-steps"></a>Volgende stappen
In dit artikel van het ophalen gestart, moet u een basic schaal ingesteld in de Azure-portal gemaakt. Breid voor betere schaalbaarheid en automatisering uw schaalset uit met de volgende praktische artikelen:

- [Deploy your application on virtual machine scale sets](virtual-machine-scale-sets-deploy-app.md) (Uw toepassing implementeren op virtuele-machineschaalsets)
- Automatisch schalen met [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), de [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md) of [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Use automatic OS upgrades for your scale set VM instances](virtual-machine-scale-sets-automatic-upgrade.md) (Automatische upgrades voor het besturingssysteem gebruiken voor de VM-instanties in uw schaalset)
