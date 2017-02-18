# Lamda-TagSpotFleetInstances

This Python function is for executing in a AWS Lambda function.
The trigger for the Lambda function should be the CloudWatch event for Ec2 instance state change notification with state 'pending'.

It creates two tags on a SpootFleet EC2 instance parallely by envoking create_tag function using pyhton multiprocessing library.
