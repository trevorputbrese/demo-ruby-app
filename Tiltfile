LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='global')
WORKLOAD_NAME = 'immigration-web-iterate'

k8s_custom_deploy(
   WORKLOAD_NAME,
   apply_cmd="tanzu apps workload apply -f tap/workload-iterate.yaml --live-update" +
       " --local-path " + LOCAL_PATH +
       " --namespace " + NAMESPACE +
       " --yes >/dev/null" +
       " && kubectl get workload " + WORKLOAD_NAME + " --namespace " + NAMESPACE + " -o yaml",
   delete_cmd="tanzu apps workload delete -f tap/workload-iterate.yaml --namespace " + NAMESPACE + " --yes" ,
   deps=['app', 'public', 'config'],
   container_selector='workload',
   live_update=[
       sync('./app', '/workspace/app'),
       sync('./config', '/workspace/config'),
       sync('./public', '/workspace/public')
   ]
)

k8s_resource(WORKLOAD_NAME, port_forwards=["8080:8080"],
   extra_pod_selectors=[{'carto.run/workload-name': WORKLOAD_NAME, 'app.kubernetes.io/component': 'run'}])
allow_k8s_contexts('aks-eus-tap-v1-6-cluster-4')