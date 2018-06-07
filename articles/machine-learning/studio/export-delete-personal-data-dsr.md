---
title: Exporteren en verwijderen van uw gegevens van Machine Learning Studio - Azure | Microsoft Docs
description: Product-gegevens die zijn opgeslagen door Azure Machine Learning Studio is beschikbaar voor exporteren en verwijderen via de Azure portal en via geverifieerde REST-API's. Telemetriegegevens zijn toegankelijk via de Azure-Portal Privacy. Dit artikel ziet u hoe.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 6317d4baba5775c1e5a4fda66de80dd8299d8fed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654979"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Exporteren en in het product gebruikersgegevens verwijderen uit de Machine Learning Studio

U kunt verwijderen of het exporteren van gegevens binnen het product opgeslagen door Azure Machine Learning Studio met behulp van de Azure-portal, de interface Studio, PowerShell en geverifieerde REST-API's. In dit artikel leest u hoe. 

Telemetriegegevens zijn toegankelijk via de Privacy van de Azure-portal. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Welke soorten gebruikersgegevens verzamelen Studio?

Voor deze service bevat gebruikersgegevens informatie over gebruikers die toegang hebben tot de werkruimten en telemetrie records van de interactie van gebruikers met de service.

Er zijn twee soorten gebruikersgegevens in Machine Learning Studio:
- **Persoonlijke accountgegevens:** Account-id en e-mailadressen die zijn gekoppeld aan een account.
- **Klantgegevens:** gegevens die u hebt geüpload voor het analyseren.

## <a name="studio-account-types-and-how-data-is-stored"></a>Studio-accounttypen en hoe gegevens worden opgeslagen

Er zijn drie soorten accounts in Machine Learning Studio. Het soort account u bepaalt hoe de gegevens worden opgeslagen en hoe u kunt verwijderen of exporteren.

- Een **Gast werkruimte** is een gratis account anonieme. U kunt zich registreren zonder referenties opgeeft, zoals een e-mailadres of wachtwoord.
    -  Gegevens worden verwijderd nadat de Gast-werkruimte is verlopen.
    - Gastgebruikers kunnen klantgegevens via de gebruikersinterface, REST-API's of PowerShell pakket exporteren.
- Een **gratis werkruimte** is een gratis account dat u zich aanmeldt bij met Microsoft accountreferenties - e-mailadres en wachtwoord.
    - U kunt exporteren en verwijderen van gegevens op persoonlijke en de klant, die onderworpen aan gegevensaanvragen onderwerp rechten (DSR zijn).
    - U kunt gegevens van de klant via de gebruikersinterface, REST-API's of PowerShell pakket exporteren.
    - Gratis kan werkruimten niet met behulp van Azure AD-accounts, telemetrie worden geëxporteerd met de Privacy-Portal.
    - Wanneer u de werkruimte verwijdert, kunt u alle persoonlijke klantgegevens verwijderen.
- Een **standaard werkruimte** is een betaald account dat u toegang hebt met aanmeldingsreferenties.
    - U kunt exporteren en verwijderen van gegevens op persoonlijke en de klant, die onderworpen aan DSR aanvragen zijn.
    - U kunt toegang tot gegevens via de Privacy van de Azure-portal
    - U kunt persoonlijke en klant gegevens via de gebruikersinterface, REST-API's of PowerShell pakket exporteren
    - U kunt uw gegevens in de Azure portal kunt verwijderen.

## <a name="delete-workspace-data-in-studio"></a>Verwijderen van gegevens in de werkruimte in Studio 

### <a name="delete-individual-assets"></a>Verwijderen van afzonderlijke elementen

Gebruikers kunnen activa in een werkruimte verwijderen door ze te selecteren en vervolgens te klikken op de knop verwijderen.

![Verwijderen van de activa in Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Een volledige werkruimte verwijderen

Gebruikers kunnen ook hun volledige werkruimte verwijderen:
- Werkruimte betaald: verwijderen via de Azure portal.
- Gratis werkruimte: Gebruik de knop verwijderen in de **instellingen** deelvenster.

![Verwijderen van een gratis werkruimte in Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>Studio-gegevens exporteren met PowerShell
Gebruik PowerShell al uw gegevens exporteren naar een draagbaar indeling van Azure Machine Learning Studio met opdrachten. Zie voor meer informatie, de [PowerShell-module voor Azure Machine Learning](powershell-module.md) artikel.

## <a name="next-steps"></a>Volgende stappen

Zie voor documentatie die betrekking hebben op web-services en streven plan facturering [naslaginformatie over REST API van Azure Machine Learning](https://docs.microsoft.com/en-us/rest/api/machinelearning/). 
