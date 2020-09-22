# aws-lambda-layer
Playbook for creating AWS Lambda Layer for Python

Reference: 
* https://dev.to/razcodes/how-to-create-a-lambda-layer-in-aws-106m

<B>SSH into Linux VM (example here is Amazon Linux AMI)</B>

Check to see Python3 version, install if necessary.
```shell
  python --version
  python3 --version
  sudo yum list | grep python3
  sudo yum install python36 python36-pip
  python --version
  python3 --version
  sudo yum update
```

Create folder structure for lambda layer.
```shell
  mkdir aws-lambda-layer-v1
  cd aws-lambda-layer-v1
  mkdir -p lambda-layer/python/lib/python3.6/sitepackages
```

Create requirements.txt file (see sample in repo).
```shell
  vi requirements.txt
  psycopg2-binary
  pandas
  requests
  sklearn
  numpy
  matplotlib
  seaborn
  sqlalchemy
  <Esc>:wq
```

Create deployment package as zip file.  Remember the "." at end of `zip` command.
```shell
  python3 -m pip install -r requirements.txt -t lambda-layer/python/lib/python3.6/site-packages
  cd lambda-layer
  zip -r9 lambda-layer.zip .
```

Copy the zip file to s3 bucket.  Remember the "/" at end of `aws s3 cp` command
```shell
  aws s3 ls
  aws s3 cp lambda-layer.zip s3://<BUCKET-NAME>/<FOLDER-NAME>/
```

From AWS Lambda, paste an S3 link URL
`s3://<BUCKET-NAME>/<FOLDER-NAME>/lambda-layer.zip`
