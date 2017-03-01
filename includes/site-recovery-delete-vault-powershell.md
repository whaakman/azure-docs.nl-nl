## <a name="delete-a-recovery-services-vault-powershell"></a>Een Recovery Services-kluis verwijderen (PowerShell)

1. De Recovery Services-kluis ophalen

        $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"

2. De kluis verwijderen

        Remove-AzureRmRecoveryServicesVault -Vault $vault

>[!WARNING]
>
> Gebruik de bovenstaande opdracht uiterst voorzichtig, want u raakt al uw gegevens kwijt als u per ongeluk een kluis verwijdert. Dit is een permanente actie die niet ongedaan kan worden gemaakt.  




<!--HONumber=Feb17_HO3-->


