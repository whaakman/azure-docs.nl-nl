---
title: Overzicht van toegangsbeheer in Azure Data Lake Storage Gen2 | Microsoft Docs
description: Begrijpen hoe toegangsbeheer werkt in Azure Data Lake Storage Gen2
services: storage
author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: jamesbak
ms.openlocfilehash: a102216a6a2a7dec471678e14f7050cb4ef41d77
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370105"
---
# <a name="access-control-in-azure-data-lake-storage-gen2"></a>Toegangsbeheer in Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 implementeert een model voor toegangsbeheer die ondersteuning biedt voor zowel Azure rollen gebaseerd toegangsbeheer (RBAC) en POSIX-achtige toegangsbeheerlijsten (ACL's). In dit artikel bevat een overzicht van de basisbeginselen van het model voor toegangsbeheer voor Data Lake Storage Gen2. 

## <a name="azure-role-based-access-control-rbac"></a>Azure Role-based Access Control (RBAC)

Azure Role-based Access Control (RBAC) maakt gebruik van roltoewijzingen effectief sets machtigingen toepassen op gebruikers, groepen en service-principals voor Azure-resources. Normaal gesproken deze Azure-resources worden beperkt tot bronnen op het hoogste niveau (*bijvoorbeeld*, Azure Storage-accounts). In het geval van Azure Storage, en als gevolg hiervan in Azure Data Lake Storage Gen2, dit mechanisme uitgebreid naar de bron van het systeem bestand.

Tijdens het gebruik van RBAC-roltoewijzingen een krachtig mechanisme voor het besturingselement gebruikersmachtigingen, is dit een zeer coarsely korrelig mechanisme ten opzichte van ACL's. De kleinste granulariteit voor RBAC is op het niveau van het bestandssysteem en dit wordt eerst geëvalueerd op een hogere prioriteit dan de ACL's. Dus als u RBAC-machtigingen op een bestandssysteem toewijst, hebt die gebruiker of service-principal die autorisatie voor alle mappen en bestanden in die bestandssysteem, ongeacht de ACL-toewijzingen.

Azure Storage biedt drie ingebouwde RBAC-rollen voor Blob-opslag: 

- [De eigenaar van een opslag-Blob-gegevens](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview)
- [Gegevensbijdrager voor Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview)
- [Gegevenslezer voor Opslagblob](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview)

Als gegevens RBAC-machtigingen via een van deze ingebouwde rollen, of via een aangepaste rol is verleend door een gebruiker of service-principal, worden deze machtigingen eerst bij de autorisatie van een aanvraag geëvalueerd. Als de aangevraagde bewerking is geautoriseerd door de aanroeper RBAC-toewijzingen en autorisatie onmiddellijk afgesloten en er geen aanvullende wordt worden ACL-controles uitgevoerd. U kunt ook als de oproepende functie heeft geen een toewijzing RBAC of bewerking van de aanvraag komt niet overeen met de toegewezen machtiging, klikt u vervolgens ACL controles uitgevoerd om te bepalen of de aanroeper is gemachtigd om uit te voeren van de aangevraagde bewerking.

Een speciale opmerking moet worden gemaakt van de ingebouwde rol van de eigenaar van een opslag-Blob-gegevens. Als de aanroeper deze RBAC-toewijzing heeft, wordt de gebruiker wordt beschouwd als een *supergebruiker* en is verleend volledige toegang tot alle mutating bewerkingen, inclusief het instellen van de eigenaar van een map of bestand, evenals ACL's voor mappen en bestanden waarvoor ze zijn niet de eigenaar. Supergebruiker toegang is de enige geautoriseerde manier om te wijzigen van de eigenaar van een resource.

## <a name="shared-key-and-shared-access-signature-authentication"></a>Gedeelde sleutels en Shared Access Signature-verificatie

Azure Data Lake Storage Gen2 ondersteunt gedeelde sleutel en Shared Access Signature-methoden voor verificatie. Een kenmerk van deze verificatiemethoden is dat er geen identiteit gekoppeld aan de oproepende functie is en daarom gebruikersautorisatie op basis van een machtiging kan niet worden uitgevoerd.

