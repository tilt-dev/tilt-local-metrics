
k8s_yaml('./configs/collector/collector.yaml')
k8s_yaml('./configs/collector/config.yaml')
k8s_yaml('./configs/prometheus/prometheus.yaml')
k8s_yaml('./configs/prometheus/config.yaml')

k8s_resource('tilt-local-metrics-collector',
             port_forwards=[
               port_forward(55678, 55678, name='receiver'),
               port_forward(55681, 55681),
             ],
             links=[
               link('http://localhost:55681/metrics', 'metrics'),
             ])

k8s_resource('tilt-local-metrics-prometheus',
             port_forwards=[
               port_forward(9090, 9090, name='prometheus'),
             ],
             resource_deps=['tilt-local-metrics-collector'])

experimental_metrics_settings(
  enabled=True, address='localhost:55678', insecure=True, reporting_period='5s')
