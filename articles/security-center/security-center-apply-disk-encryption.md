---
title: Schijf coderen in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** schijf versleuteling ** van toepassing.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 67cff664f3723b2194ecd1519729cca17069d07f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Schijfversleuteling in Azure Security Center toepassen
Azure Security Center adviseert schijfversleuteling toe te passen als u Windows of Linux-VM-schijven die niet zijn versleuteld met behulp van Azure Disk Encryption hebt. Schijfversleuteling kunt u de schijven voor Windows en Linux IaaS VM versleutelen.  Versleuteling wordt aanbevolen voor het besturingssysteem en voor de gegevensvolumes op de VM.

Schijfversleuteling maakt gebruik van het industrie-initiatief [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) functie van Linux. Deze functies bieden OS en gegevensversleuteling om te helpen beveiligen en uw gegevens beschermen en voldoen aan uw organisatie beveiliging en naleving verplichtingen. Schijfversleuteling is geïntegreerd met [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) om te controleren en beheren van de schijf versleutelingssleutels en geheimen in uw abonnement Sleutelkluis, terwijl u ervoor te zorgen dat alle gegevens in de VM-schijven zijn versleuteld in rust in uw [Azure Storage](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk Encryption wordt ondersteund op de volgende Windows-serverbesturingssystemen - Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2. Schijfversleuteling wordt ondersteund op de volgende Linux-serverbesturingssystemen - Ubuntu, CentOS, SUSE en SUSE Linux Enterprise Server (SLES).
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **schijfversleuteling toepassen**.
2. In de **schijfversleuteling toepassen** blade ziet u een lijst van virtuele machines waarvoor schijfversleuteling wordt aanbevolen.
3. Volg de instructies voor het toepassen van versleuteling op deze virtuele machines.

![][1]

Voor het versleutelen van Azure Virtual Machines die zijn geïdentificeerd door Security Center welke, raden we aan de volgende stappen uit:

* Installeer en configureer Azure PowerShell. Hiermee kunt u de vereist voor het instellen van de vereiste onderdelen voor het versleutelen van virtuele Machines van Azure PowerShell-opdrachten uit te voeren.
* Verkrijgen en voer het Azure schijf versleuteling vereisten Azure PowerShell-script.
* Versleutel uw virtuele machines.

[Een virtuele Machine van Azure versleutelen](security-center-disk-encryption.md) leidt u door deze stappen.  In dit onderwerp wordt ervan uitgegaan dat u gebruikmaakt van Windows 10 als clientmachine waarin u schijfversleuteling configureren.

Er zijn veel manieren die kunnen worden gebruikt voor Azure Virtual Machines. Als u al goed bekend bent met Azure PowerShell of Azure CLI, maakt u mogelijk liever gebruik van een andere manier. Zie voor meer informatie over deze andere benaderingen, [Azure disk encryption](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u implementeert de aanbeveling Security Center 'Toepassen disk encryption'. Voor meer informatie over schijfversleuteling, Zie de volgende:

* [Versleuteling en sleutel-beheer met Azure Key Vault](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video 36 min 39 sec)--informatie over het gebruik van versleuteling Schijfbeheer voor IaaS VM's en Azure Key Vault voor beveiliging en bescherming van uw gegevens.
* [Een virtuele Machine van Azure versleutelen](security-center-disk-encryption.md) (document)--informatie over het Azure Virtual Machines versleutelen.
* [Azure schijfversleuteling](../security/azure-security-disk-encryption.md) (document)--informatie over het inschakelen van schijfversleuteling voor Windows en Linux-machines.

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) --informatie over het configureren van beveiligingsbeleid.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
