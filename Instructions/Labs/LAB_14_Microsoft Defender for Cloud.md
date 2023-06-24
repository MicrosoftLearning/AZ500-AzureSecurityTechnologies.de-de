---
lab:
  title: 14 – Microsoft Defender für Cloud
  module: Module 04 - Microsoft Defender for Cloud
---

# Lab 14: Microsoft Defender für Cloud
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden gebeten, einen Proof of Concept der Microsoft Defender für Cloud-basierten Umgebung zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Sie sollen Microsoft Defender für Cloud für die Überwachung eines virtuellen Computers konfigurieren.
- Sehen Sie sich die Empfehlungen von Microsoft Defender für Cloud für den virtuellen Computer an.
- Implementieren der Empfehlungen für die Gastkonfiguration und den Just-in-Time-VM-Zugriff. 
- Überprüfen, wie die Sicherheitsbewertung verwendet werden kann, um den Fortschritt bei der Erstellung einer sichereren Infrastruktur zu bestimmen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Microsoft Defender für Cloud

## Microsoft Defender für Cloud-Diagramm

![image](https://user-images.githubusercontent.com/91347931/157537800-94a64b6e-026c-41b2-970e-f8554ce1e0ab.png)

## Anweisungen

### Übung 1: Implementieren von Microsoft Defender für Cloud

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud
- Aufgabe 2: Überprüfen der Empfehlungen von Microsoft Defender für Cloud
- Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender für Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

#### Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud

In dieser Aufgabe werden Sie Microsoft Defender für Cloud einführen und konfigurieren.

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben im Textfeld **Ressourcen, Dienste und Dokumente durchsuchen** **Microsoft Defender für Cloud** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Microsoft Defender for Cloud \| Erste Schritte** auf **Upgrade**.
     
4. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Erste Schritte** auf der Registerkarte „Agents installieren“ nach unten, und klicken Sie auf „Agents installieren“. 

5. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \|Erste Schritte** auf der Registerkarte **Upgrade** >> so lange nach unten, bis der Abschnitt **Arbeitsbereiche mit erweiterten Sicherheitsfeatures auswählen** angezeigt wird >> aktivieren Sie den **Microsoft Defender-Tarif**, indem Sie Ihren Log Analytics-Arbeitsbereich auswählen, und klicken Sie auf die große blaue Upgrade-Schaltfläche.  

    >**Hinweis**: Überprüfen Sie alle Features, die als Teil von Microsoft Defender-Tarifen verfügbar sind. 

6. Navigieren Sie zu **Microsoft Defender für Cloud**, und klicken Sie unter den Verwaltungseinstellungen in der vertikalen Menüleiste auf der linken Seite auf **Umgebungseinstellungen**.

7. Klicken Sie auf dem Blatt **Microsoft Defender für Cloud \| Umgebungseinstellungen** auf das entsprechende Abonnement. 

8. Wählen Sie auf dem Blatt **Defender-Tarife** die Option **Alle Microsoft Defender for Cloud-Tarife aktivieren** aus.

9. Navigieren Sie zurück zum Blatt **Microsoft Defender for Cloud \| Umgebungseinstellungen**, erweitern Sie es, bis Ihr Abonnement angezeigt wird, und klicken Sie auf den Eintrag, der den Log Analytics-Arbeitsbereich darstellt, den Sie im vorherigen Lab erstellt haben.

10. Stellen Sie sicher, dass auf dem Blatt **Einstellungen\| Defender-Tarife** die Option **Alle Microsoft Defender for Cloud-Tarife aktivieren** ausgewählt ist.

11. Wählen Sie auf dem Blatt **Microsoft Defender für Cloud**  Einstellungen **die Option \|Datensammlung** aus. Wählen Sie **Alle Ereignisse** und **Speichern** aus.

#### Aufgabe 2: Überprüfen der Empfehlung von Microsoft Defender für Cloud

In dieser Aufgabe überprüfen Sie die Empfehlungen von Microsoft Defender für Cloud. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**. 

2. Überprüfen Sie auf dem Blatt **Microsoft Defender für Cloud \| Übersicht** die Kachel **Sicherheitsbewertung**.

    >**Hinweis**: Notieren Sie sich die aktuelle Bewertung, sofern verfügbar.

3. Navigieren Sie zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**, und wählen Sie **Assessed resources** (Bewertete Ressourcen) aus.

4. Wählen Sie auf dem Blatt **Bestand** den Eintrag **myVM** aus.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Browserseite aktualisieren, bis der Eintrag angezeigt wird.
    
5. Überprüfen Sie auf dem Blatt **Ressourcenzustand** auf der Registerkarte **Empfehlungen** die Liste der Empfehlungen für **myVM**.

#### Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender für Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

In dieser Aufgabe implementieren Sie die Empfehlung von Microsoft Defender für Cloud, um den Just-in-Time-VM-Zugriff zu aktivieren. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**, und wählen Sie unter der Kachel **Cloud Security** (Cloudsicherheit) die Option **Workload protections** (Workloadschutz) aus.

2. Klicken Sie auf dem Blatt **Workloadschutz** im Abschnitt **Erweiterter Schutz** auf die Kachel **Just-In-Time-VM-Zugriff** und auf das Blatt **Just-in-Time-VM-Zugriff**.

3. Wählen Sie auf dem Blatt **Just-in-Time-VM-Zugriff** im Abschnitt **Virtuelle Computer** die Option **Nicht konfiguriert** aus, und klicken Sie dann auf den Eintrag **myVM**.

4. Klicken Sie ganz rechts im Abschnitt **Virtuelle Computer** auf die Option **JIT auf 1 VM** aktivieren.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, bis der Eintrag **myVM** verfügbar wird.

5. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** ganz rechts in der Zeile, die auf Port **22** verweist, auf die Schaltfläche mit den Auslassungspunkten und dann auf **Löschen**.

6. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Speichern**.

    >**Hinweis**: Überwachen Sie den Status der Konfiguration, indem Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** klicken, um das Blatt **Benachrichtigungen** anzuzeigen. 

    >**Hinweis**: Es kann einige Zeit dauern, bis die Implementierung von Empfehlungen in diesem Lab durch die Sicherheitsbewertung widergespiegelt wird. Überprüfen Sie regelmäßig die Sicherheitsbewertung, um die Auswirkungen der Implementierung dieser Features zu ermitteln. 

> Ergebnisse: Sie haben Microsoft Defender für Cloud eingeführt und Empfehlungen für virtuelle Computer implementiert. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.
