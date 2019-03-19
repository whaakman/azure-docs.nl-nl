---
title: Versleuteling in Azure Data Lake Storage Gen1 | Microsoft Docs
description: Versleuteling in Azure Data Lake Storage Gen1 helpt u uw gegevens te beschermen, enterprise-beveiligingsbeleid implementeren en te voldoen aan wettelijke vereisten. In dit artikel vindt u een overzicht van het ontwerp en worden een aantal technische aspecten van de implementatie besproken.
services: data-lake-store
documentationcenter: ''
author: esung22
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: yagupta
ms.openlocfilehash: a009f212bd8baaa353d602dc6090aeeccddd4936
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58098131"
---
# <a name="encryption-of-data-in-azure-data-lake-storage-gen1"></a>Versleuteling van gegevens in Azure Data Lake Storage Gen1

Versleuteling in Azure Data Lake Storage Gen1 helpt u uw gegevens te beschermen, enterprise-beveiligingsbeleid implementeren en te voldoen aan wettelijke vereisten. In dit artikel vindt u een overzicht van het ontwerp en worden een aantal technische aspecten van de implementatie besproken.

Data Lake Storage Gen1 biedt ondersteuning voor versleuteling van gegevens in rust en onderweg zijn. Voor data-at-rest, Data Lake Storage Gen1 ondersteunt "op standaard" transparante versleuteling. Hier wordt iets gedetailleerder uitgelegd wat deze termen betekenen:

* **Op standaard**: Wanneer u een nieuw Data Lake Storage Gen1-account maakt, kunt u de standaardinstelling versleuteling. Gegevens die zijn opgeslagen in Data Lake Storage Gen1 is daarna worden altijd versleuteld vóór het opslaan op permanente media. Dit is het gedrag voor alle gegevens en kan niet worden gewijzigd nadat u een account hebt gemaakt.
* **Transparante**: Data Lake Storage Gen1 automatisch versleutelt de gegevens voordat deze worden opgeslagen en ontsleutelt gegevens automatisch voordat ze worden opgehaald. De versleuteling is geconfigureerd en beheerd op het niveau van de Data Lake Storage Gen1 account door een beheerder. Er worden geen wijzigingen aangebracht aan de API's voor gegevenstoegang. Dus zijn geen wijzigingen vereist in toepassingen en services die interactie met Data Lake Storage Gen1 vanwege versleuteling hebben.

Gegevens tijdens overdracht (ook wel bekend als de gegevens in beweging) worden ook altijd versleuteld in Data Lake Storage Gen1. Naast het versleutelen van gegevens voorafgaand aan het opslaan op permanente media, worden de gegevens tijdens overdracht ook altijd beveiligd met behulp van HTTPS. HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Storage Gen1 REST-interfaces. Het volgende diagram toont hoe gegevens worden versleuteld in Data Lake Storage Gen1:

![Diagram van gegevensversleuteling in Data Lake Storage Gen1](./media/data-lake-store-encryption/fig1.png)


## <a name="set-up-encryption-with-data-lake-storage-gen1"></a>Versleuteling instellen met Data Lake Storage Gen1

Versleuteling voor Data Lake Storage Gen1 wordt ingesteld tijdens het maken van accounts en deze is standaard altijd ingeschakeld. U kunt de sleutels zelf beheren of Data Lake Storage Gen1 voor het beheren van deze voor u (dit is de standaardinstelling) toestaan.

Zie de [Introductie](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) voor meer informatie.

## <a name="how-encryption-works-in-data-lake-storage-gen1"></a>De werking van versleuteling in Data Lake Storage Gen1

De volgende informatie wordt uitgelegd hoe u voor het beheren van hoofdversleutelingssleutels en wordt de drie verschillende typen sleutels die u in gegevensversleuteling voor Data Lake Storage Gen1 gebruiken kunt uitgelegd.

### <a name="master-encryption-keys"></a>Hoofdversleutelingssleutels

