---
title: De Chef-client installeren vanuit de Azure-portal
description: Meer informatie over het implementeren en configureren uw Chef-client vanuit de Azure-portal
keywords: Azure, chef, devops, client, installeren, portal
ms.service: virtual-machines-linux
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: cf7afb50006fb273b4d685f9e4259be1cb60fe4e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58084739"
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>De Chef-client installeren vanuit de Azure-portal
U kunt de uitbreiding voor de Chef-client rechtstreeks naar een Linux- of Windows-machine toevoegen vanuit de Azure-portal. Dit artikel begeleidt u bij het proces met behulp van een nieuwe virtuele Linux-machine.

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Chef**: Als u geen een actieve Chef-account hebt, kunt u zich aanmelden voor een [gratis proefversie van Chef gehost](https://manage.chef.io/signup). Als u wilt volgen, samen met de instructies in dit artikel, moet u de volgende waarden uit uw Chef-account:
  - organization_validation sleutel
  - degressieve
  - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>De Chef-extensie installeren op een nieuwe virtuele Linux-machine
In deze sectie gaat u eerst de Azure-portal gebruiken om een Linux-machine te maken. Gedurende dit proces ook ziet u hoe u de Chef-extensie op de nieuwe virtuele machine installeert.

1. Blader naar de [Azure-portal](https://portal.azure.com).

1. Selecteer in het menu aan de linkerkant de **virtuele machines** optie. Als de **virtuele machines** optie is niet aanwezig zijn, selecteer **alle services** en selecteer vervolgens **virtuele machines**.

1. Op de **virtuele machines** tabblad **toevoegen**.

    ![Toevoegen van een nieuwe virtuele machine in Azure portal](./media/chef-extension-portal/add-vm.png)

1. Op de **Compute** tabblad, selecteert u het gewenste besturingssysteem. Voor deze demo **Ubuntu Server** is geselecteerd.

1. Op de **Ubuntu Server** tabblad **Ubuntu Server 16.04 LTS**.

    ![Bij het maken van een virtuele Ubuntu-machine, geef de versie die u nodig hebt](./media/chef-extension-portal/ubuntu-server-version.png)

1. Op de **Ubuntu Server 16.04 LTS** tabblad **maken**.

    ![Ubuntu bevat aanvullende informatie over hun product](./media/chef-extension-portal/create-vm.png)

1. Op de **maken van virtuele machine** tabblad **basisbeginselen**.

1. Op de **basisbeginselen** tabblad, geef de volgende waarden en selecteer vervolgens **OK**.

   - **Naam** -Voer een naam voor de nieuwe virtuele machine.
   - **VM-schijftype** -opgeven of **SSD** of **HDD** voor het opslagtype voor de schijf. Zie het artikel voor meer informatie over schijftypen van de virtuele machine op Azure, [Selecteer een schijftype](../virtual-machines/windows/disks-types.md).
   - **Gebruikersnaam** -Voer een gebruikersnaam die wordt verleend beheerdersbevoegdheden op de virtuele machine.
   - **Verificatietype** : Selecteer **wachtwoord**. U kunt ook selecteren **openbare SSH-sleutel**, en een openbare SSH-sleutelwaarde hebt opgegeven. Ten behoeve van deze demo (en in de schermafbeeldingen) **wachtwoord** is geselecteerd.
   - **Wachtwoord** en **wachtwoord bevestigen** -Geef een wachtwoord voor de gebruiker.
   - **Meld u aan met Azure Active Directory** : Selecteer **uitgeschakelde**.
   - **Abonnement** -Selecteer de gewenste Azure-abonnement, als u meer dan één hebt.
   - **Resourcegroep** -Voer een naam voor de resourcegroep.
   - **Locatie**: selecteer **US - oost**.

     ![Tabblad van de basisbeginselen voor het maken van een virtuele machine](./media/chef-extension-portal/add-vm-basics.png)

1. Op de **Kies een grootte** tabblad, selecteert u de grootte voor de virtuele machine en selecteer vervolgens **Selecteer**.

1. Op de **instellingen** tabblad en de meeste van de waarden worden ingevuld op basis van de waarden die u hebt geselecteerd in de vorige tabbladen. Selecteer **Extensies**.

     ![Extensies zijn toegevoegd aan virtuele machines via het tabblad instellingen](./media/chef-extension-portal/add-vm-select-extensions.png)

1. Op de **extensies** tabblad **-extensie toevoegen**.

     ![Extensie toevoegen aan een extensie toevoegen aan een virtuele machine selecteren](./media/chef-extension-portal/add-vm-add-extension.png)

1. Op de **nieuwe resource** tabblad **Linux Chef-extensie (1.2.3)**.

     ![Chef heeft uitbreidingen voor virtuele machines met Linux en Windows](./media/chef-extension-portal/select-linux-chef-extension.png)

1. Op de **Linux Chef extensie** tabblad **maken**.

1. Op de **-extensie installeren** tabblad, geef de volgende waarden en selecteer vervolgens **OK**.

    - **URL van chef** -Geef de URL van de Chef-Server met de naam van de organisatie, bijvoorbeeld *https://api.chef.io/organization/mycompany*.
    - **Naam van het knooppunt chef** -Geef de naam van de Chef-knooppunt. Dit kan een willekeurige waarde zijn.
    - **Run List** -voert u de lijst met Chef uitvoeren die wordt toegevoegd aan de machine. Dit kan leeg worden gelaten.
    - **Naam van de Client validatie** -Voer de naam van Chef validatie-Client. bijvoorbeeld, *tarcher validator*.
    - **Validatiesleutel** -Selecteer een bestand met de validatiesleutel gebruikt bij het opstarten van uw machines.
    - **Client-configuratiebestand** -Selecteer een configuratiebestand voor chef-client. Dit kan leeg worden gelaten.
    - **Chef-clientversie** -Geef de versie van de chef-client te installeren. Dit kan leeg worden gelaten. Een lege waarde installeert de meest recente versie.
    - **SSL-verificatiemodus** -Selecteer **geen** of **Peer**. *Geen* is geselecteerd voor de demo.
    - **Chef-omgeving** -Geef de Chef-omgeving met dit knooppunt lid zijn van moet. Dit kan leeg worden gelaten.
    - **Versleuteld Databag geheim** -Selecteer een bestand met de geheime sleutel voor de Databag versleuteld deze machine toegang tot moeten hebben. Dit kan leeg worden gelaten.
    - **Chef-Server SSL-certificaat** -selecteert u het SSL-certificaat dat is toegewezen aan uw Chef-Server. Dit kan leeg worden gelaten.

      ![Installatie van de Chef-Server op een Linux-machine](./media/chef-extension-portal/install-extension.png)

1. Als u terugkeert naar de **extensies** tabblad **OK**.

1. Als u terugkeert naar de **instellingen** tabblad **OK**.

1. Als u terugkeert naar de **maken** tabblad (dit geeft een samenvatting van de opties die u hebt geselecteerd en u hebt opgegeven), controleert u de gegevens, evenals de **gebruiksvoorwaarden**, en selecteer **maken**.

Wanneer het proces voor het maken en implementeren van de virtuele machine met de Chef-extensie voltooid is, geeft een melding aan het slagen of mislukken van de bewerking. De resourcepagina voor de nieuwe virtuele machine wordt bovendien automatisch geopend in Azure portal nadat deze is gemaakt.

![Installatie van de Chef-Server op een Linux-machine](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Volgende stappen

- [Een Windows-machine maken op Azure met Chef](/azure/virtual-machines/windows/chef-automation)
