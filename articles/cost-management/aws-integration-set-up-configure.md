---
title: Instellen en configureren van AWS-kosten en gebruik rapport-integratie met Azure Cost Management
description: Dit artikel helpt u bij het instellen en configureren van AWS-kosten en gebruik rapport-integratie met Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: ormaoz
ms.custom: ''
ms.openlocfilehash: 951178a82e0975f5f2af71bd48cf0f931246ae37
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002130"
---
# <a name="set-up-and-configure-aws-cost-and-usage-report-integration"></a>Instellen en configureren van AWS-kosten en gebruik rapport-integratie

Met Amazon Web Services (AWS) rapport (huidig)-integratie, kosten en gebruik, die u kunt controleren en beheren van uw AWS-uitgaven in Azure Cost Management. Dankzij de integratie kunt één locatie in de Azure-portal waar u kunt controleren en de controle voor de uitgavelimiet voor zowel Azure als AWS. In dit artikel wordt uitgelegd hoe u de integratie instellen en configureren zodat u functies van Azure Cost Management gebruiken om kosten te analyseren en controleren van budgetten.

Kosten processen voor het AWS-kosten en gebruik rapport opgeslagen in een S3-bucket met behulp van uw AWS-referenties voor toegang te krijgen rapportdefinities en GZIP CSV-bestanden te downloaden.

## <a name="create-a-cost-and-usage-report-in-aws"></a>Maken van een rapport kosten en gebruik in AWS

Met behulp van een rapport kosten en gebruik, is de AWS-aanbevolen manier om te verzamelen en verwerken van de kosten voor AWS. Zie voor meer informatie de [AWS kosten en gebruiksrapport](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-reports-costusage.html) documentatie.

Gebruik de **kosten & gebruiksrapporten** pagina van de facturering en Cost Management console in AWS een rapport kosten en gebruik te maken met de volgende stappen uit:

