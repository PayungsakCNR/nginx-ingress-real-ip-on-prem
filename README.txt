แก้ Deployment ให้ deploy nginx ingress ซัก node สั่ง lock ไว้ให้มัน deploy ใน node นั้นๆ

spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/hostname
                operator: In
                values:
                - pclo-k8s-2


แก้ Service
externalTrafficPolicy: Local
type: NodePort
externalIPs:
  - 10.200.202.11

externalIPs ใส่ IP ของ Node

แก้ configmap data

apiVersion: v1
data:
  X-Forwarded-For: $proxy_add_x_forwarded_for;
  X-Forwarded-Proto: $proxy_x_forwarded_proto;
  X-Real-IP: $remote_addr;
  allow-snippet-annotations: "true"
  enable-real-ip: "true"
  forwarded-for-header: proxy_protocol
  proxy-real-ip-cidr: 0.0.0.0/0
  real-ip-header: proxy_protocol
  use-forwarded-headers: "true"
  use-proxy-protocol: "false"
kind: ConfigMap


