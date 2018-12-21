---
title: Azure-snelstart - Een Batch AI-cluster maken - Portal | Microsoft Docs
description: Snelstart - Een Batch AI-cluster maken om machine learning- en AI-modellen te trainen - Azure-portal
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: danlep
ROBOTS: NOINDEX
ms.openlocfilehash: 78c743448a7f7439875d3598d6ba5d4eb6dc12fc
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408932"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-portal"></a>Snelstartgids: Een cluster maken voor Batch AI-trainingstaken met Azure Portal

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

In deze snelstart leert u hoe u de Azure-portal gebruikt om een Batch AI-cluster te maken dat u kunt gebruiken om AI- en machine learning-modellen te trainen. Batch AI is een beheerde service voor gegevenswetenschappers en AI-onderzoekers om AI- en machine learning-modellen op schaal te trainen in clusters virtuele Azure-machines.

Het cluster beschikt in de eerste instantie over één GPU-knooppunt en een bijgevoegde bestandsserver. Nadat u deze snelstart hebt voltooid, hebt u een cluster dat u omhoog kunt schalen en kunt gebruiken om deep learning-modellen te trainen. Dien trainingstaken in het cluster in met Batch AI, hulpprogramma's voor [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md) of de [Visual Studio-hulpprogramma's voor AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-ssh-key-pair"></a>Een SSH-sleutelpaar maken

U hebt een SSH-sleutelpaar nodig om deze snelstart te volgen. Als u een bestaand SSH-sleutelpaar hebt, kunt u deze stap overslaan.

Als u een SSH-sleutelpaar wilt maken, voert u de volgende opdracht uit vanaf een Bash-shell en volgt u de aanwijzingen op het scherm. U kunt bijvoorbeeld de [Azure Cloud Shell](../cloud-shell/overview.md) gebruiken, of wanneer u op Windows werkt, het [Windows-subsysteem voor Linux](/windows/wsl/install-win10). De uitvoer van de opdracht bevat de bestandsnaam van het bestand met de openbare sleutel. Kopieer de inhoud van het openbare sleutelbestand (`cat ~/.ssh/id_rsa.pub`) naar het klembord of een andere locatie die u in een latere stap kunt openen.

```bash
ssh-keygen -t rsa -b 2048
```

Voor meer gedetailleerde informatie over hoe u SSH-sleutelparen maakt, raadpleegt u [Create and use an SSH public-private key pair for Linux VMs in Azure](../virtual-machines/linux/mac-create-ssh-keys.md) (Een openbaar-persoonlijk SSH-sleutelpaar maken en gebruiken voor virtuele Linux-machines in Azure).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-batch-ai-workspace"></a>Een Batch AI-werkruimte maken

Maak eerst een Batch AI-werkruimte waarin u uw Batch AI-resources kunt organiseren. Een werkruimte kan een of meer clusters of andere Batch AI-resources bevatten.

1. Selecteer **Alle services** en filter op **Batch AI**.

2. Selecteer **Werkruimte toevoegen**.

3. Geef waarden op voor **Naam van de werkruimte** en **Resourcegroep**. Indien gewenst kunt u andere opties selecteren voor het **Abonnement** en de **Locatie** voor de werkruimte. Selecteer **Werkruimte maken**.

  ![Batch AI-werkruimte maken](./media/quickstart-create-cluster-portal/create-workspace.png)

Wanneer het bericht **Implementatie is voltooid** wordt weergegeven, gaat u naar de resource die u hebt gemaakt en selecteert u de werkruimte.

## <a name="create-a-file-server"></a>Een bestandsserver maken

Een Batch AI-bestandsserver is een NFS met één knooppunt dat automatisch kan worden gekoppeld aan clusterknooppunten. Dit is een van de diverse manieren waarop u in- en uitvoergegevens van uw trainingstaken kunt opslaan.

1. Selecteer in de werkruimte **Bestandsserver** > **Batch AI-bestandsserver toevoegen**.

2. Geef waarden op voor **Naam van bestandsserver** en **VM-grootte**. Voor deze snelstart raden we de VM-grootte *Standard D1_v2* aan voor de bestandsserver. Kies een andere grootte als u grotere hoeveelheden in- of uitvoergegevens voor trainingstaken wilt opslaan.

3. Geef een **Gebruikersnaam van beheerder** op en kopieer de inhoud van uw openbare SSH-sleutelbestand naar **SSH-sleutel**. Accepteer de standaardinstellingen voor de overige waarden en selecteer **Bestandsserver maken**.

  ![Een Batch AI-bestandsserver maken](./media/quickstart-create-cluster-portal/create-file-server.png)

Het implementeren van de bestandsserver duurt enkele minuten.

Wanneer de server is gemaakt, klikt u op **Eigenschappen** en noteert u de **Instellingen voor koppelen**. U kunt de SSH koppelen aan het openbare IP-adres van de server om trainingsgegevens en uitvoerbestanden te uploaden en downloaden naar de aangegeven map (*/data*).

![Eigenschappen van bestandsserver](./media/quickstart-create-cluster-portal/file-server-properties.png)

