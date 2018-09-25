---
title: Storage-accounts configureren voor Cloudyn in Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure storage-accounts en AWS opslag buckets voor Cloudyn configureren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: cost-management
manager: carmonm
ms.custom: ''
ms.openlocfilehash: 829995d40a73dc181a28a467e4d16fef0bdeb454
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990651"
---
# <a name="configure-storage-accounts-for-cloudyn"></a>Storage-accounts voor Cloudyn configureren

<!--- intent: As a Cloudyn user, I want to configure Cloudyn to use my cloud service provider storage account to store my reports. -->

U kunt de Cloudyn-rapporten opslaan in de Cloudyn-portal, Azure storage of buckets voor AWS-opslag. Uw rapporten opslaan in de Cloudyn-portal is gratis. Echter, uw rapporten opslaan in uw cloudserviceprovider opslag is optioneel en worden extra kosten in rekening gebracht. Dit artikel helpt u bij het configureren van Azure storage-accounts en Amazon Web Services (AWS) opslag buckets voor het opslaan van uw rapporten.

## <a name="prerequisites"></a>Vereisten

U moet een Azure storage-account of een bucket van Amazon opslag hebben.

Als u geen Azure storage-account hebt, moet u er een maken. Zie voor meer informatie over het maken van een Azure storage-account [een opslagaccount maken](../storage/common/storage-quickstart-create-account.md).

