# syntaxhighlight

go
```go
package main

import "fmt"
import "time"

func main() {
    go spinner(100 * time.Millisecond)
    const n = 45
    fibN := fib(n) // slow
    fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}

func spinner(delay time.Duration) {
    for {
        for _, r := range `-\|/` {
            fmt.Printf("\r%c", r)
            time.Sleep(delay)
        }
    }
}


func fib(x int) int {
    if x < 2 {
      return x
    }
    return fib(x-1) + fib(x-2)
}
```
og



# Creating S3 buckets and permissions

## S3 Bucket with publicly accessible assets, with write permissions only from your service

The following code sample will create a super cool bucket, that can then be
cached through Cloudflare (will need a ticket to infra) at the domain
super-cool-bucket.postmates.com, with appropriate CORS settings.

This code sample also sets up an admin policy that can read/write to the bucket.

Whatever service wants to then write to the bucket can assume the appropriate
role (preferably just a write role) to write contents to the bucket.

```
resource "aws_s3_bucket" "super-cool-bucket" {
    bucket = "super-cool-bucket.postmates.com"
    acl = "public-read"

    tags {
        Environment = "prod"
    }

    cors_rule {
        allowed_headers = ["*"]
        allowed_methods = ["GET"]
        allowed_origins = ["*"]
        max_age_seconds = 3000
    }
}

resource "aws_iam_policy" "super-cool-bucket" {
    name = "super-cool-bucket-admin"
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": "s3:*",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::${aws_s3_bucket.super-cool-bucket.id}",
                "arn:aws:s3:::${aws_s3_bucket.super-cool-bucket.id}/*"
            ]
        }
    ]
}
EOF
}

```

## S3 Bucket for programmatic access, but not public access

Lets create a bucket that isn't publicly served, but is programmatically used
by your super cool service.

First, create the bucket:
```
resource "aws_s3_bucket" "super-cool-bucket" {
    bucket = "super-cool-bucket-prod"
    acl = "private"

    tags {
        Environment = "prod"
    }
}
```

Now, you can setup a role that can write to the bucket, that can be assumed by your
service role (assume intentionally left blank, you would want a SID that whitelists):
```
resource "aws_iam_role" "super-cool-writer-role-prod" {
    name = "super-cool-role-prod"
    assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "sts:AssumeRole",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Effect": "Allow",
      "Sid": ""
    }
  ]
}
EOF
}
```

Now, a policy that can write to the bucket:
```
resource "aws_iam_policy" "super-cool-writer-policy" {
  name = "SuperCoolWriterPolicy"
  policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": "s3:PutObject",
      "Effect": "Allow",
      "Resource": [
          "arn:aws:s3:::${aws_s3_bucket.super-cool-bucket-prod.id}",
          "arn:aws:s3:::${aws_s3_bucket.super-cool-bucket-prod.id}/*"
      ]
    }
  ]
}
EOF
}
```

Now attach the policy to the role, so that on assuming the role you can write to the bucket:
```
resource "aws_iam_policy_attachment" "super-cool-prod-writer" {
  name = "super-cool-prod-writer-attachment"
  roles = ["${aws_iam_role.super-cool-writer-role-prod.name}"]
  policy_arn = "${aws_iam_policy.super-cool-writer-policy.arn}"
}
```
123
