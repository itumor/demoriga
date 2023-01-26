
### Create a Kubernetes secret for AWS
The provider requires credentials to create and manage AWS resources.

#### Generate an AWS key-pair file
Create a text file containing the AWS account `aws_access_key_id` and
`aws_secret_access_key`. The [AWS
documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-creds)
provides information on how to generate these keys.

```ini
[default]
aws_access_key_id = <aws_access_key>
aws_secret_access_key = <aws_secret_key>
```

Save this text file as `aws-credentials.txt`.

#### Create a Kubernetes secret with AWS credentials
Use `kubectl create secret -n upbound-system` to generate a Kubernetes secret
object inside the Kubernetes cluster.

```shell
kubectl create secret \
generic aws-secret \
-n upbound-system \
--from-file=creds=./aws-credentials.txt
```

View the secret with `kubectl describe secret`
```shell
$ kubectl describe secret aws-secret -n upbound-system
Name:         aws-secret
Namespace:    upbound-system
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
creds:  114 bytes
```
_Note:_ the size may be larger if there are extra blank space in your text file.