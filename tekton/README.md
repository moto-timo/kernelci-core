KernelCI Tekton
---------------

This is a POC/WIP inspired by the kernelci-jenkins jobs but implemented in Tekton

For now, see the README.md in https://github.com/threexc/yocto-tekton for some guidance to get a single-node cluster up and running.

Once you have Kubernetes up and running, you should be able to set up the Persistent Volume and the Persistent Volume Claim, which is where the git clones, git mirrors and kernel builds will happen in the current implementation.

NOTE: you can also use a URL to these files e.g.
kubectl apply -f https://raw.githubusercontent.com/moto-timo/kernelci-core/tekton/tekton/pv.yaml

You may want to edit pv.yaml to set the volume (directory in this case) that you want to use. On our systems, we have a (symlink) to a large partition which is used as /tekton/kernelci

kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f cronjob.yaml
kubectl apply -f eventlistener.yaml
kubectl apply -f serviceaccount.yaml
kubectl apply -f triggertemplate.yaml
kubectl apply -f pipeline-kernel.yaml
kubectl apply -f monitor-task.yaml
kubectl apply -f build-task.yaml
kubectl apply -f log-task.yaml

To trigger a pipeline run manually:
kubectl create -f pipeline-kernel-run.yaml

During development, you will likely want to update a task or pipeline, the way to do this is something similar to:
kubectl replace -f build-task.yaml

A combination of the tekton dashboard, k9s, kubectl command line and tkn command line tools will help you troubleshoot any issues you have.

TODO: rootfs pipeline

TODO:
Reuse tasks from the Tekton catalog, as presented on the hub.
https://hub-preview.tekton.dev/

(1) git-clone
https://hub-preview.tekton.dev/detail/34
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/master/task/git-clone/0.2/git-clone.yaml

(2) send-email
https://hub-preview.tekton.dev/detail/85
https://github.com/tektoncd/catalog/blob/master/task/sendmail/0.1/sendmail.yaml
