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


tf
```tf
######################
# Custom IAM Policies
######################

resource "aws_iam_policy" "help-center-stage-cs-monitoring" {
    name = "help-center-stage-cs-monitoring"
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::cs-monitoring-stage"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject", "s3:ListObject", "s3:PutObject", "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::cs-monitoring-stage/*"
            ]
        }
    ]
}
EOF
}

resource "aws_iam_policy" "help-center-prod-cs-monitoring" {
    name = "help-center-prod-cs-monitoring"
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::cs-monitoring-prod"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject", "s3:ListObject", "s3:PutObject", "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::cs-monitoring-prod/*"
            ]
        }
    ]
}
EOF
}
```
tf

xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>Owner-canonical-user-ID</ID>
    <DisplayName>display-name</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>Owner-canonical-user-ID</ID>
        <DisplayName>display-name</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
    
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>user1-canonical-user-ID</ID>
        <DisplayName>display-name</DisplayName>
      </Grantee>
      <Permission>WRITE</Permission>
    </Grant>

    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>user2-canonical-user-ID</ID>
        <DisplayName>display-name</DisplayName>
      </Grantee>
      <Permission>READ</Permission>
    </Grant>

    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group">
        <URI>http://acs.amazonaws.com/groups/global/AllUsers</URI> 
      </Grantee>
      <Permission>READ</Permission>
    </Grant>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group">
        <URI>http://acs.amazonaws.com/groups/s3/LogDelivery</URI>
      </Grantee>
      <Permission>WRITE</Permission>
    </Grant>

  </AccessControlList>
</AccessControlPolicy>
```
xml
