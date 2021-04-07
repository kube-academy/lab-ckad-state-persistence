
1. Create a pod named `vader` with image `bitnami/nginx`. Mount a volume named `vader-vol` at `/var/www/html`, which should live as long as pod lives.

    ```examiner:execute-test
    name: vol-pod-vader
    title: Pod vader with volume running?
    autostart: true
    ```

<div style="margin-top: 5em;"></div>

```section:begin
title: Solution
```

1. Begin by creating a pod spec from the command:

    ```bash
    k run vader --image=bitnami/nginx $DR > vader.yaml
    ```

1. Read about [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir), which is the type of volume you are asked to create.  The page also provides an example of how to define such a volume and how to mount it inside a container using the `mountPath` field.  Also, see [configuring a pod to use a volume for storage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/).

1. The final spec should resemble the following.

    ```yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: vader
      labels:
        run: vader
    spec:
      containers:
      - name: vader
        image: bitnami/nginx
        volumeMounts:
        - name: vader-vol
          mountPath: /var/www/html
      volumes:
      - name: vader-vol
        emptyDir: {}
    ```

1. Apply the yaml.

    ```bash
    k apply -f vader.yaml
    ```

```section:end
```