In het geval van een gedeelde sleutel, de oproepende functie effectief 'supergebruiker' om toegang te krijgen, wat betekent dat volledige toegang tot alle bewerkingen op alle resources, met inbegrip van de eigenaar instellen en wijzigen van ACL's.

Toegestane machtigingen opnemen SAS-tokens als onderdeel van het token. De machtigingen die zijn opgenomen in het SAS-token effectief worden toegepast op alle autorisatie beslissingen te nemen, maar er zijn geen aanvullende ACL-controles worden uitgevoerd.

## <a name="access-control-lists-on-files-and-directories"></a>Toegangsbeheerlijsten voor bestanden en mappen

Er zijn twee soorten toegangsbeheerlijsten (ACL's): toegang tot ACL's en standaard-ACL's.

* **Toegangs-ACL's**: Met toegangs-ACL's beheert u de toegang tot een object. Bestanden en mappen hebben beide Toegangs-ACL's.

* **Standaard-ACL's**: Een sjabloon van ACL's die zijn gekoppeld aan een map die de toegang tot een ACL's voor alle onderliggende items die zijn gemaakt onder die map. Bestanden hebben geen Standaard-ACL's.

Beide toegangs-ACL's en standaard-ACL's hebben dezelfde structuur.

> [!NOTE]
> Wijzigen van de standaard-ACL voor een bovenliggend item is niet van invloed zijn op de toegang tot de ACL of standaard-ACL van onderliggende items die al bestaan.

## <a name="permissions"></a>Machtigingen

De machtigingen voor een bestandssysteemobject zijn **lezen**, **schrijven**, en **Execute**, en ze kunnen worden gebruikt voor bestanden en mappen zoals weergegeven in de volgende tabel:

|            |    File     |   Directory |
|------------|-------------|----------|
| **Lezen (L)** | Kan de inhoud van een bestand lezen | Vereist **lezen** en **Execute** om de inhoud van de map weer te geven |
| **Schrijven (S)** | Kan schrijven of toevoegen aan een bestand | Vereist **schrijven** en **Execute** onderliggende items in een map maken |
| **Uitvoeren (U)** | Betekent niet dat alles in de context van Data Lake Storage Gen2 | Vereist om de onderliggende items van een map te gaan |

### <a name="short-forms-for-permissions"></a>Korte formulieren voor machtigingen

**LSU** wordt gebruikt om **Lezen + Schrijven + Uitvoeren** aan te geven. Er bestaat een verkorte numerieke vorm. Hierbij is **Lezen = 4**, **Schrijven = 2** en **Uitvoeren = 1**. De som van deze getallen vertegenwoordigt de machtigingen. Hier volgen enkele voorbeelden.

| Numerieke vorm | Verkorte vorm |      Wat het betekent     |
|--------------|------------|------------------------|
| 7            | `RWX`        | Lezen + Schrijven + Uitvoeren |
| 5            | `R-X`        | Lezen + Uitvoeren         |
| 4            | `R--`        | Lezen                   |
| 0            | `---`        | Geen machtigingen         |

### <a name="permissions-inheritance"></a>Overname van machtigingen

In het POSIX model dat wordt gebruikt door Data Lake Storage Gen2, worden machtigingen voor een item opgeslagen op het item zelf. Met andere woorden, is dat de machtigingen voor een item kunnen niet worden overgenomen van de bovenliggende items als de machtigingen zijn ingesteld nadat het onderliggende item is al gemaakt. Machtigingen worden alleen overgenomen als standaardmachtigingen zijn ingesteld op de bovenliggende items voordat u de onderliggende items zijn gemaakt.

## <a name="common-scenarios-related-to-permissions"></a>Algemene scenario's met machtigingen

De volgende tabel bevat enkele algemene scenario's om u te helpen u te begrijpen welke machtigingen zijn vereist om uit te voeren van bepaalde bewerkingen op een Data Lake Storage Gen2-account.

|    Bewerking             |    /    | Oregon/ | Portland / | Data.txt     |
|--------------------------|---------|----------|-----------|--------------|
| Data.txt lezen            |   `--X`   |   `--X`    |  `--X`      | `R--`          |
| Toevoegen aan Data.txt       |   `--X`   |   `--X`    |  `--X`      | `RW-`          |
| Data.txt verwijderen          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Data.txt maken          |   `--X`   |   `--X`    |  `-WX`      | `---`          |
| Lijst met /                   |   `R-X`   |   `---`    |  `---`      | `---`          |
| Lijst met /Oregon/           |   `--X`   |   `R-X`    |  `---`      | `---`          |
| Lijst met /Oregon/Portland /  |   `--X`   |   `--X`    |  `R-X`      | `---`          |


> [!NOTE]
> Schrijven machtigingen op het bestand hoeven niet te verwijderen, zolang de vorige twee voorwaarden voldaan wordt.
>
>

## <a name="users-and-identities"></a>Gebruikers en identiteiten

Alle bestanden en mappen beschikken over verschillende machtigingen voor de volgende identiteiten:

- De gebruiker die eigenaar is
- De groep die eigenaar is
- Benoemde gebruikers
- Benoemde groepen
- Benoemde service-principals
- Alle andere gebruikers

De identiteiten van gebruikers en groepen zijn Azure Active Directory-identiteiten (Azure AD). Dit het geval is, tenzij anders vermeld, een *gebruiker*, in de context van Data Lake Storage Gen2, kunnen verwijzen naar een Azure AD-gebruiker, service-principal of beveiligingsgroep.

### <a name="the-owning-user"></a>De gebruiker die eigenaar is

De gebruiker die het item heeft gemaakt, wordt automatisch de gebruiker die eigenaar is van het item. Een gebruiker die eigenaar is kan:

* De machtigingen wijzigen van een bestand waarvan hij eigenaar is.
* De groep die eigenaar van een bestand is wijzigen, zolang deze gebruiker ook lid is van de doelgroep.

> [!NOTE]
> De gebruiker die eigenaar is *kan geen* wijzigen van de gebruiker die eigenaar is van een bestand of map. Alleen supergebruikers kunnen de gebruiker die eigenaar is van een bestand of map wijzigen.

### <a name="the-owning-group"></a>De groep die eigenaar is

In de POSIX ACL's is elke gebruiker is gekoppeld aan een *hoofdgroep*. Gebruiker 'Els' kan bijvoorbeeld behoren tot de groep 'Financiën'. Els kan behoren tot meerdere groepen, maar één groep is altijd ingesteld als haar hoofdgroep. Wanneer Els in POSIX een bestand maakt, wordt de groep die eigenaar van het bestand is als haar hoofdgroep ingesteld. In dit geval is dit 'Financiën'. De groep die eigenaar is, gedraagt zich op dezelfde manier als toegewezen machtigingen voor andere gebruikers/groepen.

#### <a name="assigning-the-owning-group-for-a-new-file-or-directory"></a>De groep die eigenaar is van een nieuw bestand of map toewijzen

* **Geval 1**: De hoofdmap '/'. Deze map wordt gemaakt wanneer een bestandssysteem voor Data Lake Storage Gen2 wordt gemaakt. In dit geval de groep die eigenaar is ingesteld op de gebruiker die het bestandssysteem gemaakt als deze is uitgevoerd met behulp van OAuth. Als het bestandssysteem wordt gemaakt met behulp van de gedeelde sleutel, een Account-SAS of een SAS-Service, wordt de eigenaar en de groep die eigenaar is, zijn ingesteld op **$superuser**.
* **Geval 2** (alle andere gevallen): Wanneer een nieuw item wordt gemaakt, wordt de groep die eigenaar is van de bovenliggende map gekopieerd.

#### <a name="changing-the-owning-group"></a>De groep die eigenaar is wijzigen

De groep die eigenaar is kan worden gewijzigd door:
* Alle supergebruikers.
* De gebruiker die eigenaar is, als deze gebruiker ook lid is van de doelgroep.

> [!NOTE]
> De groep die eigenaar is kan de ACL's van een bestand of map niet wijzigen.  Terwijl de groep die eigenaar is ingesteld op de gebruiker die heeft gemaakt van het account in het geval van de hoofdmap **voorbeeld1** hierboven een gebruikersaccount is niet geldig voor het verstrekken van machtigingen via de groep die eigenaar is. U kunt deze machtiging toewijzen aan een geldige gebruikersgroep, indien van toepassing.

## <a name="access-check-algorithm"></a>Algoritme voor toegangscontrole

De volgende pseudocode vertegenwoordigt het algoritme voor toegangscontrole voor Data Lake Storage Gen2 accounts.

```
def access_check( user, desired_perms, path ) : 
  # access_check returns true if user has the desired permissions on the path, false otherwise
  # user is the identity that wants to perform an operation on path
  # desired_perms is a simple integer with values from 0 to 7 ( R=4, W=2, X=1). User desires these permissions
  # path is the file or directory
  # Note: the "sticky bit" is not illustrated in this algorithm
  
# Handle super users.
  if (is_superuser(user)) :
    return True

# Handle the owning user. Note that mask IS NOT used.
entry = get_acl_entry( path, OWNER )
if (user == entry.identity)
    return ( (desired_perms & entry.permissions) == desired_perms )

# Handle the named users. Note that mask IS used.
entries = get_acl_entries( path, NAMED_USER )
for entry in entries:
    if (user == entry.identity ) :
        mask = get_mask( path )
        return ( (desired_perms & entry.permissions & mask) == desired_perms)

# Handle named groups and owning group
member_count = 0
perms = 0
entries = get_acl_entries( path, NAMED_GROUP | OWNING_GROUP )
for entry in entries:
if (user_is_member_of_group(user, entry.identity)) :
    member_count += 1
    perms | =  entry.permissions
if (member_count>0) :
return ((desired_perms & perms & mask ) == desired_perms)

# Handle other
perms = get_perms_for_other(path)
mask = get_mask( path )
return ( (desired_perms & perms & mask ) == desired_perms)
```

### <a name="the-mask"></a>Het masker

Zoals wordt geïllustreerd in de algoritme voor het controleren van toegang, wordt in het masker hebben voor benoemde gebruikers, de groep die eigenaar is en de benoemde groepen beperkt.  

> [!NOTE]
> Voor een nieuwe Data Lake Storage Gen2 file system, is het masker voor toegang tot de ACL van de hoofdmap ('/') standaard ingesteld op 750 voor mappen en 640 voor bestanden. Bestanden wordt de X-bit niet weergegeven omdat deze niet relevant zijn voor de bestanden zich in een systeem met alleen-store.
>
> Het masker kan worden opgegeven op basis van per aanroep. Hiermee kunt verschillende systemen in beslag nemen, zoals clusters hebben verschillende effectieve maskers voor hun bestandsbewerkingen. Als een masker voor een bepaalde aanvraag is opgegeven, wordt volledig het masker standaard overschreven.

### <a name="the-sticky-bit"></a>De vergrendelde bit

De vergrendelde bit is een geavanceerdere functie van een POSIX-bestandssysteem. In de context van Data Lake Storage Gen2 is het onwaarschijnlijk dat de vergrendelde bit nodig is. Kortom, als de vergrendelde bit is ingeschakeld op een map, kan een onderliggend item alleen worden verwijderd of gewijzigd door de gebruiker die eigenaar is van het onderliggende item.

De vergrendelde bit wordt niet weergegeven in Azure Portal.

## <a name="default-permissions-on-new-files-and-directories"></a>Standaardmachtigingen voor nieuwe bestanden en mappen

Wanneer een nieuw bestand of map wordt gemaakt onder een bestaande map, de standaard-ACL voor de bovenliggende map bepaalt:

- Standaard-ACL en toegang ACL van een onderliggende map.
- Een onderliggend bestand toegang krijgen tot ACL (bestanden beschikken niet over een standaard-ACL).

### <a name="umask"></a>umask

Bij het maken van een bestand of map, wordt umask wordt gebruikt om te wijzigen hoe de standaard-ACL's zijn ingesteld op het onderliggende item. umask is een 9-bits waarde van de bovenliggende mappen waarin een LSU-waarde voor **gebruiker die eigenaar is**, **groep die eigenaar is**, en **andere**.

De umask voor Azure Data Lake Storage Gen2 een constante waarde die is ingesteld op 007. Deze waarde wordt omgezet in het:

| umask onderdeel     | Numerieke vorm | Verkorte vorm | Betekenis |
|---------------------|--------------|------------|---------|
| umask.owning_user   |    0         |   `---`      | Voor de gebruiker die eigenaar is, kopieert u de bovenliggende standaard-ACL van het kind toegang ACL | 
| umask.owning_group  |    0         |   `---`      | Voor de groep die eigenaar is, kopieert u de bovenliggende standaard-ACL van het kind toegang ACL | 
| umask.Other         |    7         |   `RWX`      | Voor andere, verwijdert u alle machtigingen voor het kind toegang ACL |

De waarde van de umask effectief gebruikt door Azure Data Lake Storage Gen2 betekent dat de waarde voor **andere** nooit standaard op de nieuwe onderliggende items, ongeacht wat de standaard ACL geeft aan dat wordt verzonden. 

De volgende pseudocode laat zien hoe de umask wordt toegepast bij het maken van de ACL's voor een onderliggend item.

```
def set_default_acls_for_new_child(parent, child):
    child.acls = []
    for entry in parent.acls :
        new_entry = None
        if (entry.type == OWNING_USER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_user))
        elif (entry.type == OWNING_GROUP) :
            new_entry = entry.clone(perms = entry.perms & (~umask.owning_group))
        elif (entry.type == OTHER) :
            new_entry = entry.clone(perms = entry.perms & (~umask.other))
        else :
            new_entry = entry.clone(perms = entry.perms )
        child_acls.add( new_entry )
```

## <a name="common-questions-about-acls-in-data-lake-storage-gen2"></a>Veelgestelde vragen over ACL's in Data Lake Storage Gen2

### <a name="do-i-have-to-enable-support-for-acls"></a>Moet ik ondersteuning voor ACL's inschakelen?

Nee. Toegangsbeheer via de ACL's is ingeschakeld voor een Data Lake Storage Gen2-account, zolang de hiërarchische Namespace (HNS) functie is ingeschakeld.

Als HNS is uitgeschakeld, de Azure RBAC-autorisatieregels nog steeds van toepassing.

### <a name="what-is-the-best-way-to-apply-acls"></a>Wat is de beste manier om de ACL's toepassen?

Gebruik altijd de Azure AD-beveiligingsgroepen als de toegewezen principal in de ACL's. De mogelijkheid om afzonderlijke gebruikers of service-principals rechtstreeks toewijzen verzetten. Met behulp van deze structuur kunt u gebruikers of service-principals zonder de noodzaak om toe te passen van ACL's voor een volledige mapstructuur toevoegen en verwijderen. ) In plaats daarvan, u gewoon wilt toevoegen of verwijderen van de juiste Azure AD-beveiligingsgroep. Houd er rekening mee dat ACL's niet overgenomen worden en opnieuw toepassen van ACL's is vereist voor het bijwerken van de ACL voor elk bestand en de submap. 