1. Aanmelden bij de AWS-beheerconsole en open de [facturering en Cost Management console](https://console.aws.amazon.com/billing).
2. Selecteer in het navigatiedeelvenster **kosten & gebruiksrapporten**.
3. Selecteer **rapport maken**.
4. Voor **rapportnaam**, voer een naam in voor uw rapport.
5. Voor **extra rapportdetails** , om op te nemen van de id's van elke resource in het rapport en selecteer **Resource-id's bevatten**.
6. Voor **instellingen voor gegevensvernieuwing**, selecteert u of u wilt dat de kosten voor AWS en gebruik rapport vernieuwen als AWS restituties, tegoeden, toepassing of ondersteuning voor kosten op uw account na het voltooien van uw factuur. Wanneer een rapport wordt vernieuwd, wordt een nieuw rapport geüpload naar Amazon S3. Het verdient laat u deze instelling op.
7. Selecteer **Next**.
8. Voor **S3-bucket**, kiest u **configureren**.
9. Doe het volgende in het dialoogvenster S3-Bucket configureren:
    1. Selecteer een bestaande bucket in de vervolgkeuzelijst en kies **volgende**.
    2. Voer een Bucketnaam en de regio waar u wilt maken van een nieuwe bucket en kies **volgende**.
10. Selecteer die ik hebt vastgesteld dat dit beleid is gecorrigeerd en kiest u opslaan.
11. (Optioneel) Voer het rapport pad-voorvoegsel dat u wilt dat prepended op de naam van uw rapport voor rapport padvoorvoegsel.
Als u een voorvoegsel niet opgeeft, is het standaardvoorvoegsel de naam die u hebt opgegeven voor het rapport in stap 4 en het datumbereik voor het rapport in de volgende indeling: `/report-name/date-range/`
12. Voor **tijdseenheid**, kiest u **per uur**.
13. Voor **rapport versiebeheer**, kies of u elke versie van het rapport in de vorige versie van het rapport te overschrijven of naast de vorige versies worden geleverd.
14. Voor **inschakelen data-integratie voor**, er is geen selectie is vereist.
15. Voor **compressie**, selecteer **GZIP**.
16. Selecteer **Next**.
17. Nadat u deze de instellingen voor uw rapport hebt bekeken, selecteer **controleren en voltooien**.

    Noteer de rapportnaam. U gebruikt deze in latere stappen.

Het kan tot 24 uur voor AWS om te beginnen met het leveren van rapporten aan uw Amazon S3-bucket duren. Na levering wordt gestart, werkt AWS de AWS-kosten en gebruik rapportbestanden ten minste één keer per dag. U kunt doorgaan met het configureren van uw AWS-omgeving zonder te wachten op voor de levering om te starten.

## <a name="create-a-role-and-policy-in-aws"></a>Maken van een rol en het beleid in AWS

Azure Cost Management heeft toegang tot de S3-bucket waar de kosten en gebruik rapport zich meerdere keren per dag bevindt. De service moet toegang hebben tot de referenties te controleren of er nieuwe gegevens. U maakt een rol en het beleid in AWS om toe te staan van Cost Management om deze te openen.

Als u wilt inschakelen op rollen gebaseerde toegang tot een AWS-account in Cost Management, is de rol gemaakt in de AWS-console. U moet beschikken over de _rol informatie_ en _externe ID_ vanuit de AWS-console. Later, u kunt het gebruiken voor de **een AWS-connector maken** pagina in Cost Management.

Gebruik de wizard nieuwe rol maken:

1. Aanmelden bij uw AWS-console en selecteer **Services**.
2. Selecteer in de lijst met services, **IAM**.
3. Selecteer **rollen** en selecteer vervolgens **rol maken**.
4. Selecteer op de volgende pagina **een andere AWS-account**.
5. In **Account-ID**, voer **432263259397**.
6. In **opties**, selecteer **externe ID (Best practice wanneer een derde partij wordt deze rol vervullen) vereisen**.
7. In **externe ID**, geef de externe ID. De externe ID is een gedeelde code tussen de AWS-rol en Azure Cost Management. Dezelfde externe ID wordt ook gebruikt voor de **nieuwe Connector** pagina in Cost Management. Bijvoorbeeld, een externe ID lijkt op _Companyname1234567890123_.

    > [!NOTE]
    > De selectie voor niet te wijzigen **MFA vereisen**. Het apparaat moet blijven uitgeschakeld.
8. Selecteer **Volgende: Permissions**.
9. Selecteer **beleid maken**. Er wordt een nieuw browsertabblad geopend. Dat is waar u een beleid maken.
10. Selecteer **kiest u een service**.

Machtiging voor het rapport kosten en gebruik configureren:

1. Voer **kosten en gebruiksrapport**.
2. Selecteer **toegangsniveau** > **lezen** > **DescribeReportDefinitions**. In deze stap kunt Cost Management om te lezen welke huidige rapporten zijn gedefinieerd en bepalen als ze overeenkomen met de vereiste van de definitie van rapport.
3. Selecteer **aanvullende machtigingen**.

Machtiging voor de S3-bucket en objecten configureren:

1. Selecteer **kiest u een service**.
2. Voer **S3**.
3. Selecteer **toegangsniveau** > **lijst** > **ListBucket**. Deze actie wordt een lijst van objecten in de S3-Bucket opgehaald.
4. Selecteer **toegangsniveau** > **lezen** > **GetObject**. Met deze actie kunt het downloaden van bestanden voor facturering.
5. Selecteer **Resources**.
6. Selecteer **bucket – informatie toevoegen**.
7. In **Bucketnaam**, voer de bucket gebruikt voor het opslaan van de huidige bestanden.
8. Selecteer **object – informatie toevoegen**.
9. In **Bucketnaam**, voer de bucket gebruikt voor het opslaan van de huidige bestanden.
10. In **objectnaam**, selecteer **eventuele**.
11. Selecteer **aanvullende machtigingen**.

Machtiging voor kosten Explorer configureren:

1. Selecteer **kiest u een service**.
2. Voer **kosten Explorer-Service**.
3. Selecteer **acties voor alle kosten Explorer-Service (ce:\*)** . Deze actie wordt gecontroleerd of de verzameling juist is.
4. Selecteer **aanvullende machtigingen**.

Machtiging voor AWS-organisaties toevoegen:

1. Voer **organisaties**.
2. Selecteer **toegangsniveau** > **lijst** > **ListAccounts**. Deze actie worden de namen van de accounts opgehaald.
3. In **Review Policy**, voer een naam in voor het nieuwe beleid. Controleer of u de juiste gegevens hebt ingevoerd, en selecteer vervolgens **beleid maken**.
4. Ga terug naar het vorige tabblad en vernieuw de webpagina van uw browser. Op de zoekbalk typt, zoekt u het nieuwe beleid.
5. Selecteer **Next: revisie**.
6. Voer een naam voor de nieuwe rol. Controleer of u de juiste gegevens hebt ingevoerd, en selecteer vervolgens **rol maken**.

    Houd er rekening mee de rol van informatie en de externe ID gebruikt in de voorgaande stappen bij het maken van de rol. U hebt ze later gebruiken bij het instellen van de Azure Cost Management-connector.

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

Gebruik de volgende informatie om te maken van een AWS-connector en beginnen met controleren van uw AWS-kosten:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga naar **kosten Management en facturering** > **kostenbeheer**.
3. Onder **instellingen**, selecteer **connectors (Preview) in de Cloud**.  
    ![Voorbeeld van de Cloud-connectors (Preview) instelling)](./media/aws-integration-setup-configure/cloud-connectors-preview01.png).
