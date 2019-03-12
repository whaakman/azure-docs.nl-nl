---
title: Een Azure-compatibele VHD maken voor de Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u een VHD voor een virtuele machine-aanbieding maken in de Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/27/2018
ms.author: pbutlerm
ms.openlocfilehash: bd085804f532811e71d421e3f683c50c1c3416cb
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57575468"
---
# <a name="create-an-azure-compatible-vhd"></a>Een Azure-compatibele VHD maken

Dit artikel vindt u de stappen die zijn vereist voor het maken van een virtuele harde schijf (VHD) voor de aanbieding van een virtuele machine (VM) in de Azure Marketplace.  Dit omvat ook aanbevolen procedures voor diverse aspecten, zoals met behulp van de Remote Desktop Protocol (RDP), het selecteren van een grootte voor de virtuele machine, de meest recente Windows-updates te installeren en generaliseren van de VHD-installatiekopie.  De volgende secties voornamelijk gericht op windows gebaseerde VHD's; Zie voor meer informatie over het maken van VHD's op basis van Linux [Linux op door Azure onderschreven distributies](../../../virtual-machines/linux/endorsed-distros.md). 

> [!WARNING]
> Het is raadzaam dat u Volg de instructies in dit onderwerp om Azure te gebruiken om een VM met een vooraf geconfigureerde, onderschreven besturingssysteem te maken.  Als dit niet compatibel met uw oplossing is, is het mogelijk te maken en configureren van een on-premises virtuele machine met behulp van een goedgekeurde besturingssysteem.  U kunt vervolgens configureren en voorbereiden voor upload, zoals beschreven in [voorbereiden van een Windows VHD of VHDX te uploaden naar Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).


## <a name="select-an-approved-base"></a>Selecteer een goedgekeurde base
Het besturingssysteem VHD voor uw VM-installatiekopie moet worden gebaseerd op een Azure goedgekeurde basisinstallatiekopie met Windows Server of SQL Server.
Om te beginnen een VM maken van een van de volgende installatiekopieën te vinden op de Microsoft Azure portal:

-   Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/), [2012 Datacenter](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/), [2008 R2 SP1](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/))
-   [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)
-   [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)

> [!TIP]
> Als u van de huidige Azure-portal of PowerShell gebruikmaakt, worden Windows Server-installatiekopieën gepubliceerd op 8 September 2014 en hoger goedgekeurd.

U kunt ook biedt Azure een scala aan goedgekeurde Linux-distributies.  Zie voor een huidige lijst [Linux op door Azure onderschreven distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).


## <a name="create-vm-in-the-azure-portal"></a>Virtuele machine maken in Azure portal 

In de Microsoft [Azure-portal](https://ms.portal.azure.com/), maken van de basisinstallatiekopie met behulp van de volgende stappen uit.

1. Meld u aan bij de portal met het Microsoft-account voor het Azure-abonnement dat u wilt publiceren van uw VM-aanbieding.
2. Maak een nieuwe resourcegroep en geef uw **groepsnaam voor Accountresources**, **abonnement**, en **resourcegroeplocatie**.  Zie voor meer richtlijnen [-resourcegroepen beheren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
3. Klik op **virtuele machines** in de menubalk linksboven om weer te geven van de pagina met details van virtuele machines. 
4. In deze nieuwe pagina, klikt u op **+ toevoegen** om weer te geven de **Compute** blade.  Als u het VM-type niet op het eerste scherm ziet, u kunt zoeken naar de naam van de basis-VM, bijvoorbeeld:

    ![COMPUTE-blade van de nieuwe virtuele machine](./media/publishvm_014.png)

5. Nadat u de installatiekopie van het juiste virtuele hebt geselecteerd, geeft u de volgende waarden:
  * Op de **basisbeginselen** blade, voer een **naam** voor de virtuele machine, tussen 1-15 alfanumerieke tekens. (In dit voorbeeld wordt `DemoVm009`.)
  * Voer een **gebruikersnaam** en een sterk **wachtwoord**, die worden gebruikt om te maken van een lokaal account op de virtuele machine.  (Hier `adminUser` wordt gebruikt.)  Het wachtwoord moet 8 tot 123 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken. Zie voor meer informatie, [gebruikersnaam en wachtwoord vereisten](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-faq#what-are-the-username-requirements-when-creating-a-vm).
  * Selecteer de resourcegroep die u hebt gemaakt (hier `DemoResourceGroup`).
  * Selecteer een Azure-Datacenter **locatie** (hier `West US`).
  * Klik op **OK** deze waarden worden opgeslagen. 

6.  Selecteer de grootte van de virtuele machine te implementeren met behulp van de volgende aanbevelingen:
  * Als u van plan de VHD bent op locatie ontwikkelen, is de grootte niet van belang. Overweeg het gebruikt van een van de kleinere VM's.
  * Als u van plan bent de installatiekopie in Azure te ontwikkelen, kunt u het gebruik van een van de aanbevolen VM-groottes voor de geselecteerde installatiekopie overwegen.
  * Raadpleeg voor informatie over de prijzen, de **aanbevolen Prijscategorieën** selector weergegeven op de portal. De drie aanbevolen groottes die is geleverd door de uitgever worden weergegeven. (Hier is de uitgever van Microsoft.)

    ![Blade grootte van de nieuwe virtuele machine](./media/publishvm_015.png)

7. In de **instellingen** blade, stel de **gebruik beheerde schijf** optie naar **geen**.  Hiermee kunt u de nieuwe VHD handmatig beheren. (De **instellingen** blade kunt u andere wijziging wijzigen ook de opslag- en netwerkopties, bijvoorbeeld selecteren **Premium (SSD)** in **schijftype**.)  Klik op **OK** om door te gaan.

    ![De instellingenblade van de nieuwe virtuele machine](./media/publishvm_016.png)

8. Klik op **Samenvatting** om uw keuzes te bekijken. Als u het bericht **Validatie geslaagd** ziet, klikt u op **OK**.

    ![Blade samenvatting van de nieuwe virtuele machine](./media/publishvm_017.png)

Azure begint met het inrichten van de virtuele machine die u hebt opgegeven.  U kunt de voortgang volgen door te klikken op **virtuele Machines** tabblad links.  Nadat deze is gemaakt, wordt de status gewijzigd naar **met**.  Op dat moment kunt u [verbinding maken met de virtuele machine](./cpp-connect-vm.md).


## <a name="next-steps"></a>Volgende stappen

Als u problemen bij het maken van uw nieuwe Azure op basis van VHD optreden, Raadpleeg [veelvoorkomende problemen tijdens het maken van VHD](./cpp-common-vhd-creation-issues.md).  Anders vervolgens moet u [verbinding maken met de virtuele machines](./cpp-connect-vm.md) u hebt gemaakt in Azure. 
