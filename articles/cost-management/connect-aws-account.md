---
title: Verbinding maken met een account met Amazon Web Services naar Azure kosten Management | Microsoft Docs
description: Verbinding maken met een Amazon Web Services-account om kosten en gebruiksgegevens in kostenbeheer rapporten weer te geven.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/06/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 109235718f085ea2912f601f0657e08230e3e91d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="connect-an-amazon-web-services-account"></a>Verbinding maken met een Amazon Web Services-account

Hebt u twee opties om te verbinden van uw account Amazon Web Services (AWS) naar Azure kosten Management. U kunt verbinding maken met een rol IAM of met een alleen-lezen IAM-gebruikersaccount. De IAM-functie wordt aanbevolen omdat Hiermee kunt u een gemachtigde met gedefinieerde machtigingen voor vertrouwde entiteiten. De IAM-rol, hoeft u voor het delen van de lange termijn toegangstoetsen. Nadat u een AWS-account verbinding met kostenbeheer, is kosten en gebruiksgegevens beschikbaar in kosten-rapporten. Dit document begeleidt u bij beide opties.

Zie voor meer informatie over de identiteiten AWS IAM [identiteiten (gebruikers, groepen en rollen)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html).

Bieden ook AWS gedetailleerde rapporten facturerings- en de gegevens opslaan in een bucket AWS eenvoudige opslagindeling service (S3). Gedetailleerde facturering rapporten bevatten kosten met code en resourcegegevens op uurbasis. De rapporten opslaan, kunt kostenbeheer van uw bucket ophalen en de informatie wordt weergegeven in de rapporten ervan.


## <a name="aws-role-based-access"></a>AWS op rollen gebaseerde toegang

De volgende secties helpt u bij het maken van een alleen-lezen IAM-rol om toegang te bieden voor beheer van kosten.

### <a name="get-your-cost-management-account-external-id"></a>Uw externe kostenbeheer account-ID ophalen

