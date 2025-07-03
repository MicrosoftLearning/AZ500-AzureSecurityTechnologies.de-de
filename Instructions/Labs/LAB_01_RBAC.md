---
lab:
  title: '01: Rollenbasierte Zugriffssteuerung'
  module: Module 01 - Manage Identity and Access
---


# Lab 01: Rollenbasierte Zugriffssteuerung
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden aufgefordert, einen Proof of Concept zu erstellen, der zeigt, wie Azure-Benutzer und -Gruppen erstellt werden. Außerdem sollen Sie zeigen, wie rollenbasierte Zugriffssteuerung verwendet wird, um Gruppen Rollen zuzuweisen. Insbesondere ist Folgendes erforderlich:

- Erstellen einer Gruppe „Senior Admins“, die das Benutzerkonto von Joseph Price als Mitglied enthält.
- Erstellen einer Gruppe „Junior Admins“, die das Benutzerkonto von Isabel Garcia als Mitglied enthält.
- Erstellen einer Gruppe „Service Desk“, die das Benutzerkonto von Dylan Williams als Mitglied enthält.
- Zuweisen der Rolle „VM-Mitwirkender“ zur Gruppe „Service Desk“. 

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Erstellen der Gruppe „Senior Admins“ mit dem Benutzerkonto Joseph Price als Mitglied (Azure-Portal). 
- Übung 2: Erstellen der Gruppe „Junior Admins“ mit dem Benutzerkonto Isabel Garcia als Mitglied (PowerShell).
- Übung 3: Erstellen der Gruppe „Service Desk“ mit dem Benutzer Dylan Williams als Mitglied (Azure CLI). 
- Übung 4: Zuweisen der Rolle „VM-Mitwirkender“ zur Gruppe „Service Desk“.

## Architekturdiagramm der rollenbasierten Zugriffssteuerung

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/506cde9c-5242-4438-a793-f88a5434a2b2)

## Anweisungen

### Übung 1: Erstellen der Gruppe „Senior Admins“ mit dem Benutzerkonto Joseph Price als Mitglied. 

#### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Verwenden des Azure-Portals, um ein Benutzerkonto für Joseph Price zu erstellen.
- Aufgabe 2: Verwenden des Azure-Portals, um eine Gruppe „Senior Admins“ zu erstellen und der Gruppe das Benutzerkonto von Joseph Price hinzuzufügen.

#### Aufgabe 1: Verwenden des Azure-Portals, um ein Benutzerkonto für Joseph Price zu erstellen 

In dieser Aufgabe erstellen Sie ein Benutzerkonto für Joseph Price. 

1. Öffnen Sie eine Browsersitzung, und melden Sie sich beim Azure-Portal **`https://portal.azure.com/`** an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, das in dem Azure-Abonnement, das Sie für dieses Lab verwenden, über die Rolle „Besitzer“ oder „Mitwirkender“ und die Rolle „Globaler Administrator“ im Microsoft Entra-Mandanten verfügt, der diesem Abonnement zugeordnet ist.

2. Geben Sie oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Microsoft Entra ID** ein und drücken Sie die **EINGABETASTE**.

3. Wählen Sie auf dem Blatt **Übersicht** des Microsoft Entra ID-Mandanten im Abschnitt **Verwalten** die Option **Benutzer*innen** und dann **+ Neue*r Benutzer*in** aus.

4. Stellen Sie auf dem Blatt **Neuer Benutzer** sicher, dass die Option **Benutzer erstellen** ausgewählt ist, und geben Sie die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**Joseph**|
   |Name|**Joseph Price**|

5. Klicken Sie auf das Kopiersymbol neben dem **Benutzernamen**, um den vollständigen Benutzernamen zu kopieren.

6. Stellen Sie sicher, dass für das Kennwort **Automatisch generieren** ausgewählt ist, und aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, um das automatisch generierte Kennwort zu identifizieren. Sie müssen dieses Kennwort zusammen mit dem Benutzernamen für Joseph bereitstellen. 

7. Klicken Sie auf **Erstellen**.

8. Aktualisieren Sie das Blatt **Benutzer \| Alle Benutzer**, um zu überprüfen, ob der neue Benutzer in Ihrem Microsoft Entra-Mandanten erstellt wurde.

#### Aufgabe 2: Verwenden des Azure-Portals, um eine Gruppe „Senior Admins“ zu erstellen und der Gruppe das Benutzerkonto von Joseph Price hinzuzufügen.

In dieser Aufgabe erstellen Sie die Gruppe *Senior Admins*, fügen der Gruppe das Benutzerkonto von Joseph Price hinzu und konfigurieren es als Gruppenbesitzer.

