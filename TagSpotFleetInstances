from __future__ import print_function
import multiprocessing
import boto3
import logging
import datetime

logger = logging.getLogger()
logger.setLevel(logging.INFO)


def lambda_handler(event, context):
    #logger.info('Event: ' + str(event))
    #print('Received event: ' + json.dumps(event indent=2))

    try:
        region = event['region']
        detail=event['detail']
        instanceid = detail['instance-id']
        print(region, instanceid, detail, event)
        client = boto3.client('ec2', region_name=region)
        response = client.describe_instances(InstanceIds=[instanceid])
        LifeCycle = response['Reservations'][0]['Instances'][0]['InstanceLifecycle']
        def tag_instance(key, value):
            client.create_tags(Resources=[instanceid], Tags=[{'Key':key, 'Value':value}])
            print("Created Tag", key)
            print(datetime.datetime.now().strftime("%y-%m-%d %H:%M:%S"))
        IsSpotFleet = 0
        for tag in response['Reservations'][0]['Instances'][0]['Tags']:
            if tag['Key'] == 'aws:ec2spot:fleet-request-id':
                IsSpotFleet=1
        if LifeCycle == 'spot' and IsSpotFleet == 1:
            tag1=['LifeCycle', 'Spot']
            tag2=['Deployment', 'Prod']
            if __name__ == '__main__':
                jobs = []
                process = multiprocessing.Process(target=tag_instance, args=(tag1[0], tag1[1]))
                jobs.append(process)
                process = multiprocessing.Process(target=tag_instance, args=(tag2[0], tag2[1]))
                jobs.append(process)
                for j in jobs:
                    j.start()
                for j in jobs:
                    j.join()
    except Exception as e:
        logger.error('Something went wrong: ' + str(e))
        return False
