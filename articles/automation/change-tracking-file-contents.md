---
title: Wijzigingen van bestandsinhoud weergeven met Azure Automation
description: Gebruik de functie voor het wijzigen van de inhoud van bestand van wijzigingen bijhouden om weer te geven van de inhoud van een bestand dat is gewijzigd.
services: automation
ms.service: automation
ms.component: change-inventory-management
author: georgewallace
ms.author: gwallace
ms.date: 07/03/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 0582505d66bbef3064359fa4047676c4ba60b4e9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37871756"
---
# <a name="view-contents-of-a-file-that-is-being-tracked-with-change-tracking"></a>Inhoud van een bestand dat wordt bijgehouden met wijzigingen bijhouden weergeven

Inhoud bijhouden bestand kunt u de inhoud van een bestand weergeven voor en na een wijziging die worden bijgehouden met wijzigingen bijhouden. U doet dit door wordt opgeslagen in de inhoud van het bestand een storage-account nadat elke wijziging optreedt.

## <a name="requirements"></a>Vereisten

* Een standard storage-account met het implementatiemodel van Resource Manager is vereist voor het opslaan van de inhoud van bestand. Premium- en klassieke implementatie model storage-accounts moeten niet worden gebruikt. Zie voor meer informatie over de storage-accounts, [over Azure storage-accounts](../storage/common/storage-create-storage-account.md)

* Het opslagaccount die gebruikt kan alleen 1 Automation-Account verbonden hebben.

* [Wijzigingen bijhouden](automation-change-tracking.md) is ingeschakeld in uw Automation-Account.

## <a name="enable-file-content-tracking"></a>Bestand inhoud bijhouden inschakelen

1. Open uw Automation-account in de Azure-portal en selecteer vervolgens **wijzigingen bijhouden**.
2. Selecteer op het bovenste menu **instellingen bewerken**.
3. Selecteer **bestandsinhoud** en klikt u op **koppeling**. Hiermee opent u de **Inhoudslocatie toevoegen voor wijzigingen bijhouden** deelvenster.

   ![inschakelen](./media/change-tracking-file-contents/enable.png)

4. Selecteer het abonnement en de storage-account te gebruiken voor het opslaan van de inhoud van het bestand op. Als u wilt een bestand inhoud bijhouden voor alle bestaande bijgehouden bestanden inschakelen, selecteert u **op** voor **bestandsinhoud voor alle instellingen uploaden**. U kunt dit wijzigen voor elk pad daarna.

   ![storage-account instellen](./media/change-tracking-file-contents/storage-account.png)

5. Eenmaal is ingeschakeld, worden de storage-account en de SAS-URI's weergegeven. De SAS-URI's 365 dagen na het verlopen en opnieuw kunnen worden gemaakt door te klikken op de **opnieuw genereren** knop.

   ![een lijst met accountsleutels](./media/change-tracking-file-contents/account-keys.png)

## <a name="add-a-file"></a>Een bestand toevoegen

De volgende stappen helpen u bij het inschakelen van bijhouden van wijzigingen voor een bestand:

1. Op de **instellingen bewerken** pagina van **bijhouden**, selecteert u **Windows bestanden** of **Linux-bestanden** tabblad en klik op  **Toevoegen**

1. Vul de gegevens voor het pad en selecteer **waar** onder **bestandsinhoud voor alle instellingen uploaden**. Deze instelling kunt bijhouden voor dit bestandspad alleen de inhoud van bestand.

   ![een linux-bestand toevoegen](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="viewing-the-contents-of-a-tracked-file"></a>De inhoud van een bijgehouden bestand weergeven

1. Wanneer een wijziging gedetecteerd voor het bestand of een bestand in het pad, ziet u in de portal. Selecteer de wijziging van het gegevensbestand in de lijst met wijzigingen. De **details wijzigen** deelvenster wordt weergegeven.

   ![lijst met wijzigingen](./media/change-tracking-file-contents/change-list.png)

1. Op de **details wijzigen** pagina, ziet u de standaard voor en na de informatie in de linkerbovenhoek het bestand, klikt u op **wijzigingen van bestandsinhoud weergeven** om te zien van de inhoud van het bestand.

  ![details wijzigen](./media/change-tracking-file-contents/change-details.png)

1. De nieuwe pagina ziet u de inhoud van het bestand in een side-by-side-weergave. U kunt ook selecteren **Inline** om een inline-weergave van de wijzigingen te zien.

  ![wijzigingen in bestanden weergeven](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="next-steps"></a>Volgende stappen

Ga naar de zelfstudie over het bijhouden van wijzigingen voor meer informatie over het gebruik van de oplossing:

> [!div class="nextstepaction"]
> [Problemen met wijzigingen in uw omgeving oplossen](automation-tutorial-troubleshoot-changes.md)

* Gebruik [zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md) om gedetailleerde bijhouden van gegevens weer te geven.
