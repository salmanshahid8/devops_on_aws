## Amazon Linux 2 user data script:

- When using the user data scripts, remember to replace the <INSERT REGION HERE> with whatever AWS region you are operating in, and ensure you remove both brackets as well.

```
 #!/bin/bash -ex

wget 
https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip

unzip FlaskApp.zip

cd FlaskApp/

yum -y install python3 mysql

pip3 install -r requirements.txt

amazon-linux-extras install epel

yum -y install stress

export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}

export AWS_DEFAULT_REGION=<INSERT REGION HERE>

export DYNAMO_MODE=on

FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
```

 

## Amazon Linux 2023 user data script: 

- When using the user data scripts, remember to replace the <INSERT REGION HERE> with whatever AWS region you are operating in, and ensure you remove both brackets as well.

```
#!/bin/bash -ex

wget 
https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip

unzip FlaskApp.zip

cd FlaskApp/

yum -y install python3-pip

pip install -r requirements.txt

yum -y install stress

export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}

export AWS_DEFAULT_REGION=<INSERT REGION HERE>

export DYNAMO_MODE=on

FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80 
```