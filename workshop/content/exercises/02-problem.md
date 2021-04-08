
1. Create a pod ``dooku``{{copy}} with two containers using the images ``bitnami/redis``{{copy}} and ``bitnami/nginx``{{copy}}.
   Create an ``emptyDir``{{copy}} scratch volume named ``dooku-logs``{{copy}} mounted at ``/var/log/dooku``{{copy}} in both containers.

    ```examiner:execute-test
    name: vol-pod-dooku
    title: Pod dooku running with two containers and a shared volume mount?
    autostart: true
    ```

    _Note_: Be sure to review the [`hostPath`](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath) and other volume types as you prepare for the exam.

<div style="margin-top: 5em;"></div>

```section:begin
title: Solution
```

1. Begin with a pod yaml spec with a single container:

    ```bash
    k run dooku --image=bitnami/nginx $DR > dooku.yaml
    ```

1. Add the spec for the second (redis) container (with env var REDIS_PASSWORD).

1. To each container spec, add the `volumeMounts` section.

1. Finally, on the pod spec, add the `volumes` section defining the `dooku-logs` volume of type `emptyDir`.

1. The final yaml should resemble the following

    ```yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: dooku
      name: dooku
    spec:
      containers:
      - name: nginx
        image: bitnami/nginx
        volumeMounts:
        - name: dooku-logs
          mountPath: /var/log/dooku
      - name: redis
        image: bitnami/redis
        env:
        - name: REDIS_PASSWORD
          value: x
        volumeMounts:
        - name: dooku-logs
          mountPath: /var/log/dooku
      volumes:
      - name: dooku-logs
        emptyDir: {}
    ```

1. Apply the yaml.

    ```bash
    k apply -f dooku.yaml
    ```

```section:end
```
