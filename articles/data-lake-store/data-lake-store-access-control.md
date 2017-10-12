---
title: Overzicht van Access Control in Data Lake Store | Microsoft Docs
description: Begrijpen hoe toegangsbeheer werkt in Azure Data Lake Store
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/03/2017
ms.author: nitinme
ms.openlocfilehash: 43582ae805d560e72fb8d03ab8e0abee4c4325b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="access-control-in-azure-data-lake-store"></a>Toegangsbeheer in Azure Data Lake Store

Azure Data Lake Store implementeert een model voor toegangsbeheer dat is afgeleid van HDFS. Dit is op zijn beurt afgeleid van het POSIX-model voor toegangsbeheer. Dit artikel bevat een overzicht van de basisbeginselen van het model voor toegangsbeheer voor Data Lake Store. Voor meer informatie over het HDFS-model voor toegangsbeheer, raadpleegt u de [Handleiding voor HDFS-machtigingen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Toegangsbeheerlijsten voor bestanden en mappen

Er zijn twee soorten toegangsbeheerlijsten (ACL's): **Toegangs-ACL's** en **Standaard-ACL's**.

* **Toegangs-ACL's**: deze beheren de toegang tot een object. Bestanden en mappen hebben Toegangs-ACL's.

* **Standaard-ACL's**: een 'sjabloon' van ACL's die zijn gekoppeld aan een map die de Toegangs-ACL's bepaalt voor alle onderliggende items die zijn gemaakt onder die map. Bestanden hebben geen Standaard-ACL's.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Toegangs-ACL's en Standaard-ACL's hebben dezelfde structuur.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> Als u de Standaard-ACL voor een bovenliggend item wijzigt, heeft dit geen invloed op de Toegangs-ACL of de Standaard ACL van bestaande onderliggende items.
>
>

## <a name="users-and-identities"></a>Gebruikers en identiteiten

Alle bestanden en mappen beschikken over verschillende machtigingen voor de volgende identiteiten:

* De gebruiker die eigenaar is van het bestand
* De groep die eigenaar is
* Benoemde gebruikers
* Benoemde groepen
* Alle andere gebruikers

De identiteiten van gebruikers en groepen zijn Azure Active Directory-identiteiten (Azure AD). Dus tenzij anders vermeld, kan een 'gebruiker' in de context van Data Lake Store een Azure AD-gebruiker of een Azure AD-beveiligingsgroep betekenen.

## <a name="permissions"></a>Machtigingen

De machtigingen voor een bestandssysteemobject zijn **Lezen**, **Schrijven** en **Uitvoeren**. Deze kunnen worden gebruikt voor bestanden en mappen zoals weergegeven in de onderstaande tabel:

|            |    File     |   Map |
|------------|-------------|----------|
| **Lezen (L)** | Kan de inhoud van een bestand lezen | **Lezen** en **Uitvoeren** zijn vereist om de inhoud van de map weer te geven|
| **Schrijven (S)** | Kan schrijven of toevoegen aan een bestand | **Schrijven** en **Uitvoeren** zijn vereist om onderliggende items in een map te maken |
| **Uitvoeren (U)** | Heeft geen functie in de context van Data Lake Store | Vereist om de onderliggende items van een map door te gaan |

### <a name="short-forms-for-permissions"></a>Korte formulieren voor machtigingen

**LSU** wordt gebruikt om **Lezen + Schrijven + Uitvoeren** aan te geven. Er bestaat een verkorte numerieke vorm. Hierbij is **Lezen = 4**, **Schrijven = 2** en **Uitvoeren = 1**. De som van deze getallen vertegenwoordigt de machtigingen. Hier volgen enkele voorbeelden.

| Numerieke vorm | Verkorte vorm |      Wat het betekent     |
|--------------|------------|------------------------|
| 7            | LSU        | Lezen + Schrijven + Uitvoeren |
| 5            | L-U        | Lezen + Uitvoeren         |
| 4            | L--        | Lezen                   |
| 0            | ---        | Geen machtigingen         |


### <a name="permissions-do-not-inherit"></a>Machtigingen worden niet overgenomen

In het POSIX-model dat wordt gebruikt door Data Lake Store worden machtigingen voor een item opgeslagen op het item zelf. Machtigingen voor een item kunnen dus niet worden overgenomen van de bovenliggende items.

## <a name="common-scenarios-related-to-permissions"></a>Algemene scenario's met machtigingen

Hier volgen enkele algemene scenario's om te begrijpen welke machtigingen zijn vereist om bepaalde bewerkingen uit te voeren op een Data Lake Store-account.

### <a name="permissions-needed-to-read-a-file"></a>Vereiste machtigingen om een bestand te lezen

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* De aanroeper heeft **Leesmachtigingen** nodig om het bestand te lezen.
* De aanroeper heeft **Uitvoeringsmachtigingen** nodig voor alle mappen in de mapstructuur die het bestand bevatten.

### <a name="permissions-needed-to-append-to-a-file"></a>Vereiste machtigingen om toe te voegen aan een bestand

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* De aanroeper heeft **Schrijfmachtigingen** nodig om toe te voegen aan het bestand.
* De aanroeper heeft **Uitvoeringsmachtigingen** nodig voor alle mappen die het bestand bevatten.

### <a name="permissions-needed-to-delete-a-file"></a>Vereiste machtigingen om een bestand te verwijderen

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* De aanroeper heeft **Schrijf- en uitvoeringsmachtigingen** nodig voor de bovenliggende map.
* De aanroeper heeft **Uitvoeringsmachtigingen** nodig voor alle andere mappen in het pad van het bestand.



> [!NOTE]
> Schrijfmachtigingen zijn niet vereist voor het verwijderen van het bestand als aan de eerder genoemde twee voorwaarden wordt voldaan.
>
>

### <a name="permissions-needed-to-enumerate-a-folder"></a>Vereiste machtigingen om een map op te sommen

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* De aanroeper heeft **Lees- en uitvoeringsmachtigingen** nodig om de map op te sommen.
* De aanroeper heeft **Uitvoeringsmachtigingen** nodig voor alle bovenliggende mappen.

## <a name="viewing-permissions-in-the-azure-portal"></a>Weergavemachtigingen in de Azure Portal

Klik in de blade **Data Explorer** in het Data Lake Store-account op **Toegang** om de ACL's voor een bestand of map te bekijken. Klik op **Toegang** om de ACL's te zien voor de map **Catalogus** in het **mydatastore**-account.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Het bovenste gedeelte van de blade geeft een overzicht weer van uw machtigingen. (In de schermopname is de gebruiker Bob.) Daaronder worden de toegangsmachtigingen weergegeven. Hierna klikt u vanaf de blade **Toegang** op **Eenvoudige weergave** om de vereenvoudigde weergave te zien.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klik op **Geavanceerde weergave** om een geavanceerdere weergave te openen waar de concepten Standaard ACL's, Masker en Supergebruiker worden weergegeven.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>De supergebruiker

Een supergebruiker heeft de meeste rechten van alle gebruikers in de Data Lake Store. Een supergebruiker:

* heeft LSU-machtigingen voor **alle** bestanden en mappen,
* kan de machtigingen voor elk bestand en elke map wijzigen,
* kan de eigenaar of groep die eigenaar is van een bestand of map wijzigen.

In Azure heeft een Data Lake Store-account meerdere Azure-rollen, waaronder:

* Eigenaren
* Inzenders
* Lezers

Iedereen met de rol **Eigenaar** voor een Data Lake Store-account wordt automatisch een supergebruiker voor dat account. Zie [Op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md) voor meer informatie.
Als u aangepast op rollen gebaseerd toegangsbeheer (RBAC) wilt maken met supergebruikersmachtigingen, moet dit de volgende machtigingen hebben:
- Microsoft.DataLakeStore/accounts/Superuser/action
- Microsoft.Authorization/roleAssignments/write


## <a name="the-owning-user"></a>De gebruiker die eigenaar is

De gebruiker die het item heeft gemaakt, wordt automatisch de gebruiker die eigenaar is van het item. Een gebruiker die eigenaar is kan:

* De machtigingen wijzigen van een bestand waarvan hij eigenaar is.
* De groep die eigenaar van een bestand is wijzigen, zolang deze gebruiker ook lid is van de doelgroep.

> [!NOTE]
> De gebruiker die eigenaar is, kan de gebruiker die eigenaar van een ander bestand is *niet* wijzigen. Alleen supergebruikers kunnen de gebruiker die eigenaar is van een bestand of map wijzigen.
>
>

## <a name="the-owning-group"></a>De groep die eigenaar is

In de POSIX ACL's is elke gebruiker gekoppeld aan een 'hoofdgroep'. Gebruiker 'Els' kan bijvoorbeeld behoren tot de groep 'Financiën'. Els kan behoren tot meerdere groepen, maar één groep is altijd ingesteld als haar hoofdgroep. Wanneer Els in POSIX een bestand maakt, wordt de groep die eigenaar van het bestand is als haar hoofdgroep ingesteld. In dit geval is dit 'Financiën'.

Wanneer een nieuw item in het bestandssysteem wordt gemaakt, wijst Data Lake Store een waarde toe aan de groep die eigenaar is.

* **Voorbeeld 1**: de hoofdmap '/'. Deze map wordt gemaakt wanneer een Data Lake Store-account wordt gemaakt. In dit geval is de groep die eigenaar is, ingesteld op de gebruiker die het account heeft gemaakt.
* **Voorbeeld 2** (alle andere gevallen): wanneer een nieuw item wordt gemaakt, wordt de groep die eigenaar is, gekopieerd van de bovenliggende map.

De groep die eigenaar is kan worden gewijzigd door:
* Alle supergebruikers.
* De gebruiker die eigenaar is, als deze gebruiker ook lid is van de doelgroep.

## <a name="access-check-algorithm"></a>Algoritme voor toegangscontrole

In de volgende afbeelding ziet u het algoritme voor toegangscontrole voor Data Lake Store-accounts.

![Algoritme voor Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Het masker en 'effectieve machtigingen'

Het **masker** is een LSU-waarde die wordt gebruikt voor het beperken van de toegang voor **benoemde gebruikers**, de **groep die eigenaar is** en **benoemde groepen** bij het uitvoeren van het algoritme voor toegangscontrole. Dit zijn de belangrijkste concepten voor het masker.

* Het masker maakt 'effectieve machtigingen'. Dat wil zeggen dat het de machtigingen wijzigt op het moment van de toegangscontrole.
* Het masker kan rechtstreeks worden bewerkt door de bestandseigenaar en eventuele supergebruikers.
* Het masker kan machtigingen verwijderen om de effectieve machtiging te maken. Het masker *kan geen* machtigingen toevoegen aan de effectieve machtiging.

We bekijken enkele voorbeelden. In het volgende voorbeeld is het masker ingesteld op **LSU**. Dit betekent dat het masker machtigingen niet verwijdert. De effectieve machtigingen voor de benoemde gebruiker, de groep die eigenaar is en de benoemde groep worden niet gewijzigd tijdens de toegangscontrole.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

In het volgende voorbeeld is het masker ingesteld op **L-U**. Dit betekent dat het **de Schrijfmachtiging uitschakelt** voor **de benoemde gebruiker**, **de groep die eigenaar is** en **de benoemde groep** tijdens de toegangscontrole.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Ter informatie: dit is waar het masker voor een bestand of map wordt weergegeven in Azure Portal.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Het masker voor de Toegangs-ACL en de Standaard-ACL van de hoofdmap ('/') zijn voor een nieuw Data Lake Store-account standaard ingesteld op LSU.
>
>

## <a name="permissions-on-new-files-and-folders"></a>Machtigingen voor nieuwe bestanden en mappen

Wanneer een nieuw bestand of een nieuwe map wordt gemaakt onder een bestaande map, bepaalt de Standaard-ACL voor de bovenliggende map:

- De Standaard-ACL en Toegangs-ACL voor een onderliggende map.
- De Toegangs-ACL van een onderliggend bestand (bestanden beschikken niet over een Standaard-ACL).

### <a name="the-access-acl-of-a-child-file-or-folder"></a>De Toegangs-ACL van een onderliggend bestand of een onderliggende map

Wanneer een onderliggend bestand of een onderliggende map wordt gemaakt, wordt de bovenliggende Standaard-ACL gekopieerd als de Toegangs-ACL van het onderliggende bestand of de onderliggende map. Als een **andere** gebruiker LSU-machtigingen heeft in de bovenliggende Standaard-ACL, wordt deze verwijderd uit de Toegangs-ACL van het onderliggende item.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

De bovenstaande informatie is in de meeste gevallen alles wat u hoeft te weten over hoe de Toegangs-ACL van een onderliggend item wordt bepaald. Als u echter bekend bent met POSIX-systemen en op geavanceerd niveau wilt begrijpen hoe deze transformatie tot stand komt, kunt u het gedeelte [De rol van Umask bij het maken van de Toegangs-ACL voor nieuwe bestanden en mappen](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) verderop in dit artikel raadplegen.


### <a name="a-child-folders-default-acl"></a>De Standaard-ACL voor een onderliggende map

Als een onderliggende map wordt gemaakt onder een bovenliggende map, wordt de Standaard-ACL van de bovenliggende map gekopieerd als de Standaard-ACL van de onderliggende map.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Geavanceerde onderwerpen om ACL's in de Data Lake Store te begrijpen

Hieronder vindt u een aantal geavanceerde onderwerpen om u te helpen begrijpen hoe de ACL's voor Data Lake Store-bestanden of -mappen worden bepaald.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>De rol van Umask bij het maken van de Toegangs-ACL voor nieuwe bestanden en mappen

In een POSIX-compatibel systeem geldt het volgende als algemeen concept: umask is een 9-bits waarde van de bovenliggende map die wordt gebruikt om de machtiging voor **de gebruiker die eigenaar is**, **de groep die eigenaar is** en **anderen** te transformeren op de Toegangs-ACL van een nieuw onderliggend bestand of een nieuwe onderliggende map. De bits van een umask identificeren welke bits moeten worden uitgeschakeld in de Toegangs-ACL van het onderliggende item. Deze wordt dus gebruikt om de verspreiding van machtigingen voor **de gebruiker die eigenaar is**, **de groep die eigenaar is** en **anderen** selectief te voorkomen.

In een HDFS-systeem is de umask doorgaans een configuratieoptie voor de gehele site die wordt beheerd door beheerders. Data Lake Store gebruikt een **umask voor het hele account** die niet kan worden gewijzigd. De volgende tabel geeft de umask van Data Lake Store weer.

| Gebruikersgroep  | Instelling | Invloed op de Toegangs-ACL van het nieuwe onderliggende item |
|------------ |---------|---------------------------------------|
| Gebruiker die eigenaar is | ---     | Geen effect                             |
| Groep die eigenaar is| ---     | Geen effect                             |
| Overige       | LSU     | Lezen + Schrijven + Uitvoeren verwijderen         |

De volgende afbeelding geeft deze umask in actie weer. Het uiteindelijke resultaat is dat **Lezen + Schrijven + Uitvoeren** voor de **andere** gebruiker worden verwijderd. Aangezien de umask geen bits heeft opgegeven voor de **gebruiker die eigenaar is** en **de groep die eigenaar is**, worden deze machtigingen niet getransformeerd.

![Data Lake Store ACL’s](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="the-sticky-bit"></a>De vergrendelde bit

De vergrendelde bit is een geavanceerdere functie van een POSIX-bestandssysteem. In de context van Data Lake Store is het onwaarschijnlijk dat de vergrendelde bit nodig is.

De volgende tabel laat zien hoe de vergrendelde bit in de Data Lake Store werkt.

| Gebruikersgroep         | File    | Map |
|--------------------|---------|-------------------------|
| Vergrendelde bit **UIT** | Geen effect   | Geen effect.           |
| Vergrendelde bit **AAN**  | Geen effect   | Zorgt ervoor dat niemand behalve **supergebruikers** en de **gebruiker die eigenaar is** van een onderliggend item het betreffende item kan verwijderen of hernoemen.               |

De vergrendelde bit wordt niet weergegeven in Azure Portal.

## <a name="common-questions-about-acls-in-data-lake-store"></a>Veelgestelde vragen over ACL's in Data Lake Store

Hier volgen enkele vragen die vaak worden gesteld over ACL's in Data Lake Store.

### <a name="do-i-have-to-enable-support-for-acls"></a>Moet ik ondersteuning voor ACL's inschakelen?

Nee. Toegangsbeheer via de ACL's is altijd ingeschakeld voor een Data Lake Store-account.

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Welke machtigingen zijn vereist voor het recursief verwijderen van een map en de inhoud ervan?

* De bovenliggende map moet de machtigingen **Schrijven + Uitvoeren** hebben.
* De map die wordt verwijderd en elke map daarin moeten de machtigingen **Lezen + Schrijven + Uitvoeren** hebben.

> [!NOTE]
> U hebt geen schrijfmachtigingen nodig voor het verwijderen van bestanden in mappen. Verder kan de hoofdmap '/' **nooit** worden verwijderd.
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a>Wie is de eigenaar van een bestand of map?

De maker van een bestand of map wordt de eigenaar.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Welke groep wordt ingesteld als eigenaar van een bestand of map bij het maken ervan?

De groep wordt gekopieerd uit de groep die eigenaar is van de bovenliggende map waaronder het nieuwe bestand of de nieuwe map wordt gemaakt.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ik ben de gebruiker die eigenaar is van een bestand, maar ik heb de LSU-machtigingen die ik nodig heb niet. Wat moet ik doen?

De gebruiker die eigenaar is kan de machtigingen van het bestand wijzigen en zichzelf de vereiste LSU-machtigingen geven.

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a>Wanneer ik ACL's bekijk in Azure Portal, zie ik gebruikersnamen, maar via API's zie ik GUID's. Hoe kan dat?

Vermeldingen worden in de ACL's opgeslagen als GUID's die zijn gekoppeld aan gebruikers in Azure AD. De API's geven de GUID's als zodanig weer. We proberen ACL's in Azure Portal eenvoudiger in gebruik te maken door de GUID's wanneer mogelijk om te zetten in beschrijvende namen.

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a>Waarom zie ik soms GUID's in de ACL's wanneer ik Azure Portal gebruik?

Er wordt een GUID weergegeven wanneer een gebruiker niet meer bestaat in Azure AD. Dit gebeurt doorgaans wanneer gebruikers het bedrijf verlaten of hun accounts zijn verwijderd in Azure AD.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Biedt Data Lake Store ondersteuning voor overname van ACL's?

Nee.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Wat is het verschil tussen een masker en een umask?

| masker | umask|
|------|------|
| De eigenschap **masker** is beschikbaar op elk bestand en elke map. | De **umask** is een eigenschap van het Data Lake Store-account. Er bevindt zich dus slechts één umask in de Data Lake Store.    |
| De eigenschap masker op een bestand of map kan worden gewijzigd door de gebruiker of groep die eigenaar is van een bestand, of door een supergebruiker. | De eigenschap umask kan niet worden gewijzigd door een gebruiker, zelfs niet door een supergebruiker. Het is een onveranderbare, constante waarde.|
| De eigenschap masker wordt gebruikt om tijdens het algoritme toegangscontrole bij runtime te bepalen of een gebruiker een bewerking op een bestand of map uit mag voeren. De rol van het masker is om op het moment van toegangscontrole 'effectieve machtigingen' te maken. | De umask wordt helemaal niet gebruikt tijdens toegangscontrole. De umask wordt gebruikt om de Toegangs-ACL van nieuwe onderliggende items van een map te bepalen. |
| Het masker is een LSU-waarde van 3-bits die van toepassing is op de benoemde gebruiker, de benoemde groep en de gebruiker die eigenaar is op het moment van de toegangscontrole.| De umask is een 9-bits-waarde die van toepassing is op de gebruiker die eigenaar is, de groep die eigenaar is en **anderen** van een nieuw onderliggend element.|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Waar kan ik meer informatie over het POSIX-model voor toegangsbeheer?

* [POSIX met behulp van toegangsbeheerlijsten op Linux](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [Handleiding voor HDFS-machtigingen](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [Veelgestelde vragen over POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1 2013](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [POSIX 1003.1 2016](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [POSIX ACL in Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)

* [ACL met behulp van toegangsbeheerlijsten op Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Zie ook

* [Overzicht van Azure Data Lake Store](data-lake-store-overview.md)
