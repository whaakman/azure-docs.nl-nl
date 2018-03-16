---
title: Een Azure Marketplace-installatiekopie gebruiken Terraform virtuele Linux-Machine maken met de Service-identiteit beheerd
description: Gebruik de Marketplace-installatiekopie Terraform virtuele Linux-Machine maken met beheer van beheerde Service-identiteit en status van externe bronnen eenvoudig te implementeren in Azure.
keywords: terraform, devops, MSI, externe status en azure virtuele machine
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: ce8a3e8b813bf4224a1fd77d60ec30f2f8e080a7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Een Azure Marketplace-installatiekopie gebruiken Terraform virtuele Linux-Machine maken met de Service-identiteit beheerd

Dit artikel laat zien hoe u een [Terraform Marketplace-installatiekopie](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) maken een `Ubuntu Linux VM (16.04 LTS)` met laatste [Terraform](https://www.terraform.io/intro/index.html) versie geïnstalleerd en geconfigureerd met behulp van [()-Service-identiteit beheerd MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Deze installatiekopie configureert ook een externe back-end om in te schakelen [status van externe](https://www.terraform.io/docs/state/remote.html) beheren met behulp van Terraform. De Terraform Marketplace-installatiekopie kunt eenvoudig aan de slag met Terraform op Azure in minuten, zonder Terraform installeren en het handmatig configureren van verificatie. 

Er zijn geen kosten voor software voor deze Terraform VM-installatiekopie. U betaalt alleen Azure hardware-gebruikskosten die worden beoordeeld op basis van de grootte van de virtuele machine ingericht. Meer informatie over de compute-kosten vindt u op de [pagina met prijzen van Linux virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Vereisten
Voordat u een Linux Terraform virtuele Machine maken kunt, moet u een Azure-abonnement hebben. Als u nog geen een, Zie [maken van uw gratis Azure-account vandaag](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Uw Terraform virtuele Machine maken 

Hier volgen de stappen voor het maken van een exemplaar van Linux Terraform virtuele Machine. 

1. Navigeer naar [maken van een Resource](https://ms.portal.azure.com/#create/hub) aanbieding in de Azure portal.
2. Zoeken naar `Terraform` in de `Search the Marketplace` zoekbalk. Selecteer de `Terraform` sjabloon. Selecteer de **maken** knop in het tabblad met details Terraform aan de rechterkant onder.
![alternatieve tekst](media\terraformmsi.png)
3. De volgende secties informatie invoeren voor elk van de stappen in de wizard (**opgesomd aan de rechterkant**) Terraform Linux virtuele Machine maken.  Hier volgen de invoerwaarden die nodig zijn voor elk van deze stappen configureren

## <a name="details-in-create-terraform-tab"></a>De details in maken Terraform tabblad

Hier volgen de details die moeten worden ingevoerd op het tabblad Terraform maken.

a. **Basisinstellingen**
    
* **Naam**: naam van uw Terraform virtuele Machine.
* **Gebruikersnaam**: eerste account aanmelden ID.
* **Wachtwoord**: eerste wachtwoord (u kunt openbare SSH-sleutel kunt gebruiken in plaats van wachtwoord).
* **Abonnement**: als u meer dan één abonnement hebt, selecteert u een op waarop de machine is gemaakt en kosten in rekening gebracht. U moet bevoegdheden van de resource maken voor dit abonnement hebben.
* **Resourcegroep**: U kunt een nieuwe maken of een bestaande groep.
* **Locatie**: Selecteer het datacenter die het meest geschikt is. Het is doorgaans het datacenter die de meeste van uw gegevens en of de dichtstbijzijnde fysieke voor snelste toegang tot het netwerk is.

b. **Extra instellingen**

* Grootte: Grootte van de virtuele Machine.
* VM-schijftype: Kies tussen SSD en HDD

c. **Samenvatting Terraform**

* Controleer of alle informatie die u hebt ingevoerd juist is. 

d. **Kopen**

* Voor het starten van de inrichting, klikt u op kopen. Een koppeling is met de voorwaarden van de transactie opgegeven. De virtuele machine heeft geen eventuele extra kosten afgezien van de berekening die voor de servergrootte van de die u hebt gekozen in de stap grootte.

De Terraform VM-installatiekopie voert de volgende stappen

* Hiermee maakt u een virtuele machine met automatisch toegewezen identiteit op basis van de installatiekopie van het Ubuntu 16.04 TNS
* De MSI-uitbreiding wordt geïnstalleerd op de virtuele machine zodat de OAuth-tokens worden afgegeven voor Azure-resources
* RBAC machtigingen toewijzen aan de identiteit beheerd eigenaar rechten voor de resourcegroep
* Maakt een map aan Terraform sjabloon (tfTemplate)
* Vooraf geconfigureerd Terraform externe staat met de Azure back-end

## <a name="how-to-access-and-configure-linux-terraform-virtual-machine"></a>Toegang tot en Terraform virtuele Linux-Machine configureren

Nadat de virtuele machine is gemaakt, kunt u aanmelden bij het gebruik van SSH. Gebruik de accountreferenties op die u hebt gemaakt in de sectie basisbeginselen van stap 3 voor de interface van de shell tekst. Op Windows, kunt u een SSH clienthulpprogramma zoals downloaden [Putty](http://www.putty.org/)

Zodra u hebt gebruikt `SSH` voor verbinding met de virtuele Machine, moet u Inzender-rechten voor het hele abonnement geven aan Service-identiteit beheerd op de virtuele Machine. Inzender machtiging helpt MSI op virtuele machine Terraform gebruiken om te maken van bronnen buiten de VM-resourcegroep. U kunt deze actie eenvoudig bereiken door één keer uitvoeren van een script. Hier volgt de opdracht om dat te doen.

`. ~/tfEnv.sh`

Maakt gebruik van het vorige script [AZ CLI v 2.0 interactief aanmelden](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) mechanisme om te verifiëren met Azure en de Service-identiteit van virtuele Machine beheerd Inzender machtigingen toewijzen aan het hele abonnement. 

 De virtuele machine heeft de status van de externe Terraform back-end gemaakt en als u wilt inschakelen op uw Terraform-implementatie, moet u remoteState.tf bestand kopiëren vanuit tfTemplate map naar de hoofdmap van de scripts Terraform.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Meer informatie over het beheer van externe status [hier](https://www.terraform.io/docs/state/remote.html). De toegangssleutel voor opslag is beschikbaar in dit bestand is gemaakt en moet zorgvuldig worden gecontroleerd in broncodebeheer.  

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd het instellen van een Terraform virtuele Linux-Machine in Azure. Hier volgen enkele aanvullende resources voor meer informatie over Terraform in Azure. 

 [Terraform-Hub in Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure Provider-documentatie](http://aka.ms/terraform)  
 [Terraform Azure Provider bron](http://aka.ms/tfgit)  
 [Terraform Azure-Modules](http://aka.ms/tfmodules)
 

















