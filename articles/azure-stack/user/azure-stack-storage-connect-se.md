---
title: Storage Explorer verbinding met een Azure-Stack-abonnement
description: Informatie over het aansluiten van Verkenner opslag met een Azure-Stack-abonnement
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: c7e6d70148d39fd74f6409a0a239833f8e9f7614
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Storage Explorer verbinding met een Azure-Stack-abonnement

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Opslagverkenner (Preview) is een zelfstandige app waardoor u eenvoudig werken met Azure Stack Storage-gegevens op Windows-, Mac OS- en Linux. Er zijn verschillende hulpprogramma's voor beschikbare gegevens te verplaatsen naar en van Azure-Storage-Stack. Zie voor meer informatie [gegevensoverdracht hulpprogramma's voor de opslag van Azure-Stack](azure-stack-storage-transfer.md).

In dit artikel leert u hoe u verbinding maken met uw Azure-Stack-opslagaccounts met Opslagverkenner. 

Als u Opslagverkenner nog niet hebt geïnstalleerd [downloaden](http://www.storageexplorer.com/) en en te installeren.

Nadat u verbinding met uw Azure-Stack-abonnement, kunt u de [Azure Opslagverkenner artikelen](../../vs-azure-tools-storage-manage-with-storage-explorer.md) werken met de gegevens van uw Azure-Stack. 

## <a name="prepare-an-azure-stack-subscription"></a>Voorbereiden van een Azure-Stack-abonnement

U moet toegang tot de Azure-Stack-hostmachine bureaublad of een VPN-verbinding voor Opslagverkenner voor toegang tot de Stack van Azure-abonnement. Raadpleeg [Verbinding maken met Azure Stack met VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) voor meer informatie over het instellen van een VPN-verbinding naar Azure Stack.

Voor de Azure-Stack Development Kit moet u exporteren van het basiscertificaat van de Azure-Stack-instantie.

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>Om te exporteren en vervolgens de Azure-Stack-certificaten importeren

1. Open `mmc.exe` op de computer van een Azure-Stack host of op een lokale computer met een VPN-verbinding met Azure-Stack. 

2. In **bestand**, selecteer **module toevoegen/verwijderen**, en voeg vervolgens **certificaten** voor het beheren van **Mijn gebruikersaccount**.



3. Onder **Console Root\Certificated (lokale Computer) \Trusted Root Certification Authorities\Certificates** vinden **AzureStackSelfSignedRootCert**.

    ![Laad het Azure Stack-basiscertificaat via mmc.exe][25]

4. Met de rechtermuisknop op het certificaat, selecteert u **alle taken** > **exporteren**, en volg de instructies voor het exporteren van het certificaat met **Base-64 gecodeerde X.509 (. CER)**.  

    Het geëxporteerde certificaat zal worden gebruikt in de volgende stap.
5. Open Opslagverkenner (Preview), en als u ziet de **verbinding maken met Azure Storage** dialoogvenster vak, te annuleren.

6. Op de **bewerken** in het menu **SSL-certificaten**, en klik vervolgens op **certificaten importeren**. Gebruik het dialoogvenster Bestand kiezen om het certificaat dat u in de vorige stap hebt gezocht te zoeken en te openen.

    Na het importeren wordt u gevraagd om Opslagverkenner opnieuw te starten.

    ![Importeer het certificaat in Opslagverkenner (Preview)][27]

U bent nu klaar Storage Explorer verbinding met een Azure-Stack-abonnement.

### <a name="to-connect-an-azure-stack-subscription"></a>Verbinding maken van een Azure-Stack-abonnement


1. Nadat Opslagverkenner (Preview) opnieuw is gestart, selecteert u het menu **Bewerken** en controleert u of de optie **Azure Stack als doel** is geselecteerd. Als dat niet het geval is, selecteert u deze en start u Opslagverkenner opnieuw om de wijziging door te voeren. Deze configuratie is vereist om compatibiliteit met uw Azure Stack-omgeving te garanderen.

    ![Zorg dat de optie Azure Stack als doel is geselecteerd][28]

7. Selecteer **Accounts beheren** in het linkerdeelvenster.  
    Alle Microsoft-accounts waarbij u bent aangemeld worden weergegeven.

8. Om verbinding te maken met een Azure Stack-account, selecteert u **Een account toevoegen**.

    ![Een Azure Stack-account toevoegen][29]

9. In de **verbinding maken met Azure Storage** dialoogvenster onder **Azure-omgeving**, selecteer **gebruik Azure Stack-omgeving**, en klik vervolgens op **volgende**.

10. Als u wilt aanmelden met de Azure-Stack-account dat is gekoppeld aan ten minste één actief Azure-Stack-abonnement, vult u de **aanmelden bij Azure Stack-omgeving** in het dialoogvenster.  

    De details voor elk veld zijn als volgt:

    * **De naam van de omgeving**: dit veld kan worden aangepast door de gebruiker.
    * **ARM-resource-eindpunt**: de voorbeelden van een Azure Resource Manager-resource-eindpunt:

        * Voor cloud-operator:<br> https://adminmanagement.local.azurestack.external   
        * Voor de tenant:<br> https://Management.local.azurestack.external
 
    * **Tenant-Id**: optioneel. Deze waarde wordt alleen ingevuld wanneer de map moet worden opgegeven.

12. Nadat u bent aangemeld met een Azure Stack-account, worden in het linkerdeelvenster de Azure Stack-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure Stack-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Door het selectievakje **Alle abonnementen** in of uit te schakelen, selecteert u of alle of geen van de vermelde Azure Stack-abonnementen.)

    ![De Azure Stack-abonnementen selecteren na het invullen van het dialoogvenster Aangepaste cloudomgeving][30]  
    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure Stack-abonnementen zijn gekoppeld.

    ![Lijst met opslagaccounts, waaronder Azure Stack-abonnementsaccounts][31]

## <a name="next-steps"></a>Volgende stappen
* [Aan de slag met Opslagverkenner (Preview)](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Storage Stack: verschillen en overwegingen](azure-stack-acs-differences.md)


* Zie voor meer informatie over Azure Storage, [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md)

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
