---
title: Verbinding maken met een account met Amazon Web Services naar Azure kosten Management | Microsoft Docs
description: Verbinding maken met een Amazon Web Services-account om kosten en gebruiksgegevens in kostenbeheer rapporten weer te geven.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 02/08/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 4a0280420132aad9f1e0b17d5998ec225bb0eaa1
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Verbinding maken met een Amazon Web Services-account

Hebt u twee opties om te verbinden van uw account Amazon Web Services (AWS) naar Azure kosten Management. U kunt verbinding maken met een rol IAM of met een alleen-lezen IAM-gebruikersaccount. De IAM-functie wordt aanbevolen omdat Hiermee kunt u een gemachtigde met gedefinieerde machtigingen voor vertrouwde entiteiten. De IAM-rol, hoeft u voor het delen van de lange termijn toegangstoetsen. Nadat u een AWS-account verbinding met kostenbeheer, is kosten en gebruiksgegevens beschikbaar in kosten-rapporten. Dit document begeleidt u bij beide opties.

Zie voor meer informatie over de identiteiten AWS IAM [identiteiten (gebruikers, groepen en rollen)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

## <a name="aws-role-based-access"></a>AWS op rollen gebaseerde toegang

De volgende secties helpt u bij het maken van een alleen-lezen IAM-rol om toegang te bieden voor beheer van kosten.

### <a name="get-your-cost-management-account-external-id"></a>Uw externe kostenbeheer account-ID ophalen

De eerste stap is de unieke verbinding wachtwoordzin ophalen van de beheerportal van Azure kosten. Het wordt gebruikt in AWS als de **externe ID**.

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar [https://azure.cloudyn.com](https://azure.cloudyn.com) en zich aanmelden.
2. Klik op **instellingen** (tandwiel pictogram) en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. In de **AWS-Account toevoegen** dialoogvenster, Kopieer de **externe ID** en slaat u deze waarde voor het maken van de stappen in de volgende sectie AWS-rol. In de volgende afbeelding, de voorbeeld-ID is _Cloudyn_. Uw ID verschilt.  
    ![Externe ID](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS alleen-lezen toegang op basis van rollen toevoegen

1. Aanmelden bij de AWS-console op https://console.aws.amazon.com/iam/home en selecteer **rollen**.
2. Klik op **rol maken** en selecteer vervolgens **een andere AWS-account**.
3. Plak de identiteit van de `432263259397` in de **Account-ID** veld. Deze Account-ID is de kostenbeheer data collector-account dat u moet gebruiken.
4. Naast **opties**, selecteer **vereisen externe ID** plak vervolgens de waarde die u eerder hebt gekopieerd in de **externe ID** veld en klik vervolgens op **volgende: Machtigingen**.  
    ![Rol maken](./media/connect-aws-account/create-role01.png)
5. Onder **koppelen van machtigingsbeleid**, in de **beleidstype** filter vak zoeken, type `ReadOnlyAccess`, selecteer **ReadOnlyAccess**, klikt u vervolgens op **volgende: Bekijk**.  
    ![Alleen-lezentoegang](./media/connect-aws-account/readonlyaccess.png)
6. Op de pagina controleren Controleer uw selecties kloppen en typ een **rolnaam**. Bijvoorbeeld: *Azure-kosten-Mgt*. Voer een **beschrijving van de functie**. Bijvoorbeeld: _roltoewijzing voor het beheer van Azure kosten_, klikt u vervolgens op **rol maken**.
7. In de **rollen** lijst, klik op de rol die u hebt gemaakt en kopieer de **rol informatie** waarde van de pagina overzicht. De waarde van de rol informatie later gebruiken wanneer u uw configuratie in Azure kosten Management registreert.  
    ![Rol informatie](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Configureer de AWS IAM roltoegang in kostenbeheer

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com/ en aanmelden.
2. Klik op **instellingen** (tandwiel pictogram) en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. In **accountnaam**, typ een naam voor het account.
5. Naast **toegangstype**, selecteer **IAM rol**.
6. In de **rol informatie** veld, plak de waarde die u eerder hebt gekopieerd en klik vervolgens op **opslaan**.  
    ![De status van de AWS-account](./media/connect-aws-account/aws-account-status01.png)

Uw AWS-account wordt weergegeven in de lijst van accounts. De **eigenaar-ID** vermeld overeenkomt met de waarde van de informatie van de rol. Uw **accountstatus** moet een groen vinkje symbool.

Kosten beheer begint verzamelen van de gegevens en rapporten in te vullen. Sommige rapporten optimalisatie vereisen echter gegevens uit een paar dagen voordat nauwkeurige aanbevelingen worden gegenereerd.

## <a name="aws-user-based-access"></a>AWS-toegang op basis van gebruiker

De volgende secties helpt u bij het maken van een gebruiker alleen-lezen toegang te hebben tot kostenbeheer.

### <a name="add-aws-read-only-user-based-access"></a>AWS alleen-lezen toegang op basis van een gebruiker toevoegen

1. Aanmelden bij de AWS-console op https://console.aws.amazon.com/iam/home en selecteer **gebruikers**.
2. Klik op **gebruiker toevoegen**.
3. In de **gebruikersnaam** veld, typ de naam van een gebruiker.
4. Voor **toegangstype**, selecteer **toegang op programmeerniveau** en klik op **volgende: machtigingen**.  
    ![gebruiker toevoegen](./media/connect-aws-account/add-user01.png)
5. Selecteer voor machtigingen, **bestaande beleidsregels rechtstreeks koppelen**.
6. Onder **koppelen van machtigingsbeleid**, in de **beleidstype** filter vak zoeken, type `ReadOnlyAccess`, selecteer **ReadOnlyAccess**, en klik vervolgens op **volgende : Bekijk**.  
    ![Machtigingen voor gebruiker instellen](./media/connect-aws-account/set-permission-for-user.png)
7. Controleer uw selecties kloppen en klik vervolgens op op de pagina controleren **gebruiker maken**.
8. Op de pagina is voltooid, worden uw sleutel-ID en geheim toegang toegangssleutel weergegeven. U kunt deze informatie gebruiken voor het configureren van inschrijving in beheer van kosten.
9. Klik op **downloaden CSV** en sla het bestand credentials.csv op een veilige locatie.  
    ![Referenties downloaden](./media/connect-aws-account/download-csv.png)


### <a name="configure-aws-iam-user-based-access-in-cost-management"></a>Configureer de AWS IAM op basis van een gebruiker toegang in kostenbeheer

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com/ en aanmelden.
2. Klik op **instellingen** (tandwiel pictogram) en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. Voor **accountnaam**, typ de naam van een account.
5. Naast **toegangstype**, selecteer **IAM gebruiker**.
6. In **toegangssleutel**, plak de **toegang ID sleutel** waarde uit het bestand credentials.csv.
7. In **geheime sleutel**, plak de **geheime toegangssleutel** waarde van het bestand credentials.csv en klik vervolgens op **opslaan**.  
    ![AWS-account gebruikersstatus](./media/connect-aws-account/aws-user-account-status.png)

Uw AWS-account wordt weergegeven in de lijst van accounts. Uw **accountstatus** moet een groen vinkje symbool.

Kosten beheer begint verzamelen van de gegevens en rapporten in te vullen. Sommige rapporten optimalisatie vereisen echter gegevens uit een paar dagen voordat nauwkeurige aanbevelingen worden gegenereerd.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over het beheer van Azure kosten, blijven de [gebruik en kosten bekijken](tutorial-review-usage.md) zelfstudie voor het beheer van kosten.
