---
title: Aanbevolen procedures voor het maken van de evaluatie met Azure Migrate-Server-evaluatie | Microsoft Docs
description: Tips biedt voor het maken van evaluaties met Azure Migrate-evaluatie Server.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: d417efd4abf14247af171ea77b479f590e14fe76
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812962"
---
# <a name="best-practices-for-creating-assessments"></a>Aanbevolen procedures voor het maken van evaluaties

[Azure Migrate](migrate-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 

In dit artikel bevat een overzicht van aanbevolen procedures bij het maken van evaluaties met het hulpprogramma Azure Migrate-Server-evaluatie. 

## <a name="about-assessments"></a>Over evaluaties

Evaluaties die u met Azure Migrate-Server-evaluatie maakt zijn een point-in-time-momentopname van gegevens. Er zijn twee soorten evaluaties in Azure Migrate.

**Evaluatie-type** | **Details** | **Gegevens**
--- | --- | ---
**Performance-based** | Beoordelingen van die op basis van verzamelde prestatiegegevens aanbevelingen | De aanbeveling voor VM-grootte is gebaseerd op CPU en geheugen gegevens over het gebruik.<br/><br/> Schijf type aanbeveling (standard of premium beheerde schijven) is gebaseerd op de IOPS en doorvoer van de on-premises-schijven.
**As-is on-premises** | Beoordelingen van die prestatiegegevens niet gebruiken om aanbevelingen te doen. | De aanbeveling voor VM-grootte is gebaseerd op de on-premises VM-grootte<br/><br> Het aanbevolen schijftype is gebaseerd op wat u in het opslagtype voor de evaluatie selecteren.

### <a name="example"></a>Voorbeeld
Een voorbeeld: als u een on-premises VM met vier kernen op 20% gebruik, en het geheugen van 8 GB met 10% gebruik hebt, evaluaties worden als volgt:

- **Beoordeling op basis van prestaties**:
    - Hiermee wordt aanbevolen kernen en geheugen op basis van core (0,8 kernen) en geheugengebruik (0,8 GB).
    - De evaluatie van toepassing is een standaard-comfortfactor van 30%.
    - VM-aanbeveling: ~1.4 kernen (0,8 x1.3) en ~1.4 GB geheugen.
- **As-is (on-premises) beoordeling**:
    -  Raadt aan om een virtuele machine met vier kernen; 8 GB geheugen.

## <a name="best-practices-for-creating-assessments"></a>Aanbevolen procedures voor het maken van evaluaties

Het apparaat Azure Migrate continu profielen van uw on-premises-omgeving, en stuurt metagegevens-en prestatiegegevens naar Azure. Volg deze aanbevolen procedures voor het maken van beoordelingen:

- **Maken-evaluaties is**: U kunt maken als-evaluaties is onmiddellijk na de detectie.
- **Op basis van prestaties evaluatie maken**: Na het instellen van detectie, raden we u wacht ten minste een dag voor het uitvoeren van een beoordeling op basis van prestaties:
    - Verzamelen van prestatiegegevens kost tijd. Ten minste een dag wachten zorgt ervoor dat er onvoldoende gegevenspunten prestaties zijn voordat u de evaluatie uitvoert.
    - Voor prestatiegegevens, het apparaat verzamelt realtime gegevenspunten elke 20 seconden voor elke metrische gegevens voor prestaties en wordt deze tot een enkel gegevenspunt van vijf minuten. Het apparaat verzendt de gegevens van de vijf minuten durende wijst naar Azure per uur voor de evaluatie van de berekening.  
- **Ophalen van de meest recente gegevens**: Evaluaties worden niet automatisch bijgewerkt met de meest recente gegevens. Als u wilt een evaluatie van updates met de meest recente gegevens, moet u deze opnieuw uit te voeren. 
- **Zorg ervoor dat de duur overeenkomen met**: Wanneer u evaluaties op basis van prestaties uitvoert, zorg ervoor dat uw profiel uw omgeving voor de duur van de evaluatie. Bijvoorbeeld, als u een evaluatie met een duur van de prestaties van ingesteld op één week maken, moet u wachten tot ten minste een week na het starten van detectie, voor alle gegevenspunten moeten worden verzameld. Als u dit niet doet, krijgen de evaluatie geen vijf sterren. 
- **Voorkomen dat ontbrekende gegevenspunten**: De volgende problemen kunnen leiden tot ontbrekende gegevenspunten in een beoordeling op basis van prestaties:
    - Virtuele machines worden uitgeschakeld tijdens de evaluatie en prestatiegegevens niet verzameld. 
    - Als u virtuele machines in de maand waarop u de geschiedenis van geheugenprestaties baseren maken. de gegevens voor deze virtuele machines is minder dan een maand. 
    - De evaluatie onmiddellijk na de detectie is gemaakt, of de evaluatie van tijd komt niet overeen met de prestaties van gegevens verzameling van tijden.

## <a name="best-practices-for-confidence-ratings"></a>Aanbevolen procedures voor het vertrouwen van classificaties

Wanneer u evaluaties op basis van prestaties uitvoert, wordt een score van 1 ster (laagste) tot 5 sterren (hoogste) vertrouwen toegekend aan de evaluatie. Gebruik van vertrouwen classificaties effectief:
- Azure Migrate-Server-evaluatie nodig de gebruiksgegevens voor de virtuele machine CPU/geheugen en de schijf-IOPS/doorvoer gegevens.
- Voor elke netwerkadapter die is gekoppeld aan een virtuele machine, moet Azure Migrate het netwerk in/uit-gegevens.
- Als er geen gebruiksgegevens beschikbaar zijn in vCenter Server, Azure Migrate doet aanbeveling voor de grootte mogelijk niet betrouwbaar. 

Afhankelijk van het percentage beschikbare gegevenspunten, worden de classificaties vertrouwen voor een evaluatie in de volgende tabel samengevat.

   **Beschikbaarheid van gegevenspunt** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

- Als u een betrouwbaarheidsclassificatie van een beoordeling die lager is dan vijf sterren krijgt, wacht u ten minste een dag en vervolgens de evaluatie opnieuw te berekenen.
- Een lage score betekent dat formaat aanbevelingen mogelijk niet betrouwbaar. In dit geval wordt aangeraden dat u de evaluatie-eigenschappen om te gebruiken als wijzigen-evaluatie van de on-premises is.

## <a name="common-assessment-issues"></a>Algemene evaluatieproblemen

Hier volgt het aanpakken van enkele veelvoorkomende omgevingsproblemen die invloed hebben op evaluaties.

###  <a name="out-of-sync-assessments"></a>Out-van-sync-evaluaties

Als u toevoegen of verwijderen van computers uit een groep, nadat u een evaluatie maken, de beoordeling die u hebt gemaakt, worden gemarkeerd **out-van-sync**. De evaluatie opnieuw uitvoeren (**opnieuw berekenen**) om de wijzigingen weer te geven.

### <a name="outdated-assessments"></a>Verouderde evaluaties

Als er wijzigingen van on-premises VM's in een groep die wordt geëvalueerd, de evaluatie is gemarkeerd als **verouderde**. Als u wilt de wijzigingen worden weergegeven, de evaluatie opnieuw worden uitgevoerd.

### <a name="missing-data-points"></a>Ontbrekende gegevenspunten

Een evaluatie kan niet alle gegevenspunten voor een aantal redenen hebben:

- Virtuele machines kunnen worden uitgeschakeld tijdens de evaluatie en prestatiegegevens niet verzameld. 
- Virtuele machines kunnen worden gemaakt tijdens de maand op welke prestaties geschiedenis is gebaseerd, dus van minder dan een maand in hun prestatiegegevens. 
- De evaluatie is onmiddellijk na de detectie gemaakt. Als u wilt verzamelen van prestatiegegevens voor een opgegeven hoeveelheid tijd, moet u wachten de opgegeven hoeveelheid tijd voordat u een evaluatie uitvoert. Als u beoordelen van prestatiegegevens voor een week wilt, moet u bijvoorbeeld een week na de detectie wachten. Als u dit niet de evaluatie van de ontvangen geen vijf sterren. 


## <a name="next-steps"></a>Volgende stappen

- [Informatie over](concepts-assessment-calculation.md) hoe evaluaties worden berekend.
- [Informatie over](how-to-modify-assessment.md) over het aanpassen van een evaluatie.
