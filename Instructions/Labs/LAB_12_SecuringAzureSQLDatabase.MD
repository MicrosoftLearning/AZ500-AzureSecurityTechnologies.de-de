---
lab:
  title: 12 – Schützen von Azure SQL-Datenbank
  module: Module 03 - Secure data and applications
---

# Lab 12: Schützen von Azure SQL-Datenbank
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie sollen die Sicherheitsfeatures für Azure SQL-Datenbank überprüfen. Insbesondere interessieren Sie sich für Folgendes:

- Schutz vor Angriffen wie die Einschleusung von SQL-Befehlen und Datenexfiltration. 
- Möglichkeit zum Erkennen und Klassifizieren von Datenbankinformationen in Kategorien wie „Vertraulich“. 
- Möglichkeit zum Überwachen von Datenbankserver- und Datenbankabfragen und relevanten Protokollereignissen. 

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Sicherheitsfeatures für SQL-Datenbank

## Schützen von Azure SQL-Datenbank (Diagramm)

![image](https://user-images.githubusercontent.com/91347931/157533836-7250fa58-a109-4882-a55b-d3fa3baf34ab.png)

## Anweisungen

## Labdateien:

- **\\Allfiles\\Labs\\11\\azuredeploy.json**

### Übung 1: Implementieren von Sicherheitsfeatures für SQL-Datenbank

### Geschätzte Dauer: 30 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Bereitstellen einer Azure SQL-Datenbank-Instanz
- Aufgabe 2: Konfigurieren von Advanced Data Protection
- Aufgabe 3: Konfigurieren der Datenklassifizierung
- Aufgabe 4: Konfigurieren der Überwachung

#### Aufgabe 1: Bereitstellen einer Azure SQL-Datenbank-Instanz

In dieser Aufgabe verwenden Sie eine Vorlage, um die Labinfrastruktur bereitzustellen. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

4. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\11\\azuredeploy.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und achten Sie darauf, dass damit eine Azure SQL-Datenbank bereitgestellt wird.

5. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

6. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB11** ein.|
   |Standort|**(USA) USA, Osten**|

7. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. 

#### Aufgabe 2: Konfigurieren von Advanced Data Protection

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB11**.

3. Klicken Sie auf dem Blatt **AZ500LAB11** auf den Eintrag, der die neu erstellte SQL Server-Instanz darstellt.

4. Klicken Sie auf dem Blatt „SQL Server“ im Abschnitt **Sicherheit** auf **Microsoft Defender für Cloud**, und wählen Sie **Enable Microsoft Defender for SQL** (Microsoft Defender für SQL aktivieren) aus.

      >**Hinweis**: Warten Sie, bis die Benachrichtigung anzeigt, dass Azure Defender erfolgreich für SQL aktiviert wurde.

5. Klicken Sie auf dem Blatt „SQL Server“ im Abschnitt **Sicherheit** auf der Seite **Microsoft Defender for Cloud** im Parameter **Microsoft Defender for SQL: Enabled at the subscription-level (Configure)** (Microsoft Defender für SQL: auf Abonnementebene aktiviert (Konfigurieren)) auf **(configure)** (konfigurieren).  

      >**Hinweis**: Aktualisieren Sie den Browser, wenn **(configure)** nicht angezeigt wird.
    
6. Lesen Sie auf dem Blatt **Servereinstellungen** die Informationen zu Preisen und zum Testzeitraum, und überprüfen Sie die **EINSTELLUNGEN FÜR DIE SICHERHEITSRISIKOBEWERTUNG** und die **ADVANCED THREAT PROTECTION-EINSTELLUNGEN**.

7. Überprüfen Sie auf dem Blatt **Microsoft Defender für Cloud** die **Empfehlungen** und **Sicherheitswarnungen**.

      >**Hinweis**: Es kann 10-15 Minuten dauern, bis auf dem Blatt **Microsoft Defender für Cloud** Empfehlungen angezeigt werden. Anstatt zu warten, können Sie mit der nächsten Aufgabe fortfahren. Kehren Sie aber zu diesem Blatt zurück, sobald Sie alle verbleibenden Aufgaben abgeschlossen haben.


#### Aufgabe 3: Konfigurieren der Datenklassifizierung

In dieser Aufgabe ermitteln und klassifizieren Sie Informationen in SQL-Datenbank in Bezug auf DSGVO- und Datenschutzcompliance.

1. Klicken Sie auf dem Blatt „SQL Server“ im Abschnitt **Einstellungen** auf **SQL-Datenbanken**.

2. Wählen Sie in der Liste der Datenbanken den Eintrag **AZ500LabDb** aus. 

3. Klicken Sie auf dem Blatt für die SQL-Datenbank **AZ500LabDb** im Abschnitt **Sicherheit** auf **Datenermittlung und -klassifizierung**.

4. Klicken Sie auf dem Blatt **Datenermittlung und -klassifizierung** auf die Registerkarte **Klassifizierung**.

    >**Hinweis**: Das Klassifizierungsmodul überprüft Ihre Datenbank auf Spalten, die potenziell vertrauliche Daten enthalten, und erstellt eine Liste der empfohlenen Spaltenklassifizierungen.

5. Klicken Sie auf die Textmeldung **Wir haben 15 Spalten mit Klassifizierungsempfehlungen gefunden**, die oben auf dem Blatt auf der blauen Leiste angezeigt wird.

6. Überprüfen Sie die aufgelisteten Spalten und die empfohlene Vertraulichkeitsbezeichnung. 

7. Aktivieren Sie das Kontrollkästchen **Alle auswählen**, und klicken Sie dann auf **Ausgewählte Empfehlungen akzeptieren**.

    >**Hinweis**: Alternativ können Sie nur bestimmte Spalten auswählen und andere verwerfen. 

    >**Hinweis**: Sie haben die Möglichkeit, den Informationstyp und die Vertraulichkeitsbezeichnung zu ändern. 

8. Klicken Sie nach Abschluss der Überprüfung auf **Speichern**. 

    >**Hinweis**: Dadurch wird die Klassifizierung abgeschlossen, und die Datenbankspalten werden dauerhaft mit den neuen Klassifizierungsmetadaten bezeichnet. 

9. Beachten Sie auf dem Blatt **Data Discovery & Classification** (Datenermittlung und -klassifizierung) auf der Registerkarte **Übersicht**, dass die Aktualisierung unter Berücksichtigung der neuesten Klassifizierungsinformationen durchgeführt wurde. 

#### Aufgabe 4: Konfigurieren der Überwachung 

In dieser Aufgabe konfigurieren Sie zuerst die Überwachung auf Serverebene und dann die Überwachung auf Datenbankebene. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt „SQL Server“.

2. Klicken Sie auf dem Blatt „SQL Server“ im Abschnitt **Sicherheit** auf **Überwachung**.

    >**Hinweis**: Dies ist die Überwachung auf Serverebene. Die Standardüberwachungseinstellungen umfassen alle für die Datenbank ausgeführten Abfragen und gespeicherten Prozeduren sowie erfolgreiche und fehlerhafte Anmeldungen.

3. Legen Sie die Option **Enable Azure SQL Auditing** (Azure SQL-Überwachung aktivieren) auf **EIN** fest, um die Überwachung zu aktivieren. 

4. Aktivieren Sie das Kontrollkästchen **Speicher**. Daraufhin werden die Eingabefelder für **Abonnement** und **Speicherkonto** angezeigt.

5. Wählen Sie in der Dropdownliste Ihr **Abonnement** aus.

6. Klicken Sie auf **Speicherkonto**, und wählen Sie **Neu erstellen** aus.

7. Geben Sie auf dem Blatt **Speicherkonto erstellen** im Feld **Name** einen global eindeutigen Namen ein, der aus 3 bis 24 Kleinbuchstaben und Ziffern besteht, und klicken Sie auf **OK**. 

    >**Hinweis**: Möglicherweise müssen Sie den Browser aktualisieren, bevor das Speicherkonto verfügbar wird.

8. Legen Sie auf dem Blatt **Überwachung** unter **Erweiterte Einstellungen** die Option **Aufbewahrung (Tage)** auf **5** fest. 

9. Klicken Sie anschließend auf dem Blatt „Überwachung“ auf **Speichern**, um die Überwachungseinstellungen zu speichern. 

    >**Hinweis**: Wenn Sie eine Fehlermeldung zu einem ungültigen Speichercontainerpfad erhalten, wurde das Speicherkonto möglicherweise noch nicht bereitgestellt. Warten Sie einige Minuten, klicken Sie auf **Speicherkonto**, und wählen Sie auf dem Blatt **Speicherkonto auswählen** das neu erstellte Speicherkonto aus. Klicken Sie anschließend auf dem Blatt „Überwachung“ auf **Speichern**.

10. Klicken Sie auf dem Serverblatt im Abschnitt **Einstellungen** auf **SQL-Datenbanken**.

11. Wählen Sie in der Liste der Datenbanken den Eintrag **AZ500LabDb** aus. 

12. Klicken Sie auf dem Blatt für die SQL-Datenbank **AZ500LabDb** im Abschnitt **Sicherheit** auf **Überwachung**. 

    >**Hinweis**: Dies ist die Überwachung auf Datenbankebene. Die Überwachung auf Serverebene ist bereits aktiviert. 
  
    >**Hinweis**: Überwachungen können in ein Azure-Speicherkonto, in einen Log Analytics-Arbeitsbereich oder in den Event Hub geschrieben werden. Sie können eine beliebige Kombination dieser Optionen konfigurieren.

    >**Hinweis**: Wenn die speicherbasierte Überwachung auf dem Server aktiviert ist, wird sie unabhängig von den Datenbankeinstellungen immer auf die Datenbank angewendet.

13. Klicken Sie auf **Überwachungsprotokolle anzeigen**.

14. Beachten Sie auf dem Blatt **Überwachungsdatensätze**, dass Sie zwischen Serverüberwachung und Datenbanküberwachung wechseln können. 

    >**Hinweis**: Da diese SQL Server-Instanz und diese SQL-Datenbank erst kürzlich erstellt wurden, ist es unwahrscheinlich, dass zu diesem Zeitpunkt Ereignisse verfügbar sind. 

> Ergebnisse: Sie haben eine SQL Server-Instanz und eine SQL-Datenbank erstellt sowie die Datenklassifizierung und die Überwachung konfiguriert. 


**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

2. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB11" -Force -AsJob
    ```
4. Schließen Sie den **Cloud Shell**-Bereich. 