Als u een AWS geen bucket simple storage-service (S3), moet u er een maken. Zie voor meer informatie over het maken van een S3-bucket [maken van een Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

## <a name="configure-your-azure-storage-account"></a>Uw Azure storage-account configureren

Configureert u is Azure storage voor gebruik door Cloudyn vrij eenvoudig. Verzamelen van gegevens over de storage-account en kopieer deze in de Cloudyn-portal.

1. Meld u aan bij Azure Portal op http://portal.azure.com.
2. Klik op **alle Services**, selecteer **opslagaccounts**, blader naar het opslagaccount dat u wilt gebruiken, en selecteer vervolgens het account.
3. Op de pagina van uw storage-account onder **instellingen**, klikt u op **toegangssleutels**.
4. Kopieer uw **opslagaccountnaam** en **Connection string** onder key1.  
![Toegangssleutels voor Azure-opslag](./media/storage-accounts/azure-storage-access-keys.png)  
5. Open de Cloudyn-portal vanuit Azure Portal of ga naar https://azure.cloudyn.com en meld u aan.
6. Klik op het tandwiel-symbool en selecteer vervolgens **rapporten opslagbeheer**.
7. Klik op **nieuwe toevoegen +** en ervoor te zorgen dat Microsoft Azure is geselecteerd. Plak de naam van uw Azure storage-account in de **naam** gebied. Plak uw **verbindingsreeks** in het desbetreffende gebied. Voer een containernaam in en klik vervolgens op **opslaan**.  
![Cloudyn opslag geconfigureerd voor Microsoft Azure](./media/storage-accounts/azure-cloudyn-storage.png)

  Uw nieuwe rapport van Azure storage-vermelding wordt weergegeven in de lijst met storage-account.  
    ![Nieuwe Azure-rapportopslag in lijst](./media/storage-accounts/azure-storage-entry.png)


U kunt nu rapporten opslaan naar Azure storage. Klik in een rapport op **acties** en selecteer vervolgens **rapport plant**. Het rapport een naam en voeg uw eigen URL of de automatisch gemaakte URL gebruiken. Selecteer **opslaan in storage** en selecteer vervolgens het opslagaccount. Voer een voorvoegsel dat wordt toegevoegd aan de naam van het rapport. Selecteer CSV of JSON-indeling en sla het rapport.

## <a name="configure-an-aws-storage-bucket"></a>Een bucket van AWS-opslag configureren

De Cloudyn maakt gebruik van bestaande AWS-referenties: gebruiker of rol, de rapporten opslaan naar de bucket. Als u wilt testen van de toegang, Cloudyn probeert op te slaan een klein tekstbestand dat aan de bucket met de naam van het _controle-bucket-permission.txt_.

U opgeven de Cloudyn-rol of de gebruiker met de machtiging PutObject aan de bucket. Vervolgens gebruikt u een bestaande bucket of een nieuwe maken om op te slaan van rapporten. Ten slotte bepalen hoe u de opslagklasse beheren, levenscyclus regels instellen of verwijder onnodige bestanden.

###  <a name="assign-permissions-to-your-aws-user-or-role"></a>Machtigingen toewijzen aan uw AWS-gebruiker of rol

Wanneer u een nieuw beleid maakt, kunt u de exacte machtigingen die nodig zijn voor een rapport opslaan in een S3-bucket opgeven.

1. Meld u aan bij de AWS-console en selecteer **Services**.
2. Selecteer **IAM** uit de lijst met services.
3. Selecteer **beleid** aan de linkerkant van de console en klik vervolgens op **beleid maken**.
4. Klik op de **JSON** tabblad.
5. Het volgende beleid kunt u een rapport opslaan in een S3-bucket. Kopieer en plak het volgende voorbeeld van beleid om de **JSON** tabblad. Vervang &lt;bucketname&gt; met de Bucketnaam van uw.

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

6. Klik op **controlebeleid**.  
    ![Controlebeleid](./media/storage-accounts/aws-policy.png)  
7. Typ een naam voor het beleid op de pagina van het beleid controleren. Bijvoorbeeld, _CloudynSaveReport2S3_.
8. Klik op **beleid maken**.

### <a name="attach-the-policy-to-a-cloudyn-role-or-user-in-your-account"></a>Het beleid te koppelen aan een Cloudyn-functie of een gebruiker in uw account

Als u wilt koppelen het nieuwe beleid, de AWS-console openen en bewerken van de Cloudyn-rol of de gebruiker.

1. Meld u aan bij de AWS-console en selecteer **Services**en selecteer vervolgens **IAM** uit de lijst met services.
2. Selecteer een **rollen** of **gebruikers** vanaf de linkerkant van de console.

**Voor rollen:**

  1. Klik op de naam van uw Cloudyn-rol.
  2. Op de **machtigingen** tabblad **beleid koppelen**.
  3. Zoeken naar het beleid dat u hebt gemaakt, selecteert u deze en klik op **beleid koppelen**.
    ![AWS - beleid voor een rol koppelen](./media/storage-accounts/aws-attach-policy-role.png)

**Voor gebruikers:**

1. Selecteer de Cloudyn-gebruiker.
2. Op de **machtigingen** tabblad **machtigingen toevoegen**.
3. In de **machtiging verlenen** sectie, selecteer **bestaande beleidsregels rechtstreeks koppelen**.
4. Zoeken naar het beleid dat u hebt gemaakt, selecteert u deze en klik op **volgende: Controleer**.
5. Klik op de machtigingen toevoegen aan de pagina voor de naam van de rol, **machtigingen toevoegen**.  
    ![AWS - beleid voor een gebruiker koppelen](./media/storage-accounts/aws-attach-policy-user.png)


### <a name="optional-set-permission-with-bucket-policy"></a>Optioneel: Machtiging met de bucket beleid instellen

U kunt ook de machtiging voor het maken van rapporten op uw S3-bucket op basis van beleid bucket instellen. In de klassieke S3-weergave:

1. Maak of Selecteer een bestaande bucket.
2. Selecteer de **machtigingen** tabblad en klik vervolgens op **Bucket beleid**.
3. Kopieer en plak het volgende voorbeeld van beleid. Vervang &lt;bucket\_naam&gt; en &lt;Cloudyn\_principe&gt; met de informatie van uw bucket. Vervang de informatie van de rol of de gebruiker die door Cloudyn worden gebruikt.

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

### <a name="add-aws-report-storage-to-cloudyn"></a>Archiefopslag van AWS-rapport toevoegen aan Cloudyn

1. Open de Cloudyn-portal vanuit Azure Portal of ga naar https://azure.cloudyn.com en meld u aan.
2. Klik op het tandwiel-symbool en selecteer vervolgens **rapporten opslagbeheer**.
3. Klik op **nieuwe toevoegen +** en zorg ervoor dat de AWS is geselecteerd.
4. Selecteer een bucket-account en de opslag. De naam van de bucket van AWS-opslag wordt automatisch ingevuld.  
    ![Rapportopslag voor AWS-bucket toevoegen](./media/storage-accounts/aws-cloudyn-storage.png)  
5. Klik op **opslaan** en klik vervolgens op **Ok**.

    Uw nieuwe AWS rapport opslag vermelding wordt weergegeven in de lijst met storage-account.  
    ![Nieuw rapport-archiefopslag van AWS in lijst](./media/storage-accounts/aws-storage-entry.png)


U kunt nu rapporten opslaan naar Azure storage. Klik in een rapport op **acties** en selecteer vervolgens **rapport plant**. Het rapport een naam en voeg uw eigen URL of de automatisch gemaakte URL gebruiken. Selecteer **opslaan in storage** en selecteer vervolgens het opslagaccount. Voer een voorvoegsel dat wordt toegevoegd aan de naam van het rapport. Selecteer CSV of JSON-indeling en sla het rapport.

## <a name="next-steps"></a>Volgende stappen

- Beoordeling [inzicht in Cloudyn-rapporten](understanding-cost-reports.md) voor meer informatie over de basisstructuur en functies van Cloudyn-rapporten.
