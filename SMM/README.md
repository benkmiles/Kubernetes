cat <<EOF > enable-dashboard-expose.yaml
spec:
  smm:
    exposeDashboard:
      meshGateway:
        enabled: true
    auth:
      forceUnsecureCookies: true
      mode: anonymous
EOF
kubectl patch controlplane --type=merge --patch "$(cat enable-dashboard-expose.yaml )" smm

smm operator reconcile

ingressip=$(kubectl get svc smm-ingressgateway-external -n smm-system --kubeconfig ~/.kube/demo1.kconf  -o jsonpath="{.status.loadBalancer.ingress[0].ip}")