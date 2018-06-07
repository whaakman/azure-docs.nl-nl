---
title: Opslagverkenner verbinding met een Azure-Stack-abonnement of een opslagaccount | Microsoft Docs
description: Leer hoe Opslagverkenner verbindt met een Azure-Stack-abonnement
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 9704f05cc6da97e33c0043b93acedc9e66bdcc36
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714898"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Opslagverkenner verbinding met een Azure-Stack-abonnement of een opslagaccount

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

In dit artikel leert u hoe u verbinding maken met uw Azure-Stack-abonnementen en opslagaccounts met Opslagverkenner. Azure Opslagverkenner is een zelfstandige app waardoor u eenvoudig werken met de Stack van Azure storage-gegevens op Windows-, Mac OS- en Linux.

> [!NOTE]  
> Er zijn verschillende hulpprogramma's beschikbaar om gegevens te verplaatsen naar en van de Stack van Azure storage. Zie voor meer informatie [gegevensoverdracht hulpprogramma's voor de opslag van Azure-Stack](azure-stack-storage-transfer.md).

Als u Opslagverkenner nog niet hebt geïnstalleerd [downloaden Opslagverkenner](http://www.storageexplorer.com/) en te installeren.

Nadat u verbinding met een Azure-Stack-abonnement of een opslagaccount maakt, kunt u de [Azure storage explorer artikelen](../../vs-azure-tools-storage-manage-with-storage-explorer.md) werken met de gegevens van uw Azure-Stack. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Voorbereiden voor het verbinden met Azure-Stack

Moet u directe toegang tot de Azure-Stack of een VPN-verbinding voor Opslagverkenner voor toegang tot de Stack van Azure-abonnement. Raadpleeg [Verbinding maken met Azure Stack met VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) voor meer informatie over het instellen van een VPN-verbinding naar Azure Stack.

Voor de Azure-Stack Development Kit moet u exporteren van het basiscertificaat van de Azure-Stack-instantie.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Exporteren en vervolgens de Azure-Stack-certificaten importeren

1. Open `mmc.exe` op de computer van een Azure-Stack host of op een lokale computer met een VPN-verbinding met Azure-Stack. 

2. In **bestand**, selecteer **module toevoegen/verwijderen**, en voeg vervolgens **certificaten** voor het beheren van **Mijn gebruikersaccount**.

3. Onder **Console Root\Certificated (lokale Computer) \Trusted Root Certification Authorities\Certificates** vinden **AzureStackSelfSignedRootCert**.

    ![Laad het Azure Stack-basiscertificaat via mmc.exe](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

4. Met de rechtermuisknop op het certificaat, selecteert u **alle taken** > **exporteren**, en volg de instructies voor het exporteren van het certificaat met **Base-64 gecodeerde X.509 (. CER)**.

    Het geëxporteerde certificaat zal worden gebruikt in de volgende stap.

5. Open Opslagverkenner, en als u ziet de **verbinding maken met Azure Storage** dialoogvenster vak, te annuleren.

6. Op de **bewerken** in het menu **SSL-certificaten**, en selecteer vervolgens **certificaten importeren**. Gebruik het dialoogvenster Bestand kiezen om het certificaat dat u in de vorige stap hebt gezocht te zoeken en te openen.

    Na het importeren van het certificaat wordt gevraagd of u Opslagverkenner opnieuw opstarten.

    ![Importeer het certificaat in Opslagverkenner](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

7. Nadat u Opslagverkenner opnieuw is opgestart, selecteert u de **bewerken** menu en controleert u of **doel Azure Stack** is geselecteerd. Dit niet het geval is, selecteert u **doel Azure Stack**, en start vervolgens opnieuw Opslagverkenner om de wijziging van kracht te laten worden. Deze configuratie is vereist om compatibiliteit met uw Azure Stack-omgeving te garanderen.

    ![Zorg dat de optie Azure Stack als doel is geselecteerd](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription"></a>Verbinding maken met een Azure Stack-abonnement

Gebruik de volgende stappen Opslagverkenner verbinding met een Azure-Stack-abonnement.

1. Selecteer in het linkerdeelvenster van Opslagverkenner **Accounts beheren**. 
    Alle Microsoft-abonnement dat u aangemeld worden weergegeven.

2. Selecteer voor verbinding met het Azure-Stack-abonnement, **account toevoegen**.

    ![Een Azure Stack-account toevoegen](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. In het verbinding maken met Azure Storage dialoogvenster onder **Azure-omgeving**, selecteer **Azure** of **Azure China**, die afhankelijk van het Azure-Stack-account dat wordt gebruikt, selecteert u **Aanmelden** zich kunnen aanmelden met de Azure-Stack-account die is gekoppeld aan ten minste één actief Azure-Stack-abonnement.

    ![Verbinding maken met Azure Storage](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Nadat u bent aangemeld met een Azure Stack-account, worden in het linkerdeelvenster de Azure Stack-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure Stack-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Door het selectievakje **Alle abonnementen** in of uit te schakelen, selecteert u of alle of geen van de vermelde Azure Stack-abonnementen.)

    ![De Azure Stack-abonnementen selecteren na het invullen van het dialoogvenster Aangepaste cloudomgeving](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure Stack-abonnementen zijn gekoppeld.

    ![Lijst met opslagaccounts, waaronder Azure Stack-abonnementsaccounts](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Verbinding maken met een Stack van Azure storage-account

U kunt ook verbinding maken met een Stack van Azure storage-account met behulp van de opslagaccountnaam en sleutelpaar.

1. Selecteer in het linkerdeelvenster van Opslagverkenner Accounts beheren. Alle Microsoft-accounts die u aangemeld worden weergegeven.

    ![Een account toevoegen](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Selecteer voor verbinding met het Azure-Stack-abonnement, **account toevoegen**.

    ![Een account toevoegen](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. Selecteer in het verbinding maken met het dialoogvenster Azure Storage, **gebruik van een naam van het opslagaccount en de sleutel**.

4. Voer de accountnaam van uw in de **accountnaam**, plak accountsleutel in de **accountsleutel** in het tekstvak, selecteer **andere (hieronder opgeven)** in **opslag eindpunten domein** en voer het Azure-Stack-eindpunt.

    Een Azure-Stack-eindpunt bestaat uit twee onderdelen: de naam van een regio en het domein van Azure-Stack. In de Azure-Stack Development Kit het standaardeindpunt is **local.azurestack.external**. Neem contact op met de cloudbeheerder als u niet zeker weet over uw eindpunt.

    ![Naam en sleutel koppelen](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. Selecteer **Verbinden**.
6. Nadat het opslagaccount is toegevoegd, wordt het opslagaccount weergegeven met (**externe, andere**) toegevoegd aan de naam ervan.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Opslagverkenner](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Stack aan Azure storage: verschillen en overwegingen](azure-stack-acs-differences.md)
* Zie voor meer informatie over Azure storage, [Inleiding tot Microsoft Azure storage](../../storage/common/storage-introduction.md)
