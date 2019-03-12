---
title: Virtuele machines migreren naar Resource Manager met Azure CLI | Microsoft Docs
description: Dit artikel helpt bij het platform ondersteunde migratie van resources van klassiek naar Azure Resource Manager met behulp van Azure CLI
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 34dad39e3784dd0bc73e3be108d6b31d4f479a1e
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57543267"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migreren IaaS-resources van klassiek naar Azure Resource Manager met behulp van Azure CLI
Deze stappen ziet u hoe u Azure-opdrachtregelinterface (CLI)-opdrachten gebruikt voor het migreren van infrastructuur als een service (IaaS)-resources van het klassieke implementatiemodel naar het Azure Resource Manager-implementatiemodel. In het artikel moet de [Azure klassieke CLI](../../cli-install-nodejs.md). Omdat Azure CLI alleen van toepassing voor Azure Resource Manager-resources is, kan deze niet worden gebruikt voor de migratie.

> [!NOTE]
> Alle bewerkingen die hier worden beschreven zijn idempotent. Als er een probleem met uitzondering van een niet-ondersteunde functie of een Configuratiefoutbericht wordt weergegeven, raden wij aan dat u het opnieuw de voorbereiden proberen afgebroken of doorgevoerd, bewerking. Het platform probeert vervolgens de actie opnieuw uit.
> 
> 

<br>
Hier volgt een stroomdiagram voor het identificeren van de volgorde waarin stappen moeten worden uitgevoerd tijdens een migratie