### <a name="which-permissions-are-required-to-recursively-delete-a-directory-and-its-contents"></a>Welke machtigingen zijn vereist voor het recursief verwijderen van een directory en de inhoud ervan?

- De oproepende functie supergebruikersmachtigingen '',

of

- De bovenliggende map moet schrijven + uitvoeren van machtigingen.
- De map moet worden verwijderd en elke map erin moeten lezen + schrijven + uitvoeren van machtigingen.

> [!NOTE]
> U doet geen schrijfmachtigingen nodig om te verwijderen van bestanden in mappen. Ook de hoofdmap '/' kan niet worden verwijderd.

### <a name="who-is-the-owner-of-a-file-or-directory"></a>Wie is de eigenaar van een bestand of map?

De maker van een bestand of map wordt de eigenaar. Dit is de identiteit van de gebruiker die het bestandssysteem gemaakt in het geval van de hoofdmap.

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-directory-at-creation"></a>Welke groep wordt ingesteld als de groep die eigenaar is van een bestand of map bij het maken ervan?

De groep die eigenaar is gekopieerd uit de groep die eigenaar is van de bovenliggende map waaronder het nieuwe bestand of map wordt gemaakt.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Ik ben de gebruiker die eigenaar is van een bestand, maar ik heb de LSU-machtigingen die ik nodig heb niet. Wat moet ik doen?

