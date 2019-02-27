---
title: Maken van een certificaat voor secure LDAP voor een Azure AD Domain Services-domein beheren | Microsoft Docs
description: Maken van een certificaat voor secure LDAP voor een Azure AD Domain Services-domein beheren
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2017
ms.author: ergreenl
ms.openlocfilehash: 801ba3b84ba9956ca8d13916ac4d90060a7f4037
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883450"
---
# <a name="create-a-pfx-file-with-the-secure-ldap-ldaps-certificate-for-a-managed-domain"></a>Maak een. PFX-bestand met het certificaat voor secure LDAP (LDAPS) voor een beheerd domein

## <a name="before-you-begin"></a>Voordat u begint
Volledige [taak 1: een certificaat voor secure LDAP verkrijgen](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2-export-the-secure-ldap-certificate-to-a-pfx-file"></a>Taak 2: Exporteren van het certificaat voor secure LDAP naar een. PFX-bestand
Voordat u deze taak start, wordt het certificaat voor secure LDAP ophalen uit een openbare certificeringsinstantie (CA) of een zelfondertekend certificaat maken.

Voor het exporteren van het LDAPS-certificaat naar een. PFX-bestand:

1. Druk op de **Start** knop en het type **R**. In de **uitvoeren** dialoogvenster, type **mmc** en klikt u op **OK**.

    ![De MMC-console starten](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Op de **User Account Control** wordt gevraagd, klikt u op **Ja** MMC (Microsoft Management Console) als administrator te starten.
3. Uit de **bestand** menu, klikt u op **module toevoegen/verwijderen...** .

    ![Module toevoegen aan de MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. In de **toevoegen of verwijderen van modules** dialoogvenster, selecteer de **certificaten** -module en klikt u op de **toevoegen >** knop.

    ![Certificatenmodule toevoegen aan MMC-console](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. In de **-module Certificaten** wizard de optie **computeraccount** en klikt u op **volgende**.

    ![Certificatenmodule voor computeraccount toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Op de **Computer selecteren** pagina, selecteert u **lokale computer: (de computer waarop deze console wordt uitgevoerd)** en klikt u op **voltooien**.

    ![Certificatenmodule - Selecteer computer toevoegen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. In de **toevoegen of verwijderen van modules** dialoogvenster, klikt u op **OK** de certificatenmodule toevoegen aan MMC.

    ![Certificatenmodule toevoegen aan een MMC - gedaan](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. Klik in het venster MMC om uit te breiden **consolebasis**. U ziet de certificatenmodule geladen. Klik op **certificaten (lokale Computer)** om uit te vouwen. Vouw de **persoonlijke** knooppunt, gevolgd door de **certificaten** knooppunt.

    ![Archief met persoonlijke certificaten openen](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Hier ziet u de gemaakte zelf-ondertekend certificaat. U kunt de eigenschappen van het certificaat om te controleren of de vingerafdruk van het overeenkomt met die in de windows PowerShell wordt gerapporteerd tijdens het maken van het certificaat.
10. Selecteer het zelfondertekend certificaat en **Klik met de rechtermuisknop**. Selecteer in het menu met de rechtermuisknop op **alle taken** en selecteer **exporteren...** .

    ![Certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. In de **Wizard Certificaat exporteren**, klikt u op **volgende**.

    ![Certificaatwizard exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Op de **persoonlijke sleutel exporteren** weergeeft, schakelt **Ja, de persoonlijke sleutel exporteren**, en klikt u op **volgende**.

    ![De persoonlijke certificaatsleutel exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > U moet de persoonlijke sleutel, samen met het certificaat exporteren. Als u een PFX die niet de persoonlijke sleutel voor het certificaat bevat, inschakelen van secure LDAP voor uw beheerde domein is mislukt.
    >
    >

13. Op de **bestandsindeling voor Export** weergeeft, schakelt **Personal Information Exchange - PKCS #12 (. (PFX)** als de bestandsindeling van het geëxporteerde certificaat.

    ![Certificaat-bestandsindeling voor export](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Alleen de. PFX-indeling wordt ondersteund. Het certificaat niet exporteren de. CER-bestandsindeling.
    >
    >

14. Op de **Security** weergeeft, schakelt de **wachtwoord** optie en typ een wachtwoord te beschermen de. PFX-bestand. Onthoud dit wachtwoord omdat deze is vereist in de volgende taak. Klik op **volgende**.

    ![Wachtwoord voor het certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Noteer dit wachtwoord. U hebt deze nodig tijdens het inschakelen van secure LDAP voor dit beheerde domein in [taak 3: secure LDAP inschakelen voor het beheerde domein](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >

15. Op de **te exporteren bestand** pagina, geef de bestandsnaam en de locatie waar u wilt het certificaat te exporteren.

    ![Pad voor het certificaat exporteren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Klik op de volgende pagina **voltooien** het certificaat te exporteren naar een PFX-bestand. U ziet bevestigingsvenster wanneer het certificaat is geëxporteerd.

    ![Exporteren van certificaat gereed](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Volgende stap
[Taak 3: secure LDAP inschakelen voor het beheerde domein](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
