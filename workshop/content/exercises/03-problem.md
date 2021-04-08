
1. Create a ConfigMap named ``my-data``{{copy}} with these two sets of key-value pairs: ``city: Paris``{{copy}}, and ``pastry: Baba au rhum``{{copy}}.  Create a Pod named ``my-web-server``{{copy}} using the image ``bitnami/nginx``{{copy}} that mounts the ConfigMap as a volume at ``/etc/app-data``{{copy}} (the volume name is of your choosing).

    ```examiner:execute-test
    name: vol-pod-cm
    title: Pod my-web-server running with ConfigMap mounted at `/etc/app-data`
    autostart: true
    ```

<div style="margin-top: 5em;"></div>

```section:begin
title: Solution
```

1. Create the ConfigMap.

    ```bash
    k create cm my-data --from-literal=city=Paris --from-literal=pastry="Baba au rhum"
    ```

1. Verify the ConfigMap was set properly.

    ```bash
    k get cm my-data -o jsonpath='{.data}'
    ```

1. Create a base yaml file for the pod spec.

    ```bash
    k run my-web-server --image=bitnami/nginx $DR > my-web-server.yaml
    ```

1. Edit the yaml file and add a volume for your ConfigMap and a volume mount for your container.  See [populate a volume with data stored in a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#populate-a-volume-with-data-stored-in-a-configmap).

    The final spec should be similar to the following.

    ```yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: my-web-server
      name: my-web-server
    spec:
      volumes:
      - name: my-data-volume
        configMap:
          name: my-data
      containers:
      - name: my-web-server
        image: bitnami/nginx
        volumeMounts:
        - name: my-data-volume
          mountPath: /etc/app-data
    ```

1. Apply the yaml.

    ```bash
    k apply -f my-web-server.yaml 
    ```

```section:end
```
