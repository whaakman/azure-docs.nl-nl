<properties
    pageTitle="De tenant-ID van de Key Vault wijzigen na een verplaatsing van een abonnement | Microsoft Azure"
    description="Meer informatie over het overschakelen op een ander tenant-ID voor een Key Vault nadat een abonnement is verplaatst naar een andere tenant"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>


# De tenant-ID van de Key Vault wijzigen na een verplaatsing van een abonnement
### V: Mijn abonnement is van tenant A verplaatst naar tenant B. Hoe wijzig ik de tenant-ID voor mijn bestaande Key Vault en stel ik de juiste ACL's in voor principals in tenant B?

Wanneer u een nieuwe Key Vault maakt in een abonnement, is het automatisch gekoppeld aan de tenant-ID van Azure Active Directory voor dit abonnement. Alle vermeldingen van het toegangsbeleid zijn ook gekoppeld aan deze tenant-ID. Wanneer u uw Azure-abonnement van tenant A naar tenant B verplaatst, hebben de principals (gebruikers en toepassingen) geen toegang tot de bestaande Key Vaults in tenant B. U kunt dit probleem oplossen door

- de tenant-ID die is gekoppeld aan alle bestaande Key Vaults in dit abonnement te wijzigen in tenant B
- alle bestaande vermeldingen van het toegangsbeleid te verwijderen
- nieuwe vermeldingen van het toegangsbeleid toe te voegen die zijn gekoppeld aan tenant B.

Als u bijvoorbeeld Kay Vault 'mijnvault' hebt in een abonnement dat is verplaatst van tenant A naar B, kunt u de tenant-ID voor deze Key Vault als volgt wijzigen en het oude toegangsbeleid verwijderen.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId $vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties $vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId $vault.Properties.AccessPolicies = @() Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Omdat deze Vault zich in tenant A bevond voor de wijziging, is de oorspronkelijke waarde **$vault. Properties.TenantId** tenant A en is **(Get-AzureRmContext). Tenant.TenantId** tenant B.

Nu uw Vault is gekoppeld aan de juiste tenant-ID en de oude vermeldingen van het toegangsbeleid zijn verwijderd, kunt u nieuwe vermeldingen van het toegangsbeleid instellen met [Set AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## Volgende stappen

- Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Key Vaults



<!--HONumber=Sep16_HO3-->