4. Selecteer **+ toevoegen** aan de bovenkant van de pagina om een connector te maken.
5. Op de **een AWS-connector maken** pagina **weergavenaam**, voer een naam in voor uw connector.  
    ![Voorbeeld van de pagina voor het maken van een AWS-connector](./media/aws-integration-setup-configure/create-aws-connector01.png)
6. (Optioneel) Selecteer de standaard-beheergroep. Alle gedetecteerde gekoppelde accounts worden opgeslagen. U kunt deze later instellen.
7. In de **facturering** sectie, selecteer **automatisch kosten in rekening gebracht van de % 1 voor algemene beschikbaarheid** als u zorgen voor doorlopende werking wilt wanneer de Preview-versie is verlopen. Als u de automatische optie selecteert, moet u een abonnement voor facturering.
8. Voor **rol informatie**, voer de waarde die u hebt gebruikt bij het instellen van de rol in AWS.
9. Voor **externe ID**, voer de waarde die u hebt gebruikt bij het instellen van de rol in AWS.
10. Voor **rapportnaam**, voer de naam die u hebt gemaakt in AWS.
11. Selecteer **volgende** en selecteer vervolgens **maken**.

Het duurt een paar uur voor de nieuwe scopes met AWS, de AWS geconsolideerde-account en gekoppelde AWS-accounts en hun kostengegevens worden weergegeven.

Nadat u de connector hebt gemaakt, wordt u aangeraden dat u toegangsbeheer aan toewijzen. Gebruikers zijn machtigingen toegewezen aan de nieuwe gedetecteerde bereiken: AWS geconsolideerde-account en gekoppelde AWS-accounts. De gebruiker die de connector maakt, is de eigenaar van de connector, het samengevoegde account en alle gekoppelde accounts.

Connector-machtigingen toewijzen aan gebruikers nadat er een detectie wordt uitgevoerd, geen machtigingen toewijzen aan de bestaande AWS-bereiken. In plaats daarvan zijn gekoppelde accounts alleen nieuwe machtigingen toegewezen.

## <a name="take-additional-steps"></a>Neem bijkomende stappen

