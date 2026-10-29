# AWS Web Application Migration: EC2 to Elastic Beanstalk

**Project:** Group SIGMA — AWS Web Application Migration  
**Focus:** Cloud migration, EC2, AMI, PHP application deployment, and AWS Elastic Beanstalk  
**AWS Region:** Europe (Stockholm) — `eu-north-1`

## Project Overview

This project demonstrates the migration of a PHP web application from a simulated on-premises server to Amazon Web Services (AWS).

The project was completed in two phases:

1. **Rehosting:** Captured the source server as an Amazon Machine Image (AMI), launched a new Amazon EC2 instance from that image, and validated application accessibility.
2. **Re-platforming:** Packaged the PHP application and deployed it to AWS Elastic Beanstalk to establish a more structured application deployment and environment-management workflow.

The objective was to understand practical cloud migration processes, validate the migrated application, compare migration approaches, and identify a suitable long-term deployment platform.

## Architecture and Migration Workflow

```text
Simulated On-Premises Web Server
              |
              v
     Create Amazon Machine Image
              |
              v
       Launch Amazon EC2
              |
              v
   Validate Migrated Application
              |
              v
     Package PHP Application
              |
              v
       AWS Elastic Beanstalk
              |
              v
    Deploy Application Version
              |
              v
  Validate Application and Health
```

The EC2 phase preserved the server environment through an AMI. The Elastic Beanstalk phase moved the application into a managed application deployment workflow.

## Technologies and AWS Services

- **Amazon EC2:** Hosted the migrated server instance.
- **Amazon Machine Image (AMI):** Captured the source server image for migration.
- **AWS Elastic Beanstalk:** Managed the PHP application deployment and environment.
- **AWS Identity and Access Management (IAM):** Supported instance role configuration.
- **Amazon S3:** Used as part of the Elastic Beanstalk deployment infrastructure.
- **AWS CloudFormation:** Supported the provisioning of environment resources.
- **Auto Scaling:** Reviewed as part of the Elastic Beanstalk environment infrastructure.
- **Amazon Linux 2023:** Operating system used in the documented server environment.
- **PHP:** Application runtime.
- **Linux shell and ZIP utilities:** Used to prepare and inspect the deployment package.

## Implementation

### Phase 1: Rehosting the Server to Amazon EC2

**Step 1 — Simulate the on-premises server**

Established the source server environment used to represent an existing on-premises web server.

**Step 2 — Install and validate the web application**

Installed and ran the PHP web application, then checked that it was reachable before migration.

**Step 3 — Create an Amazon Machine Image**

Created an AMI from the source EC2 instance to capture the server environment for reuse.

**Step 4 — Launch the migrated EC2 instance**

Launched a new EC2 instance from the AMI, providing a migrated copy of the source server.

**Step 5 — Configure the instance role**

Reviewed and configured the required IAM role attachment for the instance.

**Step 6 — Validate the migrated application**

Accessed the migrated instance and checked the application using its IP address to confirm that it was reachable at the captured validation checkpoint.

### Phase 2: Re-platforming to AWS Elastic Beanstalk

**Step 7 — Package the PHP application**

Prepared the PHP application files and created ZIP deployment bundles.

Commands documented in the project:

```bash
cp /var/www/html/index.php ~/sigma-app/
cd ~/sigma-app
zip -r ../sigma-app-v1.zip .
unzip -l ~/sigma-app-v1.zip
```

These commands copy the application entry file into the packaging directory, navigate to that directory, create a ZIP bundle, and inspect the archive contents.

**Step 8 — Create the Elastic Beanstalk environment**

Created the Elastic Beanstalk application and environment for the PHP application.

Documented environment details:

- Application: `sigma-app`
- Environment: `Sigma-app-env`
- Platform: PHP 8.5 on 64-bit Amazon Linux 2023
- Region: `eu-north-1`

**Step 9 — Review supporting AWS resources**

Reviewed the resources associated with the environment, including EC2, IAM roles, security groups, Auto Scaling, CloudFormation, and S3.

This helped demonstrate how a managed application environment relies on supporting AWS infrastructure.

**Step 10 — Deploy an updated application version**

Packaged an updated application version and deployed it through Elastic Beanstalk. The project evidence identifies version `v2` as the running version at the documented checkpoint.

**Step 11 — Review environment health and logs**

Reviewed the environment health status and logs to understand how application status and troubleshooting information can be accessed.

The captured environment overview reported `Health: OK` at that time.

**Step 12 — Compare before-and-after application behavior**

Reviewed the captured application pages and health evidence to confirm reachability at the documented checkpoints.

