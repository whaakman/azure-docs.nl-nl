---
title: Instellen en configureren van AWS-kosten en gebruik rapport-integratie met Azure Cost Management
description: Dit artikel helpt u al instellen en configureren van AWS-kosten en gebruik rapport-integratie met Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/06/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: a7a020284f44eda0da62f307866c74b0a8df493d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205706"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Instellen en configureren van AWS-kosten en gebruik rapport-integratie

U kunt met Amazon Web Services-kosten en gebruik rapport-integratie, controleren en beheren van uw AWS-uitgaven in Azure Cost Management. Dankzij de integratie kunt één locatie in de Azure-portal waar u kunt controleren en de controle voor de uitgavelimiet voor zowel Azure als AWS. In dit artikel wordt uitgelegd hoe u de integratie instellen en configureren zodat u functies van Cost Management gebruiken om kosten te analyseren en controleren van budgetten.

Kosten processen voor het AWS-kosten en gebruik rapport opgeslagen in een S3-bucket met behulp van uw AWS-referenties voor toegang te krijgen rapportdefinities en GZIP CSV-bestanden te downloaden.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Maken van een rapport kosten en gebruik in AWS

Met behulp van een rapport kosten en gebruik is AWS aanbevolen manier om te verzamelen en verwerken van de kosten voor AWS. Zie voor meer informatie de [AWS kosten en gebruiksrapport](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) documentatie-artikel.

Gebruik de **rapporten** pagina van de facturering en Cost Management console in AWS een rapport kosten en gebruik te maken met de volgende stappen uit.

1. Aanmelden bij de AWS-beheerconsole en open de console voor facturering en -kostenbeheer op https://console.aws.amazon.com/billing.
2. Klik in het navigatievenster op **rapporten**.
3. Klik op **rapport maken**.
4. Voor **rapportnaam**, typ een naam voor uw rapport.
5. Voor **tijdseenheid**, kiest u **per uur**.
6. Voor **opnemen**, het toevoegen van de id's van elke resource in het rapport en selecteer **Resource-id's**.
7. Voor **ondersteuning inschakelen voor**, er is geen selectie is vereist.
8. Voor **instellingen voor gegevensvernieuwing**, selecteer **automatisch vernieuwen van uw kosten &amp; gebruiksrapport wanneer kosten worden gedetecteerd voor de vorige maanden met gesloten facturen**.
9. Klik op **volgende**.
10. Voor **Amazon S3-bucket**, typ de naam van de Amazon S3-bucket waar u rapporten die worden geleverd aan en klikt u op **controleren**. De bucket moet de juiste machtigingen om geldig te zijn hebben. Zie voor meer informatie over het toevoegen van machtigingen aan de bucket [instelling Bucket en toegangsmachtigingen Object](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-permissions.html).
11. Voor **rapport padvoorvoegsel**, typt u het rapport pad-voorvoegsel dat u wilt dat moet worden toegevoegd aan de naam van uw rapport.
12. Voor **compressie**, selecteer **GZIP**.
13. Klik op **volgende**.
14. Nadat u de instellingen voor het rapport, klikt u op hebt bekeken **controleren en voltooien**.
    Houd er rekening mee de **rapportnaam**. U gebruikt dit in latere stappen.

Het kan tot 24 uur voor AWS om te beginnen met het leveren van rapporten aan uw Amazon S3-bucket duren. Na levering wordt gestart, werkt AWS de AWS-kosten en gebruik rapportbestanden ten minste één keer per dag. U kunt doorgaan met het configureren van uw AWS-omgeving zonder te wachten op voor de levering om te starten.

## <a name="create-a-role-and-policy-in-aws"></a>Maken van een rol en het beleid in AWS

Azure Cost Management heeft toegang tot de S3-bucket waar de kosten en gebruik rapport zich meerdere keren per dag bevindt. Kostenbeheer moet toegang hebben tot de referenties te controleren of er nieuwe gegevens. U maakt een rol en het beleid in AWS waarmee toegang op basis van Cost Management.

