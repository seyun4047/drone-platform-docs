# S3 GUIDE

## 1. Create S3 Bucket
- name: sts-s3-drone
- Block public access (bucket settings): OFF

## 2. Create IAM Policy
- name: sts-policy-drone
- Edit the Permissions
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowUploadToUploadsFolder",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl"
      ],
      "Resource": "arn:aws:s3:::sts-s3-drone/uploads/*"  # bucket arn
    }
  ]
}
```
## 3. Create IAM User
- name: sts-user-drone
 
## 4. Create IAM Role
- name: sts-role-user
- Attach the sts-policy-drone policy to the role.
- Edit the Trusted entities
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::000000000000:user/sts-user-drone"  # user arn
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## 5. IAM User Inline Policy (Allow AssumeRole)
- Edit the Permissions policy
```bash
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
				"sts:AssumeRole"
			],
			"Resource": [
				"arn:aws:iam::000000000000:role/sts-role-user" # role_arn
			]
		}
	]
}

```
## 6. S3 Bucket Policy
- Edit the Bucket policy
```bash
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowDroneRoleUpload",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::000000000000:role/sts-role-user" # user_arn
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::sts-s3-drone/uploads/*" # bucket_arn
        },
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::sts-s3-drone/uploads/*"# bucket_arn
        }
    ]
}
```
## 7. S3 CORS Configuration
- Edit the Cross-origin resource sharing (CORS)
```bash
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "PUT",
            "POST"
        ],
        "AllowedOrigins": [
            "your_domain or *"
        ],
        "ExposeHeaders": []
    }
]
```
## 8. Store IAM User Credentials on Server
- Edit your server .env
```bash
AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=YOUR_SECRET_KEY
S3_REGION=YOUR_REGION
S3_BUCKET_NAME=sts-s3-drone
S3_SERVER_ROLE=arn:aws:iam::000000000000:role/sts-role-user #role_arn
```
