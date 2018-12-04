---
title: Meld u aan bij een Linux-VM met Azure Active Directory-referenties | Microsoft Docs
description: In deze procedure leert u hoe u een Linux-VM voor het gebruik van Azure Active Directory-verificatie voor gebruikersaanmeldingen maken en configureren
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/17/2018
ms.author: cynthn
ms.openlocfilehash: e75758c5a4171adc7af56581026a727db2ef4740
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850972"
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Meld u aan bij een virtuele Linux-machine in Azure met behulp van Azure Active Directory-verificatie (Preview)

Ter verbetering van de beveiliging van virtuele Linux-machines (VM's) in Azure, kunt u integreren met Azure Active Directory (AD)-verificatie. Wanneer u Azure AD-verificatie voor virtuele Linux-machines gebruikt, centraal beheren en beleid afdwingen dat toestaan of weigeren van toegang tot de VM's. In dit artikel wordt beschreven hoe u maken en configureren van een Linux-VM voor het gebruik van Azure AD-verificatie.

> [!NOTE]
> Deze functie is beschikbaar als preview en wordt niet aanbevolen voor gebruik met virtuele productiemachines of workloads. Gebruik deze functie op een test-virtuele machine die u verwacht te verwijderen nadat u hebt getest.

Er zijn tal van voordelen van het gebruik van Azure AD-verificatie voor aanmelding bij virtuele Linux-machines in Azure, met inbegrip van:

- **Verbeterde beveiliging:**
  - Aanmelden bij Azure Linux VM's kunt u uw zakelijke AD-referenties. Er is niet nodig op de lokale administrator-accounts maken en beheren van de levensduur van de referentie.
  - U hoeft te bekommeren verlies/diefstal van referenties, gebruikers zwakke referenties enzovoort configureren doordat uw vertrouwen op de lokale administrator-accounts.
  - Help de beveiligde virtuele Linux-machines ook de complexiteit van wachtwoorden en het wachtwoord levensduur beleid geconfigureerd voor uw Azure AD-directory.
  - U kunt nog veiliger aanmelden bij virtuele machines van Azure multi-factor authentication configureren.
  - De mogelijkheid om aan te melden bij virtuele Linux-machines met Azure Active Directory werkt ook voor klanten die gebruikmaken van [Federation Services](../../active-directory/hybrid/how-to-connect-fed-whatis.md).

- **Naadloze samenwerking:** With Role-Based toegangsbeheer (RBAC), kunt u opgeven die kunnen zich aanmelden bij een bepaalde virtuele machine als een gewone gebruiker of met administrator-bevoegdheden. Wanneer gebruikers lid worden, of uw team laat, kunt u het beleid voor de virtuele machine toegang zo nodig het RBAC bijwerken. Deze ervaring is veel eenvoudiger dan dat u hoeft te verwijderen van virtuele machines als u wilt verwijderen van onnodige openbare SSH-sleutels. Wanneer werknemers uw bedrijf verlaat en hun gebruikersaccount is uitgeschakeld of verwijderd uit Azure AD, hebben ze niet langer toegang tot uw resources.

## <a name="supported-azure-regions-and-linux-distributions"></a>Ondersteunde Azure-regio's en Linux-distributies

De volgende Linux-distributies worden momenteel ondersteund tijdens de Preview-versie van deze functie:

| Distributie | Versie |
| --- | --- |
| CentOS | CentOS 6,9 en CentOS 7.4 |
| Debian | Debian 9 |
| Red Hat Enterprise Linux | RHEL 6, RHEL 7 | 
| Ubuntu Server | Ubuntu 14.04 LTS, Ubuntu Server 16.04 Ubuntu Server 17.10 en Ubuntu Server 18.04 |

De volgende Azure-regio's worden momenteel ondersteund tijdens de Preview-versie van deze functie:

- Alle globale Azure-regio 's

>[!IMPORTANT]
> Voor het gebruik van deze preview-functie, moet u alleen een ondersteunde Linux-distributie implementeren en in een ondersteunde Azure-regio. De functie wordt niet ondersteund in Azure Government of onafhankelijke clouds.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, deze zelfstudie vereist dat u de Azure CLI versie 2.0.31 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-linux-virtual-machine"></a>Een virtuele Linux-machine maken

Maak een resourcegroep met [az-groep maken](/cli/azure/group#az-group-create), maakt u een virtuele machine met [az vm maken](/cli/azure/vm#az-vm-create) met behulp van een ondersteunde distributie en in een ondersteunde regio. Het volgende voorbeeld wordt een virtuele machine met de naam geïmplementeerd *myVM* die gebruikmaakt van *Ubuntu 16.04 LTS* in een resourcegroep met de naam *myResourceGroup* in de *southcentralus*  regio. In de volgende voorbeelden kunt u uw eigen resourcegroep en de naam van virtuele machine opgeven.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Het maken van de virtuele machine en de ondersteunende resources duurt enkele minuten.

## <a name="install-the-azure-ad-login-vm-extension"></a>De Azure AD-aanmelding VM-extensie installeren

Om aan te melden met een Linux-VM met Azure AD-referenties, het Azure Active Directory-logboek in VM-extensie te installeren. VM-extensies zijn kleine toepassingen die taken van configuratie- en automatiseringstaken na de implementatie op Azure virtual machines bieden. Gebruik [az vm extension set](/cli/azure/vm/extension#az-vm-extension-set) voor het installeren van de *AADLoginForLinux* -extensie op de virtuele machine met de naam *myVM* in de *myResourceGroup* resource groep:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

De *provisioningState* van *geslaagd* wordt weergegeven nadat de extensie is geïnstalleerd op de virtuele machine.

## <a name="configure-role-assignments-for-the-vm"></a>Roltoewijzingen voor de virtuele machine configureren

Azure Role-Based Access Control (RBAC) beleid bepaalt wie zich kunt aanmelden bij de virtuele machine. Twee RBAC-rollen worden gebruikt voor de autorisatie van VM-aanmelding:

- **Beheerdersaanmelding bij virtuele Machine**: gebruikers met deze rol toegewezen kunnen aanmelden bij een Azure-machine met Windows-beheerders- of Linux-rootgebruikersbevoegdheden.
- **Gebruikersaanmelding bij virtuele Machine**: gebruikers met deze rol toegewezen kunnen aanmelden bij een Azure-machine met de gebruikelijke gebruikersbevoegdheden.

> [!NOTE]
> Als u wilt toestaan dat een gebruiker zich aanmelden bij de virtuele machine via SSH, moet u ofwel de *beheerdersaanmelding bij virtuele Machine* of *gebruikersaanmelding bij virtuele Machine* rol. Een Azure-gebruiker met de *eigenaar* of *Inzender* rollen toegewezen voor een virtuele machine automatisch geen bevoegdheden voor het aanmelden bij de virtuele machine via SSH.

Het volgende voorbeeld wordt [az roltoewijzing maken](/cli/azure/role/assignment#az-role-assignment-create) om toe te wijzen de *beheerdersaanmelding bij virtuele Machine* rol aan de virtuele machine voor uw huidige Azure-gebruiker. De gebruikersnaam van uw actieve Azure-account wordt verkregen met [az account show](/cli/azure/account#az-account-show), en de *bereik* is ingesteld op de virtuele machine gemaakt in de vorige stap met [az vm show](/cli/azure/vm#az-vm-show). Het bereik kan ook worden toegewezen op het niveau van een resource groep of abonnement, en er gelden normale RBAC overname van machtigingen. Zie voor meer informatie, [toegangsbeheer op basis van rollen](../../azure-resource-manager/resource-group-overview.md#access-control)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> Als uw AAD-domein en gebruikersnaam aanmeldingsdomein niet overeenkomen, moet u de object-ID van uw gebruikersaccount met de *--toegewezen gebruiker-object-id*, niet alleen de gebruikersnaam voor *--toegewezen gebruiker*. U kunt de object-ID verkrijgen voor uw gebruikersaccount met [az ad gebruikerslijst](/cli/azure/ad/user#az-ad-user-list).

Zie voor meer informatie over het gebruik van RBAC voor het beheren van toegang tot de resources van uw Azure-abonnement met behulp van de [Azure CLI](../../role-based-access-control/role-assignments-cli.md), [Azure-portal](../../role-based-access-control/role-assignments-portal.md), of [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

U kunt ook Azure AD om te vereisen dat multi-factor authentication voor een specifieke gebruiker zich aanmeldt bij de virtuele Linux-machine configureren. Zie voor meer informatie, [aan de slag met Azure multi-factor Authentication in de cloud](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Meld u aan bij de virtuele Linux-machine

Eerst geven het openbare IP-adres van uw virtuele machine met [az vm show](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Meld u aan bij de virtuele Azure Linux-machine met behulp van uw Azure AD-referenties. De `-l` parameter kunt u uw eigen Azure AD-account-adres opgeven. Adressen van de account moeten worden ingevoerd op alleen kleine letters. Gebruik het openbare IP-adres van uw virtuele machine met de vorige opdracht:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

U wordt gevraagd zich aanmeldt bij Azure AD met een code voor eenmalig gebruik op [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin). Kopieer en plak de code voor eenmalig gebruik in de aanmeldingspagina van het apparaat, zoals wordt weergegeven in het volgende voorbeeld:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

Voer desgevraagd uw Azure AD-aanmeldingsreferenties op de aanmeldingspagina. Het volgende bericht wordt weergegeven in de webbrowser wanneer u bent geverifieerd:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Sluit het browservenster geopend, gaat u terug naar de SSH-prompt en druk op de **Enter** sleutel. U bent nu aangemeld op de virtuele Azure Linux-machine met de machtigingen van de rol als toegewezen, zoals *VM User* of *VM-beheerder*. Als uw gebruikersaccount is toegewezen de *beheerdersaanmelding bij virtuele Machine* rol, kunt u de `sudo` opdrachten waarvoor bevoegdheden op hoofdniveau uit te voeren.

## <a name="sudo-and-aad-login"></a>Sudo en AAD-aanmelding

De eerste keer met sudo, wordt u gevraagd een tweede keer verifiëren. Als u niet wilt dat deze opnieuw te verifiëren voor het uitvoeren van sudo, kunt u uw sudo-bestand bewerken `/aad/etc/sudoers.d/aad_admins` en vervang deze regel:

```bash
%aad_admins ALL=(ALL) ALL
```
Met deze regel:

```bash
%aad_admins ALL=(ALL) NOPASSWD:ALL
```


## <a name="troubleshoot-sign-in-issues"></a>Problemen met aanmelden oplossen

Een aantal veelvoorkomende fouten wanneer u SSH met Azure AD-referenties wilt zijn geen RBAC-rollen toegewezen, en aanwijzingen om aan te melden bij herhaald. Gebruik de volgende secties om deze problemen te verhelpen.

### <a name="access-denied-rbac-role-not-assigned"></a>Toegang geweigerd: RBAC-rol is niet toegewezen

Als u de volgende fout op de SSH-opdrachtprompt ziet, controleert u of dat u hebt [RBAC-beleidsregels geconfigureerd](#configure-rbac-policy-for-the-virtual-machine) voor de virtuele machine die de gebruiker hetzij verleent de *beheerdersaanmelding bij virtuele Machine* of *virtuele Meld u aan de computer* rol:

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Voortdurende SSH aanmelden prompts

Als u de verificatie-stap in een webbrowser met succes hebt voltooid, kunt u onmiddellijk opnieuw aanmelden met een nieuwe code gevraagd. Deze fout wordt meestal veroorzaakt door een bij de prompt SSH opgegeven naam en het account dat u aangemeld bij Azure AD met komen niet overeen. Om op te lossen dit probleem:

- Controleer of dat u hebt opgegeven bij de prompt SSH naam juist is. Een typefout gemaakt in de naam van de aanmelding kan ervoor zorgen dat u hebt opgegeven bij de prompt SSH naam en het account dat u aangemeld bij Azure AD met niet overeenkomen. Bijvoorbeeld, u hebt getypt *azuresuer@contoso.onmicrosoft.com* in plaats van *azureuser@contoso.onmicrosoft.com*.
- Als u meerdere gebruikersaccounts hebt, zorg er dan voor dat u een ander gebruikersaccount in het browservenster niet opgeeft tijdens het aanmelden bij Azure AD.
- Linux is een hoofdlettergevoelige besturingssysteem. Er is een verschil tussen 'Azureuser@contoso.onmicrosoft.com'en'azureuser@contoso.onmicrosoft.com', waardoor een niet-overeenkomend. Zorg ervoor dat u de UPN met de juiste hoofdlettergevoeligheid bij de SSH-prompt opgeeft.

## <a name="preview-feedback"></a>Feedback van de Preview-versie

Deel uw feedback over deze preview-functie of het rapport problemen zijn met het op de [forum met feedback van Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Active Directory, [wat is Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md)
