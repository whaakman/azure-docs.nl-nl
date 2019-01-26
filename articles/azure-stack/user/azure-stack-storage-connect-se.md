---
title: Storage explorer verbinden met een Azure Stack-abonnement of een opslagaccount | Microsoft Docs
description: Leer hoe u storage explorer verbinden met een Azure Stack-abonnement
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/24/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: d6d1373a97b62d54d5bfc2595ee773a242af877a
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54913444"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Storage explorer verbinden met een Azure Stack-abonnement of een storage-account

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel leert u hoe u verbinding maken met uw Azure Stack-abonnementen en opslagaccounts met behulp van storage explorer. Azure storage explorer is een zelfstandige app waarmee u eenvoudig met Azure Stack-storage-gegevens kunt werken via Windows, macOS en Linux.

> [!NOTE]  
> Er zijn verschillende hulpprogramma's beschikbaar om gegevens te verplaatsen naar en van Azure Stack-opslag. Zie voor meer informatie, [hulpprogramma's voor Azure Stack-opslag voor gegevensoverdracht](azure-stack-storage-transfer.md).

Als u Opslagverkenner nog niet hebt geïnstalleerd [Opslagverkenner downloaden](http://www.storageexplorer.com/) en installeer deze.

Nadat u verbinding met een Azure Stack-abonnement of een storage-account maken, kunt u de [Azure storage explorer artikelen](../../vs-azure-tools-storage-manage-with-storage-explorer.md) wilt werken met uw Azure Stack-gegevens. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Voorbereiden voor het verbinden met Azure Stack

Moet u directe toegang tot de Azure Stack of een VPN-verbinding voor Opslagverkenner voor toegang tot de Azure Stack-abonnement. Raadpleeg [Verbinding maken met Azure Stack met VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) voor meer informatie over het instellen van een VPN-verbinding naar Azure Stack.

Voor de Azure Stack Development Kit (ASDK), moet u de Azure Stack-instantie-basiscertificaat te exporteren.

> [!Note]  
> Voor de ASDK niet als u verbinding met uw ASDK via VPN, het basiscertificaat (CA.cer) die is gemaakt tijdens het installatieproces van de VPN gebruiken.  Dit is een met DER-gecodeerd certificaat, en staat niet toe dat de Storage Explorer om op te halen van uw Azure Stack-abonnementen. Volg onderstaande stappen voor het exporteren van een Base-64 gecodeerde certificaat wilt gebruiken met Storage Explorer.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Exporteren en vervolgens het Azure Stack-certificaat importeren

1. Open `mmc.exe` op een Azure Stack-hostmachine of een lokale computer met een VPN-verbinding met Azure Stack. 

2. In **bestand**, selecteer **module toevoegen/verwijderen**, en voeg deze **certificaten** voor het beheren van **Mijn gebruikersaccount**.

3.  Onder **Console Root\Certificated (Local Computer) \Trusted Root Certification Authorities\Certificates**.

    - Vinden voor de ASDK **AzureStackSelfSignedRootCert**.

        ![Laad het Azure Stack-basiscertificaat via mmc.exe](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

    - Voor een geïntegreerd systeem, zoek het basiscertificaat van uw externe certificaten. 
    
        ![Laad het Azure Stack-basiscertificaat via mmc.exe](./media/azure-stack-storage-connect-se/azure-stack-storage-cert-location-is.png)
        

4. Met de rechtermuisknop op het certificaat, selecteert u **alle taken** > **exporteren**, en volg de instructies voor het exporteren van het certificaat met **Base-64 gecodeerde X.509 (. CER)**.

    Het geëxporteerde certificaat zal worden gebruikt in de volgende stap.

5. Open Opslagverkenner, en als u ziet de **verbinding maken met Azure Storage** dialoogvenster vak, te annuleren.

6. Op de **bewerken** in het menu **SSL-certificaten**, en selecteer vervolgens **certificaten importeren**. Gebruik het dialoogvenster Bestand kiezen om het certificaat dat u in de vorige stap hebt gezocht te zoeken en te openen.

    Na het importeren van het certificaat wordt gevraagd of u Opslagverkenner opnieuw starten.

    ![Importeer het certificaat in Opslagverkenner](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

7. Nadat Opslagverkenner opnieuw is opgestart, selecteert u de **bewerken** menu en controleert u of **Azure Stack als doel** is geselecteerd. Als dit niet, selecteer **Azure Stack als doel**, en start u Opslagverkenner om de wijziging door te voeren. Deze configuratie is vereist om compatibiliteit met uw Azure Stack-omgeving te garanderen.

    ![Zorg dat de optie Azure Stack als doel is geselecteerd](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription-with-azure-ad"></a>Verbinding maken met een Azure Stack-abonnement met Azure AD

Gebruik de volgende stappen in storage explorer verbinden met een Azure Stack-abonnement, die deel uitmaakt van een Azure Active Directory (Azure AD)-account.

1. Selecteer in het linkerdeelvenster van Opslagverkenner **Accounts beheren**. 
    Alle Microsoft-abonnement dat u aangemeld worden weergegeven.

2. Selecteer voor verbinding met de Azure Stack-abonnement, **een account toevoegen**.

    ![Een Azure Stack-account toevoegen](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. In het dialoogvenster verbinding maken met Azure Storage, onder **Azure-omgeving**, selecteer **Azure** of **Azure China**, die afhankelijk is van de Azure Stack-account dat wordt gebruikt, selecteer **Aanmelden** zich aanmelden met de Azure Stack-account dat is gekoppeld aan ten minste één actief Azure Stack-abonnement.

    ![Verbinding maken met Azure Storage](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Nadat u bent aangemeld met een Azure Stack-account, worden in het linkerdeelvenster de Azure Stack-abonnementen weergegeven die aan dat account zijn gekoppeld. Selecteer de Azure Stack-abonnementen waarmee u wilt werken en selecteer vervolgens **Toepassen**. (Door het selectievakje **Alle abonnementen** in of uit te schakelen, selecteert u of alle of geen van de vermelde Azure Stack-abonnementen.)

    ![De Azure Stack-abonnementen selecteren na het invullen van het dialoogvenster Aangepaste cloudomgeving](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure Stack-abonnementen zijn gekoppeld.

    ![Lijst met opslagaccounts, waaronder Azure Stack-abonnementsaccounts](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-subscription-with-ad-fs-account"></a>Verbinding maken met een Azure Stack-abonnement met AD FS-account

> [!Note]  
> De aanmelding van Azure voor federatieve Service (AD FS) biedt ondersteuning voor Storage Explorer 1.2.0 of nieuwere versies met Azure Stack 1804 of nieuwere update.
Gebruik de volgende stappen uit in storage explorer verbinden met een Azure Stack-abonnement dat bij een AD FS-serviceaccount hoort.

1. Selecteer **Accounts beheren**. De explorer geeft een lijst van de Microsoft-abonnementen waartoe u aangemeld bij.
2. Selecteer **een account toevoegen** verbinding maken met de Azure Stack-abonnement.

    ![Een account toevoegen](media/azure-stack-storage-connect-se/add-an-account.png)

3. Selecteer **Volgende**. In het dialoogvenster verbinding maken met Azure Storage, onder **Azure-omgeving**, selecteer **gebruik aangepaste omgeving**, klikt u vervolgens op **volgende**.

    ![Verbinding maken met Azure Storage](media/azure-stack-storage-connect-se/connect-to-azure-storage.png)

4. Voer de vereiste gegevens van aangepaste Azure Stack-omgeving. 

    | Veld | Opmerkingen |
    | ---   | ---   |
    | Naam van de omgeving | Dit veld kan worden aangepast door de gebruiker. |
    | Azure Resource Manager-eindpunt | De voorbeelden van Azure Resource Manager-resource-eindpunt van Azure Stack Development Kit.<br>Voor operators: https://adminmanagement.local.azurestack.external <br> Voor gebruikers: https://management.local.azurestack.external |

    Als u werkt in Azure Stack-geïntegreerd systeem en niet het beheereindpunt kent, neem contact op met uw provider.

    ![Een account toevoegen](./media/azure-stack-storage-connect-se/custom-environments.png)

5. Selecteer **aanmelden**, verbinding maken met de Azure Stack-account dat is gekoppeld aan ten minste één actief Azure Stack-abonnement.



6. Selecteer het Azure Stack-abonnementen die u wilt werken. Selecteer **Toepassen**.

    ![Accountbeheer](./media/azure-stack-storage-connect-se/account-management.png)

    In het linkerdeelvenster worden de opslagaccounts weergegeven die aan de geselecteerde Azure Stack-abonnementen zijn gekoppeld.

    ![Overzicht van de bijbehorende abonnementen](./media/azure-stack-storage-connect-se/list-of-associated-subscriptions.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Verbinding maken met een Azure Stack-opslagaccount

U kunt ook verbinding maken met een Azure Stack-storage-account met behulp van storage-accountnaam en -sleutelpaar.

1. Selecteer in het linkerdeelvenster van Opslagverkenner Accounts beheren. Alle Microsoft-accounts waarop u aangemeld worden weergegeven.

    ![Een account toevoegen](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Selecteer voor verbinding met de Azure Stack-abonnement, **een account toevoegen**.

    ![Een account toevoegen](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. Selecteer in het dialoogvenster verbinding maken met Azure Storage, **een opslagaccountnaam en -sleutel gebruiken**.

4. Voer de accountnaam van uw in de **accountnaam**, plak accountsleutel in het **accountsleutel** tekstvak, selecteer **andere (hieronder opgeven)** in **opslag eindpunten domein** en voert u de Azure Stack-eindpunt.

    Een Azure Stack-eindpunt bestaat uit twee onderdelen: de naam van een regio en het Azure Stack-domein. In de Azure Stack Development Kit, het standaardeindpunt is **local.azurestack.external**. Neem contact op met de cloudbeheerder als u niet zeker weet over uw eindpunt.

    ![Naam en sleutel toevoegen](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. Selecteer **Verbinden**.
6. Nadat het opslagaccount dat is gekoppeld, wordt het opslagaccount weergegeven met (**externe, andere**) toegevoegd aan de naam.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met Opslagverkenner](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack-opslag: verschillen en overwegingen](azure-stack-acs-differences.md)
* Zie voor meer informatie over Azure storage, [Inleiding tot Microsoft Azure storage](../../storage/common/storage-introduction.md)