1. Navigieren Sie im Azure-Portal zurück zu dem Blatt, das Ihren Microsoft Entra ID-Mandanten anzeigt. 

2. Klicken Sie im Abschnitt **Verwalten** auf **Gruppen**, und wählen Sie dann **+ Neue Gruppe** aus.
 
3. Geben Sie auf dem Blatt **Neue Gruppe** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Gruppentyp|**Sicherheit**|
   |Gruppenname|**Senior Admins**|
   |Mitgliedschaftstyp|**Zugewiesen**|
    
4. Klicken Sie auf den Link **Keine Besitzer ausgewählt**, wählen Sie auf dem Blatt **Besitzer hinzufügen** die Option **Joseph Price** aus, und klicken Sie dann auf **Auswählen**.

5. Klicken Sie auf den Link **Keine Mitglieder ausgewählt**, wählen Sie auf dem Blatt **Mitglieder hinzufügen** die Option **Joseph Price** aus, und klicken Sie dann auf **Auswählen**.

6. Klicken Sie auf dem Blatt **Neue Gruppe** auf **Erstellen**.

> Ergebnis: Sie haben das Azure-Portal verwendet, um einen Benutzer und eine Gruppe zu erstellen, und den Benutzer der Gruppe zugewiesen. 

### Übung 2: Erstellen Sie eine Gruppe „Junior Admins“, die das Benutzerkonto von Isabel Garcia als Mitglied enthält.

#### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Verwenden von PowerShell zum Erstellen eines Benutzerkontos für Isabel Garcia.
- Aufgabe 2: Verwenden von PowerShell, um die Gruppe „Junior Admins“ zu erstellen und der Gruppe das Benutzerkonto von Isabel Garcia hinzuzufügen. 

#### Aufgabe 1: Verwenden von PowerShell zum Erstellen eines Benutzerkontos für Isabel Garcia.

In dieser Aufgabe erstellen Sie mithilfe von PowerShell ein Benutzerkonto für Isabel Garcia.

1. **Öffnen Sie die Cloud Shell**, indem Sie auf das **Cloud Shell-Symbol** in der oberen rechten Ecke des Azure-Portals klicken.

2. **Wenn Sie dazu aufgefordert werden, richten Sie Cloud Shell ein, indem Sie ein Speicherkonto erstellen.**. Dies ist **nur beim ersten Mal** erforderlich, wenn Sie Cloud Shell starten.

3. Stellen Sie sicher, dass oben links im Bereich „Cloud Shell“ im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

   >**Hinweis**: Um kopierten Text in Cloud Shell einzufügen, klicken Sie mit der rechten Maustaste im Bereichsfenster, und wählen Sie **Einfügen** aus. Alternativ können Sie die Tastenkombination **UMSCHALT+EINFÜGEN** verwenden.

4. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um ein Kennwortprofilobjekt zu erstellen:

    ```powershell
    $passwordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    ```

5. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um den Wert des Kennworts im Profilobjekt zu erstellen:
    ```powershell
    $passwordProfile.Password = "Pa55w.rd1234"
    ```

6. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine Verbindung mit Microsoft Entra ID herzustellen:

    ```powershell
    Connect-AzureAD
    ```
      
7. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um den Namen des Microsoft Entra-Mandanten zu identifizieren: 

    ```powershell
    $domainName = ((Get-AzureAdTenantDetail).VerifiedDomains)[0].Name
    ```

8. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um ein Benutzerkonto für Isabel Garcia zu erstellen: 

    ```powershell
    New-AzureADUser -DisplayName 'Isabel Garcia' -PasswordProfile $passwordProfile -UserPrincipalName "Isabel@$domainName" -AccountEnabled $true -MailNickName 'Isabel'
    ```

9. Führen Sie in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus, um Microsoft Entra ID-Benutzer aufzulisten (die Konten von Joseph und Isabel sollten in der Liste angezeigt werden): 

    ```powershell
    Get-AzureADUser -All $true | Where-Object {$_.UserPrincipalName -like "*43846135@LOD*"} 
    ```

#### Aufgabe 2: Verwenden von PowerShell, um die Gruppe „Junior Admins“ zu erstellen und der Gruppe das Benutzerkonto von Isabel Garcia hinzuzufügen.

In dieser Aufgabe erstellen Sie die Gruppe „Junior Admins“ und fügen der Gruppe das Benutzerkonto von Isabel Garcia mit PowerShell hinzu.

1. Führen Sie zum **Erstellen einer neuen Sicherheitsgruppe** mit dem Namen „Junior Admins“ in der gleichen PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus:
   
   ```powershell
   New-AzureADGroup -DisplayName 'Junior Admins43846135' -MailEnabled $false -SecurityEnabled $true -MailNickName JuniorAdmins
   ```
   
