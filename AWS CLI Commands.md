# AWS CLI Commands

## S3

##### Copy file to S3

`aws s3 cp index.html s3://<BUCKET_NAME_PROVIDED>`

##### Execute S3 Policy

`aws s3api put-bucket-policy --bucket <BUCKET_NAME> --policy <S3_POLICY_JSON>`



## CloudFront

##### Create CloudFront OAI

`aws cloudfront create-cloud-front-origin-access-identity --cloud-front-origin-access-identity-config CallerReference=<YOUR_UNIQUE_STRING_HERE>,Comment=MyOAI`

Replace <YOUR_UNIQUE_STRING_HERE> with a random series of numbers

-> Save <ID> and <ETAG>
-> Then replace <OAI-ID> in <POLICY_JSON_FILE>
-> Execute the <POLICY_JSON_FILE>



##### Create CloudFront Distribution

`aws cloudfront create-distribution --origin-domain-name <BUCKET_NAME>.s3.amazonaws.com --default-root-object index.html`

-> Save <CF_DIST_ID> and <CF_DIST_ETAG>



##### Get the CloudFront Distribution Config and Save

`aws cloudfront get-distribution-config --id <CF_DIST_ID> | jq -r .DistributionConfig > dist-config.json'`

-> Modify *OriginAccessIdentity* as `origin-access--identity/cloudfront/<OAI_ID>`
-> Modify *PriceClass* as `PriceClasss_100`
-> Modify *ViewerPortocolPolicy* as `redirect-to-https`



##### Update Distribution with a New Distribution Config Json File

`aws cloudfront update-distribution --id <DISTRIBUTION_ID> --distribution-config file://dist-config.json --if-match <DISTRIBUTION_ETAG>`



## Basic CLI Command

##### Create file

`echo "<CONTENT>" < <FILE_NAME>`

##### Open file

`vim <FILE_NAME>`

