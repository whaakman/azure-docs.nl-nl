---
title: Exporteren en verwijderen van uw gegevens vanuit Machine Learning Studio - Azure | Microsoft Docs
description: In het product gegevens die zijn opgeslagen door Azure Machine Learning Studio is beschikbaar voor exporteren en verwijderen via de Azure-portal en via geverifieerde REST-API's. Telemetriegegevens zijn toegankelijk via de Azure-Privacy-Portal. In dit artikel ziet u hoe u.
services: machine-learning
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: 318fabf44d0a58cc0b3a9f166bc6048127038de3
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51820440"
---
# <a name="export-and-delete-in-product-user-data-from-machine-learning-studio"></a>Exporteren en verwijderen van gebruikersgegevens in het product van Machine Learning Studio

U kunt verwijderen of het exporteren van gegevens in het product die zijn opgeslagen door Azure Machine Learning Studio met behulp van Azure portal, de Studio-interface, PowerShell, en geverifieerde REST-API's. In dit artikel leest u hoe. 

Telemetriegegevens zijn toegankelijk via de Privacy van de Azure-portal. 

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-kinds-of-user-data-does-studio-collect"></a>Wat voor soort gebruikersgegevens verzamelen Studio?

Voor deze service bevat gebruikersgegevens informatie over gebruikers die zijn gemachtigd voor toegang tot werkruimten en -records van de telemetrie van de interactie van gebruikers met de service.

Er zijn twee soorten gebruikersgegevens in Machine Learning Studio:
- **Persoonlijke gegevens:** Account-id en e-mailadressen die zijn gekoppeld aan een account.
- **Gegevens van de klant:** u hebt geüpload voor het analyseren van gegevens.

## <a name="studio-account-types-and-how-data-is-stored"></a>Studio-accounttypen en hoe gegevens worden opgeslagen

Er zijn drie soorten accounts in Machine Learning Studio. Het soort account hebt, bepaalt hoe uw gegevens worden opgeslagen en hoe u kunt verwijderen of exporteren.

- Een **Gast werkruimte** is een gratis, anonieme-account. U kunt zich registreren zonder referenties, zoals een e-mailadres of wachtwoord op te geven.
    -  De gegevens is verwijderd nadat de Gast-werkruimte is verlopen.
    - Gastgebruikers kunnen gegevens van de klant via de gebruikersinterface, REST-API's of PowerShell-pakket exporteren.
- Een **gratis werkruimte** is een gratis account dat u zich aanmeldt bij met Microsoft-account referenties - e-mailadres en wachtwoord.
    - U kunt exporteren en verwijderen van persoonlijke en klant gegevens die onderworpen aan data subject rights (DSR) aanvragen zijn.
    - U kunt gegevens van de klant via de gebruikersinterface, REST-API's of PowerShell-pakket exporteren.
    - Gratis kan werkruimten niet met behulp van Azure AD-accounts, telemetrie worden geëxporteerd met behulp van de Privacy-Portal.
    - Als u de werkruimte verwijdert, verwijdert u alle persoonlijke klantgegevens.
- Een **standard-werkruimte** is een betaalde account dat u toegang hebt met de aanmeldingsreferenties.
    - U kunt exporteren en verwijderen van persoonlijke en klant gegevens die onderworpen aan DSR-aanvragen zijn.
    - U kunt toegang tot gegevens via de Privacy van de Azure-portal
    - U kunt persoonlijke en klant gegevens via de gebruikersinterface, REST-API's of PowerShell-pakket exporteren
    - U kunt uw gegevens in Azure portal verwijderen.

## <a name="delete-workspace-data-in-studio"></a>Verwijderen van gegevens in de werkruimte in Studio 

### <a name="delete-individual-assets"></a>Afzonderlijke assets verwijderen

Gebruikers kunnen activa in een werkruimte verwijderen door ze te selecteren en selecteer vervolgens de verwijderknop.

![Assets in Machine Learning Studio verwijderen](./media/export-delete-personal-data-dsr/delete-studio-asset.png)

### <a name="delete-an-entire-workspace"></a>Een volledige-werkruimte verwijderen

Gebruikers kunnen ook hun hele werkruimte verwijderen:
- Betaalde werkruimte: verwijderen via de Azure-portal.
- Gratis werkruimte: Gebruik de verwijderknop in de **instellingen** deelvenster.

![Verwijderen van een gratis werkruimte in Machine Learning Studio](./media/export-delete-personal-data-dsr/delete-studio-data-workspace.png)
 
## <a name="export-studio-data-with-powershell"></a>Studio-gegevens exporteren met PowerShell
PowerShell gebruiken voor al uw gegevens exporteren naar een indeling die vanuit Azure Machine Learning Studio en gebruik van opdrachten. Zie voor meer informatie, de [PowerShell-module voor Azure Machine Learning](powershell-module.md) artikel.

## <a name="next-steps"></a>Volgende stappen

Zie voor documentatie die betrekking hebben op webservices en toezeggingsabonnement voor facturering, [naslaginformatie over REST-API van Azure Machine Learning](https://docs.microsoft.com/rest/api/machinelearning/). 
