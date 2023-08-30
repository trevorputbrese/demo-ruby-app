LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='global')

k8s_custom_deploy(
   'immigration-web',
   apply_cmd="tanzu apps workload apply -f tap/workload.yaml --live-update" +
       " --local-path " + LOCAL_PATH +
       " --namespace " + NAMESPACE +
       " --yes >/dev/null" +
       " && kubectl get workload immigration-web --namespace " + NAMESPACE + " -o yaml",
   delete_cmd="tanzu apps workload delete -f tap/workload.yaml --namespace " + NAMESPACE + " --yes" ,
   deps=['app.rb', 'Gemfile'],
   container_selector='workload',
   live_update=[
       sync('./', '/workspace/')
   ]
)

k8s_resource('immigration-web', port_forwards=["8080:8080"],
   extra_pod_selectors=[{'carto.run/workload-name': 'immigration-web', 'app.kubernetes.io/component': 'run'}])
allow_k8s_contexts('aks-eus-tap-v1-6-cluster-4')