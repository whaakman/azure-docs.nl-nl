---
title: Over het beveiligen van toegang tot Azure Data Catalog
description: In dit artikel wordt uitgelegd hoe u beveiligde gegevenscatalogus en bijbehorende gegevensassets.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 9d80eddffaf376b6f6f0090dce74a4884b291531
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405526"
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Over het beveiligen van toegang tot de gegevenscatalogus en gegevensassets
> [!IMPORTANT]
> Deze functie is alleen beschikbaar in de standard-editie van Azure Data Catalog.

Azure Data Catalog kunt u opgeven wie toegang heeft tot de catalogus met gegevens en welke bewerkingen (registreren, annoteren, eigenaar) ze kunnen uitvoeren op metagegevens in de catalogus. 

## <a name="catalog-users-and-permissions"></a>Gebruikers van de catalogus en machtigingen
Een gebruiker of groep de toegang geven tot een gegevenscatalogus en machtigingen instellen:

1. Op de [startpagina van data catalog](http://www.azuredatacatalog.com), klikt u op **instellingen** op de werkbalk.

    ![Data catalog - instellingen](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Vouw in de instellingenpagina van de **Catalogusgebruikers** sectie.
    ![Gebruikers - catalogus toevoegen](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Klik op **Add**.
4. Voer de volledig gekwalificeerde **gebruikersnaam** of naam van de **beveiligingsgroep** in de Azure Active Directory (AAD) die zijn gekoppeld aan de catalogus. Gebruik een komma (', ') als scheidingsteken als u wilt toevoegen van meer dan één gebruiker of groep.
    ![Gebruikers van de catalogus - gebruikers of groepen](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Druk op **ENTER** of **tabblad** buiten het tekstvak. 
6.  Bevestig dat alle machtigingen (**aantekeningen**, **registreren**, en **eigenaar**) worden standaard toegewezen aan deze gebruikers of groepen. Dat wil zeggen, de gebruiker of groep kunt [gegevensassets registreren]( data-catalog-how-to-register.md), [aantekeningen toevoegen aan gegevensassets]( data-catalog-how-to-annotate.md), en [eigenaar worden van gegevensassets]( data-catalog-how-to-manage.md). 
    ![Gebruikers van de catalogus - standaardmachtigingen](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Een gebruiker of groep alleen de lezen om toegang te verlenen aan de catalogus, schakelt u de **aantekeningen toevoegen aan** optie voor de gebruiker of groep. Als u dit doet, wordt de gebruiker of groep kan niet gegevensassets in de catalogus aantekeningen maar kunnen weergeven. 
8.  Voor het weigeren van een gebruiker of groep van het registreren van gegevensassets, schakelt u de **registreren** optie voor de gebruiker of groep.
9.  Voor het weigeren van een gebruiker vanuit de eigenaar van een gegevensasset, schakelt u de **eigenaar** optie voor de gebruiker of groep. 
10. Als een gebruiker of groep van gebruikers van de catalogus verwijderen, klikt u op **x** voor de gebruiker/groep aan de onderkant van de lijst. 
    ![Gebruikers van catalogus - gebruiker verwijderen](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > We raden u aan beveiligingsgroepen voor het rechtstreeks catalogus van gebruikers in plaats van met toevoegen van gebruikers en machtigingen toewijzen. Vervolgens gebruikers toevoegen aan de beveiligingsgroepen die overeenkomen met hun rollen en de vereiste toegang tot de catalogus.

## <a name="special-considerations"></a>Speciale overwegingen

- De machtigingen worden toegewezen aan beveiligingsgroepen zijn additief. Een gebruiker zich bijvoorbeeld in twee groepen. Een groep heeft aantekeningen toevoegen aan machtigingen en andere groep komt niet hebben aantekeningen toevoegen aan machtigingen. Gebruiker heeft vervolgens aantekeningen toevoegen aan machtigingen. 
- De machtigingen die expliciet zijn toegewezen aan een gebruiker overschrijven de machtigingen worden toegewezen aan groepen waartoe de gebruiker behoort. In het vorige voorbeeld, Stel, u expliciet zijn toegevoegd catalogus van gebruikers en voer de gebruiker niet toewijzen aantekeningen toevoegen aan machtigingen. De gebruiker kan geen aantekeningen toevoegen aan gegevensassets zelfs als de gebruiker is lid van een groep die wel aantekeningen toevoegen aan machtigingen.

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Azure Data Catalog](data-catalog-get-started.md)

