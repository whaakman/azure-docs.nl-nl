---
title: Schijfversleuteling in Azure Security Center toe te passen | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **schijfversleuteling toepassen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: c8a2caf2826069824a993294f4eba514ea870d9c
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307372"
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Schijfversleuteling in Azure Security Center toepassen
Azure Security Center adviseert schijfversleuteling toe te passen als u Windows of Linux-VM-schijven die niet zijn versleuteld met Azure Disk Encryption hebt. Schijfversleuteling kunt u uw Windows- en Linux IaaS-VM-schijven te versleutelen.  Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM.

Disk Encryption gebruikt de industrienorm [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux. Deze functies bieden besturingssysteem en versleuteling om u te helpen te beschermen en beveiligen van uw gegevens en voldoen aan de beveiligings- en nalevingsverplichtingen van de organisatie. Schijfversleuteling is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de sleutels en geheimen in uw Key Vault-abonnement, terwijl u ervoor te zorgen dat alle gegevens in de VM-schijven worden versleuteld in rust in uw [Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk Encryption wordt ondersteund op de volgende Windows server-besturingssystemen: Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Schijfversleuteling wordt ondersteund op de volgende Linux-serverbesturingssystemen - Ubuntu-, CentOS, SUSE en SUSE Linux Enterprise Server (SLES).
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **schijfversleuteling toepassen**.
2. In de **schijfversleuteling toepassen** blade ziet u een lijst met virtuele machines waarvoor schijfversleuteling wordt aanbevolen.
3. Volg de instructies voor het toepassen van versleuteling op deze VM's.

![][1]

Voor het versleutelen van Azure Virtual Machines die zijn geïdentificeerd door Security Center moeten, raden we aan de volgende stappen uit:

* Installeer en configureer Azure PowerShell. Hiermee kunt u de PowerShell-opdrachten voor het instellen van de vereisten voor het versleutelen van virtuele Machines van Azure worden uitgevoerd.
* Verkrijgen en de Azure Disk Encryption vereisten Azure PowerShell-script uitvoeren.
* Versleutel uw virtuele machines.

[Versleutelen van een virtuele Windows IaaS-machine met Azure PowerShell](../security/quick-encrypt-vm-powershell.md) leidt u door deze stappen. In dit onderwerp wordt ervan uitgegaan dat u een Windows client-computer van waaruit u schijfversleuteling configureren.

Er zijn veel manieren die kunnen worden gebruikt voor Azure Virtual Machines. Als u al goed bekend bent met Azure PowerShell of Azure CLI, maakt u mogelijk liever gebruik van een andere manier. Zie voor meer informatie over deze andere methoden, [Azure disk encryption](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center "Toepassen schijfversleuteling." Voor meer informatie over schijfversleuteling, Zie de volgende:

* [Versleuteling en sleutelbeheer beheer met Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 min 39 sec): informatie over het gebruik van versleuteling Schijfbeheer voor IaaS-VM's en Azure Key Vault om te beschermen en beveiligen van uw gegevens.
* [Azure disk encryption](../security/azure-security-disk-encryption-overview.md) (document): informatie over het inschakelen van disk encryption voor Windows en Linux-machines.

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-azure-policy.md) --informatie over het configureren van beveiligingsbeleid.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
