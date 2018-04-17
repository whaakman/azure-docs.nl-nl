---
title: Een Azure Marketplace-installatiekopie gebruiken een Terraform Linux virtuele machine maken met de Service-identiteit beheerd
description: Gebruik de Marketplace-installatiekopie Terraform Linux virtuele machine maken met beheerde Service-identiteit en extern beheer van de status om de resources gemakkelijk te implementeren in Azure.
keywords: terraform, devops, MSI, externe status en azure virtuele machine
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: 5f0ee2904c1072a5ad8c5f7ae1c90e649cc4813c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Een Azure Marketplace-installatiekopie gebruiken een Terraform Linux virtuele machine maken met de Service-identiteit beheerd

Dit artikel laat zien hoe u een [Terraform Marketplace-installatiekopie](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) voor het maken van een virtuele Ubuntu Linux-machine (16.04 TNS) met de meest recente [Terraform](https://www.terraform.io/intro/index.html) versie geïnstalleerd en geconfigureerd met behulp van [beheerd Service-identiteit (MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Deze installatiekopie configureert ook een externe back-end om in te schakelen [externe status](https://www.terraform.io/docs/state/remote.html) beheren met behulp van Terraform. 

De Terraform Marketplace-installatiekopie kunt eenvoudig aan de slag met Terraform in Azure, zonder te installeren en Terraform handmatig configureren. 

Er zijn geen kosten voor software voor deze Terraform VM-installatiekopie. U betaalt alleen Azure hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine die ingericht. Zie voor meer informatie over de compute-kosten, de [pagina met prijzen virtuele Linux-machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Vereisten
Voordat u een Linux Terraform virtuele machine maken kunt, moet u een Azure-abonnement hebben. Als u dit niet al hebt, raadpleegt u [maken van uw gratis Azure-account vandaag](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Uw Terraform virtuele machine maken 

Hier volgen de stappen voor het maken van een exemplaar van een virtuele machine van Linux Terraform: 

1. In de Azure portal, gaat u naar de [maken van een Resource](https://ms.portal.azure.com/#create/hub) aanbieding.

2. In de **zoeken van de Marketplace** zoekbalk, zoekt u **Terraform**. Selecteer de **Terraform** sjabloon. 

3. Selecteer op het tabblad Terraform details in de rechterbenedenhoek de **maken** knop.

    ![Een Terraform virtuele machine maken](media\terraformmsi.png)

4. De volgende secties informatie invoeren voor elk van de stappen in de wizard om de Terraform Linux virtuele machine te maken. De volgende sectie geeft een lijst van de invoerwaarden die nodig zijn voor het configureren van elk van deze stappen.

## <a name="details-on-the-create-terraform-tab"></a>Details op het tabblad Terraform maken

Voer de volgende gegevens op de **Terraform maken** tabblad:

1. **Basisinstellingen**
    
   * **Naam**: de naam van uw Terraform virtuele machine.
   * **Gebruikersnaam**: het eerste account aanmelden ID.
   * **Wachtwoord**: het wachtwoord voor het eerste. (U kunt een openbare SSH-sleutel gebruiken in plaats van een wachtwoord.)
   * **Abonnement**: het abonnement op waarop de machine is gemaakt en kosten in rekening gebracht. U moet bevoegdheden van de resource maken voor dit abonnement hebben.
   * **Resourcegroep**: een nieuwe of bestaande resourcegroep.
   * **Locatie**: het datacenter die het meest geschikt is. Het is doorgaans het datacenter waarin de meeste van uw gegevens, of de naam die zich het dichtst bij de fysieke locatie voor de snelste toegang tot het netwerk heeft.

2. **Extra instellingen**

   * **De grootte van**: grootte van de virtuele machine. 
   * **VM-schijftype**: SSD of harde schijf.

3. **Samenvatting Terraform**

   * Controleer of alle informatie die u hebt ingevoerd juist is. 

4. **Kopen**

   * Selecteer om te starten tijdens het inrichtingsproces, **kopen**. Een koppeling is met de voorwaarden van de transactie opgegeven. De virtuele machine heeft geen eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt geselecteerd in de stap grootte.

De Terraform VM-installatiekopie voert de volgende stappen uit:

* Maakt een virtuele machine met het systeem toegewezen identiteit die gebaseerd op de installatiekopie van het Ubuntu 16.04 TNS.
* De MSI-uitbreiding wordt geïnstalleerd op de virtuele machine zodat de OAuth-tokens worden afgegeven voor Azure-resources.
* RBAC machtigingen toekent aan de identiteit van de beheerde, eigenaar-rechten voor de resourcegroep.
* Maakt een Terraform sjabloonmap (tfTemplate).
* Vooraf configureert u een externe Terraform-status met het Azure back-end.

## <a name="access-and-configure-a-linux-terraform-virtual-machine"></a>Toegang tot en Linux Terraform virtuele machines configureren

Nadat u de virtuele machine maakt, kunt u aanmelden bij deze met behulp van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de sectie 'Basisbeginselen' van stap 3 voor de interface van de shell tekst. Op Windows, kunt u een SSH clienthulpprogramma zoals downloaden [Putty](http://www.putty.org/).

Nadat u via SSH verbinding maken met de virtuele machine, moet u Inzender-rechten voor het hele abonnement geven tot de Service-identiteit beheerd op de virtuele machine. 

Inzender machtiging helpt MSI op virtuele machine Terraform gebruiken om te maken van bronnen buiten de VM-resourcegroep. U kunt deze actie eenvoudig bereiken door een script eenmaal uit te voeren. Gebruik de volgende opdracht:

`. ~/tfEnv.sh`

Het vorige script gebruikt de [AZ CLI v 2.0 interactief aanmelden](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) mechanisme om te verifiëren met Azure en de virtuele machine beheerde Service-identiteit Inzender machtiging voor het hele abonnement toewijzen. 

 De virtuele machine heeft een Terraform externe status back-end. Als u wilt inschakelen op uw Terraform-implementatie, moet u de remoteState.tf-bestand kopiëren vanuit tfTemplate map naar de hoofdmap van de scripts Terraform.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Zie voor meer informatie over het beheer van externe status [deze pagina over de externe status Terraform](https://www.terraform.io/docs/state/remote.html). De toegangssleutel voor opslag is beschikbaar in dit bestand is gemaakt en moet worden uitgesloten voordat toezeggen Terraform configuratiebestanden in broncodebeheer.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Terraform virtuele Linux-machine in Azure instellen. Hier volgen enkele aanvullende resources waarmee u kunt meer informatie over Terraform op Azure: 

 [Terraform-Hub in Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure provider-documentatie](http://aka.ms/terraform)  
 [Terraform Azure provider bron](http://aka.ms/tfgit)  
 [Terraform Azure-modules](http://aka.ms/tfmodules)
 

















