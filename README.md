# NodeDirectUploader

## Setup

Replace `<bucket-name>` below with the name of your bucket

### IAM policy

To give your User / Role read/write permissions to your bucket, create this IAM
policy and attach it to your user / role

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket-name>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket-name>/*"
            ]
        }
    ]
}
```

### Bucket CORS

To allow upload via a javascript client, add these rules to your bucket's
Permissions > CORS Configuration

_Note_ - the configuration below allows upload from all origins. You may want to be more restrictive. See https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html#cors-allowed-origin for details.

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>POST</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

### Bucket Policy

_if_ you want public-read access to all bucket objects, then add these rules to
your bucket's Permissions > Bucket Policy

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<bucket-name>/*"
        }
    ]
}
```

## Licensing

The files in this repository are, unless stated otherwise, released under the Apache License. You are free to redistribute this code with or without modification. The full license text is available [here](http://www.apache.org/licenses/LICENSE-2.0).


## Direct-to-S3 uploads in a Node.js application running on Heroku

Simple example demonstrating how to accomplish a direct upload to Amazon's S3 in a Node.js web application.

This example uses the [express](http://expressjs.com/) web framework to facilitate request-handling. However, the process of signing the S3 PUT request would be identical in most Node apps.

This code is mostly ready to be run as cloned, but a function will need to be properly defined to handle the storing of the POSTed information. The current example simply demonstrates the upload to S3.


## Dependencies and Installation

Ensure Node is installed. This can be done through your package manager or from their [website](http://nodejs.org/).

Clone this repository:
```term
$ git clone https://github.com/willwebberley/NodeDirectUploader.git
```

Change directory into the application and install the application's dependencies:
```term
$ cd NodeDirectUploader
$ yarn
```

If you prefer `npm` to `yarn`, then run `npm install` instead.

## Running the application
* Set environment variables for your AWS access key, secret, and bucket name (see [companion article](https://devcenter.heroku.com/articles/s3-upload-node))
* Run `yarn start` (or `npm start`)
* Visit [localhost:3000/account](http://localhost:3000/account) to try it out


## Deploying the application

See the article [Deploying with Git](https://devcenter.heroku.com/articles/git) for more detailed information on deploying to Heroku.

* Download and install the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* Commit your application to a local Git repository (e.g. `git init`, `git add .`, `git commit -m "version 1 commit"`, etc.)
* Create the application on Heroku by adding a Git remote (`$ heroku create`)
* Push your code to the new Heroku repo (`$ git push heroku master`)