De eerste stap is de unieke verbinding wachtwoordzin ophalen van de beheerportal van Azure kosten. Het wordt gebruikt in AWS als de **externe ID**.

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar [ https://azure.cloudyn.com ](https://azure.cloudyn.com) en zich aanmelden.
2. Klik op het symbool tandwiel en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. In de **AWS-Account toevoegen** dialoogvenster, Kopieer de **externe ID** en de waarde voor het maken van de stappen in de volgende sectie AWS-rol op te slaan. De externe ID is uniek voor uw account. In de volgende afbeelding in het voorbeeld externe ID is _Contoso_ gevolgd door een getal. Uw ID verschilt.  
    ![Externe ID](./media/connect-aws-account/external-id.png)

### <a name="add-aws-read-only-role-based-access"></a>AWS alleen-lezen toegang op basis van rollen toevoegen

1. Aanmelden bij de AWS-console op https://console.aws.amazon.com/iam/home en selecteer **rollen**.
2. Klik op **rol maken** en selecteer vervolgens **een andere AWS-account**.
3. In de **Account-ID** vak, plak `432263259397`. Deze Account-ID is het kostenbeheer collector account toegewezen door AWS aan de Cloudyn-service. Gebruik de exacte Account-ID weergegeven.
4. Naast **opties**, selecteer **vereisen externe ID**. Plak uw unieke waarde die u eerder hebt gekopieerd uit de **externe ID** kostenbeheer veld. Klik vervolgens op **volgende: machtigingen**.  
    ![Rol maken](./media/connect-aws-account/create-role01.png)
5. Onder **koppelen van machtigingsbeleid**, in de **beleidstype** filter vak zoeken, type `ReadOnlyAccess`, selecteer **ReadOnlyAccess**, klikt u vervolgens op **volgende: Bekijk**.  
    ![Alleen-lezentoegang](./media/connect-aws-account/readonlyaccess.png)
6. Op de pagina controleren Controleer uw selecties kloppen en typ een **rolnaam**. Bijvoorbeeld: *Azure-kosten-Mgt*. Voer een **beschrijving van de functie**. Bijvoorbeeld: _roltoewijzing voor het beheer van Azure kosten_, klikt u vervolgens op **rol maken**.
7. In de **rollen** lijst, klik op de rol die u hebt gemaakt en kopieer de **rol informatie** waarde van de pagina overzicht. De waarde van de rol informatie (bronnaam Amazon) later gebruiken wanneer u uw configuratie in Azure kosten Management registreert.  
    ![Rol informatie](./media/connect-aws-account/role-arn.png)

### <a name="configure-aws-iam-role-access-in-cost-management"></a>Configureer de AWS IAM roltoegang in kostenbeheer

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com/ en zich aanmelden.
2. Klik op het symbool tandwiel en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. In **accountnaam**, typ een naam voor het account.
5. Naast **toegangstype**, selecteer **IAM rol**.
6. In de **rol informatie** veld, plak de waarde die u eerder hebt gekopieerd en klik vervolgens op **opslaan**.  
    ![Vak AWS-Account toevoegen](./media/connect-aws-account/add-aws-account-box.png)


Uw AWS-account wordt weergegeven in de lijst van accounts. De **eigenaar-ID** vermeld overeenkomt met de waarde van de informatie van de rol. Uw **accountstatus** moet een groen vinkje symbool die aangeeft of kostenbeheer toegang hebt tot uw AWS-account hebben. Totdat u gedetailleerde AWS facturering inschakelt, wordt de consolidatiestatus van uw weergegeven als **zelfstandige**.

![De status van de AWS-account](./media/connect-aws-account/aws-account-status01.png)

Kosten beheer begint verzamelen van de gegevens en rapporten in te vullen. Vervolgens [inschakelen gedetailleerde AWS facturering](#enable-detailed-aws-billing).


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

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com/ en zich aanmelden.
2. Klik op het symbool tandwiel en selecteer vervolgens **Accounts in de Cloud**.
3. Selecteer in het beheer van Accounts de **AWS Accounts** tabblad en klik vervolgens op **nieuwe toevoegen +**.
4. Voor **accountnaam**, typ de naam van een account.
5. Naast **toegangstype**, selecteer **IAM gebruiker**.
6. In **toegangssleutel**, plak de **toegang ID sleutel** waarde uit het bestand credentials.csv.
7. In **geheime sleutel**, plak de **geheime toegangssleutel** waarde van het bestand credentials.csv en klik vervolgens op **opslaan**.  

Uw AWS-account wordt weergegeven in de lijst van accounts. Uw **accountstatus** moet een groen vinkje symbool.

Kosten beheer begint verzamelen van de gegevens en rapporten in te vullen. Vervolgens [inschakelen gedetailleerde AWS facturering](#enable-detailed-aws-billing).

## <a name="enable-detailed-aws-billing"></a>Gedetailleerde AWS facturering inschakelen

Gebruik de volgende stappen uit om op te halen van de informatie van uw AWS-rol. De informatie van de rol kunt u lezen machtigingen toewijzen aan een facturering bucket.

1. Aanmelden bij de AWS-console op https://console.aws.amazon.com en selecteer **Services**.
2. In de zoekopdracht Service vak type *IAM*, en deze optie selecteert.
3. Selecteer **rollen** uit in het menu links.
4. Selecteer de rol die u hebt gemaakt voor Cloudyn toegang in de lijst met rollen.
5. Op de pagina Functieoverzicht Hiermee kopieert u de **rol informatie**. Houd de rol informatie handig zijn voor de volgende stappen.

### <a name="create-an-s3-bucket"></a>S3-bucket maken

U maakt een S3-bucket voor het opslaan van gedetailleerde informatie over de facturering.

1. Aanmelden bij de AWS-console op https://console.aws.amazon.com en selecteer **Services**.
2. In de zoekopdracht Service vak type *S3*, en selecteer **S3**.
3. Klik op de pagina Amazon S3 **maken bucket**.
4. In de wizard bucket, kies een Bucket-naam en de regio en klik vervolgens op **volgende**.  
    ![Bucket maken](./media/connect-aws-account/create-bucket.png)
5. Op de **eigenschappen instellen** up, behoud de standaardwaarden en klik op **volgende**.
6. Klik op de pagina controleren op **maken bucket**. De lijst met bucket wordt weergegeven.
7. Klik op de bucket die u hebt gemaakt en selecteer de **machtigingen** tabblad en selecteer vervolgens **Bucket beleid**. De Bucket-editor wordt geopend.
8. Het volgende JSON-voorbeeld Kopieer en plak deze in de beleidseditor Bucket.
  - Vervang `<BillingBucketName>` met de naam van uw S3-bucket.
  - Vervang `<ReadOnlyUserOrRole>` met de rol of de gebruiker informatie die u eerder hebt gekopieerd.

  ```
  {
    "Version": "2012-10-17",
    "Id": "Policy1426774604000",
    "Statement": [
        {
            "Sid": "Stmt1426774604000",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": [
                "s3:GetBucketAcl",
                "s3:GetBucketPolicy"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>"
        },
        {
            "Sid": "Stmt1426774604001",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::386209384616:root"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        },
        {
            "Sid": "Stmt1426774604002",
            "Effect": "Allow",
            "Principal": {
                "AWS": "<ReadOnlyUserOrRole>"
            },
            "Action": [
                "s3:List*",
                "s3:Get*"
            ],
            "Resource": "arn:aws:s3:::<BillingBucketName>/*"
        }
    ]
  }
  ```

9. Klik op **Opslaan**.  
    ![Beleidseditor bucket](./media/connect-aws-account/bucket-policy-editor.png)


### <a name="enable-aws-billing-reports"></a>AWS facturering rapporten inschakelen

Nadat u maken en configureren van de S3-bucket, gaat u naar [voorkeuren facturering](https://console.aws.amazon.com/billing/home?#/preference) in de AWS-console.

1. Selecteer op de pagina Voorkeuren **facturering rapporten ontvangen**.
2. Onder **facturering rapporten ontvangen**, voer de naam van de bucket die u hebt gemaakt en klik vervolgens op **controleren**.  
3. Selecteer alle vier rapporteren granulatie opties en klik op **opslaan van voorkeuren**.  
    ![Rapporten inschakelen](./media/connect-aws-account/enable-reports.png)

Beheer van kosten gedetailleerde informatie over facturering opgehaald uit uw S3-bucket en rapporten gevuld nadat gedetailleerde facturering is ingeschakeld. Het kan tot 24 uur totdat gedetailleerde factureringsgegevens wordt weergegeven in de console Cloudyn duren. Wanneer gedetailleerde factureringsgegevens beschikbaar is, wordt de status van uw account consolidatie weergegeven als **geconsolideerde**. Status van de account wordt weergegeven als **voltooid**.

![Account geconsolideerd Status](./media/connect-aws-account/consolidated-status.png)

Enkele de optimalisatie rapporten vereisen mogelijk een paar dagen aan gegevens ophalen van de grootte van een steekproef voldoende gegevens voor nauwkeurige aanbevelingen.

## <a name="next-steps"></a>Volgende stappen

- Voor meer informatie over het beheer van Azure kosten, blijven de [gebruik en kosten bekijken](tutorial-review-usage.md) zelfstudie voor het beheer van kosten.
