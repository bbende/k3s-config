# Longhorn

`00-longhorn.yaml` is a copy of the original  [v1.1.1/deploy/longhorn.yaml](https://raw.githubusercontent.com/longhorn/longhorn/v1.1.1/deploy/longhorn.yaml) with the default number of replicas modified from 2 to 3.

An alternative is to create a custom storage class that has 2 replicas. For an example, see `05-longhorn-custom-storage-class.yaml`.

If creating the longhorn ingress with TLS, then the `cert-manager` setup must be completed first.
