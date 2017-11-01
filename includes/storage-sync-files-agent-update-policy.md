Regelmatig worden updates van de Azure File Sync-agent uitgebracht om nieuwe functionaliteit toe te voegen en eventuele aangetroffen problemen op te lossen. We raden u aan om Microsoft Update in te schakelen om alle updates van de Azure File Sync-agent te ontvangen op het moment dat we ze uitbrengen. We begrijpen dat sommige organisaties een strikt beleid hebben voor updates en ze eerst willen testen. Voor implementaties met oudere versies van de Azure File Sync-agent:

- De opslagsynchronisatieservice houdt gedurende drie maanden na de eerste release van een nieuwe primaire versie rekening met de vorige primaire versie. Versie 1. \* wordt bijvoorbeeld tot drie maanden na de release van versie 2.\* door de opslagsynchronisatieservice ondersteund.
- Nadat de drie maanden zijn verstreken, gaat de opslagsynchronisatieservice geregistreerde servers die de verlopen versie gebruiken, blokkeren van synchronisatie met de synchronisatiegroepen.
- Binnen de drie maanden voor een eerdere primaire versie, worden alle opgeloste fouten alleen voor de huidige primaire versie doorgevoerd.

> [!Note]  
> U ontvangt een pop-upmelding in Azure Portal als u een versie van Azure File Sync gebruikt die binnen de komende drie maanden verloopt.