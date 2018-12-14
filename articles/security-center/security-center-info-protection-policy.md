---
title: Aanpassen van de SQL information protection-beleid in Azure Security Center | Microsoft Docs
description: Informatie over het aanpassen van beleidsregels voor gegevensbeveiliging in Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 2ebf2bc7-232a-45c4-a06a-b3d32aaf2500
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: rkarlin
ms.openlocfilehash: b24e10abd7c0d4dd5a20f8f85c340de03dae696a
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343816"
---
# <a name="customize-the-sql-information-protection-policy-in-azure-security-center-preview"></a>Aanpassen van de SQL information protection-beleid in Azure Security Center (Preview)
 
Een SQL information protection-beleid kan worden gedefinieerd en die is afgestemd op uw gehele Azure-tenant, in Azure Security Center.

Beveiliging van gegevens is een geavanceerde beveiliging-mogelijkheid voor het detecteren, classificeren, labelen en beveiligen van gevoelige gegevens in uw Azure-gegevensbronnen. Detecteren en classificeren van uw meest gevoelige gegevens (business, financiën, gezondheidszorg, persoonlijke gegevens, enzovoort) kan een cruciale rol spelen in uw organisatie informatie protection-status. Deze kan dienen als infrastructuur voor:
- Helpen voldoen aan de privacystandaarden en wettelijke vereisten
- Verschillende security-scenario's, zoals bewaking (controle) en waarschuwingen voor afwijkende toegang tot gevoelige gegevens
- Toegang tot beheren en beperken van de beveiliging van gegevens met uiterst gevoelige gegevens worden opgeslagen
 
[SQL Information Protection](../sql-database/sql-database-data-discovery-and-classification.md) implementeert dit paradigma voor uw SQL-gegevensarchieven, die momenteel worden ondersteund voor Azure SQL Database. SQL Information Protection automatisch worden gedetecteerd en classificeert potentieel gevoelige gegevens, biedt een mechanisme labelen voor de gevoelige gegevens met de classificatie-kenmerken permanent tagging en biedt een gedetailleerde dashboard van de de status van de classificatie van de database. Bovendien berekent het resultaat gevoeligheid van de SQL-query's, zo instellen dat query's die gevoelige gegevens extraheren expliciet kunnen worden gecontroleerd en de gegevens kunnen worden beveiligd. Zie voor meer informatie over SQL Information Protection [Azure SQL Database-gegevensdetectie en classificatie](../sql-database/sql-database-data-discovery-and-classification.md).
 
Het Classificatiemechanisme voor is gebaseerd op twee primaire constructies die gezamenlijk de taxonomie classificatie - **Labels** en **informatietypen**.
- **Labels** – de kenmerken van de belangrijkste classificatie, gebruikt voor het definiëren van het vertrouwelijkheidsniveau van de gegevens die zijn opgeslagen in de kolom. 
- **Informatietypen** : meer granulatie in het type van de gegevens die zijn opgeslagen in de kolom bevat.
 
Information Protection wordt geleverd met een ingebouwde groep labels en gegevenstypen, die standaard worden gebruikt. Voor het aanpassen van deze, kunt u de information protection-beleid in Azure Security Center.
 
## <a name="customize-the-information-protection-policy"></a>Het informatiebeschermingsbeleid aanpassen
Voor het aanpassen van de information protection-beleid voor uw Azure-tenant, moet u beschikken over [administratorbevoegdheden voor de beheergroep van de tenant-hoofdmap](security-center-management-groups.md). 
 
1. Selecteer in het hoofdmenu van Security Center **beveiligingsbeleid**.
2. Kies **hiërarchische weergave (preview)**, en klik vervolgens onder **Tenant hoofdgroep**, klikt u op **instellingen bewerken**.
 
   ![Information protection-beleid configureren](./media/security-center-info-protection-policy/security-policy.png) 
 
3. Onder **beleidsonderdelen**, klikt u op **Information protection**. In de **instellingen van Information protection** pagina vindt u de huidige reeks labels. Dit zijn de belangrijkste classificatie-kenmerken die worden gebruikt voor het categoriseren van het vertrouwelijkheidsniveau van uw gegevens. Hier kunt u de **Information protection-labels** en **informatietypen** voor de tenant. 
 
### <a name="customizing-labels"></a>Labels aanpassen
 