De gebruiker die eigenaar is kan de machtigingen van het bestand wijzigen en zichzelf de vereiste LSU-machtigingen geven.

### <a name="why-do-i-sometimes-see-guids-in-acls"></a>Waarom zie ik soms GUID's in de ACL's?

Er wordt een GUID weergegeven als de betekenis van een gebruiker en dat de gebruiker niet in meer Azure AD bestaat. Dit gebeurt doorgaans wanneer gebruikers het bedrijf verlaten of hun accounts zijn verwijderd in Azure AD. Bovendien service-principals en beveiligingsgroepen beschikt niet over een UPN (User Principal Name) om ze te identificeren en zodat ze worden vertegenwoordigd door de OID-kenmerk (guid).

### <a name="how-do-i-set-acls-correctly-for-a-service-principal"></a>Hoe stel ik ACL's goed voor een service principal?

Bij het definiëren van ACL's voor service-principals, is het belangrijk dat u de Object-ID (OID) van de *service-principal* voor de appregistratie van de die u hebt gemaakt. Het is belangrijk te weten dat geregistreerde apps een afzonderlijke service-principal in de specifieke hebben Azure AD-tenant. Geregistreerde apps hebben een OID die zichtbaar is in de Azure-portal, maar de *service-principal* heeft een andere (verschillende) OID.