Als u wilt inschakelen op rollen gebaseerde toegang tot een AWS-account in Azure Cost Management, is de rol gemaakt in de AWS-console. U moet beschikken over de _rol informatie_ en _externe ID_ vanuit de AWS-console. Later kunt u ze in de maken een AWS-connector-pagina in Azure Cost Management.

Gebruik de wizard nieuwe rol maken:

1. Aanmelden bij uw AWS-console en selecteer **Services**.
2. Selecteer in de lijst met services, **IAM**.
3. Selecteer **rollen** en klik vervolgens op **rol maken**.
4. Selecteer in de volgende pagina **een andere AWS-account**.
5. In **Account-ID** , voer _432263259397_.
6. In **opties**, selecteer **externe ID (Best practice wanneer een derde partij wordt deze rol vervullen) vereisen**.
7. In **externe ID**, geef de externe ID. De externe ID is een gedeelde code tussen de AWS-rol en Azure Cost Management. Dezelfde externe ID wordt ook gebruikt op de pagina nieuwe verbindingslijn in Cost Management. Bijvoorbeeld, een externe ID lijkt op _Companyname1234567890123_.
    Wijzig de selectie voor **MFA vereisen**. Het apparaat moet blijven uitgeschakeld.
8. Klik op **Next: Permissions**.
9. Klik op **beleid maken**. Een nieuw browsertabblad geopend waarin u een nieuw beleid maken.
10. Klik op **kiest u een service**.

Kosten en gebruiksrapport machtiging configureren:

1. Type **kosten en gebruiksrapport**.
2. Selecteer **toegangsniveau**, **lezen** > **DescribeReportDefinitions**. Hiermee wordt dat Cost Management lezen huidige rapporten zijn gedefinieerd en bepalen als ze overeenkomen met de vereiste van de definitie van rapport.
3. Klik op **aanvullende machtigingen**.

Configureer uw S3-bucket en objecten-machtiging:

1. Klik op **kiest u een service**.
2. Type _S3_.
3. Selecteer **toegangsniveau**, **lijst** > **ListBucket**. Deze actie wordt een lijst van objecten in de S3-Bucket opgehaald.
4. Selecteer **toegangsniveau**, **lezen** > **GetObject**. Met deze actie kunt downloaden van bestanden facturering.
5. Selecteer **Resources**.
6. Selecteer **bucket – informatie toevoegen**.
7. In **Bucketnaam**, voer de bucket gebruikt voor het opslaan van de huidige bestanden.
8. Selecteer **object – informatie toevoegen**.
9. In **Bucketnaam**, voer de bucket gebruikt voor het opslaan van de huidige bestanden.
10. In **objectnaam**, selecteer **eventuele**.
11. Klik op **aanvullende machtigingen**.

Kosten Explorer machtiging configureren:

1. Klik op **kiest u een service**.
2. Type _kosten Explorer-Service_.
3. Selecteer **acties voor alle kosten Explorer-Service (ce:\*)**. Deze actie wordt gecontroleerd of de verzameling juist is.
4. Klik op **aanvullende machtigingen**.

Organisaties machtiging toevoegen:

1. Type **organisaties**.
2. Selecteer **toegangsniveau, lijst** > **ListAccounts**. Deze actie worden de namen van de accounts opgehaald.
3. In **Review Policy**, voer een naam in voor het nieuwe beleid. Controleer of u de juiste gegevens hebt ingevoerd en klik vervolgens op **beleid maken**.
4. Ga terug naar het vorige tabblad en vernieuw de webpagina van uw browser. In de zoekbalk typt, zoekt u het nieuwe beleid.
5. Selecteer **Next: revisie**.
6. Voer een naam voor de nieuwe rol. Controleer of u de juiste gegevens hebt ingevoerd en klik vervolgens op **rol maken**.
    Houd er rekening mee de **rol informatie** en de **externe ID** gebruikt in de voorgaande stappen bij het maken van de rol. U gebruikt deze later bij het instellen van de Azure Cost Management-connector.

