# kiro-demo

This demo showcases the use of the **AWS Kiro IDE** in **Vibe mode** to build and deploy a static website on S3.

The generated site highlights what the Kiro IDE is, how it works, and its unique features — all scaffolded through a chat-driven, agentic workflow inside the IDE.

## What’s Inside

* Static website files generated using Vibe coding
* Deployment script for S3
* Example prompt used in Kiro:

  ```plaintext
  I want to create a static website on AWS using S3, and make it public. The content of the site should showcase what AWS Kiro IDE is, how it works, and its unique features.
  ```

## Deployment

✅ **Successfully deployed to Amazon S3.**

📄 For documentation and visibility, the same static files have also been copied into this repository and served via **GitHub Pages**.

## Requirements

* AWS credentials with S3 access
* AWS CLI configured (optional for manual deployment)

## Deploy

To deploy the static website to Amazon S3, follow the instructions in [`deploy-to-s3.md`](./deploy-to-s3.md).