Data Lake Storage Gen1 biedt twee modi voor het beheer van hoofdversleutelingssleutels (MEK's). Op dit moment wordt ervan uitgegaan dat de hoofdversleutelingssleutel de sleutel op het hoogste niveau is. Toegang tot de hoofdversleutelingssleutel is vereist voor het ontsleutelen van gegevens die zijn opgeslagen in Data Lake Storage Gen1.

De twee modi voor het beheer van de hoofdversleutelingssleutel zijn als volgt:

*   Door service beheerde sleutels
*   Door klant beheerde sleutels

In beide modi wordt de hoofdversleutelingssleutel beveiligd door opslag in Azure Key Vault. Key Vault is een volledig beheerde, zeer veilige service in Azure die kan worden gebruikt ter bescherming van de cryptografische sleutels. Zie voor meer informatie [Key Vault](https://azure.microsoft.com/services/key-vault).

Hier volgt een korte vergelijking van de mogelijkheden van de twee modi voor het beheren van de MEK's.

|  | Door service beheerde sleutels | Door klant beheerde sleutels |
| --- | --- | --- |
|Hoe worden gegevens opgeslagen?|Altijd versleuteld voordat deze worden opgeslagen.|Altijd versleuteld voordat deze worden opgeslagen.|
|Waar wordt de hoofdversleutelingssleutel opgeslagen?|Key Vault|Key Vault|
|Worden versleutelingssleutels opgeslagen buiten Key Vault? |Nee|Nee|
|Kan de MEK worden opgehaald uit Key Vault?|Nee. Eenmaal opgeslagen in de Key Vault kan de MEK alleen worden gebruikt voor versleuteling en ontsleuteling.|Nee. Eenmaal opgeslagen in de Key Vault kan de MEK alleen worden gebruikt voor versleuteling en ontsleuteling.|
|Wie is eigenaar van de Key Vault-instantie en de MEK?|De Data Lake Storage Gen1-service|U bent eigenaar van de Key Vault-instantie in uw eigen Azure-abonnement. De MEK in de Key Vault kan worden beheerd door software of hardware.|
|Kunt u toegang tot de MEK voor de Data Lake Storage Gen1 service intrekken?|Nee|Ja. U kunt toegangscontrolelijsten in Key Vault beheren en verwijderen van vermeldingen voor toegangsbeheer voor de service-id voor de Data Lake Storage Gen1-service.|
|Kunt u de MEK permanent verwijderen?|Nee|Ja. Als u de MEK uit de Key Vault verwijdert, kan de gegevens in de Gen1 van Data Lake Storage-account kan niet door iedereen, met inbegrip van de Data Lake Storage Gen1-service worden ontsleuteld. <br><br> Als u een expliciete back-up van de MEK maakt voordat u deze uit de Key Vault verwijdert, kan deze worden hersteld en kunnen de gegevens vervolgens worden hersteld. Echter, als u niet een back up van de MEK maakt voordat u deze uit de Sleutelkluis hebt gemaakt, de gegevens in de Gen1 van Data Lake Storage-account kunt daarna nooit meer worden ontsleuteld.|


Afgezien van het verschil wat betreft wie de MEK en de Key Vault-instantie waarin deze zich bevindt beheert, is de rest van het ontwerp hetzelfde voor beide modi.

Het is belangrijk het volgende te onthouden wanneer u de modus voor de hoofdversleutelingssleutels kiest:

*   U kunt kiezen of gebruik van de klant beheerde sleutels of service beheerde sleutels wanneer u een Data Lake Storage Gen1-account inricht.
*   Nadat een Data Lake Storage Gen1-account is ingericht, kan de modus kan niet worden gewijzigd.

### <a name="encryption-and-decryption-of-data"></a>Versleuteling en ontsleuteling van gegevens

Er zijn drie soorten sleutels die worden gebruikt in het ontwerp van gegevensversleuteling. De volgende tabel geeft een samenvatting:

| Sleutel                   | Afkorting | Gekoppeld aan | Opslaglocatie                             | Type       | Opmerkingen                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Hoofdversleutelingssleutel | MEK          | Een Data Lake Storage Gen1-account | Key Vault                              | Asymmetrisch | Het kan worden beheerd door Data Lake Storage Gen1 of u.                                                              |
| Gegevensversleutelingssleutel   | DEK          | Een Data Lake Storage Gen1-account | Permanente opslag, beheerd door de Data Lake Storage Gen1-service | Symmetrisch  | De DEK is versleuteld door de MEK. De versleutelde DEK is hetgeen wat wordt opgeslagen op persistente media. |
| Blokversleutelingssleutel  | BEK          | Een gegevensblok | Geen                                         | Symmetrisch  | De BEK wordt afgeleid van de DEK en het gegevensblok.                                                      |

Het volgende diagram illustreert deze concepten:

![Sleutels bij gegevensversleuteling](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudo-algoritme wanneer een bestand moet worden ontsleuteld:
1.  Controleer of de DEK voor het Data Lake Storage Gen1 account zich in de cache en klaar voor gebruik.
    - Als dat niet het geval is, leest u de versleutelde DEK uit de permanente opslag en stuurt u deze naar de Key Vault voor ontsleuteling. Sla de ontsleutelde DEK op in het cachegeheugen. Deze is nu gereed voor gebruik.
2.  Voor elk gegevensblok in het bestand:
    - Lees het versleutelde gegevensblok uit de permanente opslag.
    - Genereer de BEK uit de DEK en het versleutelde gegevensblok.
    - Gebruik de BEK om gegevens te ontsleutelen.


#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudo-algoritme wanneer een gegevensblok moeten worden versleuteld:
1.  Controleer of de DEK voor het Data Lake Storage Gen1 account zich in de cache en klaar voor gebruik.
    - Als dat niet het geval is, leest u de versleutelde DEK uit de permanente opslag en stuurt u deze naar de Key Vault voor ontsleuteling. Sla de ontsleutelde DEK op in het cachegeheugen. Deze is nu gereed voor gebruik.
2.  Genereer een unieke BEK voor het gegevensblok uit de DEK.
3.  Versleutel het gegevensblok met de BEK met behulp van AES-256-codering.
4.  Sla het versleutelde gegevensblok op in permanente opslag.

> [!NOTE] 
> De DEK wordt altijd versleuteld door de MEK opgeslagen, op permanente media of in het cachegeheugen.

## <a name="key-rotation"></a>Sleutelroulatie

Wanneer u door de klant beheerde sleutels gebruikt, kunt u de MEK rouleren. Zie voor meer informatie over het instellen van een Data Lake Storage Gen1-account met de klant beheerde sleutels, [aan de slag](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="prerequisites"></a>Vereisten

Bij het instellen van het Data Lake Storage Gen1-account, kunt u hebt gekozen om uw eigen sleutels te gebruiken. Deze optie kan niet worden gewijzigd nadat het account is gemaakt. In de volgende stappen wordt ervan uitgegaan dat u door de klant beheerde sleutels gebruikt (u hebt uw eigen sleutels uit Key Vault gekozen).

Houd er rekening mee dat als u de standaardopties voor versleuteling, uw gegevens altijd worden versleuteld met behulp van sleutels die worden beheerd door Data Lake Storage Gen1. Bij deze optie hoeft u de mogelijkheid om te laten draaien van sleutels, zoals ze worden beheerd door Data Lake Storage Gen1.

### <a name="how-to-rotate-the-mek-in-data-lake-storage-gen1"></a>Hoe u de MEK rouleren in Data Lake Storage Gen1

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Blader naar de Key Vault-instantie waarin de sleutels die zijn gekoppeld aan uw Data Lake Storage Gen1-account. Selecteer **Sleutels**.

    ![Schermafdruk van Key Vault](./media/data-lake-store-encryption/keyvault.png)

3. Selecteer de sleutel die is gekoppeld aan uw Data Lake Storage Gen1-account en maak een nieuwe versie van deze sleutel. Houd er rekening mee dat Data Lake Storage Gen1 momenteel alleen ondersteuning biedt voor sleutelroulatie naar een nieuwe versie van een sleutel. Roulatie naar een andere sleutel wordt niet ondersteund.

   ![Schermafdruk van het venster Sleutels met de nieuwe versie gemarkeerd](./media/data-lake-store-encryption/keynewversion.png)

4. Blader naar het Data Lake Storage Gen1-account en selecteer **versleuteling**.

   ![Schermafbeelding van Data Lake Storage Gen1 account venster met versleuteling gemarkeerd](./media/data-lake-store-encryption/select-encryption.png)

5. Er wordt een bericht weergegeven dat een nieuwe sleutelversie van de sleutel beschikbaar is. Klik op **Sleutel rouleren** om de sleutel naar de nieuwe versie bij te werken.

   ![Schermafbeelding van Data Lake Storage Gen1 venster met het bericht en sleutel rouleren gemarkeerd](./media/data-lake-store-encryption/rotatekey.png)

Deze bewerking duurt minder dan twee minuten en er is geen verwachte uitvaltijd vanwege het rouleren van de sleutel. Nadat de bewerking is voltooid, wordt de nieuwe versie van de sleutel gebruikt.

> [!IMPORTANT]
> Nadat de sleutelroulatiebewerking voltooid is, wordt de oude versie van de sleutel niet meer actief gebruikt voor het versleutelen van uw gegevens.  In zeldzame gevallen van onverwachte fouten waardoor zelfs redundante exemplaren van uw gegevens zijn getroffen, kunnen gegevens echter mogelijk worden hersteld vanuit een back-up waarvoor nog de oude sleutel wordt gebruikt. Bewaar een kopie van de vorige versie van de versleutelingssleutel, om ervoor te zorgen dat uw gegevens in deze zeldzame omstandigheden toegankelijk zijn. Zie [richtlijnen voor herstel na noodgevallen voor gegevens in Data Lake Storage Gen1](data-lake-store-disaster-recovery-guidance.md) voor aanbevolen procedures voor de planning voor noodherstel gaat. 