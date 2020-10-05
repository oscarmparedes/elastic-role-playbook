# to deploy cluster

    ansible-playbook -i inventories/vagrant --ask-become-pass cluster-prep.yaml #deploys packages, configure hostnames and disables firewall
    ansible-playbook -i inventories/vagrant --ask-become-pass deploy_cluster_uri.yaml  #deploys cluster, users and roles for fms, creates default index temlate, adds s3 repo
	ansible-playbook -i inventories/vagrant --ask-become-pass deploy_kibana.yaml #deploys kibana, creates fms space.


# testing repo with minio (tested version elastic 7.7.0):
  - Deploy minio in docker in masternode, and make sure port is accessible. Also install mc client as explained in minio docs.
  -     mc config host add myminio http://127.0.0.1:9000 minioadmin minioadmin
  -     mc mb myminio/elasticsearch
  -     export S3_SECRET_KEY=minioadmin; export S3_ACCESS_KEY=minioadmin
  -     echo $S3_SECRET_KEY | /usr/share/elasticsearch/bin/elasticsearch-keystore add --stdin s3.client.default.secret_key
  -     echo $S3_ACCESS_KEY | /usr/share/elasticsearch/bin/elasticsearch-keystore add --stdin s3.client.default.access_key
  -     elasticsearch.yml: 
        s3.client.default.endpoint: "http://127.0.0.1:9000"
        s3.client.default.protocol: "http"
  - Create a json file with the following contents:

    {
      "type": "s3",
      "settings": {
        "bucket": "elasticsearch"
      }
    }       

  - elasticsearch defaults to amazon s3, so thats why you need to add those settings in elasticsearch.yml
  - in 7.7 version (probably also in 7.x) keys to access s3 repo MUST be stored in the keystore, so there you have the commands to add them.
  - in the json to create the repo, you don't need to add anything else than the bucket name, and the default path.
  - DO A RESTART OF ELASTIC IN ALL NODES!!!
  - Do not freak out when you see amazon s3 exceptions when troubleshooting this, just make sure to have this config.
  - The key for this to work with minio is make sure setting up the exact same endpoint in minio that you setup in the mc command.