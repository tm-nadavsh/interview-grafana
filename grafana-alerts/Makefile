deploy-devops:
	helm upgrade --install grafana-alerts ./ -n monitoring --wait

deploy-backend:
	helm upgrade --install grafana-alerts-backend ./ -n monitoring --wait
make template-local:
	helm template grafana-alerts ./ -n monitoring

