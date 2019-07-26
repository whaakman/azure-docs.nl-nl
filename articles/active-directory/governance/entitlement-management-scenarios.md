---
title: Algemene scenario's in het beheer van rechten van Azure AD (preview)-Azure Active Directory
description: Meer informatie over de stappen op hoog niveau die u moet volgen voor algemene scenario's in Azure Active Directory rechten beheer (preview).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8e7709abcc97baac9bf657b9fff2110cb2e72c1
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489037"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Algemene scenario's in het beheer van rechten van Azure AD (preview-versie)

> [!IMPORTANT]
> Azure Active Directory (Azure AD)-rechts beheer is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Er zijn verschillende manieren waarop u het rechten beheer voor uw organisatie kunt configureren. Als u echter net aan de slag gaat, is het handig om de algemene scenario's voor beheerders, goed keurders en aanvragers te begrijpen.

## <a name="administrators"></a>Beheerders

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Ik ben geen recht op het beheer van rechten en ik wil graag aan de slag

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | [Volg de zelf studie voor het maken van uw eerste toegangs pakket](entitlement-management-access-package-first.md) | [![Azure Portal pictogram](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Ik wil dat gebruikers in mijn Directory toegang kunnen aanvragen tot groepen, toepassingen of share point-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Een nieuw toegangs pakket maken in een catalogus](entitlement-management-access-package-create.md#start-new-access-package) | ![Een toegangs pakket maken](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Resource rollen toevoegen voor toegang tot pakket](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Groepen</li><li>Toepassingen</li><li>SharePoint-sites</li></ul> | ![Resource rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Een beleid toevoegen](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Voor gebruikers in uw Directory</li><li>Goedkeuring vereisen</li><li>Instellingen voor het verloop</li></ul> | ![Beleid toevoegen](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Ik wil gebruikers van mijn zakelijke partners toestaan (met inbegrip van gebruikers die nog niet in mijn map zijn opgenomen) om toegang aan te vragen voor groepen, toepassingen of share point-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Een nieuw toegangs pakket maken in een catalogus](entitlement-management-access-package-create.md#start-new-access-package) | ![Een toegangs pakket maken](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Resource rollen toevoegen voor toegang tot pakket](entitlement-management-access-package-edit.md#add-resource-roles) | ![Resource rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Een beleid voor externe gebruikers toevoegen](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Voor gebruikers die niet in uw Directory voor komt</li><li>Goedkeuring vereisen</li><li>Instellingen voor het verloop</li></ul> | ![Beleid voor externe gebruikers toevoegen](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [De koppeling naar de portal van mijn Access verzenden om het toegangs pakket aan uw zakelijke partner aan te vragen](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>Zakelijke partner kan koppeling met hun gebruikers delen</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Ik wil de groepen, toepassingen of share point-sites in een toegangs pakket wijzigen

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** Open het toegangs pakket | ![Resource rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Resource rollen toevoegen of verwijderen](entitlement-management-access-package-edit.md#add-resource-roles) | ![Resource rollen toevoegen](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Ik wil weer geven wie een toewijzing heeft voor groepen, toepassingen of share point-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** Een toegangs pakket openen | ![Resource rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Toewijzingen weer geven](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Weer geven welke gebruikers toegang hebben tot een toegangs pakket</li><li>Weer gave van de toegang van de gebruiker is verlopen</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Ik wil groepen, toepassingen of share point-sites weer geven waartoe een gebruiker toegang heeft

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | [Rapport met gebruikers toewijzingen weer geven](entitlement-management-reports.md)<ul><li>Weer geven wanneer deze zijn aangevraagd en die zijn goedgekeurd</li></ul> |  |

## <a name="approvers"></a>Goedkeurders

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Ik wil aanvragen goed keuren voor toegang tot groepen, toepassingen of share point-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Aanvraag openen in mijn Access-Portal](entitlement-management-request-approve.md#open-request) | [![Pictogram mijn Access-Portal](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Toegangs aanvraag goed keuren](entitlement-management-request-approve.md#approve-or-deny-request) | ![Toegang goedkeuren](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>Aanvragers

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Ik wil de groepen, toepassingen of share point-sites weer geven die beschikbaar zijn voor mij en toegang aanvragen

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Meld u aan bij de portal van mijn toegang](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Pictogram mijn Access-Portal](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Toegangs pakket zoeken |  |
> | **3.** [Toegang aanvragen](entitlement-management-request-access.md#request-an-access-package) | ![Toegang aanvragen](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Ik ben een externe gebruiker en ik wil toegang aanvragen tot groepen, toepassingen of share point-sites met een directe koppeling

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [De koppeling naar de portal van mijn toegang zoeken die u hebt ontvangen](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [Meld u aan bij de portal van mijn toegang](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Pictogram mijn Access-Portal](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Toegang aanvragen](entitlement-management-request-access.md#request-an-access-package) | ![Toegang aanvragen voor externe gebruiker](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Ik wil de groepen, toepassingen of share point-sites weer geven waarvoor ik al toegang heb

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Meld u aan bij de portal van mijn toegang](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Pictogram mijn Access-Portal](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Actieve toegangs pakketten weer geven |  |

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Uw eerste toegangs pakket maken](entitlement-management-access-package-first.md)
- [Taken delegeren](entitlement-management-delegate.md)
