---
title: Versleuteling in Azure Data Lake Store | Microsoft Docs
description: Informatie over de werking van versleuteling en sleutelroulatie in Azure Data Lake Store
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 9bd9996a4a22b2f57510b6f3b6625e22b3183b1c
ms.contentlocale: nl-nl
ms.lasthandoff: 04/29/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Versleuteling van gegevens in Azure Data Lake Store

## <a name="overview-of-encryption-in-azure-data-lake-store"></a>Overzicht van versleuteling in Azure Data Lake Store

Versleuteling in Azure Data Lake Store (ADLS) biedt u de mogelijkheid om uw gegevens te beveiligen, beveiligingsbeleid voor uw onderneming te implementeren en te voldoen aan wettelijke vereisten. Dit artikel geeft u een overzicht van het ontwerp en bespreekt technische aspecten van hoe de Data Lake Store gegevensversleuteling implementeert.

ADLS ondersteunt on-by-default, transparante versleuteling van data-at-rest. Hier wordt iets gedetailleerder uitgelegd wat deze termen betekenen:

* On-by-default: wanneer u een nieuwe Azure Data Lake Store-account maakt, wordt versleuteling standaard ingeschakeld. Daarna worden gegevens die zijn opgeslagen in de Data Lake Store vóór het opslaan op permanente media altijd versleuteld. Dit is het gedrag voor alle gegevens en kan niet worden gewijzigd nadat u een account hebt gemaakt.
* Transparant: ADLS versleutelt gegevens automatisch voordat ze permanent worden gemaakt en ontsleutelt gegevens automatisch voordat ze worden opgehaald. De versleuteling wordt door een beheerder geconfigureerd en beheerd op het niveau van de Data Lake Store. Er worden geen wijzigingen aangebracht aan de API's voor gegevenstoegang en er zijn dus geen wijzigingen vereist in toepassingen en services die interactie hebben met de Data Lake Store vanwege versleuteling.

Gegevens tijdens overdracht (ook wel gegevens in beweging genoemd) worden ook altijd versleuteld in de Data Lake Store. Naast het versleutelen van gegevens voorafgaand aan het opslaan op permanente media, worden de gegevens tijdens overdracht of in beweging ook altijd beveiligd met behulp van HTTPS (HTTP via Secure Sockets Layer). HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Store REST-interfaces.

![Afbeelding 1](./media/data-lake-store-encryption/fig1.png)


## <a name="setting-up-encryption-with-azure-data-lake-store"></a>Versleuteling instellen met Azure Data Lake Store

Versleuteling voor Azure Data Lake Store wordt ingesteld tijdens het maken van het account en is standaard altijd ingeschakeld. Klanten kunnen sleutels zelf beheren of Azure Data Lake Store toestaan (standaardinstelling) sleutels voor hen te beheren.

Zie [Aan de slag](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) voor meer informatie over het instellen van versleuteling met Azure Data Lake Store

## <a name="under-the-hood--how-encryption-works-in-azure-data-lake-store"></a>Achter de schermen: hoe versleuteling werkt in Azure Data Lake Store

### <a name="master-encryption-keys"></a>Hoofdversleutelingssleutels