1. U kunt bewerken of een bestaand label verwijderen of een nieuw label toevoegen. Als u wilt een bestaand label bewerken, selecteert u dat label en klik vervolgens op **configureren**, aan het begin of in het contextmenu aan de rechterkant. Als u wilt een nieuw label toevoegen, klikt u op **maken label** onder aan de tabel labels of in de bovenste menubalk.
2. In de **configureren gevoeligheid, label** scherm, u kunt maken of wijzigen van de labelnaam en de beschrijving. U kunt ook instellen of het label is niet actief of uitgeschakeld door het omschakelen van de **ingeschakeld** in of uit te schakelen. Ten slotte kunt u toevoegen of verwijderen van de gegevenstypen die zijn gekoppeld aan het label. Geen gegevens gevonden die overeenkomt met dat informatietype automatisch de bijbehorende gevoeligheid, label in de aanbevelingen voor classificatie bevat.
3. Klik op **OK**.
 
   ![Gevoeligheidslabel configureren](./media/security-center-info-protection-policy/config-sensitivity-label.png)
 
4. Labels worden weergegeven in de volgorde van oplopende gevoeligheid. Als u wilt de volgorde tussen de labels wijzigen, sleept u de labels die u wilt de volgorde in de tabel, of gebruik de **omhoog** en **omlaag** knoppen om de volgorde te wijzigen. 
 
    ![Information protection-beleid configureren](./media/security-center-info-protection-policy/move-up.png)
 
5. Klik op **opslaan** aan de bovenkant van het scherm wanneer u klaar bent.
 
 
## <a name="adding-and-customizing-information-types"></a>Toevoegen en informatietypen aanpassen
 
1. U kunt beheren en aanpassen van informatietypen door te klikken op **informatietypen beheren**.
2. Toevoegen van een nieuwe **informatietype**, selecteer **informatietype maken** in het bovenste menu. U kunt een naam en beschrijving, configureren en patroon zoekreeksen voor de **informatietype**. Patroon zoekreeksen kunt trefwoorden met jokertekens (met het teken '%'), die de automatische detectie-engine gebruikt om te identificeren van gevoelige gegevens in uw databases, op basis van metagegevens van de kolommen (optioneel).
 
    ![Information protection-beleid configureren](./media/security-center-info-protection-policy/info-types.png)
 
3. U kunt ook configureren met de ingebouwde **informatietypen** door toe te voegen extra patroon zoekreeksen, uitschakelen van enkele van de bestaande tekenreeksen, of door het veranderen van de beschrijving. U kunt ingebouwde niet verwijderen **informatietypen** of bewerken van hun namen. 
4. **Informatietypen** worden vermeld in de volgorde van oplopende volgorde van de detectie, wat betekent dat de hoger in de lijst met typen probeert het overeenkomt met de eerste. Als u wilt de volgorde tussen gegevenstypen wijzigen, sleept u de typen naar de juiste plaats in de tabel of gebruik de **omhoog** en **omlaag** knoppen om de volgorde te wijzigen. 
5. Klik op **OK** wanneer u klaar bent.
6. Nadat u het beheer van uw informatietypen voltooid, moet u de relevante typen koppelen aan de relevante labels, door te klikken op **configureren** voor een bepaald label en typen van de informatie toevoegen of verwijderen, waar nodig.
7. Klik op **opslaan** in het hoofdvenster **Labels** blade om toe te passen van al uw wijzigingen.
 
Nadat het beveiligingsbeleid van uw gegevens is volledig gedefinieerd en opgeslagen, wordt deze toepassing op de indeling van gegevens op alle Azure SQL-databases in uw tenant.
 
 
## <a name="next-steps"></a>Volgende stappen
 
In dit artikel hebt u geleerd over het definiëren van een SQL Information Protection-beleid in Azure Security Center. Zie voor meer informatie over het gebruik van SQL Information Protection om te classificeren en beveiligen van gevoelige gegevens in uw SQL-databases, [Azure SQL Database-gegevensdetectie en classificatie](../sql-database/sql-database-data-discovery-and-classification.md). 

Zie de volgende artikelen voor meer informatie over beleidsregels voor veiligheid en beveiliging van gegevens in Azure Security Center:
 
- [Overzicht van beveiligingsbeleid](security-center-policies-overview.md): Bekijk een overzicht van beveiligingsbeleid in Security Center
- [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md): Informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md): Meer informatie over hoe Security Center beheert en beschermt gegevens


