---
title: Meld u aan een Linux-VM met Azure Active Directory-referenties bij | Microsoft Docs
description: In deze procedure u informatie over het maken en configureren van een Linux-VM voor het gebruik van Azure Active Directory-verificatie voor gebruikersaanmeldingen
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/01/2018
ms.author: iainfou
ms.openlocfilehash: 1a2bb6def032d1790a67e458afb162402c1443a7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Aanmelden bij een virtuele Linux-machine in Azure met Azure Active Directory-verificatie (Preview)

Ter verbetering van de beveiliging van Linux virtuele machines (VM's) in Azure, kunt u integreren met Azure Active Directory (AD)-verificatie. Wanneer u Azure AD-verificatie voor virtuele Linux-machines, centraal beheren en beleid afdwingen dat toestaan of weigeren van toegang aan de virtuele machines. In dit artikel leest u hoe maken en configureren van een Linux-VM voor het gebruik van Azure AD-verificatie.

> [!NOTE]
> Deze functie is Preview-versie en wordt niet aanbevolen voor gebruik met productie virtuele machines of werkbelastingen. Gebruik deze functie op een virtuele machine die u verwacht te verwijderen nadat u hebt getest.

Er zijn veel voordelen van het gebruik van Azure AD-verificatie voor aanmelding bij Linux virtuele machines in Azure, met inbegrip van:

- **Verbeterde beveiliging:**
  - Aanmelden bij Azure Linux VM's kunt u uw zakelijke AD-referenties. Er is niet nodig lokale administrator-accounts maken en beheren van de levensduur van de referentie.
  - Vermindert uw vertrouwen op de lokale administrator-accounts, hoeft u niet te hoeven maken over verlies/diefstal van referenties, gebruikers zwakke verwijzingen enzovoort configureert.
  - De complexiteit van wachtwoorden en levensduur wachtwoordbeleid geconfigureerd voor uw Azure AD-directory kunnen beveiligde virtuele Linux-machines ook.
  - U kunt multi-factor authentication-server configureren voor verdere beveiligde aanmelding bij virtuele machines in Azure.

- **Naadloze samenwerking:** With Role-Based toegangsbeheer (RBAC), kunt u die bij een bepaalde virtuele machine als een gewone gebruiker of met administrator-bevoegdheden aanmelden kunt. Wanneer gebruikers lid of uw team laat, kunt u het beleid voor de virtuele machine toegang zo nodig het RBAC bijwerken. Deze ervaring is veel eenvoudiger dan hoeft te verwijderen van virtuele machines voor het verwijderen van onnodige openbare SSH-sleutels. Wanneer werknemers uw bedrijf verlaat en hun gebruikersaccount is uitgeschakeld of verwijderd uit Azure AD, hebben ze niet langer toegang tot uw resources.

### <a name="supported-azure-regions-and-linux-distributions"></a>Ondersteunde Azure-regio's en Linux-distributies

De volgende Linux-distributies worden momenteel ondersteund tijdens de preview van deze functie:

| Distributie | Versie |
| --- | --- |
| Ubuntu Server | Ubuntu 16.04 TNS en Ubuntu Server 17.10 |

De volgende Azure-regio's worden momenteel ondersteund tijdens de preview van deze functie:

- Zuid-centraal VS

>[!IMPORTANT]
> Voor het gebruik van deze preview-functie, moet u alleen een ondersteunde Linux-distro implementeren en in een ondersteunde Azure-regio.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.31 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-linux-virtual-machine"></a>Een virtuele Linux-machine maken

Maak een resourcegroep met [az groep maken](/cli/azure/group#az-group-create), maakt u een virtuele machine met [az vm maken](/cli/azure/vm#az-vm-create) met behulp van een ondersteunde distro en in een ondersteunde regio. Het volgende voorbeeld wordt geïmplementeerd voor een virtuele machine met de naam *myVM* die gebruikmaakt van *Ubuntu 16.04 LTS* in een resourcegroep met de naam *myResourceGroup* in de *southcentralus*  regio. U kunt uw eigen resourcegroep en de VM-namen naar behoefte opgeven in de volgende voorbeelden.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Het duurt enkele minuten voor het maken van de virtuele machine en de ondersteunende resources.

## <a name="install-the-azure-ad-login-vm-extension"></a>Installeer de Azure AD-aanmelding VM-extensie

Installeer het Azure Active Directory-logboek in VM-extensie te melden voor een Linux-VM met Azure AD-referenties. VM-extensies zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op virtuele machines in Azure. Gebruik [az vm-extensie set](/cli/azure/vm/extension#az-vm-extension-set) voor het installeren van de *AADLoginForLinux* uitbreiding op de virtuele machine met de naam *myVM* in de *myResourceGroup* resource groep:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH.Edp \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

De *provisioningState* van *geslaagd* wordt weergegeven wanneer de uitbreiding is geïnstalleerd op de virtuele machine.

## <a name="configure-role-assignments-for-the-vm"></a>Roltoewijzingen voor de virtuele machine configureren

Azure op rollen gebaseerde toegangsbeheer (RBAC)-beleid wordt bepaald wie kunt aanmelden bij de virtuele machine. Twee RBAC-rollen worden gebruikt voor het autoriseren van VM-aanmelding:

- **Aanmeldingsnaam van de beheerder van de virtuele Machine**: gebruikers met deze rol toegewezen kunnen aanmelden bij een virtuele machine van Azure met Windows of Linux hoofdmap gebruikersbevoegdheden.
- **Virtuele Machine gebruikersaanmelding**: gebruikers met deze rol toegewezen kunnen aanmelden bij een virtuele machine van Azure met de gebruikelijke gebruikersbevoegdheden.

> [!NOTE]
> Als een gebruiker zich aanmelden bij de virtuele machine via SSH, moet u ofwel de *aanmeldingsnaam van de beheerder van de virtuele Machine* of *virtuele Machine gebruikersaanmelding* rol. Een Azure-gebruiker met de *eigenaar* of *Inzender* rollen toegewezen voor een virtuele machine hoeft rechten aan te melden bij de virtuele machine via SSH niet automatisch.

Het volgende voorbeeld wordt [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create) om toe te wijzen de *aanmeldingsnaam van de beheerder van de virtuele Machine* rol aan de virtuele machine voor uw huidige Azure-gebruiker. De gebruikersnaam van uw actief Azure-account wordt verkregen met [az account weergeven](/cli/azure/account#az-account-show), en de *bereik* is ingesteld op de virtuele machine gemaakt in de vorige stap met [az vm weergeven](/cli/azure/vm#az-vm-show). Het bereik kan ook worden toegewezen op een resource-groep of -abonnement niveau en normale RBAC overname machtigingen van toepassing. Zie voor meer informatie [toegangsbeheer op basis van rollen](../../azure-resource-manager/resource-group-overview.md#access-control)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

Zie voor meer informatie over het gebruik van RBAC voor het beheren van toegang tot de resources van uw Azure-abonnement met behulp van de [Azure CLI 2.0](../../role-based-access-control/role-assignments-cli.md), [Azure-portal](../../role-based-access-control/role-assignments-portal.md), of [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

U kunt ook Azure AD voor meervoudige authenticatie voor een specifieke gebruiker zich aanmeldt bij de virtuele Linux-machine. Zie voor meer informatie [aan de slag met Azure multi-factor Authentication in de cloud](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Aanmelden bij de virtuele Linux-machine

Bekijk eerst het openbare IP-adres van uw virtuele machine met [az vm weergeven](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Aanmelden bij de Azure Linux virtuele machine met behulp van uw Azure AD-referenties. De `-l` parameter kunt u uw eigen Azure AD-account-adres opgeven. Geef het openbare IP-adres van uw virtuele machine als uitvoer in de voorgaande opdracht:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

U wordt gevraagd aan te melden bij Azure AD met een code voor eenmalig gebruik op [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin). Kopieer en plak de code voor eenmalig gebruik in de aanmeldingspagina apparaat zoals weergegeven in het volgende voorbeeld:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

Wanneer u wordt gevraagd, typt u uw Azure AD-aanmeldingsreferenties op de aanmeldingspagina. Het volgende bericht wordt weergegeven in de webbrowser wanneer u bent geverifieerd:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Sluit het browservenster terug naar de SSH-prompt en druk op de **Enter** sleutel. U bent nu aangemeld bij de Azure Linux virtuele machine met de rolmachtigingen die is toegewezen, zoals *VM gebruiker* of *VM beheerder*. Als uw gebruikersaccount is toegewezen de *aanmeldingsnaam van de beheerder van de virtuele Machine* rol, kunt u de `sudo` opdrachten waarvoor hoofdmap bevoegdheden uit te voeren.

## <a name="troubleshoot-sign-in-issues"></a>Problemen met aanmelden

Een aantal veelvoorkomende fouten wanneer u te SSH met Azure AD-referenties probeert bevatten geen RBAC-rollen toegewezen, en aanwijzingen voor het aanmelden wordt herhaald. Gebruik de volgende secties om deze problemen te verhelpen.

### <a name="access-denied-rbac-role-not-assigned"></a>Toegang geweigerd: RBAC-rol is niet toegewezen

Als u de volgende fout op de SSH-prompt ziet, Controleer of u [RBAC-beleidsregels hebt geconfigureerd](#configure-rbac-policy-for-the-virtual-machine) voor de virtuele machine die de gebruiker hetzij verleent de *aanmeldingsnaam van de beheerder van de virtuele Machine* of *virtuele Gebruikersaanmelding machine* rol:

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Blijvende SSH aanmelden prompts

Als u de verificatie-stap in een webbrowser met succes hebt voltooid, kunt u onmiddellijk opnieuw aan te melden met een nieuwe code gevraagd. Deze fout wordt meestal veroorzaakt door een bij de prompt SSH opgegeven naam en het account dat u aangemeld bij Azure AD met komen niet overeen. Dit probleem corrigeren:

- Controleer of u hebt opgegeven bij de prompt SSH naam juist is. Een typefout gemaakt in de naam van de aanmelding kan ervoor zorgen dat u hebt opgegeven bij de prompt SSH naam en het account dat u aangemeld bij Azure AD met niet overeenkomen. Bijvoorbeeld: u hebt getypt *azuresuer@contoso.onmicrosoft.com* in plaats van *azureuser@contoso.onmicrosoft.com*.
- Als u meerdere gebruikersaccounts hebt, moet dat u een ander gebruikersaccount in het browservenster niet opgeeft tijdens het aanmelden bij Azure AD.
- Linux is een hoofdlettergevoelige besturingssysteem. Er is een verschil tussen 'Azureuser@contoso.onmicrosoft.com'en'azureuser@contoso.onmicrosoft.com', waardoor een niet-overeenkomend. Zorg ervoor dat u de UPN met de juiste hoofdlettergevoeligheid bij de SSH-prompt opgeeft.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Active Directory [wat is Azure Active Directory](../../active-directory/active-directory-whatis.md) en [hoe u aan de slag met Azure Active Directory](../../active-directory/get-started-azure-ad.md)