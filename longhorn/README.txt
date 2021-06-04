Notes:

00-longhorn.yaml is a copy of https://raw.githubusercontent.com/longhorn/longhorn/v1.1.1/deploy/longhorn.yaml
Default number of replicas modified from 2 to 3

If you forgot to do this step, a work around is to create a custom storage class that had 2 replicas,
see 05-longhorn-custom-storage-class.yaml

The cert-manager setup must be completed first if creating the longhorn ingress with TLS.
