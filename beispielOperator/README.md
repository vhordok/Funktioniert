# Beispieloperator für die Demo am 02.09.2022
#
#
## in das Projekt gehen:
#
cd beispieleOperator
#
## Hier ist die Struktur des Projekts (wie zu sehen, handelt es sich um ein Go-Projekt): ##
#
ll -a
#
## main.go ist der Einstiegspunkt des Projekts; es richtet den Manager ein und führt ihn aus.
## config/ enthält die Manifeste für die Bereitstellung des Operators in Kubernetes.
## Dockerfile ist die Containerdatei, mit der das Image des Managers erstellt wird.
#



## api/v1 die unsere Foo CRD enthält  (siehe --> foo_types.go).
## controllers die unseren Foo controller (siehe --> foo_controller.go).
#
## Das CRD Foo hat in seiner Spezifikation ein Namensfeld, das sich auf den Namen des Freundes bezieht, den Foo sucht. Wenn Foo einen Freund findet (einen Pod mit demselben Namen wie sein Freund), wird sein Glücksstatus auf true gesetzt.
#
## Die API-Definitionen und des Controllers sind bereits im Programm erledigt, so dass wir den folgenden Befehl ausführen können, um die Betreibermanifeste zu aktualisieren. 
make manifests
#

## Ich verwende einen lokalen Kubernetes-Cluster, der mit minikube eingerichtet wurde. Es ist sehr einfach zu bedienen. Zuerst installieren wir die CRDs im Cluster.
make install
#
## Wir können sehen, dass das Foo CRD erstellt wurde:
kubectl get crds
#
## Dann führen wir den Controller im Terminal aus. Denke daran, dass wir ihn auch als Deployment im Kubernetes-Cluster einsetzen können:
make run
#
## Wie wir sehen können, wurde der Manager gestartet und dann der Foo-Controller gestartet. Der Controller läuft jetzt und wartet auf Ereignisse!
#
## Um zu testen, ob alles richtig funktioniert, benutzen wir die zwei benutzerdefinierten Foo-Ressourcen und Pods, um zu sehen, wie sich der Controller verhält.
#
## Führe den folgenden Befehl aus, um die Ressourcen in dem lokalen Kubernetes-Cluster zu erstellen:
kubectl apply -f config/samples
#
## Zu sehen ist ein Controller mit zwei Abstimmungsschleifen für jedes Ereignis zur Erstellung einer benutzerdefinierten Foo-Ressource.
#
## Wenn wir den Status der benutzerdefinierten Foo-Ressourcen überprüfen, können wir sehen, dass ihr Status leer ist. Das ist genau das, was wir erwarten, also ist so weit alles gut!:
kubectl describe foos
#
## Nun setzen wir einen Pod mit dem Namen jack ein, um zu sehen, wie das System reagiert.
#
## Danach sollten wir sehen, dass der Controller auf das Pod-Erstellungsereignis reagiert. Er aktualisiert dann den Status der ersten benutzerdefinierten Ressource Foo wie erwartet. Wir können dies selbst überprüfen, indem wir die benutzerdefinierten Ressourcen Foo beschreiben.
#
#
#
