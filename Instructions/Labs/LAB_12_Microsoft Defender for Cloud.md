---
lab:
  title: 12 – Microsoft Defender for Cloud
  module: Module 04 - Microsoft Defender for Cloud
---

# Lab 12: Microsoft Defender for Cloud
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden gebeten, einen Proof of Concept der Microsoft Defender für Cloud-basierten Umgebung zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Sie sollen Microsoft Defender für Cloud für die Überwachung eines virtuellen Computers konfigurieren.
- Sehen Sie sich die Empfehlungen von Microsoft Defender für Cloud für den virtuellen Computer an.
- Implementieren Sie die Empfehlungen für die Gastkonfiguration und den Just-In-Time-VM-Zugriff. 
- Überprüfen, wie die Sicherheitsbewertung verwendet werden kann, um den Fortschritt bei der Erstellung einer sichereren Infrastruktur zu bestimmen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Microsoft Defender für Cloud

## Microsoft Defender für Cloud-Diagramm

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/c31055cc-de95-41f6-adef-f09d756a68eb)

## Anweisungen

### Übung 1: Implementieren von Microsoft Defender für Cloud

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud
- Aufgabe 2: Überprüfen der Empfehlungen von Microsoft Defender für Cloud
- Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender for Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

#### Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud

In dieser Aufgabe werden Sie Microsoft Defender für Cloud einführen und konfigurieren.

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben im Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** **Microsoft Defender für Cloud** ein, und drücken Sie die **EINGABETASTE**.

3. Wählen Sie im linken Navigationsbereich **Erste Schritte** aus. Klicken Sie auf dem Blatt **Microsoft Defender for Cloud \| Erste Schritte** auf **Upgrade**.
     
4. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Erste Schritte** auf der Registerkarte „Agents installieren“ nach unten und klicken Sie auf **Agents installieren**. 

5. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \|Erste Schritte** auf der Registerkarte **Upgrade** >> so lange nach unten, bis der Abschnitt **Arbeitsbereiche mit erweiterten Sicherheitsfeatures auswählen** angezeigt wird >> aktivieren Sie den **Microsoft Defender-Tarif**, indem Sie Ihren Log Analytics-Arbeitsbereich auswählen, und klicken Sie auf die große blaue Upgrade-Schaltfläche.  

    >**Hinweis**: Überprüfen Sie alle Features, die als Teil von Microsoft Defender-Tarifen verfügbar sind. 

6. Navigieren Sie zu **Microsoft Defender for Cloud** und klicken Sie im linken Navigationsbereich unter dem Abschnitt Verwaltung auf **Umgebungseinstellungen**.

7. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Umgebungseinstellungen** nach unten, erweitern Sie die Punkte, bis Ihr Abonnement angezeigt wird, und klicken Sie auf das entsprechende Abonnement. 

8. Wählen Sie auf dem Blatt **Einstellungen \| Azure Defender-Pläne** die Option **Alle Pläne aktivieren** aus und klicken Sie auf **Speichern**.

9. Navigieren Sie zurück zum Blatt **Microsoft Defender for Cloud \| Umgebungseinstellungen** und erweitern Sie die Punkte, bis Ihr Abonnement angezeigt wird. Klicken Sie dann auf den Eintrag des Log Analytics-Arbeitsbereichs, den Sie im vorherigen Lab erstellt haben.

10. Stellen Sie auf dem Blatt **Einstellungen \| Defender-Pläne** sicher, dass alle Optionen „Ein“ sind. Klicken Sie bei Bedarf auf **Alle Pläne aktivieren** und dann auf **Speichern**.

11. Wählen Sie **Datensammlung** auf dem Blatt **Einstellungen \| Defender-Pläne** aus. Klicken Sie auf **Alle Ereignisse** und dann auf **Speichern**.

#### Aufgabe 2: Überprüfen der Empfehlung von Microsoft Defender für Cloud

In dieser Aufgabe überprüfen Sie die Empfehlungen von Microsoft Defender für Cloud. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**. 

2. Überprüfen Sie auf dem Blatt **Microsoft Defender für Cloud \| Übersicht** die Kachel **Sicherheitsbewertung**.

    >**Hinweis**: Notieren Sie sich die aktuelle Bewertung, sofern verfügbar.

3. Navigieren Sie zurück zum Blatt **Microsoft Defender for Cloud \| Übersicht** und wählen Sie **Bewertete Ressourcen** aus.

4. Wählen Sie auf dem Blatt **Bestand** den Eintrag **myVM** aus.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Browserseite aktualisieren, bis der Eintrag angezeigt wird.
    
5. Überprüfen Sie auf dem Blatt **Ressourcenzustand** auf der Registerkarte **Empfehlungen** die Liste der Empfehlungen für **myVM**.

#### Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender for Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

In dieser Aufgabe implementieren Sie die Empfehlung von Microsoft Defender for Cloud, um den Just-in-Time-VM-Zugriff zu aktivieren. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender for Cloud \| Übersicht** und wählen Sie dort die Option **Workloadschutz** aus (zu finden unter **Cloudsicherheit**).

2. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Workloadschutz** nach unten zum Abschnitt **Erweiterter Schutz** und klicken Sie auf die Kachel **Just-in-Time-VM-Zugriff**.

3. Wählen Sie auf dem Blatt **Just-in-Time-VM-Zugriff** im Abschnitt **VMs** die Option **Nicht konfiguriert** aus und klicken Sie dann auf den Eintrag **myVM**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, die Browserseite aktualisieren und **Nicht konfiguriert** auswählen, dass der Eintrag angezeigt wird.

4. Klicken Sie ganz rechts im Abschnitt **Virtuelle Computer** auf die Option **JIT auf 1 VM** aktivieren.

5. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** ganz rechts in der Zeile, die auf Port **22** verweist, auf die Schaltfläche mit den Auslassungspunkten und dann auf **Löschen**.

6. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Speichern**.

    >**Hinweis**: Überwachen Sie den Status der Konfiguration, indem Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** klicken, um das Blatt **Benachrichtigungen** anzuzeigen. 

    >**Hinweis**: Es kann einige Zeit dauern, bis die Implementierung von Empfehlungen in diesem Lab durch die Sicherheitsbewertung widergespiegelt wird. Überprüfen Sie regelmäßig die Sicherheitsbewertung, um die Auswirkungen der Implementierung dieser Features zu ermitteln. 

> Ergebnisse: Sie haben Microsoft Defender für Cloud eingeführt und Empfehlungen für virtuelle Computer implementiert. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.