2. Führen Sie zum **Auflisten von Gruppen** in Ihrem Microsoft Entra-Mandanten in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus (die Liste sollte die Gruppen „Senior Admins“ und „Junior Admins“ enthalten)
   
   ```powershell
   Get-AzureADGroup
   ```

3. Führen Sie zum **Abrufen eines Verweises** auf das Benutzerkonto von Isabel Garcia in der PowerShell-Sitzung im Cloud Shell-Bereich Folgendes aus:

   ```powershell
   $user = Get-AzureADUser -Filter "UserPrincipalName eq 'Isabel-43846135@LODSPRODMCA.onmicrosoft.com'"
   ```

4. Führen Sie in der PowerShell-Sitzung im Bereich Cloud Shell den folgenden Befehl aus, um das Benutzerkonto von Isabel zur Gruppe Junior Admins43846135 hinzuzufügen:
   ```powershell
   Add-AzADGroupMember -MemberUserPrincipalName $user.userPrincipalName -TargetGroupDisplayName "Junior Admins43846135"
   ```

5. Führen Sie in der PowerShell-Sitzung im Bereich Cloud Shell den folgenden Befehl aus, um das Benutzerkonto von Isabel zur Gruppe Junior Admins43846135 hinzuzufügen:
   
   ```powershell
    Get-AzADGroupMember -GroupDisplayName "Junior Admins43846135"
    ```
   
> Ergebnis: Sie haben PowerShell verwendet, um einen Benutzer und ein Gruppenkonto zu erstellen, und das Benutzerkonto dem Gruppenkonto hinzugefügt. 

### Übung 3: Erstellen Sie eine Gruppe „Service Desk“, die das Benutzerkonto von Dylan Williams als Mitglied enthält.

#### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Verwenden der Azure CLI, um ein Benutzerkonto für Dylan Williams zu erstellen.
- Aufgabe 2: Verwenden der Azure CLI, um die Gruppe „Service Desk“ zu erstellen der Gruppe und das Benutzerkonto von Dylan hinzuzufügen. 

#### Aufgabe 1: Verwenden der Azure CLI, um ein Benutzerkonto für Dylan Williams zu erstellen.

In dieser Aufgabe erstellen Sie ein Benutzerkonto für Dylan Williams.

1. Wählen Sie im Dropdownmenü oben links im Cloud Shell-Bereich die Option **Bash** aus, und klicken Sie auf **Bestätigen**, wenn Sie dazu aufgefordert werden. 

2. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um den Namen Ihres Microsoft Entra-Mandanten zu ermitteln:

    ```cli
    DOMAINNAME=$(az ad signed-in-user show --query 'userPrincipalName' | cut -d '@' -f 2 | sed 's/\"//')
    ```

3. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um einen Benutzer zu erstellen: Dylan Williams. Verwenden Sie *yourdomain*.
 
    ```cli
    az ad user create --display-name "Dylan Williams" --password "Pa55w.rd1234" --user-principal-name Dylan@$DOMAINNAME
    ```
      
4. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um Microsoft Entra ID-Benutzerkonten aufzulisten (die Liste sollte die Benutzerkonten von Joseph, Isabel und Dylan enthalten).
    
    ```cli
    az ad user list --output table
    ```

#### Aufgabe 2: Verwenden der Azure CLI, um die Gruppe „Service Desk“ zu erstellen der Gruppe und das Benutzerkonto von Dylan hinzuzufügen. 

In dieser Aufgabe erstellen Sie die Gruppe „Service Desk“ und weisen der Gruppe Dylan zu. 

1. Führen Sie in der gleichen Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um eine neue Sicherheitsgruppe namens „Service Desk“ zu erstellen.

    ```cli
    az ad group create --display-name "Service Desk" --mail-nickname "ServiceDesk"
    ```
 
2. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um die Microsoft Entra ID-Gruppen aufzulisten (die Liste sollte die Gruppen „Service Desk“, „Senior Admins“ und „Junior Admins“ enthalten):

    ```cli
    az ad group list -o table
    ```

3. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um einen Verweis auf das Benutzerkonto von Dylan Williams abzurufen: 

    ```cli
    USER=$(az ad user list --filter "displayname eq 'Dylan Williams'")
    ```

4. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um die objectId-Eigenschaft des Benutzerkontos von Dylan Williams abzurufen: 

    ```cli
    OBJECTID=$(echo $USER | jq '.[].id' | tr -d '"')
    ```

5. Führen Sie in der Bash-Sitzung Cloud Shell-Bereich Folgendes aus, um der Gruppe „Service Desk“ das Benutzerkonto von Dylan hinzuzufügen: 

    ```cli
    az ad group member add --group "Service Desk" --member-id $OBJECTID
    ```

