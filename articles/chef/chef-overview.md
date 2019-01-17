---
title: Met behulp van Chef met Azure
description: Inleiding tot het gebruik van Chef configureren en testen van uw Azure-infrastructuur
ms.service: virtual-machines-linux
keywords: Azure, chef, devops, virtuele machines, overzicht, automatiseren
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: be1e7ab953c55581645a9702fc4759cb603e7ecc
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54357858"
---
# <a name="using-chef-with-azure"></a>Met behulp van Chef met Azure
[Chef](http://www.chef.io) is een krachtige automatisering-platform waarmee VM-infrastructuur in Azure worden getransformeerd in code. Chef automatiseert hoe infrastructuur is geconfigureerd, geïmplementeerd en beheerd via het netwerk, ongeacht de grootte ervan.

Dit artikel beschrijft de voordelen van het gebruik van Chef voor het beheren van Azure-infrastructuur.

## <a name="chef-extension-on-azure"></a>Chef-extensie op Azure
Een virtuele machine inrichten met Chef-Client die wordt uitgevoerd als een achtergrondservice met de [Chef extensie](https://docs.microsoft.com/en-us/azure/chef/chef-extension-portal) in de Azure Portal. Eenmaal ingericht, worden deze virtuele machines kunnen worden beheerd door een Chef-server.

## <a name="chef-cloud-shell"></a>Chef Cloud Shell
Gebruik Chef werkstation rechtstreeks in Azure Cloudshell. Uitvoeren van al uw Chef-hulpprogramma's en InSpec rechts van Cloud Shell. U kunt gebruikmaken van de Chef-opdrachten uit:

* [chef](https://docs.chef.io/ctl_chef.html)
* [keuken](https://docs.chef.io/ctl_kitchen.html)
* [inspec](https://www.inspec.io/docs/reference/cli/)
* [knife](https://docs.chef.io/knife.html)
* [cookstyle](https://docs.chef.io/cookstyle.html)
* [foodcritic](https://docs.chef.io/foodcritic.html)
* [chef-run](https://www.chef.sh/docs/chef-workstation/getting-started/)

Onze hulpprogramma's voor opdracht worden gecombineerd met de andere hulpprogramma's beschikbaar in Cloud Shell, zoals `git`, `az-cli`, en `terraform`, en het schrijven van uw automation-infrastructuur en naleving van de browser.

## <a name="automate-infrastructure-apps-and-compliance-with-one-platform"></a>Infrastructuur, apps en de naleving van één platform automatiseren
Bedrijven moeten snelheid, snelheid en veiligheid voor deelname in de digital marketplace. Samen Chef en Microsoft helpen personen, teams en ondernemingen die al deze dingen doen. U kunt met één platform, Chef Automate, automatiseren en continu afleveren van uw infrastructuur, toepassingen en naleving via uw Microsoft-omgeving.

## <a name="test-drive-chef-automate-on-azure"></a>Probeer Chef automate op Azure
Ondersteund door Chef, de [Chef automatiseren van Azure Marketplace-oplossing](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/chef-software.chef-automate) kunt u bouwen, implementeren en beheren van uw infrastructuur en toepassingen samen. Met één klik kunt u direct toegang tot alle commerciële functies die zijn opgenomen in Chef Automate; end-to-end inzicht in uw gehele apparatenpark, continue naleving en beheert elke wijziging met een uniforme werkstroom.

## <a name="next-steps"></a>Volgende stappen

* [Een Windows-machine maken op Azure met Chef](/azure/virtual-machines/windows/chef-automation)
