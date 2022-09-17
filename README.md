# Creating a new standalone microk8s server

These commands were tested using Ubuntu 22.04.1 LTS distro.

1. Make sure you can use snaps. https://snapcraft.io/docs/installing-snapd 
2. Install `microk8s`. https://microk8s.io/#install-microk8s 
3. Allow your user to run microk8s
	`sudo usermod -a -G microk8s $USER`	
    `sudo chown -f -R $USER ~/.kube`
4. Enable modules ( dns | metallb | community | istio )
5. Install cert manager
	`kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml`
6. Check cluster status
	`kubectl get all -A`
7. Install a sample webserver.  (https://github.com/rasueno/whoami)  
    1. Create whomi namespace
    2. Create service and deployment
		`kubectl apply -n whomi -f whoami.yml`
    3. Create gateway
    4. Create virtualservice
    5. Going to the load balancer IP should now present the whomi site
8. Create cluster issuer 
	`kubectl apply -n whomi -f clusterissuer.yaml`
9. Create certificate
  `kubectl apply -n istio-system -f certificate-raffy.ga.yaml`
10. Update gateway
  `kubectl apply -n whoami -f gateway-whoami-tls.yaml`
