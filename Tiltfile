load('ext://configmap', 'configmap_create')
k8s_yaml('./configs/collector/collector.yaml')
k8s_yaml('./configs/collector/config.yaml')
k8s_yaml('./configs/prometheus/prometheus.yaml')
k8s_yaml('./configs/prometheus/config.yaml')
k8s_yaml('./configs/grafana/grafana.yaml')
configmap_create('tilt-grafana-config',
                 from_file=[
                   'grafana.ini=./configs/grafana/grafana.ini',
                   'dashboards.yaml=./configs/grafana/dashboards.yaml',
                   'datasource-prometheus.yaml=./configs/grafana/datasource-prometheus.yaml',
                 ])
configmap_create('tilt-grafana-dashboards',
                 from_file=[
                   'tiltfile-execution.json=./configs/grafana/tiltfile-execution.json',
                 ])

k8s_resource('tilt-local-metrics-collector',
             port_forwards=[
               port_forward(10351, 55678, name='receiver'),
               port_forward(10352, 55681),
             ],
             links=[
               link('http://localhost:10352/metrics', 'metrics'),
             ])

k8s_resource('tilt-local-metrics-prometheus',
             port_forwards=[
               port_forward(10353, 9090, name='prometheus'),
             ],
             resource_deps=['tilt-local-metrics-collector'])

k8s_resource('tilt-local-metrics-grafana',
             port_forwards=[
               port_forward(10354, 3000, name='grafana'),
             ],
             resource_deps=['tilt-local-metrics-prometheus'])

experimental_metrics_settings(
  enabled=True, address='localhost:10351', insecure=True, reporting_period='5s')
