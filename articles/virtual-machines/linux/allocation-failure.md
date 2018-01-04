---
title: Het Linux-VM toewijzingsfouten oplossen | Microsoft Docs
description: Toewijzingsfouten bij het maken, opnieuw opstarten of het formaat van een Linux VM in Azure oplossen
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 626968c463d76abe6becaa85813336567f108d0d
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Toewijzingsfouten bij het maken, opnieuw opstarten of het formaat van Linux virtuele machines in Azure oplossen
Wanneer u een virtuele machine maken, gestopt (toewijzing ongedaan gemaakt) virtuele machines opnieuw opstarten of het formaat van een VM, wijst Microsoft Azure compute-bronnen aan uw abonnement. Tijd tot tijd krijgt u fouten bij het uitvoeren van deze bewerkingen--zelfs voordat u de Azure-abonnement limieten bereiken. Dit artikel wordt uitgelegd van de oorzaken van een deel van de algemene toewijzingsfouten en mogelijk doorvoeren wordt voorgesteld. De gegevens zijn mogelijk ook nuttig bij het plannen van de implementatie van uw services. U kunt ook [toewijzingsfouten bij het maken, opnieuw opstarten of vergroten of verkleinen Windows virtuele machines in Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

