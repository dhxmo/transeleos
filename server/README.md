1. Generate your origin server certificates and put them in the cert folder: cert.pem, cloudflare.crt and key.pem (or if you want to use certbot, please feel free to modify the nginx etc etc)

2. Make a S3 bucket and add the following policy to it:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicRead",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject",
                "s3:GetObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::<your bucket name here>/translated_audio/*"
            ]
        },
        {
            "Sid": "CORSPolicy",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetBucketCORS",
            "Resource": [
                "arn:aws:s3:::<your bucket name here>",
                "arn:aws:s3:::<your bucket name here>/translated_audio/*"
            ]
        }
    ]
}
```

3. create role and add access key to .env
4. set up some cloud compute. once in, clone your repo into it and install docker-compose on the server machine
5. `docker-compose build && docker-compose up`
6. the server should be up now. take the server IP and S3 bucket name and add to the extension .env
