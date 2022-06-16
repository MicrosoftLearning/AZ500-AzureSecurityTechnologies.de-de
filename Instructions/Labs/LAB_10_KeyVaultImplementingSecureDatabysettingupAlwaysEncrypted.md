---
lab:
  title: 10 – Key Vault (Implementieren sicherer Daten durch Einrichten von „Always Encrypted“)
  module: Module 03 - Secure Data and Applications
ms.openlocfilehash: c31dd6e930e0f1d1b82e7c6ea502bb6fa51a7dd7
ms.sourcegitcommit: 967cb50981ef07d731dd7548845a38385b3fb7fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2022
ms.locfileid: "145955381"
---
# <a name="lab-10-key-vault-implementing-secure-data-by-setting-up-always-encrypted"></a>Lab 10: Key Vault (Implementieren sicherer Daten durch Einrichten von „Always Encrypted“)
# <a name="student-lab-manual"></a>Labhandbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie sollen eine Proof of Concept-Anwendung erstellen, die die Azure SQL-Datenbank-Unterstützung für die Funktion „Always Encrypted“ nutzt. Alle in diesem Szenario verwendeten Geheimnisse und Schlüssel sollten in Key Vault gespeichert werden. Die Anwendung sollte in Azure Active Directory (Azure AD) registriert werden, um ihren Sicherheitsstatus zu verbessern. Um diese Ziele zu erreichen, sollte die Machbarkeitsstudie (Proof of Concept) Folgendes umfassen:

- Erstellen einer Azure Key Vault-Ressource und Speichern von Schlüsseln und Geheimnissen im Tresor
- Erstellen einer SQL-Datenbank und Verschlüsseln des Inhalts von Spalten in Datenbanktabellen mithilfe von „Always Encrypted“.

>**Hinweis**: Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

Um den Fokus auf die Sicherheitsaspekte von Azure im Zusammenhang mit der Erstellung dieser Proof of Concept-Anwendung zu legen, beginnen Sie mit einer automatisierten ARM-Vorlagenbereitstellung und richten einen virtuellen Computer (Virtual Machine, VM) mit Visual Studio 2019 und SQL Server Management Studio 2018 ein.

## <a name="lab-objectives"></a>Labziele

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Bereitstellen der Basisinfrastruktur anhand einer ARM-Vorlage
- Übung 2: Konfigurieren der Key Vault-Ressource mit einem Schlüssel und einem Geheimnis
- Übung 3: Konfigurieren einer Azure SQL-Datenbank und einer datengesteuerten Anwendung
- Übung 4: Veranschaulichen der Verwendung von Azure Key Vault beim Verschlüsseln der Azure SQL-Datenbank

## <a name="key-vault-diagram"></a>Key Vault-Diagramm