The evidence demonstrates successful validation at those checkpoints; it does not establish zero downtime or a completed load test.

## Migration Strategy: Re-platforming

Our preferred long-term strategy was **re-platforming the PHP application to AWS Elastic Beanstalk**.

Rehosting to EC2 was a useful first step because it allowed the server image to be recreated in AWS with minimal changes. However, we selected Elastic Beanstalk as the preferred final platform for the following reasons:

- **Reduced operational overhead:** Elastic Beanstalk automates common environment provisioning and deployment tasks, reducing routine manual work compared with directly managing every deployment on EC2.
- **Repeatable deployments:** Application versions provide a more structured process for packaging, deploying, validating, and managing releases.
- **A practical path to scaling:** Elastic Beanstalk can coordinate Auto Scaling and load balancing when configured appropriately.
- **Health and troubleshooting visibility:** Environment health information and logs help teams review application status and investigate issues.
- **No complete application rewrite:** The existing PHP application could be retained rather than redesigned solely to move to a managed deployment platform.
- **Improved team handover:** A defined application, environment, and version workflow makes the deployment process easier to document and review.

### Why not stop at direct EC2 management?

Direct EC2 management remains useful when detailed control over the server is required. However, it leaves more responsibility for server maintenance, deployment procedures, scaling configuration, and operational monitoring.

For this project, Elastic Beanstalk offered a better balance between retaining the existing PHP application and simplifying routine deployment and environment management.

## Validation and Results

The project evidence documented the following outcomes:

- A source server was captured as an AMI.
- A new EC2 instance was launched from the AMI.
- The migrated application was reachable at the captured validation checkpoint.
- The PHP application was packaged into ZIP deployment bundles.
- An Elastic Beanstalk environment was created for the application.
- An updated application version was deployed, with `v2` identified as running in the evidence.
- The environment health overview reported `Health: OK` at the captured checkpoint.
- Supporting AWS resources and environment logs were reviewed.

These results demonstrate a practical migration and deployment workflow. They should not be interpreted as proof of zero downtime, production readiness, or performance under load.

## Screenshots and Evidence

The repository includes screenshots documenting key implementation stages.

### 1. Server access

![EC2 instance connection evidence](<step 1 instance connect.png>)

### 2. Application packaging

![PHP web server application packaging](<step 7 packaging the webserver application.png>)

### 3. Deployment bundle creation

![First application ZIP bundle](<step 7 packaging version of app created_zip1.png>)

![Second application ZIP bundle](<step 7 packaging version of app created_zip2.png>)

### 4. Migrated instance validation

![Migrated instance IP address validation](<validation of migrated instance ip address_step 6.png>)

### 5. Updated application deployment

![Updated and redeployed web server application](<updated and redeployed webserver application step 10.png>)

*Note: These image references assume the screenshots are stored in the repository root with the exact filenames shown. If you move them into a `screenshots/` folder, update the paths accordingly.*

## Key Lessons Learned

- An AMI can help reproduce an existing server environment in AWS.
- Application reachability should be checked before and after migration.
- Application packaging must include the files required for deployment.
- Managed platforms can simplify application deployment and environment operations.
- Application versions improve release traceability and can support recovery planning.
- IAM roles, security groups, and supporting resources are important parts of a cloud deployment.
- Health indicators and logs provide useful operational evidence, but broader monitoring and performance testing require additional validation.

## Skills Demonstrated

- AWS cloud migration fundamentals
- Amazon EC2 and AMI-based server migration
- Linux server and application administration
- PHP application packaging
- AWS Elastic Beanstalk deployment
- IAM role and security group configuration
- Application validation and troubleshooting
- Migration strategy evaluation
- Technical documentation and screenshot-based evidence
- Collaborative project delivery

## Project Limitations and Future Improvements

Potential next steps for extending this project include:

- Configure and test HTTPS with a suitable domain and certificate.
- Review CloudWatch metrics and configure appropriate alarms.
- Perform a controlled load test to evaluate scaling behavior.
- Test application recovery and rollback procedures.
- Review IAM permissions and network access for least privilege.
- Document cleanup steps to prevent unnecessary AWS charges.

These are proposed improvements, not claims that the tasks were completed in this project.

## Conclusion

This project provided hands-on experience with two AWS migration approaches: rehosting a server to EC2 and re-platforming a PHP application to Elastic Beanstalk.

The team used EC2 and an AMI to demonstrate server migration, then selected Elastic Beanstalk as the preferred long-term deployment platform because of its structured deployment workflow and reduced routine environment-management effort.

The project strengthened practical understanding of cloud infrastructure, application deployment, validation, migration planning, and technical documentation.
