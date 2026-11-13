# AWS VPC as Code with CloudFormation

## What I built
I wrote a CloudFormation template that builds a complete network and web server in one step: a VPC (`10.0.0.0/16`), a public subnet, an internet gateway, a route table with a default route, a security group allowing only HTTP, and an EC2 instance (Amazon Linux 2023, t3.micro) running NGINX. I manually built the same network earlier, which took about 20 console steps. The template is `fixit-vpc-stack.yaml`.

## Architecture
```
Browser -> Internet gateway -> Route table (0.0.0.0/0 -> igw) -> Public subnet -> Security group (port 80) -> EC2 + NGINX
```

## How I ran it
1. Uploaded the template in the CloudFormation console and created the stack `fixit-vpc-stack`
2. Waited for `CREATE_COMPLETE` and opened the `WebsiteUrl` output
3. Deleted the stack, which removed every resource

## Troubleshooting
- **Symptom:** the first submit failed with "Role ... is invalid or cannot be assumed".
- **Diagnosis:** the error was about the role the stack tried to use, not the template. The console offered three permission options.
- **Fix:** I chose "Automatic permissions" and submitted again, and the stack was created.
- I also checked the **Resources** tab to confirm that every resource showed `CREATE_COMPLETE` while the Events list still showed older in-progress rows.

## What I learned
- Infrastructure as code is repeatable, fast, easy to review, and easy to remove.
- A stack deletes everything it created in the right order with one action.
- A role error can come from the stack's permissions and not from the template.
- Terraform is my next step. CloudShell was unavailable on my new account while verification was in progress.

## Clean-up
I deleted the stack and confirmed the instance was terminated and the VPC was gone.
