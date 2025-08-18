# Overview

This repository contains a minimal Grafana alerting setup intended for an interview exercise.

## Current infra (what you have):

* kube-prometheus-stack installed with only Prometheus + Grafana enabled.

* Grafana alerts sidecar is enabled. It watches ConfigMaps (and/or Secrets) with a specific label and hot-reloads alerting definitions without restarting Grafana.

* Alerts are defined via Grafana file provisioning (YAML embedded in a ConfigMap). Right now there is a single alert that detects pod restarts using the kube_pod_container_status_restarts_total metric (requires kube-state-metrics).

* Notifications currently go to the default contact point(found in *grafna-infra* folder just for reference).

## Your task

Refactor the setup into a **generic Helm-based solution** so the *same* alert can be deployed to multiple **teams/environments** from one chart, with the following items configurable **per release**:

* **Namespace** filter used in the alert expression (e.g., kube_pod_container_status_restarts_total{namespace="devops/backend/prod"}).

* **Grafana datasource UID** used by the alert query (e.g., prometheus, so if we have different env's they have different datasource ID).

* **Alert name/title** (so a team/env specific, human-friendly name appears in Grafana).

* **Contact point routing** so alerts for each team/env go to the team’s own contact point (i.e., not the global default).

There are currently two contact points, **devops and backend**, We want to have a chart that is deployed twice, the first release should deploy this alert for the backend team with the changes stated above and any more needed.

The second release should be for the devops team and include the same as above but for the devops team.

#### Definition Of Done
1. We want to have 2 helm releases **deployed live on a single cluster!** so we will see the 2 new alerts for both teams with all correct configuration stated.
2. **IMPORTANT:** Currently this chart is deploying one simple alert for 2 teams, in the future, we will have tens of alerts for a combination of teams and environments and for multiple areas of alerting (pods, cronjobs, databases, argo applications etc...). 
We want to have next to the *templates* folder an *alerts* folder **from which we load the alerts to the configmap**, this way we can have stracture for our different alerts in the future. 
**For this exercise create the alert in *alerts/pods-health/pod-restarts.yaml***

## Notes & tips
* Port forward Grafana service/pod to log in(username and password is admin).
* Existing alerts can be [exported into YAML via GUI inside Grafana](https://grafana.com/docs/grafana/latest/alerting/set-up/provision-alerting-resources/export-alerting-resources/#export-from-the-grafana-ui). Alerting -> Alert rules -> choose folder -> More -> Export -> With modifications.
* [glob-patterns](https://helm.sh/docs/chart_template_guide/accessing_files/#glob-patterns)
* [helm tips and tricks](https://helm.sh/docs/howto/charts_tips_and_tricks)