![Schermafbeelding van de migratiestappen](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>Stap 1: Voorbereiden voor migratie
Hier volgen enkele aanbevolen procedures die wij adviseren kijkt u bij het evalueren van migratie IaaS-resources van klassiek naar Resource Manager:

* Lees de [lijst van niet-ondersteunde configuraties of functies](../windows/migration-classic-resource-manager-overview.md). Als u virtuele machines die gebruikmaken van niet-ondersteunde configuraties of functies hebt, raden wij u wacht de functieconfiguratie /-ondersteuning wordt aangekondigd. U kunt ook deze functie te verwijderen of verplaatsen uit dat de configuratie van het inschakelen van de migratie als deze aan uw behoeften voldoet.
* Als u beschikken over een scripts die vandaag nog uw infrastructuur en toepassingen implementeren geautomatiseerde, proberen te maken van een vergelijkbare installatie van de test met behulp van deze scripts voor migratie. U kunt ook voorbeeldomgevingen instellen met behulp van de Azure-portal.

> [!IMPORTANT]
> Toepassingsgateways zijn momenteel niet ondersteund voor migratie van klassiek naar Resource Manager. Als u wilt migreren van een klassiek virtueel netwerk met een Application gateway, moet u de gateway verwijderen voordat u een voorbereidingsbewerking voor het verplaatsen van het netwerk uitvoert. Nadat u de migratie hebt voltooid, sluit u de gateway in Azure Resource Manager. 
>
>ExpressRoute-gateways verbinding maakt met ExpressRoute-circuits in een ander abonnement worden niet automatisch gemigreerd. In dergelijke gevallen de ExpressRoute-gateway verwijderen, te migreren van het virtuele netwerk en maak de gateway opnieuw. Raadpleeg [migreren ExpressRoute-circuits en gekoppelde virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md) voor meer informatie.
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Stap 2: Uw abonnement instellen en de provider registreren
Voor migratiescenario's, moet u voor het instellen van uw omgeving voor zowel klassieke en Resource Manager. [Azure CLI installeren](../../cli-install-nodejs.md) en [Selecteer uw abonnement](/cli/azure/authenticate-azure-cli).

Meld u aan uw account.

    azure login

Selecteer het Azure-abonnement met behulp van de volgende opdracht uit.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> De registratie is een tijdstap, maar deze moet worden uitgevoerd zodra voordat u probeert te migreren. Zonder te registreren ziet u de volgende strekking weergegeven 
> 
> *BadRequest: Abonnement is niet geregistreerd voor migratie.* 
> 
> 

Registreren bij de resourceprovider voor migratie met behulp van de volgende opdracht uit. Houd er rekening mee dat in sommige gevallen kan deze opdracht optreedt time-out. De registratie is geslaagd.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Wacht vijf minuten voor de registratie te voltooien. U kunt de status van de goedkeuring controleren met behulp van de volgende opdracht uit. Zorg ervoor dat RegistrationState `Registered` voordat u doorgaat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Nu visuele CLI voor het `asm` modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 3: Zorg ervoor dat er voldoende virtuele Machine van Azure Resource Manager-vcpu's in de Azure-regio van uw huidige implementatie of VNET
Voor deze stap moet u overschakelen naar `arm` modus. Dit doen met de volgende opdracht uit.

```
azure config mode arm
```

U kunt de volgende CLI-opdracht gebruiken om te controleren of het huidige aantal vcpu's die u in Azure Resource Manager hebt. Zie voor meer informatie over vCPU-quota, [limieten en de Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Wanneer u klaar bent voor het verifiëren van deze stap die u kunt terugschakelen naar `asm` modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Stap 4: Optie 1 - virtuele machines in een service in de cloud migreren
De lijst met cloudservices ophalen met behulp van de volgende opdracht uit en kies vervolgens de service in de cloud die u wilt migreren. Merk op dat als de virtuele machines in de cloudservice zich in een virtueel netwerk of als ze web/worker-rollen hebben, u een foutbericht weergegeven krijgt.

    azure service list

Voer de volgende opdracht om op te halen van de naam van de implementatie voor de cloudservice in de uitgebreide uitvoer. In de meeste gevallen is de implementatienaam van de hetzelfde als de naam van de cloudservice.

    azure service show <serviceName> -vv

Valideer eerst als u de cloudservice met de volgende opdrachten kunt migreren:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Bereid de virtuele machines in de cloudservice voor migratie. U hebt twee opties om uit te kiezen.

Als u wilt dat de virtuele machines migreren naar een virtueel netwerk met platform is gemaakt, gebruikt u de volgende opdracht uit.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Als u migreren naar een bestaand virtueel netwerk in het Resource Manager-implementatiemodel wilt, gebruikt u de volgende opdracht uit.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Nadat de voorbereidingsbewerking voltooid is, kunt u zoeken via de uitgebreide uitvoer voor het ophalen van de migratiestatus van de virtuele machines en ervoor te zorgen dat deze zich in de `Prepared` staat.

    azure vm show <vmName> -vv

Controleer de configuratie voor de voorbereide resources met behulp van CLI of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude staat, gebruikt u de volgende opdracht uit.

    azure service deployment abort-migration <serviceName> <deploymentName>

Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Stap 4: Optie 2 - migreren van virtuele machines in een virtueel netwerk
Kies het virtuele netwerk dat u wilt migreren. Houd er rekening mee dat als het virtuele netwerk web/werkrollen of VM's met niet-ondersteunde configuraties bevat, u een validatiefoutbericht krijgt.

De virtuele netwerken in het abonnement ophalen met behulp van de volgende opdracht uit.

    azure network vnet list

De uitvoer ziet er ongeveer als volgt:

![Schermopname van de opdrachtregel met de naam van de volledige virtuele netwerk is gemarkeerd.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

In het bovenstaande voorbeeld de **virtualNetworkName** is de volledige naam **"Groep classicubuntu16 classicubuntu16"**.

Valideer eerst als u het virtuele netwerk met de volgende opdracht kunt migreren:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Het virtuele netwerk van uw keuze voorbereiden voor migratie met behulp van de volgende opdracht uit.

    azure network vnet prepare-migration <virtualNetworkName>

Controleer de configuratie voor de voorbereide virtuele machines met behulp van CLI of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude staat, gebruikt u de volgende opdracht uit.

    azure network vnet abort-migration <virtualNetworkName>

Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Stap 5: Migreren van een storage-account
Wanneer u klaar bent de virtuele machines migreert, wordt aangeraden migreren van de storage-account.

Het opslagaccount voorbereiden voor migratie met behulp van de volgende opdracht uit

    azure storage account prepare-migration <storageAccountName>

Controleer de configuratie voor de voorbereide storage-account met behulp van CLI of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude staat, gebruikt u de volgende opdracht uit.

    azure storage account abort-migration <storageAccountName>

Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-resources van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over migratie IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
