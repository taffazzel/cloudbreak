#### Cloubreak
  * The only requirement is we must have Docker running.
  * registrator: listen to docker container
  * Periscope: Auto scaling
  * OAuth2: UAA
  * Sultans: USer mgmt UI
  * Uluwatu: cbreak UI


#### Requirements
  * RHEL / CentOS /Oracle Linux 6 (64-bit)
  * Docker 1.6.0 or later


#### Installing Cloudbreak
  * Create seperate directory
```
mkdir cloudbreak
cd cloudbreak
```
  * Download and extract package
```
wget http://public-repo-1.hortonworks.com/HDP/cloudbreak/cloudbreak-deployer_1.0.0_Linux_x86_64.tgz
tar xvf cloudbreak-deployer_1.0.0_Linux_x86_64.tgz
```
  * Copy cbd installer to /usr/local/bin (if not available then /usr/bin)
`sudo cp cbd /usr/local/bin/`


#### Installing Cloudbreak components
  * Initialize cloudbreak
`cbd init`
  * This create **Profile** file in which we need to configure IP for Cloudbreak UI
`echo export PUBIC_IP=(Public IP of host) > Profile`
  * Generate Cloudbreak-related configurations
`cbd generate`
    * This generate two files: 
      docker-compose.yml: Full configuration of Cloudbreak Docker containers.
      uaa.yml: Default Identity Server and User Management configurations.
  * Retrieve and modify Cloudbreak URL user name and password
`cbd login` This will output default credentials
  * Add following lines to Profile
```
export UAA_DEFAULT_USER_EMAIL=<user email>
export UAA_DEFAULT_USER_PW=<password>
```
  * Regenerate configuration with new Profile
`cbd regenerate`


#### To start Cloudbreak
`cbd start`


#### Public cloud
  * Hadoop can be launched on: AWS, Azure, GCP, Mesos and Openstack
  * Images used by cloud provider is:
    * Amazon Web Services: RHEL 7.1
    * Microsoft Azure: CentOS 7.1 OpenLogic
    * Google Cloud Platform: CentOS 7: centos-7-v20150603


#### Creating IAM role using the console
  1. Login to AWS
  2. Select IAM and **Create New Role**
  3. Select Role for Cross-Account access
  4. Add following policies:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudformation:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    },
        {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```