Azure Data Lake Store biedt twee modi voor het beheer van hoofdversleutelingssleutels (MEK's). Het gebruik van de hoofdversleutelingssleutels wordt hieronder gedetailleerder uitgelegd. Op dit moment wordt ervan uitgegaan dat de hoofdversleutelingssleutel de sleutel op het hoogste niveau is. Toegang tot de hoofdversleutelingssleutel is vereist voor het ontsleutelen van gegevens die zijn opgeslagen in de Data Lake Store.

De twee modi voor het beheer van de hoofdversleutelingssleutel zijn als volgt:

1.    Door service beheerde sleutels
2.    Door klant beheerde sleutels

In beide modi wordt de hoofdversleutelingssleutel beveiligd door opslag in de Azure Key Vault. Azure Key Vault is een volledig beheerde, zeer veilige service in Azure die kan worden gebruikt ter bescherming van de cryptografische sleutels. Meer informatie over Azure Key Vault vindt u [hier](https://azure.microsoft.com/services/key-vault)

Hier volgt een korte vergelijking van de mogelijkheden van de twee modi voor het beheren van de MEK's.

|  | Door service beheerde sleutels | Door klant beheerde sleutels |
| --- | --- | --- |
|Hoe worden gegevens opgeslagen?|Altijd versleuteld voordat deze worden opgeslagen|Altijd versleuteld voordat deze worden opgeslagen|
|Waar wordt de hoofdversleutelingssleutel opgeslagen?|Azure Key Vault|Azure Key Vault|
|Worden versleutelingssleutels opgeslagen buiten Azure Key Vault?|Nee|Nee|
|Kan de MEK worden opgehaald uit de Azure Key Vault?|Nee. Eenmaal opgeslagen in de Key Vault kan deze alleen worden gebruikt voor versleuteling en ontsleuteling.|Nee. Eenmaal opgeslagen in de Key Vault kan deze alleen worden gebruikt voor versleuteling en ontsleuteling.|
|Wie is eigenaar van de Azure Key Vault en de MEK?|Azure Data Lake Store service.|De klant is eigenaar van Azure Key Vault, die deel uitmaakt van zijn eigen Azure-abonnement. De MEK in de Key Vault kan worden beheerd door software of hardware (HSM).|
|Kan de klant toegang tot de MEK voor de Azure Data Lake Store-service intrekken?|Nee|Ja. Ze kunnen toegangscontrolelijsten in de Azure Key Vault beheren en kunnen toegangsbeheervermeldingen voor de service-ID voor de Azure Data Lake Store-service verwijderen.|
|Kan de klant de MEK permanent verwijderen?|Nee|Ja. Als de klant de MEK uit de Azure Key Vault verwijdert, kunnen de gegevens in het ADLS-account door niemand meer worden ontsleuteld, met inbegrip van de Azure Data Lake Store-service. <br><br> Als een expliciete back-up van de MEK wordt gemaakt door de klant voordat deze uit de Azure Key Vault wordt verwijderd, kan deze worden hersteld en kunnen de gegevens vervolgens worden hersteld. Als er echter geen back-up van de MEK wordt gemaakt door de klant voordat deze uit de Azure Key Vault wordt verwijderd, kunnen de gegevens in het ADLS-account daarna nooit meer worden ontsleuteld.|


Afgezien van het verschil op het hoogste niveau, wat betreft wie de MEK en de Key Vault waarin deze zich bevindt beheert, is de rest van het ontwerp hetzelfde voor beide modi.

Er zijn enkele belangrijke aspecten om te onthouden wat betreft het kiezen van de modus voor de hoofdversleutelingssleutels.

1.    U kunt kiezen of u door de klant beheerde sleutels of door ADLS beheerde sleutels wilt gebruiken wanneer u een ADLS-account inricht.
2.    Zodra een ADLS-account is ingericht, kan de modus niet meer worden gewijzigd.

### <a name="encryption-and-decryption-of-data"></a>Versleuteling en ontsleuteling van gegevens

Er zijn drie soorten sleutels die worden gebruikt in het ontwerp van gegevensversleuteling voor Azure Data Lake. De volgende tabel bevat een samenvatting over hun rol:

| Sleutel                   | Afkorting | Gekoppeld aan | Opslaglocatie                             | Type       | Opmerkingen                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Hoofdversleutelingssleutel | MEK          | Een ADLS-account | Azure Key Vault                              | Asymmetrisch | Kan door ADLS beheerd of door de klant beheerd zijn                                                              |
| Gegevensversleutelingssleutel   | DEK          | Een ADLS-account | Permanente opslag - beheerd door ADLS-service | Symmetrisch  | De DEK wordt versleuteld door de MEK en de versleutelde DEK is wat wordt opgeslagen op permanente media van de service |
| Blokversleutelingssleutel  | BEK          | Een gegevensblok | Geen                                         | Symmetrisch  | De BEK wordt afgeleid van de DEK en het gegevensblok                                                      |

Het volgende diagram illustreert deze concepten:

![Afbeelding 2](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudo-algoritme wanneer een bestand moet worden ontsleuteld:
1.    Controleer of de DEK voor het ADLS-account zich in de cache bevindt en klaar voor gebruik is.
    * Als dat niet het geval is, leest u de versleutelde DEK uit de permanente opslag en stuurt u deze naar de Azure Key Vault voor ontsleuteling. Plaats de ontsleutelde DEK in het cachegeheugen, waarna deze gereed is voor gebruik.
2.    Voor elk gegevensblok in het bestand
    * Lees het versleutelde gegevensblok uit de permanente opslag
    * Genereer de BEK uit de DEK en het versleutelde gegevensblok
    * Gebruik de BEK om gegevens te ontsleutelen
#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudo-algoritme wanneer een gegevensblok moeten worden versleuteld:
1.    Controleer of de DEK voor het ADLS-account zich in de cache bevindt en klaar voor gebruik is.
    * Als dat niet het geval is, leest u de versleutelde DEK uit de permanente opslag en stuurt u deze naar de Azure Key Vault voor ontsleuteling. Plaats de ontsleutelde DEK in het cachegeheugen, waarna deze gereed is voor gebruik.
2.    Genereer een unieke BEK voor het gegevensblok uit de DEK.
3.    Versleutel het gegevensblok met de BEK met behulp van AES-256-codering.
4.    Sla het versleutelde gegevensblok op in permanente opslag.

> [!NOTE] 
> Uit prestatieoverwegingen wordt de DEK tijdelijk in het cachegeheugen geplaatst voor een korte periode en onmiddellijk gewist nadat de periode is verstreken. Op permanente media wordt deze altijd opgeslagen terwijl deze versleuteld is door de MEK.

## <a name="key-rotation"></a>Sleutelroulatie

Azure Data Lake Store biedt bij het gebruik van door de klant beheerde sleutels de mogelijkheid tot roulatie van de MEK. Zie de pagina [Aan de slag](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal) voor informatie over het instellen van een ADLS-account met door de klant beheerde sleutels.

### <a name="pre-requisites"></a>Vereisten

Bij het instellen van het Azure Data Lake-account, hebben klanten hun eigen sleutels gekozen. Deze optie kan niet worden gewijzigd nadat het account is gemaakt. Als u de standaardopties gebruikt voor versleuteling, worden uw gegevens altijd versleuteld met sleutels die worden beheerd door Azure Data Lake. Bij deze optie beschikt de klant niet over de mogelijkheid om te sleutels te rouleren omdat ze worden beheerd door Azure Data Lake. In de onderstaande stappen wordt ervan uitgegaan dat u door de klant beheerde sleutels gebruikt (u hebt uw eigen sleutels uit uw Key Vault gekozen).

### <a name="how-to-rotate-the-key-mek-in-azure-data-lake-store"></a>De sleutel (MEK) rouleren in Azure Data Lake Store

1. Meld u aan bij de nieuwe [Azure Portal](https://portal.azure.com/).
2. Navigeer naar de Key Vault waarin de sleutels die zijn gekoppeld aan uw Azure Data Lake Store-account en selecteer sleutels.

    ![Sleutels](./media/data-lake-store-encryption/keyvault.png)

3.    Selecteer de sleutel die is gekoppeld aan uw Azure Data Lake Store-account en maak een nieuwe versie van deze sleutel.
  
   Op dit moment biedt Azure Data Lake alleen ondersteuning voor sleutelroulatie naar een nieuwe versie van een sleutel. Roulatie naar een andere sleutel wordt niet ondersteund

   ![Nieuwe versie](./media/data-lake-store-encryption/keynewversion.png)

4.    Navigeer naar de Azure Data Lake Storage-account en selecteer Versleuteling.

    ![Nieuwe versie](./media/data-lake-store-encryption/select-encryption.png)

5.    U ziet een melding waarin u wordt geïnformeerd dat er een nieuwe sleutelversie van de sleutel beschikbaar is en een knop waarmee de sleutel naar deze nieuwe versie wordt gerouleerd. Klik op Sleutel rouleren om de sleutel naar de nieuwe versie bij te werken.

    ![Gereed](./media/data-lake-store-encryption/rotatekey.png)

6. Deze bewerking duurt minder dan 2 minuten duren en er is geen verwachte uitvaltijd vanwege het rouleren van de sleutel. Zodra de bewerking is voltooid, wordt de nieuwe versie van de sleutel gebruikt.