- [Instellen van beheergroepen](../governance/management-groups/index.md#initial-setup-of-management-groups), als u dat nog niet gedaan hebt.
- Controleer dat nieuwe scopes worden toegevoegd aan de bereikskiezer. Selecteer **vernieuwen** om de meest recente gegevens weer te geven.
- Op de **Cloud connectors** pagina, selecteert u de connector en selecteer **gaat u naar factureringsaccount** het gekoppelde account toewijzen aan beheergroepen.

## <a name="manage-cloud-connectors"></a>Beheren van cloud-connectors

Wanneer u een connector selecteert op de **Cloud connectors** pagina die u kunt:

- Selecteer **gaat u naar factureringsaccount** geconsolideerd account om gegevens voor de AWS weer te geven.
- Selecteer **Access Control** voor het beheren van de roltoewijzing voor de connector.
- Selecteer **bewerken** om bij te werken van de connector. U kunt het aantal van AWS-account, niet wijzigen omdat deze wordt weergegeven in de rol van informatie. Maar u kunt een nieuwe connector maken.
- Selecteer **controleren** de verificatietest om ervoor te zorgen dat Cost Management van gegevens verzamelen kunt met behulp van de connectorinstellingen opnieuw uit te voeren.

![Voorbeeld van de lijst met gemaakte AWS-connectors](./media/aws-integration-setup-configure/list-aws-connectors.png)

## <a name="set-up-azure-management-groups"></a>Instellen van Azure-beheergroepen

Voor het maken van één plek om cross-cloudprovider informatie weer te geven, moet u uw Azure-abonnementen en gekoppelde AWS-accounts in dezelfde beheergroep plaatsen. Als u dit nog niet hebt al uw Azure-omgeving is geconfigureerd met beheergroepen, [voor de eerste installatie van beheergroepen](../governance/management-groups/index.md#initial-setup-of-management-groups).

Als u wilt de afzonderlijke kosten, kunt u een beheergroep waarin de zojuist gekoppelde AWS-accounts maken.

## <a name="set-up-an-aws-consolidated-account"></a>Stel een AWS geconsolideerd account

Het account AWS geconsolideerd combineert facturering en betaling voor meerdere AWS-accounts. Het fungeert ook als een gekoppelde AWS-account.

![Voorbeeld van de details voor een AWS geconsolideerd account](./media/aws-integration-setup-configure/aws-consolidated-account01.png)

Op de pagina kunt u het volgende doen:

- Selecteer **bijwerken** bulksgewijs bijwerken gekoppeld de koppeling van AWS-accounts met een beheergroep.
- Selecteer **Access Control** om in te stellen van de roltoewijzing voor de scope.

### <a name="permissions-for-an-aws-consolidated-account"></a>Machtigingen voor een AWS geconsolideerd account

Machtigingen voor een geconsolideerd AWS-account zijn standaard ingesteld bij het maken van het account op basis van de machtigingen van AWS-connector. De maker van de connector is de eigenaar.

U het toegangsniveau van de beheren met behulp van de **toegangsniveau** pagina van de AWS geconsolideerd account. Echter, AWS gekoppelde accounts machtigingen aan de geconsolideerde AWS-account niet overnemen.

## <a name="set-up-an-aws-linked-account"></a>Een gekoppelde AWS-account instellen

De gekoppelde AWS-account is waar de AWS-resources worden gemaakt en beheerd. Een gekoppeld account fungeert ook als een beveiligingsgrens.

Op deze pagina kunt u het volgende doen:

- Selecteer **bijwerken** gekoppeld aan het bijwerken van de koppeling van een AWS-account met een beheergroep.
- Selecteer **Access Control** om in te stellen van een roltoewijzing voor de scope.

![Voorbeeld van de gekoppelde AWS-Account-pagina](./media/aws-integration-setup-configure/aws-linked-account01.png)

### <a name="permissions-for-an-aws-linked-account"></a>Machtigingen voor een AWS-account gekoppeld

Machtigingen voor een gekoppelde AWS-account zijn standaard ingesteld nadat deze is gemaakt, op basis van de machtigingen van AWS-connector. De maker van de connector is de eigenaar. U het toegangsniveau van de beheren met behulp van de **toegangsniveau** pagina van de AWS-account gekoppeld. AWS gekoppelde accounts niet machtigingen overnemen van een geconsolideerd AWS-account.

AWS gekoppelde accounts altijd machtigingen overnemen van de beheergroep waarvan ze lid.

## <a name="next-steps"></a>Volgende stappen

- Nu dat u hebt ingesteld en integratie van AWS-kosten en gebruik rapport geconfigureerd, gaat u verder met [AWS beheren van kosten en het verbruik](aws-integration-manage.md).
- Als u niet bekend met kostenanalyse bent, Zie [verkennen en analyseren van kosten met kostenanalyse](quick-acm-cost-analysis.md) Quick Start.
- Als u niet bekend met budgetten in Azure bent, Zie [maken en beheren van Azure budgetten](tutorial-acm-create-budgets.md).
