---
title: Beveiligde LDAP (LDAPS) in Azure AD Domain Services configureren | Microsoft Docs
description: Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 5d46f376d46b8bbf3f93de57a7d4e31abdbcdb2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Beveiligde LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services configureren

## <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u hebt voltooid [taak 1: een certificaat verkrijgen voor beveiligde LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Taak 2: het beveiligde LDAP-certificaat te exporteren een. PFX-bestand
Voordat u deze taak, zorg ervoor dat u het beveiligde LDAP-certificaat hebt verkregen van een openbare certificeringsinstantie (CA) of een zelfondertekend certificaat hebt gemaakt.

De volgende stappen uitvoeren, om te exporteren van het certificaat LDAPS naar een. PFX-bestand.

1. Druk op de **Start** knop en het type **R**. In de **uitvoeren** dialoogvenster, type **mmc** en klik op **OK**.

    ![De MMC-console starten](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Op de **User Account Control** gevraagd, klikt u op **Ja** MMC (Microsoft Management Console) als administrator te starten.
3. Van de **bestand** menu, klikt u op **module toevoegen/verwijderen...** .

    ![Module toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. In de **toevoegen of verwijderen van modules** dialoogvenster, selecteer de **certificaten** -module en klikt u op de **toevoegen >** knop.

    ![Module Certificaten toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. In de **-module Certificaten** wizard selecteert u **computeraccount** en klik op **volgende**.

    ![Module Certificaten voor computeraccount toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Op de **Computer selecteren** pagina **lokale computer: (de computer waarop deze console wordt uitgevoerd)** en klik op **voltooien**.

    ![Module Certificaten - Selecteer computer toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. In de **toevoegen of verwijderen van modules** dialoogvenster, klikt u op **OK** de module Certificaten toevoegen aan MMC.

    ![Module Certificaten toevoegen aan MMC - gedaan](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Klik in het venster MMC om uit te breiden **consolebasis**. U ziet de module Certificaten geladen. Klik op **certificaten (lokale Computer)** uit te breiden. Vouw de **persoonlijke** knooppunt, gevolgd door de **certificaten** knooppunt.

    ![Archief met persoonlijke certificaten openen](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. U ziet het zelfondertekend certificaat gemaakt. Bekijk de eigenschappen van het certificaat om ervoor te zorgen die op de windows PowerShell gerapporteerd tijdens het maken van het certificaat komt overeen met de vingerafdruk.
10. Selecteer het zelfondertekend certificaat en **met de rechtermuisknop op**. Selecteer in het snelmenu **alle taken** en selecteer **exporteren...** .

    ![Certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. In de **Wizard Certificaat exporteren**, klikt u op **volgende**.

    ![Certificaatwizard exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Op de **persoonlijke sleutel exporteren** pagina **Ja, de persoonlijke sleutel exporteren**, en klik op **volgende**.

    ![De persoonlijke certificaatsleutel exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > De persoonlijke sleutel samen met het certificaat, moet u exporteren. Als u een PFX die de persoonlijke sleutel voor het certificaat niet bevat, mislukt beveiligde LDAP voor uw beheerde domein inschakelen.
    >
    >
13. Op de **bestandsindeling voor Export** pagina **Personal Information Exchange - PKCS #12 (. (PFX)** als de bestandsindeling voor het geëxporteerde certificaat.

    ![Certificaat-bestandsindeling voor export](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Alleen de. PFX-indeling wordt ondersteund. Het certificaat niet exporteren de. CER-bestandsindeling.
    >
    >
14. Op de **beveiliging** pagina de **wachtwoord** optie en typ een wachtwoord beveiligen de. PFX-bestand. Onthoud dit wachtwoord omdat in de volgende taak vereist wel. Klik op **volgende** om door te gaan.

    ![Wachtwoord voor het exporteren van certificaat ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Noteer dit wachtwoord. U moet deze tijdens het inschakelen van beveiligde LDAP voor dit beheerde domein in [taak 3 - beveiligde LDAP voor het beheerde domein inschakelen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >
15. Op de **te exporteren bestand** pagina, geef de bestandsnaam en de locatie waar u wilt het certificaat te exporteren.

    ![Pad voor het exporteren van certificaat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Klik op de volgende pagina **voltooien** het certificaat te exporteren naar een PFX-bestand. U ziet bevestigingsvenster wanneer het certificaat is geëxporteerd.

    ![Gedaan certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Volgende stap
[Taak 3: beveiligde LDAP voor het beheerde domein inschakelen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
