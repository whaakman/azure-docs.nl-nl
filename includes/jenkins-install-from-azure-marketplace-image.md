---
description: bestand opnemen
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: 552e93e9bd1b17c73fb1638fbae2ac30b051c261
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
1. Open in uw browser de [Azure Marketplace-installatiekopie voor Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Selecteer **ophalen IT nu**.

    ![Selecteer deze nu GIT om het installatieproces voor de Jenkins Marketplace-installatiekopie te starten.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Bekijk de informatie over de prijzen details en voorwaarden en selecteer **doorgaan**.

    ![Jenkins Marketplace installatiekopie prijzen en voorwaarden informatie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Selecteer **maken** de Jenkins-server configureren in de Azure portal. 

    ![Installeer de Jenkins Marketplace-installatiekopie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. In de **basisbeginselen** tabblad, geeft u de volgende waarden:

    - **Naam** -Voer `Jenkins`.
    - **Gebruikersnaam** -Geef de gebruikersnaam op voor gebruik bij het aanmelden bij de virtuele machine waarop Jenkins wordt uitgevoerd. De gebruikersnaam moet voldoen aan [specifieke vereisten](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
    - **Verificatietype** : Selecteer **openbare SSH-sleutel**.
    - **Openbare SSH-sleutel** -kopiëren en plakken een openbare RSA-sleutel in de indeling van één regel (beginnen met `ssh-rsa`) of meerdere regels PEM-indeling. U kunt met behulp van ssh-keygen in Linux en Mac OS of PuTTYGen in Windows SSH-sleutels genereren. Zie het artikel voor meer informatie over de SSH-sleutels en Azure [hoe SSH gebruiken sleutels met Windows in Azure](/azure/virtual-machines/linux/ssh-from-windows).
    - **Abonnement** -het Azure-abonnement waaraan u wilt installeren Jenkins selecteren.
    - **Resourcegroep** : Selecteer **nieuw**, en voer een naam voor de resourcegroep die fungeert als een logische container voor de verzameling van resources die gezamenlijk uw Jenkins-installatie.
    - **Locatie** : Selecteer **VS-Oost**.

    ![Voer de verificatie- en groepsgegevens voor Jenkins op het tabblad basis.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Selecteer **OK** om door te gaan naar de **extra instellingen** tabblad. 

1. In de **extra instellingen** tabblad, geeft u de volgende waarden:

    - **De grootte van** -formaat van de juiste optie voor uw Jenkins virtuele machine.
    - **VM-schijftype** – Geef de harde schijf (harde schijf) of SSD (solid-state drive) om aan te geven welk type opslagschijf is toegestaan voor de Jenkins virtuele machine.
    - **Virtueel netwerk** -(optioneel) Selecteer **virtueel netwerk** de standaardinstellingen wijzigen.
    - **Subnetten** : Selecteer **subnetten**, Controleer de informatie en selecteer **OK**.
    - **Openbaar IP-adres** -naam van het IP-adres wordt standaard ingesteld op de Jenkins-naam die u hebt opgegeven in de vorige pagina en het achtervoegsel - IP. U kunt de optie die standaard wijzigen.
    - **Domeinnaamlabel** -Geef de waarde voor de volledige URL naar de Jenkins virtuele machine.
    - **Versietype Jenkins** -Selecteer het type van de gewenste versie in de opties: `LTS`, `Weekly build`, of `Azure Verified`. De `LTS` en `Weekly build` opties worden beschreven in het artikel [Jenkins TNS Release regel](https://jenkins.io/download/lts/). De `Azure Verified` optie verwijst naar een [Jenkins LTS versie](https://jenkins.io/download/lts/) die worden uitgevoerd op Azure is geverifieerd. 

    ![Voer de virtuele machine-instellingen voor Jenkins op het tabblad instellingen.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Selecteer **OK** om door te gaan naar de **instellingen voor Adreslijstintegratie** tabblad.

1. In de **instellingen voor Adreslijstintegratie** tabblad, geeft u de volgende waarden:

    - **Service-Principal** -de service-principal is toegevoegd aan Jenkins als referentie voor verificatie met Azure. `Auto` betekent dat de principal wordt gemaakt door MSI (Service-identiteit beheerd). `Manual` betekent dat de principal door u moet worden gemaakt. 
        - **Toepassings-ID** en **geheim** : als u selecteert de `Manual` optie voor de **Service-Principal** optie, moet u om op te geven de `Application ID` en `Secret` voor uw service-principal. Wanneer [maken van een service-principal](/cli/azure/create-an-azure-service-principal-azure-cli), Let op de standaardrol is **Inzender**, dit is voldoende voor het werken met Azure-resources.
    - **Inschakelen van Cloud-Agents** -Geef de standaardsjabloon cloud voor agents waar `ACI` verwijst naar een exemplaar van Azure-Container, en `VM` verwijst naar virtuele machines. U kunt ook opgeven `No` als u niet wilt inschakelen van een cloud-agent.

1. Selecteer **OK** om door te gaan naar de **samenvatting** tabblad.

1. Wanneer de **samenvatting** tabblad weergegeven, wordt de informatie hebt ingevoerd, wordt gevalideerd. Eenmaal u ziet de **validatie geslaagd** bericht (aan de bovenkant van het tabblad), selecteer **OK**. 

    ![Het tabblad Overzicht wordt weergegeven en valideert de geselecteerde opties.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Wanneer de **maken** tabblad worden weergegeven, selecteert u **maken** de Jenkins virtuele machine maken. Wanneer de server klaar is, wordt een melding weergegeven in de Azure portal.

    ![Jenkins is gereed melding.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)