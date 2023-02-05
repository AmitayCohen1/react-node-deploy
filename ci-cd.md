//Not ready

Process:
1. Create an IAM user
2. Add aws env to GitHub secrets
3. Create a .github/workflows/<work_flow_name>.yamel
4. test deploymet






1. Create an IAM role with necessary permissions (e.g. s3:PutObject, s3:ListBucket, s3:GetBucketLocation, cloudfront:CreateInvalidation) in the AWS Management Console.
2. Store the AWS credentials as secrets in your GitHub repository.
3. Create a GitHub Actions workflow file with the following steps:
	a. Checkout the code from the repository
	b. Build the React application
	c. Configure the AWS CLI with the stored credentials
	d. Sync the build directory to the S3 bucket
	e. Invalidate the CloudFront cache
4. Commit and push the changes to the repository to trigger the deployment pipeline.



Links - 
https://www.youtube.com/watch?v=4mnJyUYTf8E&ab_channel=banananeer
https://www.youtube.com/watch?v=0tMkRSdp-Go&ab_channel=CodingTech
https://www.youtube.com/watch?v=l97zYgiB57k&t=554s&ab_channel=AntonPutra
https://www.youtube.com/watch?v=_DIRSI07kxY&ab_channel=SamMeech-Ward
https://www.youtube.com/watch?v=lPVgfSXTE1Y&ab_channel=SamMeech-Ward