De JSON van het beleid moet lijken op het volgende voorbeeld. Vervang _bucketname_ met de naam van de S3-bucket.

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
"organizations:ListAccounts",
            "ce:*",
            "cur:DescribeReportDefinitions"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::bucketname",
                "arn:aws:s3:::bucketname/*"
            ]
        }
    ]
}
```

## <a name="set-up-a-new-aws-connector-in-azure"></a>Een nieuwe AWS-connector in Azure instellen

Gebruik de volgende informatie om te maken van een nieuwe AWS-connector en beginnen met controleren van de kosten voor AWS.

1. Aanmelden bij Azure portal op https://portal.azure.com.
2. Navigeer naar **kosten Management en facturering** > **kostenbeheer**.
3. Onder **instellingen**, klikt u op **connectors (Preview) in de Cloud**.  
    ![Voorbeeld van de Cloud-connectors (Preview-instelling)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Klik op **+ toevoegen** aan de bovenkant van de pagina om een nieuwe connector te maken.
5. Typ op het maken van een pagina van de connector (AWS), een **weergavenaam** om de naam van de connector.  
    ![Voorbeeld van het maken van een pagina AWS-Connector](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. (Optioneel) Selecteer de standaard-beheergroep. Alle gedetecteerde gekoppelde accounts worden opgeslagen. U kunt deze later instellen.
7. Onder de **facturering** sectie, selecteer **automatisch kosten in rekening gebracht van de % 1 voor algemene beschikbaarheid** als u zorgen voor doorlopende werking wilt wanneer de Preview-versie is verlopen. Als u de automatische optie selecteert, moet u een facturering **abonnement**.
8. Voer de **rol informatie**. Dit is de waarde die u hebt gebruikt bij het instellen van de rol in AWS.
9. Voer de **externe ID**. Dit is de waarde die u hebt gebruikt bij het instellen van de rol in AWS.
10. Voer de **rapportnaam** die u hebt gemaakt in AWS.
11. Klik op **volgende** en klik vervolgens op **maken**.

Het duurt een paar uur voor de nieuwe AWS-bereiken, de geconsolideerde AWS-Account en gekoppelde AWS-Accounts en hun gegevens van cost worden weergegeven.

Nadat de connector is gemaakt, is het raadzaam dat u toegangsbeheer aan de connector toewijzen. Gebruikers zijn machtigingen toegewezen aan de nieuwe gedetecteerde bereiken: AWS-Account en AWS geconsolideerd Accounts gekoppeld. De gebruiker die de connector maakt, is de eigenaar van de connector, geconsolideerde-account en alle gekoppelde accounts.

Connector-machtigingen toewijzen aan gebruikers nadat er een detectie wordt uitgevoerd, geen machtigingen toewijzen aan de bestaande AWS-bereiken. In plaats daarvan zijn gekoppelde accounts alleen nieuwe machtigingen toegewezen.

## <a name="additional-steps"></a>Extra stappen

- [Instellen van beheergroepen](../governance/management-groups/index.md#initial-setup-of-management-groups), als u dat nog niet gedaan hebt.
- Controleer dat nieuwe scopes worden toegevoegd aan de bereikskiezer. Vergeet niet om op **vernieuwen** om de meest recente gegevens weer te geven.
- In de Cloud connector-pagina, selecteer de connector en klik op **gaat u naar factureringsaccount** het gekoppelde account toewijzen aan beheergroepen.

## <a name="manage-cloud-connectors"></a>Beheren van cloud-connectors

Wanneer u een connector op de pagina Cloud connectors selecteert, kunt u het volgende doen:

- Klik op **gaat u naar factureringsaccount** om AWS-Account samengevoegd informatie weer te geven.
- Klik op **Access Control** voor het beheren van de roltoewijzing voor de connector.
- Klik op **bewerken** om bij te werken van de connector. U kunt het aantal AWS-Account, niet wijzigen, zoals deze wordt weergegeven in de informatie van de rol. U kunt echter een nieuwe connector maken.
- Klik op **controleren** de verificatietest om ervoor te zorgen dat Cost Management kan worden gebruikt voor het verzamelen van gegevens met behulp van de connector-instellingen opnieuw uit te voeren.

![Voorbeeld van de lijst met gemaakte AWS-connectors](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="role-of-azure-management-groups"></a>Rol van Azure-beheergroepen

Voor het maken van één plek om cross-cloudprovider informatie weer te geven, moet u uw Azure-abonnementen en gekoppelde AWS-accounts in dezelfde beheergroep plaatsen. Als u dit nog niet hebt al uw Azure-omgeving is geconfigureerd met beheergroepen, [voor de eerste installatie van beheergroepen](../governance/management-groups/index.md#initial-setup-of-management-groups).

Als u wilt de afzonderlijke kosten, kunt u een beheergroep waarin de zojuist gekoppelde AWS-accounts maken.

## <a name="aws-consolidated-account"></a>Consolideren van AWS-account

De geconsolideerde van AWS-account wordt gebruikt om te consolideren facturering en betaling voor meerdere AWS-accounts. Uw account AWS geconsolideerd fungeert ook als een gekoppelde AWS-account.

![Voorbeeld van geconsolideerde AWS-accountgegevens](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Op de pagina kunt u het volgende doen:

- Klik op **bijwerken** bulksgewijs update koppeling met de gekoppelde AWS-accounts met een beheergroep.
- Klik op **Access Control** om in te stellen van de roltoewijzing voor de scope.

### <a name="aws-consolidated-account-permissions"></a>Accountmachtigingen (AWS)-geconsolideerd

Standaard geconsolideerd de AWS account machtigingen zijn ingesteld nadat deze is gemaakt, op basis van de machtigingen van AWS-connector. De maker van de connector is de eigenaar.

U beheren het toegangsniveau van de met behulp van de pagina toegangsniveau van de geconsolideerde AWS-account. Echter machtigingen voor de AWS Account geconsolideerd worden niet overgenomen door de gekoppelde AWS-accounts.

## <a name="aws-linked-account"></a>Gekoppelde AWS-account

De gekoppelde AWS-account is waar de AWS-resources worden gemaakt en beheerd. Een gekoppeld account fungeert ook als een beveiligingsgrens.

Op deze pagina kunt u het volgende doen:

- Klik op **bijwerken** om bij te werken van de gekoppelde AWS-account-koppeling met een beheergroep.
- Klik op **Access Control** om in te stellen van een roltoewijzing voor de scope.

![Voorbeeld van de gekoppelde AWS-Account-pagina](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="aws-linked-account-permissions"></a>AWS gekoppelde accountmachtigingen

Standaard worden de gekoppelde AWS-accountmachtigingen ingesteld nadat deze is gemaakt, op basis van de machtigingen van AWS-connector. De maker van de connector is de eigenaar. U beheren het toegangsniveau van de met behulp van de pagina toegangsniveau van de gekoppelde AWS-account. Machtigingen voor het account AWS geconsolideerd worden niet overgenomen door de gekoppelde AWS-accounts.

Gekoppelde AWS-accounts worden altijd machtigingen overnemen van de beheergroep waartoe ze behoren.

## <a name="next-steps"></a>Volgende stappen

- Nu dat u hebt ingesteld en integratie van AWS-kosten en gebruik rapport geconfigureerd, gaat u verder met [AWS beheren van kosten en het verbruik](aws-integration-manage.md).
- Als u niet bekend met kostenanalyse bent, Zie [verkennen en analyseren van kosten met Cost analysis](quick-acm-cost-analysis.md) Quick Start.
- Als u niet bekend met budgetten in Azure bent, Zie [maken en beheren van Azure budgetten](tutorial-acm-create-budgets.md) zelfstudie.
