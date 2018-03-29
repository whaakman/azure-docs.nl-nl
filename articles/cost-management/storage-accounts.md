---
title: Storage-accounts configureren voor Azure kosten Management | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure storage-accounts en AWS opslag buckets voor Azure kosten Management configureren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/27/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 4ddd70501821941120d0d22e273b69842b567532
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="configure-storage-accounts-for-cost-management"></a>Storage-accounts configureren voor het beheer van kosten

<!--- intent: As a Cost Management user, I want to configure Cost Management to use my cloud service provider storage account to store my reports. -->

U kunt kostenbeheer rapporten opslaan in de Cloudyn portal, Azure storage of AWS opslag buckets. Uw rapporten opslaan in de portal Cloudyn is gratis. Echter uw rapporten opslaan in uw cloudserviceprovider opslag is optioneel en extra kosten in rekening worden gebracht. In dit artikel helpt u bij het configureren van Azure storage-accounts en Amazon Web Services (AWS) opslag buckets voor het opslaan van uw rapporten.

## <a name="prerequisites"></a>Vereisten

U moet een Azure storage-account of een Amazon opslag bucket hebben.

Als u geen Azure storage-account hebt, moet u een maken. Zie voor meer informatie over het maken van een Azure storage-account [een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Als u een AWS geen bucket eenvoudige opslagindeling service (S3), moet u een maken. Zie voor meer informatie over het maken van een S3-bucket [maken van een Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Uw Azure storage-account configureren

Configureert u is Azure-opslag voor gebruik door kostenbeheer eenvoudig. Meer informatie over het opslagaccount verzamelen en deze in de portal Cloudyn kopiëren.

1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Klik op **alle Services**, selecteer **opslagaccounts**, blader naar het opslagaccount dat u wilt gebruiken en selecteer vervolgens het account.
3. Op de pagina opslag account onder **instellingen**, klikt u op **toegangstoetsen**.
4. Kopieer uw **opslagaccountnaam** en **verbindingsreeks** onder key1.  
![Toegangssleutels voor Azure-opslag](./media/storage-accounts/azure-storage-access-keys.png)  
5. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com en zich aanmelden.
6. Klik op het symbool tandwiel en selecteer vervolgens **rapporten opslagbeheer**.
7. Klik op **nieuwe toevoegen +** en controleer of de Microsoft Azure is geselecteerd. Plak de naam van uw Azure-opslag in de **naam** gebied. Plak uw **verbindingsreeks** in het desbetreffende gebied. Voer een containernaam en klik vervolgens op **opslaan**.  
![Cloudyn opslag geconfigureerd voor Azure](./media/storage-accounts/azure-cloudyn-storage.png)

  Uw nieuwe Azure rapport opslag vermelding verschijnt in de lijst met opslagaccounts.  
    ![Nieuwe Azure rapportopslag in de lijst](./media/storage-accounts/azure-storage-entry.png)


U kunt nu rapporten opslaan naar Azure-opslag. Klik in elk rapport op **acties** en selecteer vervolgens **rapport plant**. Het rapport een naam en vervolgens de URL van uw eigen toevoegen of gebruikt u de automatisch gemaakte URL. Selecteer **opslaan naar opslag** en selecteer vervolgens het opslagaccount. Voer een voorvoegsel dat wordt toegevoegd aan de naam van het rapport. Selecteer CSV- of JSON-indeling en sla het rapport.

## <a name="configure-an-aws-storage-bucket"></a>Een bucket AWS-opslag configureren

De Cloudyn bestaande AWS-referenties worden gebruikt: gebruiker of rol, de rapporten in uw bucket opslaan. Wilt u de toegang, Cloudyn probeert een klein tekstbestand opslaan in de bucket met de bestandsnaam _selectievakje-bucket-permission.txt_.

U opgeven de Cloudyn rol of de gebruiker met de machtiging PutObject naar uw bucket. Vervolgens gebruikt u een bestaande bucket of een nieuwe maken voor het opslaan van rapporten. Ten slotte bepalen hoe u de opslagklasse beheren, lifecycle regels instellen of verwijderen van overbodige bestanden.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>Wijs machtigingen toe aan uw AWS-gebruiker of rol

Wanneer u een nieuw beleid maakt, kunt u de exacte machtigingen om een rapport opslaan in een S3-bucket opgeven.

1. Aanmelden bij de AWS-console en selecteer **Services**.
2. Selecteer **IAM** uit de lijst met services.
3. Selecteer **beleid** aan de linkerkant van de console en klik vervolgens op **beleid maken**.
4. Klik op de **JSON** tabblad.
5. Het volgende beleid kunt u een rapport opslaan in een S3-bucket. Kopieer en plak het volgende voorbeeld van het beleid om de **JSON** tabblad. Vervang &lt;bucketname&gt; met de Bucketnaam van uw.

  ```
{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid":  "CloudynSaveReport2S3",
        "Effect":      "Allow",
        "Action": [
          "s3:PutObject"
        ],
        "Resource": [
          "arn:aws:s3:::<bucketname>/*"
        ]
      }
    ]
}
```

6. Klik op **beleid bekijken**.  
    ![Beleid voor het bekijken](./media/storage-accounts/aws-policy.png)  
7. Typ een naam voor het beleid op de pagina van het beleid controleren. Bijvoorbeeld: _CloudynSaveReport2S3_.
8. Klik op **beleid maken**.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>Het beleid te koppelen aan een Cloudyn functie of de gebruiker in uw account

Als u wilt koppelen van het nieuwe beleid, de AWS-console openen en bewerken van het Cloudyn rol of de gebruiker.

1. Aanmelden bij de AWS-console en selecteer **Services**, selecteer daarna **IAM** uit de lijst met services.
2. Selecteer een **rollen** of **gebruikers** vanaf de linkerkant van de console.

**Voor rollen:**

  1. Klik op de naam van uw Cloudyn-rol.
  2. Op de **machtigingen** tabblad **beleid koppelen**.
  3. Zoeken naar het beleid dat u hebt gemaakt, selecteert u deze en klikt u op **beleid koppelen**.
    ![AWS - beleid voor een rol koppelen](./media/storage-accounts/aws-attach-policy-role.png)

**Voor gebruikers:**

1. Selecteer de gebruiker Cloudyn.
2. Op de **machtigingen** tabblad **machtigingen toevoegen**.
3. In de **machtiging verlenen** sectie **bestaande beleidsregels rechtstreeks koppelen**.
4. Zoeken naar het beleid dat u hebt gemaakt, selecteert u deze en klikt u op **volgende: Bekijk**.
5. Klik op de machtigingen toevoegen aan de pagina met de naam van de functie **machtigingen toevoegen**.  
    ![AWS - beleid voor een gebruiker koppelen](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>Optioneel: Machtiging met bucket beleid instellen

U kunt ook de machtiging om rapporten te maken op uw S3-bucket met behulp van een beleid bucket instellen. In de klassieke weergave S3:

1. Maak of Selecteer een bestaande bucket.
2. Selecteer de **machtigingen** tabblad en klik vervolgens op **Bucket beleid**.
3. Kopieer en plak het volgende voorbeeld van beleid. Vervang &lt;bucket\_naam&gt; en &lt;Cloudyn\_principe&gt; met de informatie van uw bucket. De informatie van de functie of de gebruiker die wordt gebruikt door Cloudyn vervangen.

  ```
{
  "Id": "Policy1485775646248",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "SaveReport2S3",
      "Action": [
        "s3:PutObject"
      ],
      "Effect": "Allow",
      "Resource": "<bucket_name>/*",
      "Principal": {
        "AWS": [
          "<Cloudyn_principle>"
        ]
      }
    }
  ]
}
```

4. Klik in de beleidseditor Bucket **opslaan**.

### <a name="add-aws-report-storage-to-cloudyn"></a>AWS rapportopslag toevoegen aan Cloudyn

1. De portal Cloudyn openen vanuit de Azure-portal of Ga naar https://azure.cloudyn.com en zich aanmelden.
2. Klik op het symbool tandwiel en selecteer vervolgens **rapporten opslagbeheer**.
3. Klik op **nieuwe toevoegen +** en controleer of de AWS is geselecteerd.
4. Selecteer een bucket-account en opslag. De naam van de bucket AWS-opslag is automatisch ingevuld.  
    ![Rapportopslag voor de bucket AWS toevoegen](./media/storage-accounts/aws-cloudyn-storage.png)  
5. Klik op **opslaan** en klik vervolgens op **Ok**.

    Uw nieuwe AWS rapport opslag item weergegeven in de lijst met opslagaccounts.  
    ![Nieuwe AWS rapportopslag in lijst](./media/storage-accounts/aws-storage-entry.png)


U kunt nu rapporten opslaan naar Azure-opslag. Klik in elk rapport op **acties** en selecteer vervolgens **rapport plant**. Het rapport een naam en vervolgens de URL van uw eigen toevoegen of gebruikt u de automatisch gemaakte URL. Selecteer **opslaan naar opslag** en selecteer vervolgens het opslagaccount. Voer een voorvoegsel dat wordt toegevoegd aan de naam van het rapport. Selecteer CSV- of JSON-indeling en sla het rapport.

## <a name="next-steps"></a>Volgende stappen

- Bekijk [Understanding kosten verbonden aan rapporten voor](understanding-cost-reports.md) voor meer informatie over de basisstructuur en functies van de kosten-rapporten.