## <a name="create-a-cluster"></a>Een cluster maken

Met de volgende stappen maakt u een cluster met één GPU-knooppunt. Het clusterknooppunt voert een standaard Ubuntu Server-installatiekopie uit die is ontworpen om op container gebaseerde toepassingen te hosten en die u voor de meeste trainingsworkloads kunt gebruiken. Het clusterknooppunt koppelt de bestandsserver op het koppelpunt. 

1. In uw Batch AI-werkruimte selecteert u **Cluster** > **Batch AI-cluster toevoegen**.

2. Geef waarden op voor **Clusternaam** en de volgende instellingen. De aanbevolen VM-grootte is één NVIDIA Tesla K80 GPU.
  
   |Instelling  |Waarde  |
   |---------|---------|
   |**VM-grootte**     |Standard NC6|
   |**Beoogd aantal knooppunten**     |1|

3. Geef een **Gebruikersnaam van beheerder** op en kopieer de inhoud van uw openbare SSH-sleutelbestand naar **SSH-sleutel**. Accepteer de standaardinstellingen voor de resterende waarden op deze pagina en selecteer **Volgende: knooppunt instellen**.

   ![Basisgegevens voor cluster invoeren](./media/quickstart-create-cluster-portal/create-cluster.png)

4. Selecteer onder **Volumes koppelen** de optie **Verwijzingen naar bestandsserver** > **Toevoegen**. Selecteer de bestandsserver die u eerder hebt gemaakt. Geef een **Relatief koppelingspad** op waar de bestandsserver is gekoppeld aan elk clusterknooppunt. Selecteer **Opslaan en doorgaan**.

   ![Verwijzing naar bestandsserver toevoegen](./media/quickstart-create-cluster-portal/file-server-reference.png)

Sla de knooppuntinstellingen op en selecteer **Cluster maken**.

Het duurt een paar minuten totdat Batch AI het knooppunt heeft toegewezen. De **Toewijzingsstatus** van het cluster is dan ingesteld op **Formaat wijzigen**. Na enkele minuten verandert de status van het cluster in **Onveranderlijk** en wordt het knooppunt gestart.

![Cluster starten](./media/quickstart-create-cluster-portal/cluster-resizing.png)

Selecteer de clusternaam om de status van het knooppunt te controleren. Wanneer een knooppunt de status **Niet-actief** heeft, kunt u trainingstaken op het knooppunt uitvoeren.

### <a name="list-cluster-nodes"></a>Clusterknooppunten vermelden

Als u clusterknooppunten (in dit geval een enkel knooppunt) moet verbinden om toepassingen te installeren of onderhoud uit te voeren, haalt u verbindingsinformatie op in de portal. Nadat het cluster is gemaakt, klikt u op **Knooppunten** en noteert u de instellingen voor SSH bij **Verbinding** (IP-adres en poortnummer).

![Clusterknooppunten](./media/quickstart-create-cluster-portal/cluster-nodes.png)

Gebruik deze informatie om een SSH-verbinding met het knooppunt te maken. Vervang het juiste IP-adres en poortnummer van uw knooppunt bijvoorbeeld door de volgende opdracht:

```bash
ssh myusername@137.135.82.15 -p 50000
``` 

### <a name="resize-the-cluster"></a>Het formaat van het cluster wijzigen

Wanneer u uw cluster gebruikt om een model te trainen, hebt u mogelijk meer rekenresources nodig. Als u het formaat bijvoorbeeld wilt verhogen naar 2 knooppunten voor een gedistribueerde trainingstaak, selecteert u **Schaal aanpassen** en stelt u **Beoogd aantal knooppunten** in op 2. Sla de configuratie op.

![Schaal cluster aanpassen](./media/quickstart-create-cluster-portal/scale-cluster.png)

Het duurt een aantal minuten voordat het cluster in grootte is aangepast.

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt doorgaan met Batch AI-zelfstudies en -voorbeelden, gebruikt u de Batch AI-werkruimte, de bestandsserver en het cluster die u in deze snelstart hebt gemaakt.

Er worden kosten in rekening gebracht voor het Batch AI-cluster en de bestandsserver wanneer onderliggende virtuele machines worden uitgevoerd, ook als er geen taken zijn gepland. Als u de clusterconfiguratie wilt behouden wanneer u geen taken hebt om uit te voeren, verandert u het formaat van het cluster in 0 knooppunten. U kunt het formaat later wijzigen naar 1 of meer knooppunten om uw taken uit te voeren. 

Wanneer u deze niet meer nodig hebt, verwijdert u de Batch AI-werkruimte met het cluster en de bestandsserver. Selecteer hiervoor de Batch AI-werkruimte en kies **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u geleerd hoe u een Batch AI-cluster met een bijgevoegde bestandsserver maakt met behulp van de Azure-portal. Voor meer informatie over hoe u een Batch AI-cluster kunt gebruiken om een model te trainen, gaat u door met de snelstart over de training van een deep learning-model.

> [!div class="nextstepaction"]
> [Een deep learning-model trainen](./quickstart-tensorflow-training-cli.md)
