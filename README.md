## Hosting a Website in EC2 with the img folder contents in S3 Bucket

In this project the webfiles are hosted in an EC-2 instance with the exception of img folder which is hosted in a S3 Bucket. When the site loads the contents, images are directly loaded from the S3 bucket. We do this by syning / copying the img folder to an s3 bucket and attaching a policy to the bucket that allows the contents in the bucket to be read and displayed also we provide the necessary rewrite rules in the configuration files.





## Prerequisites for this project

- Needs AWS CLI Access / IAM User with S3 Admin access
- AWS CLI needs to be installed on your system

## Package Installation

```sh
#yum install httpd -y
#systemctl restart httpd
#systemctl enable httpd
```

## Fetching a Sample Website & Giving Necessary Permissions

```sh
#wget https://www.tooplate.com/zip-templates/2101_insertion.zip
#unzip 2101_insertion.zip
#cp -r 2101_insertion/* /var/www/html/
#chown -R apache.apache /var/www/html/
```
Go to your S3 bucket, In permissions tab edit and turn off Block Public Access. Provide the below bucket policy.

## IAM Bucket Policy To Make Bucket Public
```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "ENTER-YOUR-BUCKET-NAME/*"
        }
    ]
}
```

## Configuring AWS CLI User

```sh
#aws configure
# Provide AWS Access Key ID
# Provide AWS Secret Access Key
# Provide Default region name
# Provide Default output format
```

## Syncing contents of img folder with your bucket
```sh
#aws s3 sync /var/www/html/img s3://ENTER-YOUR-BUCKET-NAME
```

## httpd.conf rewrite rules
```sh
# vim /etc/httpd/conf/httpd.conf

RewriteEngine On
RewriteRule ^/img/(.*)$  https://s3.ap-south-1.amazonaws.com/YOUR-S3-BUCKET-NAME/$1 [L]

# systemctl restart httpd

```

Please map your domain name to the appropriate public IP and load the site. We could see that now the images are loading from our S3 Bucket.
