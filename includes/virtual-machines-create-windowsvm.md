---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: d4075f644ed1ce42479e35544f0e1442488f78eb
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
ms.locfileid: "29958908"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Beginnen in de linkerbovenhoek, klikt u op **maken van een resource** > **Compute** > **Windows Server 2016 Datacenter**.

    ![Naar de installatiekopieën van de virtuele machine van Azure in de portal navigeren](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. Selecteer het model van de klassieke implementatie in het Windows Server 2016 Datacenter. Klik op Maken.

    ![Schermafbeelding van de installatiekopieën voor virtuele machines in Azure die in de portal beschikbaar zijn](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Blade Grondbeginselen

Op de blade Grondbeginselen worden administratieve gegevens voor de virtuele machine opgevraagd.

1. Voer een **Naam** in voor de virtuele machine. In het voorbeeld is _HeroVM_ de naam van de virtuele machine. De naam moet 1-15 tekens lang zijn en mag geen speciale tekens bevatten.

2. Voer een **Gebruikersnaam** en een sterk **Wachtwoord** in om te gebruiken voor het maken van een lokaal account op de virtuele machine. Het lokale account wordt gebruikt voor aanmelding bij en beheer van de VM. In het voorbeeld is _azureuser_ de naam van de gebruiker.

 Het wachtwoord moet 8 tot 123 tekens lang zijn en aan drie van de vier volgende complexiteitsvereisten voldoen: ten minste één kleine letter, één hoofdletter, één cijfer en één speciaal teken. Zie meer informatie over [vereisten voor gebruikersnaam en wachtwoord](../articles/virtual-machines/windows/faq.md).

3. Het **abonnement** is optioneel. Een algemene instelling is 'Betalen naar gebruik'.

4. Selecteer een bestaande **Resourcegroep** of typ de gewenste naam voor een nieuwe resourcegroep. In het voorbeeld is _HeroVMRG_ de naam van de resourcegroep.

5. Selecteer een **locatie** van het Azure Datacenter waar u de virtuele machine wilt uitvoeren. In het voorbeeld is **VS - oost** de locatie.

6. Wanneer u klaar bent, klikt u op **Volgende** om door te gaan naar de volgende blade.

    ![Schermafbeelding met de instellingen op de blade Grondbeginselen voor het configureren van een virtuele machine in Azure](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Blade Grootte

Op de blade Grootte worden de configuratiegegevens van de virtuele machine en een lijst met verschillende opties weergegeven, waaronder het besturingssysteem, het aantal processors, het type schijfopslag en de geschatte maandelijkse gebruikskosten.  

Kies een grootte voor de VM en klik vervolgens op **Selecteren** om door te gaan. In dit voorbeeld is _DS1_\__V2 Standard_ de VM-grootte.

  ![Schermafbeelding van de blade Grootte, waarop de groottes worden getoond die u kunt selecteren voor een virtuele machine in Azure](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Blade Instellingen

Op de blade Instellingen worden de opslag- en netwerkopties opgevraagd. U kunt de standaardinstellingen accepteren. Azure maakt waar nodig de juiste vermeldingen.

Als u voor uw virtuele machine een grootte hebt geselecteerd die hierdoor wordt ondersteund, kunt u Azure Premium Storage uitproberen. Selecteer hiervoor in Schijftype de optie Premium SSD.

Wanneer u alle wijzigingen hebt aangebracht, klikt u op **OK**.

## <a name="4-summary-blade"></a>4. Blade Samenvatting

De blade Samenvatting geeft een lijst weer met de instellingen die zijn opgegeven op de vorige blades. Klik op **OK** wanneer u klaar bent om de installatiekopie te maken.

 ![Rapport van de blade Samenvatting met de opgegeven instellingen van de virtuele machine](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

Nadat de virtuele machine is gemaakt, wordt de nieuwe virtuele machine in de portal onder **Alle resources** weergegeven. Op het dashboard wordt een tegel van de virtuele machine weergegeven. Ook worden de bijbehorende cloudservice en het opslagaccount gemaakt en weergegeven. Zowel de virtuele machine als de cloudservice wordt automatisch gestart. Deze krijgen de status **Actief**.

 ![VM-agent en de eindpunten van de virtuele machine configureren](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
