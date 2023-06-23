![image](https://github.com/Nikhil-Singh25/ec2alarm-taskschdlr/assets/92309384/31435b26-2e53-445f-b8bb-f28c81eb8922)# ec2alarm-taskschdlr
 This project will help to get alarm notification whenever Disk usage is &lt;= 15%, and a script which runs whenever ec2 instace starts and deletes all the temporary files from all the users.


[1] Create an IAM Role (cwa_ssm_role)and attach below permission(s):
           a. AmazonEC2RoleforSSM (soon to be deprecated, can use AmazonSSMManagedInstanceCore policy) 
           b. CloudWatchAgentAdminPolicy 
	 c. CloudWatchAgentServerPolicy 
	
[2] Attach the IAM role to the respective EC2 Instance or create a new EC2 instance with Windows Image if required.

[3] a. In SSM Console > Node Management> Run Command >Select 'Aws-ConfigureAWSPackage' command document > Go to command parameters below and put Name = 'AmazonCloudWatchAgent' .
      b. In Target selection choose the EC2, in which you want to run the Command > Run 
[4]  Create a standard SNS Topic & Create a subscription to the topic (Email End point)
 
[5] Connect to EC2 instance using RDP and go to C:\Program Files\Amazon\AmazonCloudWatchAgent, run amazon-cloudwatch-agent-config-wizard.exe as Administrator file, CMD prompt will open , below is the history of commands which you will see:
