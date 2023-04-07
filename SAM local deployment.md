1. ##### Install prerequisites

   - AWS CLI : `aws --version`
   - AWS SAM : `sam --version`
   - Docker : `docker --version`

2. ##### Prepare `template.yaml' and 'app.py' file

3. ##### Sam build

   `sam build`

   (build using docker)

   `sam build --user-container`

4. ##### Test the application

   `sam local start-api`

   `sam local invoke --event events/event.json`

5. ##### Deploy the application

   Package the application(not necessary)

   `sam package --template-file template.yaml --output-template-file deploy.yaml --s3-bucket demo-bucket`

   Deploy the application

   `sam deploy --guided`

