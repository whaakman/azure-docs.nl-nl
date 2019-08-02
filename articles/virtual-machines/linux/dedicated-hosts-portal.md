---
title: Implementeer Azure dedicated hosts met behulp van de Azure Portal | Microsoft Docs
description: Implementeer Vm's op toegewezen hosts met behulp van de Azure Portal.
services: virtual-machines-linux
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/25/2019
ms.author: cynthn
ms.openlocfilehash: 03d8b5353a2dc9840c8231978243e7f94a5bff56
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68700390"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-portal"></a>Preview: Vm's implementeren op toegewezen hosts met behulp van de portal

Dit artikel begeleidt u bij het maken van een toegewezen Azure- [host](dedicated-hosts.md) voor het hosten van uw virtuele machines (vm's). 

[!INCLUDE [virtual-machines-common-dedicated-hosts-portal](../../../includes/virtual-machines-common-dedicated-hosts-portal.md)]

## <a name="create-a-vm"></a>Een virtuele machine maken

1. Kies in de linkerbovenhoek van de Azure-portal **Een resource maken**.
1. Zoek via het zoekvak boven de lijst met Azure Marketplace-resources naar **Ubuntu Server 16.04 LTS by Canonical**, selecteer deze en kies **Maken**.
1. Controleer op het tabblad **basis beginselen** onder **Project Details**of het juiste abonnement is geselecteerd en selecteer vervolgens *myDedicatedHostsRG* als de **resource groep**. 
1. Typ onder **Exemplaardetails** *myVM* als **Naam van de virtuele machine** en kies *US - oost* als de **Locatie**.
1. Selecteer in **beschikbaarheids opties** **beschikbaarheids zone**selecteren, selecteer *1* in de vervolg keuzelijst.
1. Voor de grootte selecteert u **grootte wijzigen**. Kies in de lijst met beschik bare grootten een van de Esv3-reeksen, zoals **Standard E2s v3**. Mogelijk moet u het filter wissen om alle beschik bare grootten weer te geven.
1. Selecteer onder **Administrator-account** **Openbare SSH-sleutel**, typ uw gebruikersnaam en plak uw openbare sleutel in het tekstvak. Verwijder eventuele voorloop- en volgspaties van uw openbare sleutel.

    ![Beheerdersaccount](./media/quick-create-portal/administrator-account.png)

1. Onder **Binnenkomende poort regels** > **open bare binnenkomende poorten**kiest u **geselecteerde poorten toestaan** en selecteert u **SSH (22)** in de vervolg keuzelijst. 
1. Selecteer boven aan de pagina het tabblad **Geavanceerd** en selecteer in de sectie **host** *MyHostGroup* voor **hostgroep** en *myHost* voor de **host**. 
    ![Hostgroep en host selecteren](./media/dedicated-hosts-portal/advanced.png)
1. Laat de resterende standaardwaarden staan ​​en selecteer vervolgens de knop **Beoordelen en maken** aan de onderkant van de pagina.
1. Wanneer u het bericht ziet dat de validatie is voltooid, selecteert u **maken**.

Het duurt een paar minuten voor uw virtuele machine is geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie het overzicht [gespecialiseerde hosts](dedicated-hosts.md) .

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)vindt u een voor beeld van een sjabloon, die zowel zones als fout domeinen gebruikt voor maximale tolerantie in een regio.

- U kunt ook een speciale host implementeren met behulp van de [Azure cli](dedicated-hosts-cli.md).