Als u de OID voor de service-principal die corresonds aan een app-registratie, kunt u de `az ad sp show` opdracht. Geef de toepassings-ID als de parameter. Hier volgt een voorbeeld over het verkrijgen van de OID voor de service-principal die overeenkomt met de appregistratie van een met App-Id = 18218b12-1895-43e9-ad80-6e8fc1ea88ce. Voer de volgende opdracht uit in de Azure CLI:

`az ad sp show --id 18218b12-1895-43e9-ad80-6e8fc1ea88ce --query objectId
<<OID will be displayed>>`

Als u de juiste OID voor de service-principal hebt, gaat u naar de Storage Explorer **Manage Access** pagina de OID toevoegen en toewijzen van de juiste machtigingen voor de OID. Zorg ervoor dat u selecteert **opslaan**.

### <a name="does-data-lake-storage-gen2-support-inheritance-of-acls"></a>Data Lake Storage Gen2 biedt ondersteuning voor overname van ACL's?

Azure RBAC-toewijzingen nemen. De stroom van de toewijzingen van het abonnement, resourcegroep en de resources van opslagaccount omlaag naar de bron van het systeem bestand.

ACL's worden niet overgenomen. Standaard ACL's kan echter worden gebruikt om in te stellen van ACL's voor onderliggende submappen en bestanden die zijn gemaakt op basis van de bovenliggende map. 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Waar kan ik meer informatie over het POSIX-model voor toegangsbeheer?

* [POSIX met behulp van toegangsbeheerlijsten op Linux](https://www.linux.com/news/posix-acls-linux)
* [Handleiding voor HDFS-machtigingen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)
* [Veelgestelde vragen over POSIX](https://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](https://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1 2013](https://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)
* [POSIX 1003.1 2016](https://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)
* [POSIX ACL in Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs)
* [ACL met behulp van toegangsbeheerlijsten op Linux](https://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Zie ook

* [Overzicht van Azure Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md)
