---
title: Het beveiligen van toegang tot Azure Data Catalog | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe data catalog en de gegevensassets beveiligen.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 89346113c6231442beb1147c4c4fea524d03d909
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-secure-access-to-data-catalog-and-data-assets"></a>Het beveiligen van toegang tot de catalogus met gegevens en gegevens activa
> [!IMPORTANT]
> Deze functie is alleen beschikbaar in de standard-editie van Azure Data Catalog.

Azure Data Catalog kunt u opgeven wie toegang heeft tot de catalogus met gegevens en welke bewerkingen (registreren, aantekeningen toevoegen aan, eigenaar) ze kunnen uitvoeren op de metagegevens in de catalogus. 

## <a name="catalog-users-and-permissions"></a>: Catalogusgebruikers en machtigingen
Geef een gebruiker of groep de toegang tot een catalogus met gegevens en machtigingen instellen:

1. Op de [startpagina van uw data catalog](http://www.azuredatacatalog.com), klikt u op **instellingen** op de werkbalk.

    ![catalogus voor gegevens - instellingen](media/data-catalog-how-to-secure-catalog/data-catalog-settings.png)
2. Vouw in de instellingenpagina van de **Catalogusgebruikers** sectie.
    ![Gebruikers - catalogus toevoegen](media/data-catalog-how-to-secure-catalog/data-catalog-add-button.png)
3. Klik op **Add**.
4. Voer de volledig gekwalificeerde **gebruikersnaam** of naam van de **beveiligingsgroep** in de Azure Active Directory (AAD) die zijn gekoppeld aan de catalogus. Gebruik komma (', ') als scheidingsteken als u wilt toevoegen van meer dan één gebruiker of groep.
    ![Gebruikers van de catalogus - gebruikers of groepen](media/data-catalog-how-to-secure-catalog/data-catalog-users-groups.png)
5. Druk op **ENTER** of **tabblad** buiten het tekstvak. 
6.  Controleer of alle machtigingen (**aantekening**, **registreren**, en **eigenaar**) worden standaard toegewezen aan deze gebruikers of groepen. Dat wil zeggen, de gebruiker of groep kunt [registreren gegevensassets]( data-catalog-how-to-register.md), [aantekeningen toevoegen aan gegevensassets]( data-catalog-how-to-annotate.md), en [eigenaar worden van gegevensassets]( data-catalog-how-to-manage.md). 
    ![Gebruikers van de catalogus - standaardmachtigingen](media/data-catalog-how-to-secure-catalog/data-catalog-default-permissions.png)
7.  Een gebruiker of groep alleen de lezen om toegang te verlenen aan de catalogus, schakelt u de **aantekeningen toevoegen aan** optie voor die gebruiker of groep. Wanneer u doet dit, wordt de gebruiker of groep kan niet gegevensassets in de catalogus aantekeningen maar kunnen weergeven. 
8.  Voor het weigeren van een gebruiker of groep van het registreren van gegevensassets, schakelt u de **registreren** optie voor die gebruiker of groep.
9.  Voor het weigeren van een gebruiker van de eigenaar van een gegevensasset, schakelt u de **eigenaar** optie voor die gebruiker of groep. 
10. Als u wilt een gebruiker of groep uit de gebruikers van de catalogus verwijderen, klikt u op **x** voor de gebruiker of groep aan de onderkant van de lijst. 
    ![Gebruikers van catalogus - gebruiker verwijderen](media/data-catalog-how-to-secure-catalog/data-catalog-delete-user.png)

    > [!IMPORTANT]
    > Het is raadzaam dat u, beveiligingsgroepen toevoegt voor gebruikers in plaats van gebruikers toe te voegen rechtstreeks catalogus en machtigingen toe te wijzen. Vervolgens gebruikers toevoegen aan de beveiligingsgroepen die overeenkomen met hun rollen en de vereiste toegang tot de catalogus.

## <a name="special-considerations"></a>Speciale overwegingen

- De machtigingen worden toegewezen aan beveiligingsgroepen zijn additieve. Een gebruiker is, en wel in twee groepen. Één groep heeft aantekeningen toevoegen aan machtigingen en andere groep heeft geen hebben aantekeningen machtigingen. Gebruiker heeft vervolgens aantekeningen toevoegen aan machtigingen. 
- De machtigingen die expliciet zijn toegewezen aan een gebruiker overschrijven de machtigingen worden toegewezen aan de groepen waartoe de gebruiker behoort. In het vorige voorbeeld bijvoorbeeld u expliciet zijn toegevoegd catalogus van gebruikers en voer de gebruiker niet toewijzen aantekeningen toevoegen aan machtigingen. De gebruiker kan geen aantekeningen toevoegen aan gegevensassets ondanks dat de gebruiker is lid van een groep die beschikt over aantekeningen toevoegen aan machtigingen.

## <a name="next-steps"></a>Volgende stappen
- [Aan de slag met Azure Data Catalog](data-catalog-get-started.md)

