---
title: Azure Active Directory-toegang configureren
description: Azure Blockchain-Service configureren met Azure Active Directory-toegang
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: seal
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: 616e342f1d52179c40c225c5dafc9de13ce85e06
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028213"
---
# <a name="how-to-configure-azure-active-directory-access"></a>Azure Active Directory-toegang configureren

In dit artikel leert u hoe u toegang verlenen en verbinding maken met Azure Blockchain Service knooppunten met behulp van de gebruiker, groep of toepassing-id's van Azure Active Directory (Azure AD).

Azure AD biedt identiteitsbeheer cloud-gebaseerde en kunt u één identiteit gebruiken voor een hele onderneming en toegang tot toepassingen in Azure. Azure Blockchain-Service is geïntegreerd met Azure AD en biedt voordelen zoals ID-federation, eenmalige aanmelding en meervoudige verificatie.

## <a name="prerequisites"></a>Vereisten

* [Een blockchain-lid met de Azure portal maken](create-member.md)

## <a name="grant-access"></a>Toegang verlenen

U kunt toegang verlenen op zowel het lidniveau van het en het knooppuntniveau van het. Verlenen van rechten op het lidniveau van het wordt op zijn beurt toegang verlenen tot alle knooppunten onder het lid.

### <a name="grant-member-level-access"></a>Niveau toegang verlenen lid

Om toegangsmachtiging te verlenen op het lidniveau van het.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar **toegangsbeheer (IAM) > toevoegen > roltoewijzing toevoegen**.
1. Selecteer de **Blockchain lid knooppunt toegang (Preview)** rol en de Azure AD-ID-object dat u wilt toegang verlenen tot toevoegen. Azure AD-ID-object kan zijn:

    | Azure AD-object | Voorbeeld |
    |-----------------|---------|
    | Azure AD-gebruiker   | `frank@contoso.onmicrosoft.com` |
    | Azure AD-groep  | `sales@contoso.onmicrosoft.com` |
    | Toepassings-id  | `13925ab1-4161-4534-8d18-812f5ca1ab1e` |

    ![Roltoewijzing toevoegen](./media/configure-aad/add-role-assignment.png)

1. Selecteer **Opslaan**.

### <a name="grant-node-level-access"></a>Knooppunt niveau toegang verlenen

1. U kunt toegang verlenen op het knooppuntniveau van het door te navigeren naar de beveiliging van knooppunt en klik op de naam van het knooppunt dat u wilt om toegang te verlenen.
1. Selecteer de rol Blockchain lid knooppunt toegang (Preview) en de Azure AD-ID-object dat u wilt toegang verlenen tot toevoegen. 

## <a name="connect-using-azure-blockchain-connector"></a>Verbinding maken met behulp van Azure Blockchain-Connector

Download of kloon de [Azure Blockchain-Connector vanuit GitHub](https://github.com/Microsoft/azure-blockchain-connector/).

```bash
git clone https://github.com/Microsoft/azure-blockchain-connector.git
```

De volgende de Quick Start-sectie in de **Leesmij-bestand** aan het bouwen van de connector van de broncode.

### <a name="connect-using-an-azure-ad-user-account"></a>Verbinding maken met behulp van een Azure AD-gebruikersaccount

1. Voer de volgende opdracht om te verifiëren met behulp van een Azure AD-gebruikersaccount. Vervang \<myAADDirectory\> met een Azure AD-domein. Bijvoorbeeld `yourdomain.onmicrosoft.com`.

    ```
    connector.exe -remote <myMemberName>.blockchain.azure.com:3200 -method aadauthcode -tenant-id <myAADDirectory> 
    ```

1. Azure AD wordt gevraagd om referenties.
1. Meld u aan met uw gebruikersnaam en wachtwoord.
1. Bij een geslaagde verificatie, is uw lokale proxy maakt verbinding met uw blockchain-knooppunt. U kunt nu uw Geth-client met het lokale eindpunt koppelen.

    ```bash
    geth attach http://127.0.0.1:3100
    ```

### <a name="connect-using-an-application-id"></a>Verbinding maken met behulp van een toepassings-ID

Veel toepassingen worden geverifieerd met Azure AD met behulp van een toepassings-ID in plaats van een Azure AD-gebruikersaccount.

Voor verbinding met het knooppunt met behulp van een toepassings-ID, Vervang **aadauthcode** met **aadclient**.

```
connector.exe -remote <myBlockchainEndpoint>  -method aadclient -client-id <myClientID> -client-secret "<myClientSecret>" -tenant-id <myAADDirectory>
```

| Parameter | Description |
|-----------|-------------|
| tenant-id | Azure AD-domein, bijvoorbeeld: `yourdomain.onmicrosoft.com`
| client-id | Client-ID van de geregistreerde toepassing in Azure AD
| client-secret | Clientgeheim van de geregistreerde toepassing in Azure AD

Zie voor meer informatie over het registreren van een toepassing in Azure AD [het: De portal gebruiken om een Azure AD-toepassing en service-principal die toegang hebben tot resources te maken](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="connect-a-mobile-device-or-text-browser"></a>Verbinding maken met een mobiel apparaat of de tekst-browser

Voor een mobiel apparaat of de op tekst gebaseerde browser wanneer de pop-weergave van Azure AD-verificatie niet mogelijk is, genereert Azure AD een eenmalige wachtwoordcode. U kunt kopiëren van de wachtwoordcode en doorgaan met de Azure AD-verificatie in een andere omgeving.

Voor het genereren van de wachtwoordcode, Vervang **aadauthcode** met **aaddevice**. Vervang \<myAADDirectory\> met een Azure AD-domein. Bijvoorbeeld `yourdomain.onmicrosoft.com`.

```
connector.exe -remote <myBlockchainEndpoint>  -method aaddevice -tenant-id <myAADDirectory>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de gegevensbeveiliging in Azure Blockchain-Service:

> [!div class="nextstepaction"]
> [Azure Blockchain-Service-beveiliging](data-security.md)