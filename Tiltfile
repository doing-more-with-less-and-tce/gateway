WORKLOAD_NAME = 'gateway-default'
SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='docker.io/dashaun/gateway-default-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
  WORKLOAD_NAME,
  apply_cmd="tanzu apps workload apply -f tce/workload.yaml --live-update" +
            " --local-path " + LOCAL_PATH +
            " --source-image " + SOURCE_IMAGE +
            " --namespace " + NAMESPACE +
            " --yes >/dev/null" +
            " && kubectl get workload " + WORKLOAD_NAME + " --namespace " + NAMESPACE + " -o yaml",
  delete_cmd="tanzu apps workload delete -f tce/workload.yaml --namespace " + NAMESPACE + " --yes",
  deps=['pom.xml', './target/classes'],
  container_selector='workload',
  live_update=[
    sync('./target/classes', '/workspace/BOOT-INF/classes')
  ]
)

k8s_resource(WORKLOAD_NAME, port_forwards=["8080:8080"], extra_pod_selectors=[{'serving.knative.dev/service': WORKLOAD_NAME}])
allow_k8s_contexts('tanzu-community-edition')