![image](https://user-images.githubusercontent.com/91347931/157532938-c724cc40-f64f-4d69-9e91-d75344c5e0a2.png)

## <a name="instructions"></a>Anweisungen

## <a name="lab-files"></a>Labdateien:

- **\\Allfiles\\Labs\\10\\az-500-10_azuredeploy.json**

- **\\Allfiles\\Labs\\10\\program.cs**

### <a name="total-lab-time-estimate-60-minutes"></a>Geschätzte Gesamtdauer des Labs: 60 Minuten

### <a name="exercise-1-deploy-the-base-infrastructure-from-an-arm-template"></a>Übung 1: Bereitstellen der Basisinfrastruktur anhand einer ARM-Vorlage

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers und einer Azure SQL-Datenbank

#### <a name="task-1-deploy-an-azure-vm-and-an-azure-sql-database"></a>Aufgabe 1: Bereitstellen eines virtuellen Azure-Computers und einer Azure SQL-Datenbank

In dieser Aufgabe stellen Sie einen virtuellen Azure-Computer bereit, auf dem Visual Studio 2019 und SQL Server Management Studio 2018 als Teil der Bereitstellung automatisch installiert werden. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

4. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\10\\az-500-10_azuredeploy.json**, und klicken Sie auf **Öffnen**.

5. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

6. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** unter **Bereitstellungsbereich** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

   |Einstellung|value|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB10** ein.|
   |Standort|**(USA) USA, Osten**|
   |Administratorbenutzername|**Kursteilnehmer**|
   |Administratorkennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|
   
    >**Hinweis**: Sie können zwar die für die Anmeldung beim virtuellen Computer verwendeten Administratoranmeldeinformationen ändern, dies ist jedoch nicht erforderlich.

    >**Hinweis**: Informationen zum Identifizieren von Azure-Regionen, in denen Sie Azure-VMs bereitstellen können, finden Sie unter [ **https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/).

7. Klicken Sie auf die Schaltfläche **Überprüfen und erstellen**, und bestätigen Sie die Bereitstellung, indem Sie auf die Schaltfläche **Erstellen** klicken. 

    >**Hinweis**: Dadurch wird die Bereitstellung des virtuellen Azure-Computers und der Azure SQL-Datenbank initiiert, die für dieses Lab erforderlich sind. 

    >**Hinweis**: Warten Sie nicht, bis die ARM-Vorlagenbereitstellung abgeschlossen ist, sondern fahren Sie stattdessen mit der nächsten Übung fort. Die Bereitstellung kann **20 bis25 Minuten** dauern. 

### <a name="exercise-2-configure-the-key-vault-resource-with-a-key-and-a-secret"></a>Übung 2: Konfigurieren der Key Vault-Ressource mit einem Schlüssel und einem Geheimnis

>**Hinweis**: Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen und Konfigurieren einer Key Vault-Ressource
- Aufgabe 2: Hinzufügen eines Schlüssels zur Key Vault-Ressource
- Aufgabe 3: Hinzufügen eines Geheimnisses zur Key Vault-Ressource

#### <a name="task-1-create-and-configure-a-key-vault"></a>Aufgabe 1: Erstellen und Konfigurieren einer Key Vault-Ressource

In dieser Aufgabe erstellen Sie eine Azure Key Vault-Ressource. Außerdem konfigurieren Sie die Azure Key Vault-Berechtigungen.

1. Öffnen Sie den Cloud Shell-Dienst, indem Sie oben rechts im Azure-Portal auf das erste Symbol (neben der Suchleiste) klicken. Wenn Sie dazu aufgefordert werden, wählen Sie **PowerShell** und dann **Speicher erstellen** aus.

2. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um in der Ressourcengruppe **AZ500LAB10** eine Azure Key Vault-Ressource zu erstellen. (Wenn Sie in diesem Lab in Aufgabe 1 einen anderen Namen für die Ressourcengruppe ausgewählt haben, verwenden Sie diesen Namen auch für diese Aufgabe.) Der Name der Key Vault-Ressource muss eindeutig sein. Merken Sie sich den Namen, den Sie ausgewählt haben. Sie benötigen ihn in diesem Lab.  

    ```powershell
    $kvName = 'az500kv' + $(Get-Random)

    $location = (Get-AzResourceGroup -ResourceGroupName 'AZ500LAB10').Location

    New-AzKeyVault -VaultName $kvName -ResourceGroupName 'AZ500LAB10' -Location $location
    ```

    >**Hinweis**: In der Ausgabe des letzten Befehls werden der Tresorname und der Tresor-URI angezeigt. Der Tresor-URI weist das folgende Format auf: `https://<vault_name>.vault.azure.net/`

4. Schließen Sie den Cloud Shell-Bereich. 

5. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

6. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB10** (oder einen anderen Namen, den Sie zuvor für die Ressourcengruppe ausgewählt haben).

7. Klicken Sie auf dem Blatt „Ressourcengruppe“ auf den Eintrag, der die neu erstellte Key Vault-Ressource darstellt. 

8. Klicken Sie auf dem Blatt der Key Vault-Ressource im Abschnitt **Einstellungen** auf **Zugriffsrichtlinien** und klicken Sie dann auf **+ Zugriffsrichtlinie hinzufügen**.

9. Geben Sie auf dem Blatt **Zugriffsrichtlinie hinzufügen** die folgenden Einstellungen an (übernehmen Sie für alle anderen Einstellungen die Standardwerte): 

    |Einstellung|Wert|
    |----|----|
    |Anhand einer Vorlage konfigurieren (optional)|**Schlüssel, Geheimnisse und Zertifikate verwalten**|
    |Schlüsselberechtigungen|Klicken Sie auf **Alles auswählen**. Dadurch werden **17 ausgewählte** Berechtigungen angezeigt. (Stellen Sie sicher, dass die Berechtigungen für **Rotationsrichtlinien-Vorgänge** **nicht aktiviert** sind.) |
    |Berechtigungen für Geheimnis|Klicken Sie auf **Alle auswählen**, was zu insgesamt **8 ausgewählten** Berechtigungen führt.|
    |Zertifikatberechtigungen|Klicken Sie auf **Alle auswählen**, was zu insgesamt **16 ausgewählten** Berechtigungen führt.|
    |Prinzipal auswählen|Klicken Sie auf **Nichts ausgewählt**, wählen Sie auf dem Blatt **Prinzipal** Ihr Benutzerkonto aus, und klicken Sie dann auf **Auswählen**.|

10. Klicken Sie anschließend auf dem Blatt **Zugriffsrichtlinie hinzufügen** auf **Hinzufügen**, um die Zugriffsrichtlinie hinzuzufügen, und klicken Sie dann auf dem Blatt „Zugriffsrichtlinien“ der Key Vault-Ressource auf **Speichern**, um Ihre Änderungen zu speichern. 

#### <a name="task-2-add-a-key-to-key-vault"></a>Aufgabe 2: Hinzufügen eines Schlüssels zur Key Vault-Ressource

In dieser Aufgabe fügen Sie der Key Vault-Ressource einen Schlüssel hinzu und zeigen Informationen zum Schlüssel an. 

1. Öffnen Sie im Azure-Portal im Cloud Shell-Bereich eine PowerShell-Sitzung.

2. Stellen Sie sicher, dass im Dropdownmenü oben links im Cloud Shell-Bereich der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um der Key Vault-Ressource einen softwaregeschützten Schlüssel hinzuzufügen: 

    ```powershell
    $kv = Get-AzKeyVault -ResourceGroupName 'AZ500LAB10'

    $key = Add-AZKeyVaultKey -VaultName $kv.VaultName -Name 'MyLabKey' -Destination 'Software'
    ```

    >**Hinweis**: Der Name des Schlüssels lautet **MyLabKey**.

4. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um zu überprüfen, ob der Schlüssel erstellt wurde:

    ```powershell
    Get-AZKeyVaultKey -VaultName $kv.VaultName
    ```

5. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um den Schlüsselbezeichner anzuzeigen:

    ```powershell
    $key.key.kid
    ```

6. Minimieren Sie den Cloud Shell-Bereich. 

7. Klicken Sie im Azure-Portal auf dem Blatt „Key Vault“ im Abschnitt **Einstellungen** auf **Schlüssel**.

8. Klicken Sie in der Liste der Schlüssel auf den Eintrag **MyLabKey**, und klicken Sie dann auf dem Blatt **MyLabKey** auf den Eintrag, der die aktuelle Version des Schlüssels darstellt.

    >**Hinweis**: Überprüfen Sie die Informationen zu dem von Ihnen erstellten Schlüssel.

    >**Hinweis**: Sie können mithilfe des Schlüsselbezeichners auf einen beliebigen Schlüssel verweisen. Um die aktuelle Version abzurufen, verweisen Sie auf `https://<key_vault_name>.vault.azure.net/keys/MyLabKey`, oder rufen Sie eine bestimmte Version wie folgt ab: `https://<key_vault_name>.vault.azure.net/keys/MyLabKey/<key_version>`


#### <a name="task-3-add-a-secret-to-key-vault"></a>Aufgabe 3: Hinzufügen eines Geheimnisses zur Key Vault-Ressource

1. Wechseln Sie zurück zum Cloud Shell-Bereich.

2. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Variable mit einem sicheren Zeichenfolgenwert zu erstellen:

    ```powershell
    $secretvalue = ConvertTo-SecureString 'Pa55w.rd1234' -AsPlainText -Force
    ```

3.  Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um dem Tresor das Geheimnis hinzuzufügen:

    ```powershell
    $secret = Set-AZKeyVaultSecret -VaultName $kv.VaultName -Name 'SQLPassword' -SecretValue $secretvalue
    ```

    >**Hinweis**: Der Name des Geheimnisses lautet „SQLPassword“. 

4.  Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um zu überprüfen, ob das Geheimnis erstellt wurde.

    ```powershell
    Get-AZKeyVaultSecret -VaultName $kv.VaultName
    ```

5. Minimieren Sie den Cloud Shell-Bereich. 

6. Navigieren Sie im Azure-Portal zurück zum Blatt „Key Vault“, und klicken Sie im Abschnitt **Einstellungen** auf **Geheimnisse**.

7. Klicken Sie in der Liste der Geheimnisse auf den Eintrag **SQLPassword**, und klicken Sie dann auf dem Blatt **SQLPassword** auf den Eintrag, der die aktuelle Version des Geheimnisses darstellt.

    >**Hinweis**: Überprüfen Sie die Informationen zu dem von Ihnen erstellten Geheimnis.

    >**Hinweis**: Um die aktuelle Version eines Geheimnisses abzurufen, verweisen Sie auf `https://<key_vault_name>.vault.azure.net/secrets/<secret_name>`. Wenn Sie eine bestimmte Version abrufen möchten, verweisen Sie auf `https://<key_vault_name>.vault.azure.net/secrets/<secret_name>/<secret_version>`.


### <a name="exercise-3-configure-an-azure-sql-database-and-a-data-driven-application"></a>Übung 3: Konfigurieren einer Azure SQL-Datenbank und einer datengesteuerten Anwendung

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Ermöglichen des Zugriffs einer Clientanwendung auf den Azure SQL-Datenbank-Dienst
- Aufgabe 2: Erstellen einer Richtlinie, die der Anwendung den Zugriff auf die Key Vault-Ressource ermöglicht
- Aufgabe 3: Abrufen der ADO.NET-Verbindungszeichenfolge für Azure SQL-Datenbank 
- Aufgabe 4: Anmelden bei dem virtuellen Azure-Computer, auf dem Visual Studio 2019 und SQL Server Management Studio 2018 ausgeführt werden
- Aufgabe 5: Erstellen einer Tabelle in der SQL-Datenbank und Auswählen von Datenspalten für die Verschlüsselung


#### <a name="task-1-enable-a-client-application-to-access-the-azure-sql-database-service"></a>Aufgabe 1: Ermöglichen des Zugriffs einer Clientanwendung auf den Azure SQL-Datenbank-Dienst 

In dieser Aufgabe ermöglichen Sie einer Clientanwendung den Zugriff auf den Azure SQL-Datenbank-Dienst. Dies erfolgt durch Einrichten der erforderlichen Authentifizierung und durch Abrufen der Anwendungs-ID und des Geheimnisses, die Sie zum Authentifizieren Ihrer Anwendung benötigen. T

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **App-Registrierungen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **App-Registrierungen** auf **+ Neue Registrierung**. 

3. Geben Sie auf dem Blatt **Anwendung registrieren** die folgenden Einstellungen an (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

    |Einstellung|value|
    |----|----|
    |Name|**sqlApp**|
    |Umleitungs-URI (optional)|**Web** und **https://sqlapp**|

4. Klicken Sie auf dem Blatt **Anwendung registrieren** auf **Registrieren**. 

    >**Hinweis**: Nach Abschluss der Registrierung werden Sie vom Browser automatisch zum Blatt **sqlApp** umgeleitet. 

5. Identifizieren Sie auf dem Blatt **sqlApp** den Wert der **Anwendungs-ID (Client-ID)** . 

    >**Hinweis**: Notieren Sie sich diesen Wert. Sie werden dies in der nächsten Aufgabe benötigen.

6. Klicken Sie auf dem Blatt **sqlApp** im Abschnitt **Verwalten** auf **Zertifikate und Geheimnisse**.

7. Klicken Sie auf dem Blatt **sqlApp | Zertifikate und Geheimnisse** im Abschnitt **Geheime Clientschlüssel** auf **+ Neuer geheimer Clientschlüssel**.

8. Geben Sie im Bereich **Geheimen Clientschlüssel hinzufügen** die folgenden Einstellungen an:

    |Einstellung|value|
    |----|----|
    |BESCHREIBUNG|**Key1**|
    |Läuft ab|**12 Monate**|
    
9. Klicken Sie auf **Hinzufügen**, um die Anmeldeinformationen der Anwendung zu aktualisieren.

10. Identifizieren Sie auf dem Blatt **sqlApp | Zertifikate und Geheimnisse** den Wert von **Key1**.

    >**Hinweis**: Notieren Sie sich diesen Wert. Sie werden dies in der nächsten Aufgabe benötigen. 

    >**Hinweis**: Kopieren Sie unbedingt diesen Wert, *bevor* Sie dieses Blatt verlassen. Nachdem Sie das Blatt verlassen haben, ist es nicht mehr möglich, den Klartextwert abzurufen.


#### <a name="task-2-create-a-policy-allowing-the-application-access-to-the-key-vault"></a>Aufgabe 2: Erstellen einer Richtlinie, die der Anwendung den Zugriff auf die Key Vault-Ressource ermöglicht

In dieser Aufgabe erteilen Sie der neu registrierten App Berechtigungen für den Zugriff auf die in der Key Vault-Ressource gespeicherten Geheimnisse.

1. Öffnen Sie im Azure-Portal im Cloud Shell-Bereich eine PowerShell-Sitzung.

2. Stellen Sie sicher, dass im Dropdownmenü oben links im Cloud Shell-Bereich der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Variable zu erstellen, die die **Anwendungs-ID (Client-ID)** speichert, die Sie sich in der vorherigen Aufgabe notiert haben (ersetzen Sie den Platzhalter `<Azure_AD_Application_ID>` durch den Wert der **Anwendungs-ID (Client-ID)** ):
   
    ```powershell
    $applicationId = '<Azure_AD_Application_ID>'
    ```
4. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Variable zu erstellen, die den Namen der Key Vault-Ressource speichert.
    ```
    $kvName = (Get-AzKeyVault -ResourceGroupName 'AZ500LAB10').VaultName

    $kvName
    ```

5. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um der Anwendung, die Sie in der vorherigen Aufgabe registriert haben, Berechtigungen für die Key Vault-Ressource zu erteilen:

    ```powershell
    Set-AZKeyVaultAccessPolicy -VaultName $kvName -ResourceGroupName AZ500LAB10 -ServicePrincipalName $applicationId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
    ```

6. Schließen Sie den Cloud Shell-Bereich. 


#### <a name="task-3-retrieve-sql-azure-database-adonet-connection-string"></a>Aufgabe 3: Abrufen der ADO.NET-Verbindungszeichenfolge für Azure SQL-Datenbank 

Im Rahmen der ARM-Vorlagenbereitstellung in Übung 1 haben Sie eine Azure SQL Server-Instanz und eine Azure SQL-Datenbank namens **medical** bereitgestellt. Sie müssen die leere Datenbankressource mit einer neuen Tabellenstruktur aktualisieren und Datenspalten für die Verschlüsselung auswählen.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **SQL-Datenbanken** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie in der Liste der SQL-Datenbanken auf den Eintrag **medical(<randomsqlservername>)** .

    >**Hinweis**: Wenn die Datenbank nicht gefunden werden kann, bedeutet dies wahrscheinlich, dass die von Ihnen in Übung 1 initiierte Bereitstellung noch nicht abgeschlossen ist. Sie können dies überprüfen, indem Sie zur Azure-Ressourcengruppe „AZ500LAB10“ (bzw. zur Ressourcengruppe mit dem von Ihnen ausgewählten Namen) navigieren und im Bereich „Einstellungen“ die Option **Bereitstellungen** auswählen.  

3. Klicken Sie auf dem Blatt der SQL-Datenbank im Abschnitt **Einstellungen** auf **Verbindungszeichenfolgen**. 

    >**Hinweis**: Die Schnittstelle enthält Verbindungszeichenfolgen für ADO.NET, JDBC, ODBC, PHP und Go. 
   
4. Notieren Sie sich die **ADO.NET-Verbindungszeichenfolge**. Sie benötigen sie später.

    >**Hinweis:** Wenn Sie die Verbindungszeichenfolge verwenden, müssen Sie den Platzhalter `{your_password}` durch das Kennwort ersetzen, das Sie mit der Bereitstellung in Übung 1 konfiguriert haben.

#### <a name="task-4-log-on-to-the-azure-vm-running-visual-studio-2019-and-sql-management-studio-2018"></a>Aufgabe 4: Anmelden bei dem virtuellen Azure-Computer, auf dem Visual Studio 2019 und SQL Server Management Studio 2018 ausgeführt werden

In dieser Aufgabe melden Sie sich bei dem virtuellen Azure-Computer an, dessen Bereitstellung Sie in Übung 1 initiiert haben. Auf diesem dem virtuellen Azure-Computer werden Visual Studio 2019 und SQL Server Management Studio 2018 gehostet.

    >**Note**: Before you proceed with this task, ensure that the deployment you initiated in the first exercise has completed successfully. You can validate this by navigating to the blade of the Azure resource group "Az500Lab10" (or other name you chose) and selecting **Deployments** from the Settings pane.  

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

2. Wählen Sie in der Liste der angezeigten virtuellen Computer den Eintrag **az500-10-vm1** aus. Notieren Sie sich die auf dem Blatt **az500-10-vm1** im Bereich **Zusammenfassung** angegebene **öffentliche IP-Adresse**. Sie verwenden diesen Wert später. 

#### <a name="task-5-create-a-table-in-the-sql-database-and-select-data-columns-for-encryption"></a>Aufgabe 5: Erstellen einer Tabelle in der SQL-Datenbank und Auswählen von Datenspalten für die Verschlüsselung

In dieser Aufgabe stellen Sie eine Verbindung mit der SQL-Datenbank mit SQL Server Management Studio her und erstellen eine Tabelle. Anschließend verschlüsseln Sie zwei Datenspalten mithilfe eines automatisch generierten Schlüssels aus der Azure Key Vault-Ressource. 

1. Navigieren Sie im Azure-Portal zum Blatt der SQL-Datenbank **medical**, ermitteln Sie im Abschnitt **Essentials** den **Servernamen** (kopieren Sie ihn in die Zwischenablage), und klicken Sie dann auf der Symbolleiste auf **Serverfirewall festlegen**.  

    >**Hinweis**: Notieren Sie sich den Servernamen. Sie benötigen den Servernamen später in dieser Aufgabe.

2. Scrollen Sie auf dem Blatt **Firewalleinstellungen** nach unten zu **Regelname**, und geben Sie die folgenden Einstellungen an: 

    |Einstellung|Wert|
    |---|---|
    |Regelname|**Allow Mgmt VM**|
    |Start-IP|Die öffentliche IP-Adresse von „az500-10-vm1“|
    |End-IP|Die öffentliche IP-Adresse von „az500-10-vm1“|

3. Klicken Sie auf **Speichern** und dann auf **OK**, um die Änderung zu speichern und den Bestätigungsbereich zu schließen. 

    >**Hinweis**: Dadurch werden die Firewalleinstellungen des Servers geändert, sodass Verbindungen mit der Datenbank „medical“ über die öffentliche IP-Adresse des virtuellen Azure-Computers zulässig sind, den Sie in diesem Lab bereitgestellt haben.

4. Navigieren Sie zurück zum Blatt **az500-10-vm1**, und klicken Sie auf **Übersicht**. Klicken Sie auf **Verbinden**, und klicken Sie dann im Dropdownmenü auf **RDP**. 

5. Klicken Sie auf **RDP-Datei herunterladen**, und stellen Sie damit über Remotedesktop eine Verbindung mit dem virtuellen Azure-Computer **az500-10-vm1** her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

    |Einstellung|Wert|
    |---|---|
    |Benutzername|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|

    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung und der **Server-Manager** geladen werden. Schließen Sie den Server-Manager. 

    >**Hinweis**: Die restlichen Schritte in diesem Lab werden in der Remotedesktopsitzung mit dem virtuellen Azure-Computer **az500-10-vm1** ausgeführt. 

6. Klicken Sie auf **Start**, erweitern Sie im **Startmenü** den Ordner **Microsoft SQL Server-Tools 18**, und klicken Sie auf das Menüelement **Micosoft SQL Server Management Studio**.

7. Geben Sie im Dialogfeld **Verbindung mit dem Server herstellen** die folgenden Einstellungen an: 

    |Einstellung|Wert|
    |---|---|
    |Servertyp|**Datenbank-Engine**|
    |Servername|Der Servername, den Sie zuvor in dieser Aufgabe identifiziert haben|
    |Authentifizierung|**SQL Server-Authentifizierung**|
    |Anmelden|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 04 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|

8. Klicken Sie im Dialogfeld **Verbindung mit dem Server herstellen** auf **Verbinden**.

9. Erweitern Sie in der **SQL Server Management Studio**-Konsole im **Objekt-Explorer**-Fenster den Ordner **Datenbanken**.

10. Klicken Sie im **Objekt-Explorer**-Fenster mit der rechten Maustaste auf die Datenbank **medical**, und klicken Sie dann auf **Neue Abfrage**.

11. Fügen Sie den folgenden Code in das Abfragefenster ein, und klicken Sie auf **Ausführen**. Dadurch wird die Tabelle **Patients** erstellt.

     ```sql
     CREATE TABLE [dbo].[Patients](
        [PatientId] [int] IDENTITY(1,1),
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NULL,
        [LastName] [nvarchar](50) NULL,
        [MiddleName] [nvarchar](50) NULL,
        [StreetAddress] [nvarchar](50) NULL,
        [City] [nvarchar](50) NULL,
        [ZipCode] [char](5) NULL,
        [State] [char](2) NULL,
        [BirthDate] [date] NOT NULL 
     PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
     ```
12. Erweitern Sie nach der erfolgreichen Erstellung der Tabelle im **Objekt-Explorer**-Fenster den Knoten der Datenbank **medical** und den Knoten **Tabellen**, klicken Sie mit der rechten Maustaste auf den Knoten **dbo.Patients**, und klicken Sie dann auf **Spalten verschlüsseln**. 

    >**Hinweis**: Dadurch wird der **Always Encrypted-Assistent** initiiert.

13. Klicken Sie auf der Seite **Einführung** auf **Weiter**.

14. Wählen Sie auf der Seite **Spaltenauswahl** die Spalten **SSN** und **Geburtsdatum** aus, legen Sie den **Verschlüsselungstyp** der Spalte **SSN** auf **Deterministisch** und den der Spalte **Geburtsdatum** auf **Zufällig** fest, und klicken Sie dann auf **Weiter**.

    >**Hinweis:** Wenn während der Verschlüsselung ein Fehler wie **Exception has been thrown by the target of an innvocation** (Eine Ausnahme wurde vom Ziel eines Aufrufs ausgelöst) im Zusammenhang mit **Rotary(Microsoft.SQLServer.Management.ServiceManagement)** ausgelöst wurde, stellen Sie sicher, dass die Werte der **Rotationsrichtlinienvorgänge** für die **Schlüsselberechtigung** **nicht aktiviert** sind. Andernfalls sollten Sie im Azure-Portal zu **Schlüsseltresor** >> **Zugriffsberechtigungen** >> **Schlüsselberechtigungen** navigieren. Deaktivieren Sie dort alle Werte unter den **Rotationsrichtlinienvorgängen** sowie unter **Vorgänge mit privilegiertem Schlüssel** die Option **Veröffentlichen**.

15. Wählen Sie auf der Seite **Konfiguration des Hauptschlüssels** die Option **Azure Key Vault** aus, und klicken Sie auf **Anmelden**. Authentifizieren Sie sich bei entsprechender Aufforderung mit demselben Benutzerkonto, das Sie zuvor in diesem Lab zum Bereitstellen der Azure Key Vault-Instanz verwendet haben. Stellen Sie sicher, dass diese Key Vault-Instanz in der Dropdownliste **Azure Key Vault-Instanz auswählen** angezeigt wird, und klicken Sie auf **Weiter**.

16. Klicken Sie auf der Seite **Ausführungseinstellungen** auf **Weiter**.
    
17. Klicken Sie auf der Seite **Zusammenfassung** auf **Fertig stellen**, um mit der Verschlüsselung fortzufahren. Melden Sie sich bei entsprechender Aufforderung erneut mit dem Benutzerkonto an, das Sie zuvor in diesem Lab zum Bereitstellen der Azure Key Vault-Instanz verwendet haben.

18. Klicken Sie nach Abschluss des Verschlüsselungsprozesses auf der Seite **Ergebnisse** auf **Schließen**.

19. Erweitern Sie in der **SQL Server Management Studio**-Konsole im **Objekt-Explorer**-Fenster unter dem Knoten **medical** die Unterknoten **Sicherheit** und **Always Encrypted-Schlüssel**. 

    >**Hinweis**: Der Unterknoten **Always Encrypted-Schlüssel** enthält die Unterordner **Spaltenhauptschlüssel** und **Spaltenverschlüsselungsschlüssel**.


### <a name="exercise-4-demonstrate-the-use-of-azure-key-vault-in-encrypting-the-azure-sql-database"></a>Übung 4: Veranschaulichen der Verwendung von Azure Key Vault beim Verschlüsseln der Azure SQL-Datenbank

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Ausführen einer datengesteuerten Anwendung zum Veranschaulichen der Verwendung von Azure Key Vault beim Verschlüsseln der Azure SQL-Datenbank

#### <a name="task-1-run-a-data-driven-application-to-demonstrate-the-use-of-azure-key-vault-in-encrypting-the-azure-sql-database"></a>Aufgabe 1: Ausführen einer datengesteuerten Anwendung zum Veranschaulichen der Verwendung von Azure Key Vault beim Verschlüsseln der Azure SQL-Datenbank

Sie erstellen mit Visual Studio eine Konsolenanwendung, um Daten in die verschlüsselten Spalten zu laden und dann sicher auf diese Daten zuzugreifen, indem eine Verbindungszeichenfolge verwendet wird, die auf den Schlüssel in der Key Vault-Instanz zugreift.

1. Starten Sie in der RDP-Sitzung mit **az500-10-vm1** über das **Startmenü** die Anwendung **Visual Studio 2019**.

2. Wechseln Sie zu dem Fenster, in dem die Visual Studio 2019-Willkommensnachricht angezeigt wird, klicken Sie auf die Schaltfläche **Anmelden**, und geben Sie bei entsprechender Aufforderung die Anmeldeinformationen an, die Sie für die Authentifizierung bei dem in diesem Lab verwendeten Azure-Abonnement verwendet haben.

3. Klicken Sie auf der Seite **Erste Schritte** auf **Neues Projekt erstellen**. 

4. Suchen Sie in der Liste der Projektvorlagen nach **Konsolen-App (.NET Framework)** , klicken Sie in der Ergebnisliste auf **Konsolen-App (.NET Framework)** für **C#** , und klicken Sie dann auf **Weiter**.

5. Geben Sie auf der Seite **Neues Projekt konfigurieren** die folgenden Einstellungen an (übernehmen Sie für andere Einstellungen die Standardwerte), und klicken Sie dann auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |Projektname|**OpsEncrypt**|
    |Projektmappenname|**OpsEncrypt**|
    |Framework|**.NET Framework 4.7.2**|

6. Klicken Sie in der Visual Studio-Konsole auf das Menü **Extras**, klicken Sie im Dropdownmenü auf **NuGet-Paket-Manager**, und klicken Sie im hierarchischen Menü auf **Paket-Manager-Konsole**.

7. Führen Sie im Bereich **Paket-Manager-Konsole** Folgendes aus, um das erste erforderliche **NuGet**-Paket zu installieren:

    ```powershell
    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    ```

8. Führen Sie im Bereich **Paket-Manager-Konsole** Folgendes aus, um das zweite erforderliche **NuGet**-Paket zu installieren:

    ```powershell
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```
    
9. Minimieren Sie die RDP-Sitzung auf Ihrem virtuellen Azure-Computer. Navigieren Sie dann zu **\\Allfiles\\Labs\\10\\program.cs**, öffnen Sie die Datei in Editor, und kopieren Sie den Inhalt in die Zwischenablage.

10. Kehren Sie zur RDP-Sitzung zurück, klicken Sie in der Visual Studio-Konsole im Fenster **Projektmappen-Explorer** auf **Program.cs**, und ersetzen Sie den Inhalt der Datei durch den Code, den Sie in die Zwischenablage kopiert haben.

11. Ersetzen Sie im Visual Studio-Fenster im Bereich **Program.cs** in Zeile 15 den Platzhalter `<connection string noted earlier>` durch die **ADO.NET**-Verbindungszeichenfolge für Azure SQL-Datenbank, die Sie sich zuvor im Lab notiert haben. Ersetzen Sie in der Verbindungszeichenfolge den Platzhalter `{your_password}` durch `Pa55w.rd1234`. Wenn Sie die Zeichenfolge auf dem Lab-Computer gespeichert haben, müssen Sie möglicherweise die RDP-Sitzung verlassen, um die ADO-Zeichenfolge zu kopieren, und dann zum virtuellen Azure-Computer zurückkehren, um sie einzufügen.

12. Ersetzen Sie im Visual Studio-Fenster im Bereich **Program.cs** in Zeile 16 den Platzhalter `<client id noted earlier>` durch den Wert der **Anwendungs-ID (Client-ID)** der registrierten App, die Sie sich zuvor im Lab notiert haben. 

13. Ersetzen Sie im Visual Studio-Fenster im Bereich **Program.cs** in Zeile 17 den Platzhalter `<key value noted earlier>` durch den Wert von **Key1** der registrierten App, den Sie sich zuvor im Lab notiert haben. 

14. Klicken Sie in der Visual Studio-Konsole auf die Schaltfläche **Start**, um die Erstellung der Konsolenanwendung zu initiieren und um sie zu starten.

15. Die Anwendung wird mit einem Eingabeaufforderungsfenster gestartet. Geben Sie bei entsprechender Aufforderung das Kennwort ein, das Sie in der Bereitstellung in Übung 1 angegeben haben, um eine Verbindung mit Azure SQL-Datenbank herzustellen. 

16. Führen Sie die Konsolenanwendung weiterhin aus, und wechseln Sie zur **SQL Server Management Studio**-Konsole. 

17. Klicken Sie im **Objekt-Explorer**-Fenster mit der rechten Maustaste auf die Datenbank **medical**, und klicken Sie dann im Kontextmenü auf **Neue Abfrage**.

18. Führen Sie im Abfragefenster die folgende Abfrage aus, um zu überprüfen, ob die von der Konsolen-App in die Datenbank geladenen Daten verschlüsselt sind.

    ```sql
    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;
    ```

19. Wechseln Sie zurück zur Konsolenanwendung, in der Sie zur Eingabe einer gültigen SSN aufgefordert werden. Dadurch wird die verschlüsselte Spalte nach den Daten abgefragt. Geben Sie an der Eingabeaufforderung Folgendes ein, und drücken Sie die EINGABETASTE:

    ```cmd
    999-99-0003
    ```

    >**Hinweis**: Vergewissern Sie sich, dass die von der Abfrage zurückgegebenen Daten nicht verschlüsselt sind.

20. Drücken Sie die EINGABETASTE, um die Konsolen-App zu beenden.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

1. Öffnen Sie im Azure-Portal den Cloud Shell-Dienst, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. 

2. Wählen Sie im Dropdownmenü oben links im Cloud Shell-Bereich die Option **PowerShell** aus, und klicken Sie auf **Bestätigen**, wenn Sie dazu aufgefordert werden.

3. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppen zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB10" -Force -AsJob
    ```

4.  Schließen Sie den **Cloud Shell**-Bereich. 
