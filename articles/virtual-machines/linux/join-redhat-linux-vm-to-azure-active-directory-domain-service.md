---
title: Een RedHat Linux VM koppelen aan een Azure Active Directory-DS | Microsoft Docs
description: Hoe een bestaande RedHat Enterprise Linux 7 VM koppelen aan een Azure Active Directory Domain Services.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/14/2016
ms.author: v-livech
ms.openlocfilehash: 68971372a3c9d9671ccad43db8d7b543d286fc7a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="join-a-redhat-linux-vm-to-an-azure-active-directory-domain-service"></a>Een RedHat Linux VM koppelen aan een Azure Active Directory Domain Services

In dit artikel laat zien hoe een virtuele machine met Red Hat Enterprise Linux (RHEL) 7 toevoegen aan een beheerd domein van Azure Active Directory Domain Services (AADDS).  De vereisten zijn:

- [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)

- [bestanden voor openbare en persoonlijke SSH-sleutels](mac-create-ssh-keys.md)

- [an Azure Active Directory Domain Services DC](../../active-directory-domain-services/active-directory-ds-getting-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Snelle opdrachten

_Geen voorbeelden vervangen door uw eigen instellingen._

### <a name="switch-the-azure-cli-to-classic-deployment-mode"></a>De azure-cli overschakelen naar de klassieke implementatiemodus

```azurecli
azure config mode asm
```

### <a name="search-for-a-rhel-version-and-image"></a>Zoeken naar een RHEL-versie en een installatiekopie

```azurecli
azure vm image list | grep "Red Hat"
```

### <a name="create-a-redhat-linux-vm"></a>Een Redhat Linux VM maken

```azurecli
azure vm create myVM \
-o a879bbefc56a43abb0ce65052aac09f3__RHEL_7_2_Standard_Azure_RHUI-20161026220742 \
-g ahmet \
-p myPassword \
-e 22 \
-t "~/.ssh/id_rsa.pub" \
-z "Small" \
-l "West US"
```

### <a name="ssh-to-the-vm"></a>SSH naar virtuele machine

```bash
ssh -i ~/.ssh/id_rsa ahmet@myVM
```

### <a name="update-yum-packages"></a>YUM pakketten bijwerken

```bash
sudo yum update
```

### <a name="install-packages-needed"></a>Installeren van pakketten nodig

```bash
sudo yum -y install realmd sssd krb5-workstation krb5-libs
```

Nu de vereiste pakketten zijn geïnstalleerd op de virtuele Linux-machine, de volgende taak is de virtuele machine toevoegen aan het beheerde domein.

### <a name="discover-the-aad-domain-services-managed-domain"></a>Het beheerde domein met AAD Domain Services detecteren

```bash
sudo realm discover mydomain.com
```

### <a name="initialize-kerberos"></a>Initialiseren van kerberos

Zorg ervoor dat u een gebruiker die lid is van de groep 'AAD DC Administrators' opgeven. Alleen deze gebruikers kunnen computers toevoegen aan het beheerde domein.

```bash
kinit ahmet@mydomain.com
```

### <a name="join-the-machine-to-the-domain"></a>De machine toevoegen aan het domein

```bash
sudo realm join --verbose mydomain.com -U 'ahmet@mydomain.com'
```

### <a name="verify-the-machine-is-joined-to-the-domain"></a>Controleer of dat de machine is gekoppeld aan het domein

```bash
ssh -l ahmet@mydomain.com mydomain.cloudapp.net
```

## <a name="next-steps"></a>Volgende stappen

* [Red Hat Update-infrastructuur (RHUI) voor on-demand Red Hat Enterprise Linux virtuele machines in Azure](update-infrastructure-redhat.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Sleutelkluis instellen voor virtuele machines in Azure Resource Manager](key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Implementeren en beheren van virtuele machines met behulp van Azure Resource Manager-sjablonen en de Azure CLI](../linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
