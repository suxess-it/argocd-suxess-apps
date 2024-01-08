# suxess-apps

das repo wo wir deklarativ unsere apps über argocd deployen

# Konventionen

## Staging

Aktuell sind noch keine Staging-Konventionen implementiert.
Da wir hauptsächlich nur Plattform-Services betreiben, die es nur einmal im Cluster gibt, ist das aktuell auch noch nicht so dringend.
Will man eine Applikation in mehreren Stages deployen, muss man dafür getrennte Application-Defintions anlegen und sich um die Promotion selber kümmern.

## Application Definition

Aktuell wird für jede Applikation ein eigenes Application-Definition-YAML angelegt. ApplicationSets werden noch nicht verwendet weil wir noch keinen generischen Ansatz wissen, der für alle unsere Applikationen passt.
In weiterer Folge könnte es sein dass wir ApplicationSets einführen, sobald wir wissen wie so eine generische Application-Definition aussehen könnte.

Es zeichnet sich ab, dass alle unsere Plattform-Apps unter https://github.com/suxess-it/charts definiert werden. Vlt können wir hier in Zukunft mit ApplicationSets arbeiten.

### Helm-Charts

### Values-Files

aktuell haben alle Apps in ihren Directories unter https://github.com/suxess-it/charts ein eigenes values.yaml . Dadurch dass wir aktuell kein Staging haben, reicht dieses eine values.yaml aus.
Wichtig ist, dass keine values in den App-Definitions angegeben werden, sondern immer direkt in den values-Files bei der Applikation selbst (im https://github.com/suxess-it/charts).

### Wichtige Defaults

Um den gitops-Prinzipien treu zu bleiben sollte jede Applikation folgende Attribute gesetzt haben, außer es gibt gute Gründe dagegen:

```
metadata:
  finalizers:
  - resources-finalizer.argocd.argoproj.io
  [...]
spec:
  project: default
  [...]
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
    - CreateNamespace=true
```

### Konventionen

1. Es wrden keine Helm-Chart-Versionen oder Values direkt in AppDef angeben, die AppDef dienen also nur zum erstmaligen Onboarding der Apps, aber sollen in der laufenden Weiterentwicklung nicht ständig verändert werden müssen. Das hat den Vorteil dass Veränderungen dann nur in den App-Repos selbst stattfindet aber nicht in der Parent-App. D.h. auch 3rd-Party-Helm-Charts werden entweder nach https://github.com/suxess-it/charts geklont oder über ein Umbrella-Chart eingebunden.

4. sx- Prefix bei Appnamen? notwendig oder nicht?

5. Derzeit haben wir alle unsere Plattform-Apps unter https://github.com/suxess-it/charts jeweils in einem Unterverzeichnis. Lt. gängiger Praxis in der Community ist das ok. Andere Varianten wären ein Repo pro App, fühlt sich aber momentan zuviel an.


## Applikationen deaktivieren

Will man Applikationen temporär nicht deployen, können sie einfach mit dem Suffix ".yaml.disabled" umbenannt werden. Somit werden sie automatisch von ArgoCD ignoriert.