6. Führen Sie in der Bash-Sitzung im Cloud Shell-Bereich Folgendes aus, um Mitglieder der Gruppe „Service Desk“ aufzulisten und zu bestätigen, dass das Benutzerkonto von Dylan in ihr enthalten ist:

    ```cli
    az ad group member list --group "Service Desk"
    ```

7. Schließen Sie den Cloud Shell-Bereich.

> Ergebnis: Mit der Azure CLI haben Sie einen Benutzer und ein Gruppenkonto erstellt und der Gruppe das Benutzerkonto hinzugefügt. 


### Übung 4: Zuweisen der Rolle „VM-Mitwirkender“ zur Gruppe „Service Desk“.

#### Geschätzte Zeit: 10 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen einer Ressourcengruppe. 
- Aufgabe 2: Zuweisen der Service Desk-Berechtigung „VM-Mitwirkender“ zur Ressourcengruppe.  

#### Aufgabe 1: Erstellen einer Ressourcengruppe

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Ressourcengruppen** auf **+ Erstellen**, und geben Sie die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Abonnementname|Der Name Ihres Azure-Abonnements|
   |Ressourcengruppenname|**AZ500Lab01**|
   |Location|**USA, Osten**|

3. Klicken Sie auf **Überprüfen + erstellen** und danach auf **Erstellen**.

   >**Hinweis**: Warten Sie, bis die Ressourcengruppe bereitgestellt wurde. Verwenden Sie das **Benachrichtigungssymbol** (oben rechts), um den Fortschritt des Bereitstellungsstatus nachzuverfolgen.

4. Aktualisieren Sie auf dem Blatt **Ressourcengruppen** die Seite, und vergewissern Sie sich, dass Ihre neue Ressourcengruppe in der Liste der Ressourcengruppen angezeigt wird.


#### Aufgabe 2: Zuweisen der Service Desk-Berechtigungen „VM-Mitwirkender“. 

1. Klicken Sie auf dem Blatt **Ressourcengruppen** auf den Ressourcengruppeneintrag **AZ500LAB01**.

2. Klicken Sie auf dem Blatt **AZ500Lab01** im mittleren Bereich auf **Zugriffssteuerung (IAM)** .

3. Klicken Sie auf dem Blatt **AZ500Lab01 \| Zugriffssteuerung (IAM)** auf **+ Hinzufügen** und dann im Dropdownmenü auf **Rollenzuweisung hinzufügen**.

4. Nehmen Sie auf dem Blatt **Rollenzuweisung hinzufügen** die folgenden Einstellungen vor, bevor Sie auf „Weiter“ klicken:

   **Hinweis:** Klicken Sie nach Abschluss aller Schritte auf **Weiter**.

   |Einstellung|Wert|
   |---|---|
   |Rolle auf der Registerkarte „Suche“|**Mitwirkender von virtuellen Computern**|
   |Zuweisen des Zugriffs für (unter dem Bereich „Mitglieder“)|**Benutzer, Gruppe oder Dienstprinzipal**|
   |Auswählen (+ Mitglieder auswählen)|**Service Desk**|

6. Klicken Sie zweimal auf **Überprüfen und zuweisen**, um die Rollenzuweisung zu erstellen.

7. Wählen Sie auf dem Blatt **Zugriffssteuerung (IAM)** die Option **Rollenzuweisungen** aus.

8. Geben Sie auf dem Blatt **AZ500Lab01 \| Zugriffssteuerung (IAM)** auf der Registerkarte **Zugriff überprüfen** im Textfeld **Nach Name oder E-Mail-Adresse suchen** die Angabe **Dylan Williams** ein.

9. Wählen Sie in der Liste der Suchergebnisse das Benutzerkonto von Dylan Williams aus, und zeigen Sie auf dem Blatt **Dylan Williams Zuweisungen - AZ500Lab01** die neu erstellte Zuweisung an.

10. Schließen Sie das Blatt **Dylan Williams Zuweisungen - AZ500Lab01**.

11. Wiederholen Sie die beiden letzten Schritte, um den Zugriff für **Joseph Price** zu überprüfen. 

> Ergebnis: Sie haben RBAC-Berechtigungen zugewiesen und überprüft. 

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen.

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. 

2. Wählen Sie im Dropdownmenü oben links im Bereich „Cloud Shell“ die Option **PowerShell** aus, und klicken Sie bei der entsprechenden Aufforderung auf **Bestätigen**. 

3. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```
    Remove-AzResourceGroup -Name "AZ500LAB01" -Force -AsJob
    ```

4.  Schließen Sie den **Cloud Shell**-Bereich. 
