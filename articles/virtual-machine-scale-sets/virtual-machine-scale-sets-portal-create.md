---
title: Maak een virtuele-Machineschaalset met de Azure portal | Microsoft Docs
description: Met Azure portal-schaalsets implementeren.
keywords: Virtuele-machineschaalsets
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Het maken van een virtuele-Machineschaalset met de Azure-portal
Deze zelfstudie laat zien hoe eenvoudig het is een virtuele-Machineschaalset maken in een paar minuten met behulp van de Azure-portal. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Een installatiekopie voor de virtuele machine kiezen in de marketplace
U kunt een schaal CentOS, virtuele CoreOS, Debian, Ubuntu Server, andere Linux-installatiekopieën en installatiekopieën van Windows Server in te stellen eenvoudig implementeren vanuit de portal.

Eerst, navigeer naar de [Azure-portal](https://portal.azure.com) in een webbrowser. Klik op **nieuw**, zoeken naar **schaalset**, en selecteer vervolgens de **virtuele-machineschaalset** post:

![virtuele machine van Azure-portal zoeken schaalset](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>De schaalset maken
U kunt nu de standaardinstellingen gebruiken en de schaalaanpassingsset snel maken.

* Voer een naam voor de scale-set.  
Deze naam wordt het grondtal van de FQDN-naam van de load balancer voor de schaalaanpassingsset, dus zorg ervoor dat de naam uniek is in alle Azure.

* Selecteer het gewenste OS-type.

* De gewenste gebruikersnaam invoeren en selecteer het verificatietype u liever.  
Als u een wachtwoord kiest, moet deze zijn minstens 12 tekens lang en drie van de volgende vier complexiteitsvereisten voldoen aan: één kleine letter, één hoofdletter, één cijfer en één speciaal teken. Zie meer informatie over [vereisten voor gebruikersnaam en wachtwoord](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Als u ervoor kiest **openbare SSH-sleutel**, zorg ervoor dat alleen plakken in uw openbare sleutel *niet* uw persoonlijke sleutel:

* Selecteer **Ja** of **Nee** naar **inschakelen schalen voorbij 100 exemplaren**.  
Als u Ja, de schaal ingesteld kan meerdere plaatsing groepen overspannen. Zie voor meer informatie [deze documentatie](./virtual-machine-scale-sets-placement-groups.md).

* Selecteer een geschikte **exemplaar grootte**.  
Raadpleeg voor meer informatie over de grootte van virtuele machines, [Windows VM-grootten](..\virtual-machines\windows\sizes.md) of [Linux VM-grootten](..\virtual-machines\linux\sizes.md).

* Geef uw naam gewenste resourcegroep en locatie.  
Als uw regio en **exemplaar grootte** beschikbaarheid zones, ondersteunt de **beschikbaarheid zones** veld is ingeschakeld. Zie voor meer informatie over beschikbaarheid zones [overzicht](../availability-zones/az-overview.md) artikel.

* Geef de gewenste domeinnaamlabel (de basis van de FQDN-naam voor de load balancer voor de schaalaanpassingsset).  
Dit label moet uniek zijn binnen alle Azure.

* Kies uw schijfimage gewenste besturingssysteem, het aantal exemplaren en de grootte van de machine.

* De gewenste schijf kiezen: beheerd of onbeheerd.  
Zie voor meer informatie [deze documentatie](./virtual-machine-scale-sets-managed-disks.md). Als u hebt gekozen om de schaal instelt span meerdere plaatsing groepen, deze optie niet meer beschikbaar omdat de beheerde schijf is vereist voor schaalsets meerdere groepen voor plaatsing.

* In- of uitschakelen voor automatisch schalen en configureren als ingeschakeld.

![virtuele machine van Azure scale set portal vragen maken](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbinding maken met een virtuele machine in de schaalset
Als u wilt beperken schaal ingesteld op een enkele plaatsing-groep, wordt klikt u vervolgens de schaalaanpassingsset geïmplementeerd met NAT-regels geconfigureerd waarmee u verbinding maken met de schaal eenvoudig ingesteld (zo niet, verbinding maken met de virtuele machines in de schaalset, u waarschijnlijk hoeft te maken van een jumpbox in dezelfde  virtueel netwerk als de schaal instellen). Om ze te bekijken, gaat u naar de `Inbound NAT Rules` tabblad van de load balancer voor de schaalaanpassingsset:

![virtuele machine van Azure-portal nat-regels schaalset](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

U kunt verbinding maken met elke virtuele machine in de schaal instelt met behulp van deze NAT-regels. Bijvoorbeeld: voor een Windows-scale set, als er een NAT-regel op de binnenkomende poort 50000 u kan verbinding maken met die machine via RDP op `<load-balancer-ip-address>:50000`. Voor een Linux-scale set, zou u verbinding met de opdracht `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Volgende stappen
Zie voor documentatie over het implementeren van de schaal wordt ingesteld vanuit de CLI, [deze documentatie](virtual-machine-scale-sets-cli-quick-create.md).

Zie voor documentatie over het implementeren van de schaal wordt ingesteld vanuit PowerShell, [deze documentatie](virtual-machine-scale-sets-windows-create.md).

Zie voor documentatie over het implementeren van de schaal wordt ingesteld vanuit Visual Studio, [deze documentatie](virtual-machine-scale-sets-vs-create.md).

Bekijk voor algemene documentatie, de [documentatie overzichtspagina voor schaalsets](virtual-machine-scale-sets-overview.md).

Bekijk voor algemene informatie, de [belangrijkste startpagina voor schaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

