RP sits in DMZ

6.6.2025
Kubernetes 1.31 fully linked and ready
9.6.2025
discovered HA was not enabled
10.6.2025
disassembled kubernetes 1.31 and upgraded to 1.32
HA was enabled
swap was turned off, directory ~/scrips a scp looping script was made to turn swap off on all remaining nodes
15.6.2025
Added a new Kubernetes Cluster node: UI (Pi 4 with a touchscreen GUI)
Created microk8s dashboard-proxy and sucessfully tested dashboard-proxy locally as well as remotely, documentation for access added to docs
16.6.2025
Researching kubernetes application and created roadmap for self-hosted services
17.6.2025
Updated inconsistency with hostnames on Odrerir, a Pi 4 node. relabelled roles on kubernetes cluster

Inventory
Dell
24 TB 
64 gb ECC ram

🧠 1. Core Cluster Management & GitOps
Tool	                Purpose	                                      Architecture	                      Install on
MicroK8s	            Lightweight Kubernetes distribution	          ARM64 + x86_64	                    All nodes
FluxCD or ArgoCD	    Declarative app management	                  ARM64 + x86_64	                    Master (x86) (preferred)
Sealed Secrets	      Encrypt secrets for GitOps	                  ARM64 + x86_64	                    Master (x86) or shared
KubeStateMetrics	    Export Kubernetes state metrics	              ARM64 + x86_64	                    Master (x86)

🧱 2. Storage & Backup
Tool	                Purpose	                                      Architecture	                      Install on
OpenEBS	              CSI volume provisioning (ZFS + ext4)	        ARM64 + x86_64	                    ZFS engine on Master, hostpath engine on Pi 5s
Velero	              Kubernetes-native backup + restore	          ARM64 + x86_64	                    Backup Pi node (preferred), Master (x86) for archive location
Restic or Kopia	      File-level PVC backups	                      ARM64 + x86_64	                    Used by Velero on Backup Pi node
ZFS	                  Native volume + snapshot system	              x86_64 only	                        Master (x86) only
CSI snapshot CRDs	    Enables snapshot management	                  Architecture-independent	          All nodes
MinIO (optional)	    S3-compatible object storage	                ARM64 + x86_64	                    Master (x86) or Backup Pi node if you want remote backup format

📡 3. Observability & Monitoring
Tool	                              Purpose	                        Architecture	                      Install on
Prometheus	                        Metrics & cluster health	        ✅	                              Master (x86)
Grafana	                            Dashboards	                      ✅	                              Master (x86)
Node Exporter	                      Node-level metrics	              ✅	                              All nodes
Alertmanager	                      Send alerts	                      ✅	                              Master (x86)
KubeLens / Lens IDE (optional)	    GUI for cluster	                  ✅	                              Local dev machine

🔁 4. Job Scheduling & Automation
Tool	                              Purpose	                        Architecture	                     	Install on
CronJobs	                          Schedule GC, snapshots, syncs	    ✅	                              Backup Pi node & Master (x86)
Rsync	                              Copy snapshots to Odin	          ✅	                              Backup Pi node & Master (x86)
Kustomize	                          Declarative overlay management	  ✅	                              Dev workstation
Helm	                              App install & config templating	  ✅	                              Master (x86) (for mgmt)

🌐 5. Access & Control
Tool	                              Purpose                        	Architecture		                    Install on
Traefik or NGINX Ingress	          Routing apps	                    ✅	                              Master (x86)
Cert-Manager	                      TLS management	                  ✅	                              Master (x86)
OAuth2 Proxy	                      Protect services with auth	      ✅	                              Master (x86)
