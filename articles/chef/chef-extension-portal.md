---
title: De Chef-client installeren vanuit de Azure-portal
description: Meer informatie over het implementeren en configureren van uw client Chef vanuit de Azure-portal
keywords: Azure, chef, devops, client, installeren, portal
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 52f34361d7c1f3dff47f2571a714b8be7764cc6f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260033"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>De Chef-client installeren vanuit de Azure-portal
Bij het maken of wijzigen van een Linux- of Windows virtuele machine van de Azure-portal, kunt u de extensie Chef kunt toevoegen aan de virtuele machine. Dit artikel begeleidt u bij dit proces met behulp van een nieuwe virtuele Linux-machine.

## <a name="prerequisites"></a>Vereisten
- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Chef**: als u een actieve Chef-account niet hebt, zich aanmelden voor een [gratis proefversie van Chef gehost](https://manage.chef.io/signup). Als u wilt volgen samen met de instructies in dit artikel, moet u de volgende waarden uit uw account Chef: 
    - organization_validation sleutel
    - RB
    - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>De extensie Chef installeren op een nieuwe virtuele Linux-machine
In deze sectie gaat u eerst de Azure portal gebruiken een Linux-machine maken. Tijdens het wordt ook ziet u hoe u de extensie Chef installeert op de nieuwe virtuele machine.

1. Blader naar de [Azure-portal](http://portal.azure.com).

1. Selecteer in het menu aan de linkerkant de **virtuele machines** optie. Als de **virtuele machines** optie is niet aanwezig is, selecteer **alle services** en selecteer vervolgens **virtuele machines**.

1. Op de **virtuele machines** tabblad **toevoegen**.

    ![Voeg een nieuwe virtuele machine in de Azure portal](./media/chef-extension-portal/add-vm.png)

1. Op de **Compute** tabblad, selecteert u het gewenste besturingssysteem. Voor deze demo **Ubuntu Server** is geselecteerd.

1. Op de **Ubuntu Server** tabblad **Ubuntu Server 16.04 LTS**.

    ![Bij het maken van een virtuele Ubuntu-machine, geef de versie die u nodig hebt](./media/chef-extension-portal/ubuntu-server-version.png)

1. Op de **Ubuntu Server 16.04 LTS** tabblad **maken**.

    ![Ubuntu biedt aanvullende informatie over hun product](./media/chef-extension-portal/create-vm.png)

1. Op de **virtuele machine maken** tabblad **basisbeginselen**.

1. Op de **basisbeginselen** tabblad en selecteer vervolgens de volgende waarden opgeven **OK**.

    - **Naam** -Voer een naam voor de nieuwe virtuele machine.
    - **VM-schijftype** -Geef **SSD** of **HDD** voor het opslagtype voor de schijf. Raadpleeg het artikel voor meer informatie over schijftypen van de virtuele machine in Azure, [beheerde schijven voor virtuele machines en hoge prestaties Premium-opslag](/azure/virtual-machines/windows/premium-storage).
    - **Gebruikersnaam** -Voer een gebruikersnaam dat beheerdersbevoegdheden op de virtuele machine wordt verleend.
    - **Verificatietype** : Selecteer **wachtwoord**. U kunt ook selecteren **openbare SSH-sleutel**, en een openbare SSH-sleutelwaarde op te geven. Ten behoeve van deze demo (en in de schermafbeeldingen) **wachtwoord** is geselecteerd.
    - **Wachtwoord** en **wachtwoord bevestigen** -Voer een wachtwoord voor de gebruiker.
    - **Aanmelden met Azure Active Directory** : Selecteer **uitgeschakelde**.
    - **Abonnement** -Selecteer de gewenste Azure-abonnement, hebt u meer dan één.
    - **Resourcegroep** -Voer een naam voor de resourcegroep.
    - **Locatie** : Selecteer **VS-Oost**.

    ![Tabblad basisprincipes voor het maken van een virtuele machine](./media/chef-extension-portal/add-vm-basics.png)

1. Op de **een grootte kiezen** tabblad, selecteer de grootte voor de virtuele machine en selecteer vervolgens **Selecteer**.

1. Op de **instellingen** tabblad en de meeste van de waarden worden ingevuld op basis van de waarden die u hebt geselecteerd in de vorige tabbladen. Selecteer **extensies**.

    ![Uitbreidingen worden toegevoegd aan virtuele machines via het tabblad instellingen](./media/chef-extension-portal/add-vm-select-extensions.png)

1. Op de **extensies** tabblad **-extensie toevoegen**.

    ![Selecteer een extensie extensie toevoegen aan een virtuele machine toevoegen](./media/chef-extension-portal/add-vm-add-extension.png)

1. Op de **nieuwe resource** tabblad **Linux Chef-extensie (1.2.3)**.

    ![Chef heeft uitbreidingen voor Linux en Windows virtuele machines](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Op de **Linux Chef extensie** tabblad **maken**.

1. Op de **uitbreiding installeren** tabblad en selecteer vervolgens de volgende waarden opgeven **OK**.

    - **URL van de Server chef** -Voer de URL van de Chef-Server met de naam van de organisatie. Ik heb gebruikt *https://api.chef.io/organization/hessco* voor de demo.
    - **Naam van het knooppunt chef** -Voer de naam Chef-knooppunt. Dit kan een willekeurige waarde zijn.
    - **Uitvoeren van de lijst** -voert u de lijst Chef uitvoeren die wordt toegevoegd aan de machine. Dit kan leeg zijn.
    - **Naam van de Client validatie** -Voer de naam van de klant van Chef validatie. Ik heb gebruikt *tarcher validator* voor de demo.
    - **Validatiesleutel** -Selecteer een bestand met de validatiesleutel gebruikt bij het uitvoeren van de bootstrap uw machines. 
    - **Client-configuratiebestand** -configuratiebestand selecteren voor chef-client. Dit kan leeg zijn.
    - **Chef-clientversie** -Geef de versie van de chef-client te installeren. Een lege waarde zorgt ervoor dat de meest recente versie om te worden geïnstalleerd. Dit kan leeg zijn.
    - **SSL-verificatie modus** -optie **geen** of **Peer**. Ik heb geselecteerd *geen* voor de demo.
    - **Chef omgeving** -Voer de dit knooppunt lid van zijn moet Chef-omgeving. Dit kan leeg zijn.
    - **Versleuteld Databag geheim** -Selecteer een bestand met het geheim voor de Databag versleuteld deze machine toegang tot hebben moet. Dit kan leeg zijn.
    - **Chef Server SSL-certificaat** -selecteert u het SSL-certificaat dat is toegewezen aan uw Chef-Server. Dit kan leeg zijn.

    ![De Chef-Server installeren op een virtuele Linux-machine](./media/chef-extension-portal/install-extension.png)

1. Wanneer u keert terug naar de **extensies** tabblad **OK**.

1. Wanneer u keert terug naar de **instellingen** tabblad **OK**.

1. Wanneer u keert terug naar de **maken** tabblad (dat geeft een samenvatting van de opties die u hebt geselecteerd en ingevoerd), Controleer de informatie, evenals de **gebruiksvoorwaarden**, en selecteer **maken**.

Wanneer het proces van het maken en implementeren van de virtuele machine met de extensie Chef voltooid is, geeft een melding aan het slagen of mislukken van de bewerking. Bovendien de resourcepagina voor de nieuwe virtuele machine automatisch wordt geopend in de Azure-portal zodra deze gemaakt.

![De Chef-Server installeren op een virtuele Linux-machine](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Volgende stappen
* [Een virtuele Windows-machine in Azure met behulp van Chef maken](/azure/virtual-machines/windows/chef-automation)