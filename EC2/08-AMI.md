<!-- filepath: /workspaces/awstraining/EC2/08-AMI.md -->

# Amazon Machine Image (AMI)

## 1. What is AMI?

An Amazon Machine Image (AMI) is a pre-configured template that contains the information required to launch an instance in Amazon EC2. It includes the operating system, application server, applications, and related configuration settings.

## 2. What are its benefits?

- **Rapid Deployment:** Quickly launch new EC2 instances with pre-configured environments.
- **Consistency:** Ensures all instances are identical, reducing configuration drift.
- **Scalability:** Easily scale infrastructure by launching multiple instances from the same AMI.
- **Backup & Recovery:** Use AMIs to back up instances and restore them when needed.
- **Customization:** Create custom AMIs tailored to specific application or security requirements.

## 3. Golden Image

A "Golden Image" is a standardized, pre-configured AMI that serves as the baseline for all EC2 instances in an environment. It typically includes the operating system, security patches, monitoring agents, and application dependencies, ensuring consistency and compliance across deployments.

## 4. How to Create an AMI

1. **Launch and Configure an EC2 Instance:** Set up the instance with the desired OS, software, and configurations.
2. **Prepare the Instance:** Clean up temporary files and ensure the system is in the desired state.
3. **Create the AMI:**
   - In the AWS Management Console, select the instance.
   - Choose "Actions" > "Image and templates" > "Create image".
   - Provide a name and description, then create the image.
4. **AMI Creation:** AWS creates the AMI and associated snapshots of the instance volumes.

## 5. Copying AMIs

AMIs can be copied across regions and accounts to support disaster recovery, migration, or multi-region deployments. This is done via the AWS Console or CLI, ensuring the image is available where needed.

## 6. Typical Tasks for DevOps Engineers with AMI

- Creating and maintaining golden images for application environments.
- Automating AMI creation and updates using CI/CD pipelines.
- Copying AMIs across regions for high availability and disaster recovery.
- Managing AMI lifecycle, including deprecation and cleanup of outdated images.
- Ensuring AMIs comply with security and compliance standards.