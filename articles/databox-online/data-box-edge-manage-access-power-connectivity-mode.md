---
title: Toegang tot Microsoft Azure Data Box Edge-apparaat, kracht en verbindingsmodus | Microsoft Docs
description: Hierin wordt beschreven hoe u voor het beheren van toegang, kracht en verbindingsmodus voor de Azure Data Box-Edge-apparaat dat helpt gegevens naar Azure overbrengen
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/21/2019
ms.author: alkohli
ms.openlocfilehash: e0b8b35c654f0716fae1e6ab785f57dcf04e1a5a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400888"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge-preview"></a>Beheren van toegang, kracht en verbindingsmodus voor uw Azure Data Box Edge (preview)

In dit artikel wordt beschreven hoe u de modus voor toegang, kracht en connectiviteit voor uw Azure Data Box-Edge beheren. Deze bewerkingen worden uitgevoerd via de lokale web-UI of de Azure-portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Toegang tot het apparaat beheren
> * Verbindingsmodus beheren
> * Energiebeheer

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert.

## <a name="manage-device-access"></a>Toegang tot het apparaat beheren

De toegang tot uw gegevens in het Edge-apparaat wordt bepaald door het gebruik van het wachtwoord van een apparaat. U kunt het wachtwoord via de lokale webgebruikersinterface wijzigen. U kunt ook het wachtwoord van het apparaat in Azure portal opnieuw.

### <a name="change-device-password"></a>Wachtwoord voor apparaat wijzigen

Volg deze stappen in de gebruikersinterface van de lokale wachtwoord van het apparaat te wijzigen.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > wachtwoordwijziging**.
2. Voer in het huidige wachtwoord en klik vervolgens op het nieuwe wachtwoord. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Controleer of het nieuwe wachtwoord.

    ![Wachtwoord wijzigen](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Selecteer **wachtwoord wijzigen**.
 
### <a name="reset-device-password"></a>Apparaatwachtwoord opnieuw instellen

De werkstroom opnieuw instellen is niet vereist voor de gebruiker aan de hand van het oude wachtwoord en is handig als u het wachtwoord is verloren gegaan. Deze werkstroom wordt uitgevoerd in Azure portal.

1. In de Azure-portal, gaat u naar **overzicht > beheerderswachtwoord opnieuw instellen van**.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Voer het nieuwe wachtwoord en bevestigt u dit. Het opgegeven wachtwoord moet tussen 8 en 16 tekens lang zijn. Het wachtwoord moet bestaan 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens. Selecteer **opnieuw**.

    ![Wachtwoord opnieuw instellen](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Verbindingsmodus beheren

Uw apparaat kunt naast de standaard volledig verbonden modus, ook uitvoeren in de modus gedeeltelijk verbonden of volledig niet-verbonden. Elk van deze modi wordt hieronder beschreven:

- **Volledig wordt verbonden** -dit is de normale standaardmodus waarin het apparaat werkt. De cloud uploaden en downloaden van gegevens is in deze modus ingeschakeld. U kunt de Azure portal of de lokale webgebruikersinterface voor het beheren van het apparaat.

- **Gedeeltelijk verbroken** : In deze modus wordt het apparaat kan niet uploaden of downloaden van een share die gegevens echter kunnen worden beheerd via de Azure-portal.

    In deze modus wordt doorgaans gebruikt wanneer u op een netwerk met een datalimiet satelliet en het doel is om het gebruik van netwerkbandbreedte te minimaliseren. Minimale netwerkverbruik nog steeds optreden voor het controleren van de bewerkingen van apparaten.

- **De verbinding verbroken** : In deze modus wordt het apparaat is volledig losgekoppeld van de cloud en zowel de cloud uploads en downloads zijn uitgeschakeld. Het apparaat kan alleen worden beheerd via de lokale webgebruikersinterface.

    In deze modus wordt doorgaans gebruikt wanneer u offline wilt halen uw apparaat.

Apparaatmodus, Ga als volgt te werk:

1. In de lokale webgebruikersinterface van uw apparaat, gaat u naar **Configuration > Cloudinstellingen**.
2. In de vervolgkeuzelijst, selecteer de modus die u wilt werken met het apparaat in. U kunt kiezen uit **volledig wordt verbonden**, **gedeeltelijk verbonden**, en **volledig losgekoppeld**. Inschakelen om uit te voeren van het apparaat in de gedeeltelijk niet-verbonden modus, **beheer van Azure portal**.

    ![Verbindingsmodus](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Energiebeheer

U kunt afsluiten of opnieuw opstarten van uw fysieke apparaat met behulp van de lokale webgebruikersinterface. We aanbevolen die u opnieuw start, neemt u even voordat de shares offline op de server en klik vervolgens op het apparaat. Deze actie wordt geminimaliseerd een mogelijkheid van beschadiging van gegevens.

1. Ga in de lokale webgebruikersinterface naar **onderhoud > energie-instellingen**.
2. Selecteer **afsluiten** of **opnieuw** , afhankelijk van wat u van plan bent om te doen.

    ![Energie-instellingen](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Wanneer u hierom wordt gevraagd om bevestiging, selecteert u **Ja** om door te gaan.

> [!NOTE]
> Als u het fysieke apparaat afsluit, moet u push-knop op het apparaat deze in te schakelen.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van bestandsshares](data-box-edge-manage-shares.md).