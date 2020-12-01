Hey there :sunglasses:  
Following are my answers to the excercise questions, I think I have written down everything you are looking for but if there is a screenshot or a script or anything is missing or you simply want to see more, please please let me know as I did every step in the excercise and would really welcome an oppotunity to chat with you about it!   
Thank you! :relaxed:

## Collecting Metrics
Question: Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.  
Answer: ![Alt text](https://github.com/yangli3/hiring-engineers/blob/master/Host-Map-Datadog.png "Host Map with Tags")

Bonus Question: Can you change the collection interval without modifying the Python check file you created?  
Answer: The collection interval can be changed in the yaml configuration file of the custom check. Following is my configuration file for setting the collection interval of my_metric to every 45 seconds.
```
init_config:

instances:
  - min_collection_interval: 45
```
## Visualizing Data
Question: Utilize the Datadog API to create a Timeboard.  
Answer: https://p.datadoghq.com/sb/912oapovcm9ys7nf-6d2ff9da5161f89c0f8772071c07b87f
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/Yang-First-Timeboard-Datadog.png "Timeboard Created from API")

Question: Please be sure, when submitting your hiring challenge, to include the script that you've used to create this Timeboard.
Answer: following is the body of the "Create a new dashboard" POST request.
```
{
  "description": "string",
  "is_read_only": false,
  "layout_type": "ordered",
  "notify_list": [],
  "template_variable_presets": [
    {
      "name": "string",
      "template_variables": [
        {
          "name": "string",
          "value": "string"
        }
      ]
    }
  ],
  "template_variables": [
    {
      "default": "my-host",
      "name": "host1",
      "prefix": "host"
    }
  ],
  "title": "Yang First Timeboard",
  "widgets": [
    {
      "definition": {
          "title": "Custom Metric",
          "type": "timeseries",
          "requests": [
              {
                  "q": "my_metric{host:vagrant}"
              }
          ],
          "yaxis": {
              "scale": "linear",
              "min": "0",
              "max": "1000",
              "include_zero": true
          }
      },
      "id": 1,
      "layout": {
        "height": 0,
        "width": 0,
        "x": 0,
        "y": 0
      }
    },
    {
      "definition": {
          "title": "MySQL Performance CPU Time",
          "type": "timeseries",
          "requests": [
              {
                  "q": "anomalies(mysql.performance.cpu_time{host:vagrant}, 'basic', 3)"
              }
          ],
          "yaxis": {
              "scale": "linear",
              "min": "0",
              "max": "1.5e-3",
              "include_zero": true
          }
      },
      "id": 2,
      "layout": {
        "height": 0,
        "width": 0,
        "x": 0,
        "y": 0
      }
    },
    {
      "definition": {
          "title": "Custom Metric with Rollup Function",
          "type": "timeseries",
          "requests": [
              {
                  "q": "my_metric{host:vagrant}.rollup(sum, 3600)"
              }
          ],
          "yaxis": {
              "scale": "linear",
              "min": "0",
              "max": "80000",
              "include_zero": true
          }
      },
      "id": 3,
      "layout": {
        "height": 0,
        "width": 0,
        "x": 0,
        "y": 0
      }
    }
  ]
}
```
Bonus Question: What is the Anomaly graph displaying?  
Answer: The blue part of the Anomaly graph means the data fall into its normal range and is expected, the red part means the data is anomalous and have gone beyond the expected behavior based on the past data, the gray band represents the upper bound and lower bound of the data in order to be considered as normal and not anomalous.
## Monitoring Data
Question: When this monitor sends you an email notification, take a screenshot of the email that it sends you.  
Answer: I got the email notification for the 500 warning (see screenshot), no email notification for the 800 alerting and the no data alerting as these situations didn't happen so couldn't trigger the alerts. I did set all 3 up in the Datadog console, see screenshot.
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/Alert-Email-Monitor-500.png "Warn Email")
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/Monitors-Datadog.png "Alerts Setup")

Bonus Question: Since this monitor is going to alert pretty often, you don’t want to be alerted when you are out of the office. Set up two scheduled downtimes for this monitor.    
Answer: following are two screenshots of the two notification emails for setting up the two downtimes.
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/Downtime-Weekday.png "Downtime for Weekdays")
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/Downtime-Weekend.png "Downtime for weekend")
## Collecting APM Data
Bonus Question: What is the difference between a Service and a Resource?  
Answer: a service is a building block of a modern microservice architecture, it groups together endpoints, queries, or jobs for the purposes of building an application. A resource represents a particular domain of a customer application, it is typically an instrumented web endpoint, database query, or background job.

Question: Provide a link and a screenshot of a Dashboard with both APM and Infrastructure Metrics.  
Answer: https://p.datadoghq.com/sb/912oapovcm9ys7nf-c75d97b6ab46ba4c6c4c40e7c3d0745b
![Alt text](https://github.com/yangli3/hiring-engineers/blob/solutions-engineer/APM-Dashboard.png "APM and Infrastructure Metrics Dashboard")

Question: Please include your fully instrumented app in your submission, as well.  
Answer: following is the python application, I used ddtrace-run to automatically instrument the application, I didn't change the python code to manually instrument the app.
```
from flask import Flask
import logging
import sys

# Have flask use stdout as the logger
main_logger = logging.getLogger()
main_logger.setLevel(logging.DEBUG)
c = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c.setFormatter(formatter)
main_logger.addHandler(c)

app = Flask(__name__)

@app.route('/')
def api_entry():
    return 'Entrypoint to the Application'

@app.route('/api/apm')
def apm_endpoint():
    return 'Getting APM Started'

@app.route('/api/trace')
def trace_endpoint():
    return 'Posting Traces'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port='5050')
```
## Final Question
Question: Datadog has been used in a lot of creative ways in the past. We’ve written some blog posts about using Datadog to monitor the NYC Subway System, Pokemon Go, and even office restroom availability! Is there anything creative you would use Datadog for?  

Answer: considering the strange time we are all in this year, I would use Datadog to monitor COVID test vailability at different testing sites, allowing someone who wants to be tested to be able to check which testing site has a shorter waiting time, including the testing fee so it can help people make a smarter decision, i.e. you may want to go to a paid testing site with 0 waiting time rather than going to a free testing site that has a 4-hour waiting line which will increase the change of getting infected 
