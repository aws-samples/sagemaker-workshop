+++
title = "CloudFormation Stacks"
chapter = false
weight = 5
+++

To delete all the underlying resources, assume the Account Admin Role and navigate to CloudFormation.

If the "view nested" button is checked, click on it, so you only see the root stacks. You should see a root stack associated with the Notebook, one associated with the DS Environment and a secure-ds-core stack which builds the core environment with the Shared Services VPC. Delete this one last.

Step by Step Instructions

1. First ensure that your S3 buckets associated with this workshop are deleted. They should contain your Project Name. 


2. Next, delete the **most recent stack** starting with "SC-<account_num>-pp-####". **Be Careful**: Make sure to delete the root stack, nested stacks have a tab that says "nested" on top of the stack. These are deleted automatically when the root stack is deleted, and don't need to be separately deleted. 

3. Then delete the second stack starting with "SC-<account_num>-pp-####". This stack is created by the Data Science Admin to provision the secure environment for data scientists. Once again **do not** delete *nested* stacks separately. 

4. Finally, delete the secure-ds-core stack. **Note**: nested stacks will be automatically deleted by the root stack. 

Note, that if you choose to build this environment in a different region, you may get an error that the Data Science Adminstrator IAM roles already exists. Delete that role first before deploying to another region. 
