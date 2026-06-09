# Network Troubleshooting

See the visual flow in the embedded diagrams in the root `README.md`.

Main scope:
- Pod-to-Pod connectivity
- Service routing
- DNS resolution
- CNI status
- kube-proxy rules

Reference: `DEMO/troubleshoot/03-NETWORK.md`.

## Stale k3s Node IP

Symptoms after the single-node host changes networks:

- `metrics-server` times out connecting to the Kubernetes API or kubelet.
- `node-exporter` probes target an address the host no longer owns.
- `kubectl top nodes` reports that the Metrics API is unavailable.
- The `kubernetes` service endpoint still contains the old node address.

Check the advertised address against the host:

```bash
ip -br addr
kubectl get node -o wide
kubectl get endpoints kubernetes -o wide
kubectl get apiservice v1beta1.metrics.k8s.io
```

For a single-node k3s lab, configure the reachable LAN address in
`/etc/rancher/k3s/config.yaml`:

```yaml
node-ip: 172.17.11.37
advertise-address: 172.17.11.37
tls-san:
  - 172.17.11.37
```

Restart and verify:

```bash
sudo systemctl restart k3s
kubectl rollout status deployment/metrics-server -n kube-system
kubectl top nodes
```

Use a DHCP reservation or stable address for a persistent cluster. Otherwise,
repeat this after the host address changes.
