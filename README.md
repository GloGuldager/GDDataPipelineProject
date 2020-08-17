# GD Data Pipeline Final Project

## Project Description

I am currently working as a DigiDem on the Minnesota Coordinated Campaign for the 2020 Campaign Cycle.

In this capacity I work on the data team and have been asked to create a Flake Rate report for our field team so that they can track how many volunteers that are scheduled to do phone bank shifts actually complete these shifts. A general rule of thumb for flake rate is 50% of the people recruited for any event, phone bank, or canvass, will not show up as promised. The field team will be able to monitor this flake rate by week (current week is Saturday to today), region and organizer (eventually) so that they can better plan how many volunteers to recruit and monitor confirmation best practices.

In addition, we will build on this report in the coming weeks to redo PTG goals and metrics based on newly defined regions and turf as well as other contact methods.

## Data sources
The data source used for this report is from the Minnesota DFL VAN MyCampaign Events Signup and related data tables to pull the source data and lists, specifically phonebank volunteer shift signups. This data is synced with the DNC Phoenix/BigQuery on a daily basis and so the actual data source tables are located in BigQuery. We are specifically pulling from the vansync_derived_mn table that contains data for the most recent update per volunteer sign-up so that we know we are getting the final result. Future iterations created for PTG and other metrics will also use Activity and Turf tables.

![VanSyncTable](MNBigQueryVanSyncTable.png)

## Query
The Query created for this report pulls the last result for each volunteer sign-up by event_date and from that result categorizes by event_location_id (our region), by week (we count weeks from Saturday to Friday), defines Current_Week, and counts No_Show, Cancelled, Completed, Total_Flakes, Total sign-ups, and the Flake_Rate.

There is also a version of the Query that will be used for additional reports that adds columns for Future_Week and counts Scheduled events.



## Pipeline Setup
What tools do you plan to use?
BigQuery/DNC Phoenix (holds synced VAN data), DNC Portal, Google Sheets, Data Studio for Visualization/Dashboard
What QA (or other sanity) checks would you add to ensure the pipeline is working as intended?
Periodically run an Events Participation report out of VAN to check totals for a day/week. 




## Data Studio Visualization


![DFLBrand](DFLBrand.png)



Final Project Benchmark 3
Gloria Guldager
Pipeline Project: Field Team Organizer Flake Rate report for Volunteer Shifts Completed

What is your plan to complete the project? 

My plan to complete the project is to spend time on Monday tweeking my dashboard visualization and also create a draft of the slide deck and executive summary. Monday evening I have signed up for office hours with Shoham to address any final questions I have on my visualization.

On Tuesday I will make final edits to the slide deck and executive summary. In addition I will practice my presentation to prepare for the evening presentation.

At this point, I have all of the data, query, pipeline components, automation and the visualization dashboard created. I have tweeks I would like to make to the visualization and I need to create the slide deck and the executive summary.













AWS Infrastructure
The AWS infrastructure is set up according to this tutorial
Upload the CloudFormation script to create the resources, such as EC2 instance, RSD database for Airflow, security groups, S3 bucket
Then connect to the EC2 instance:
sudo su
cd ~/airflow
source ~/.bash_profile
bash start.sh
bash run_dags.sh
ETL
dag_cluster: start the EMR cluster, and wait for all data transformation is finished, then terminate the cluster
alt text

dag_normalize: wait for EMR cluster to be ready, then use Apache Livy REST API to create interactive Spark session on the cluster, submit a Spark script to read data from S3, do transformation and write the output to S3
This DAG handles normalized tables
alt text

dag_analytics: wait for EMR cluster to be ready, and that normalized tables are processed, then read normalized tables to create analytics tables, and write to S3
This DAG handles immigration data, which is partitioned for 12 months from jan-2016 to dec-2016
To re-run this DAG, change the DAG name, then delete the existing DAG from Airflow, and refresh the UI
alt text

Possible errors
Livy session NOT started, restart EMR, restart Airflow scheduler
Scenarios
Data increase by 100x. read > write. write > read

Redshift: Analytical database, optimized for aggregation, also good performance for read-heavy workloads
Cassandra: Is optimized for writes, can be used to write online transactions as it comes in, and later aggregated into analytics tables in Redshift
Increase EMR cluster size to handle bigger volume of data
Pipelines would be run on 7am daily. how to update dashboard? would it still work?

DAG retries, or send emails on failures
daily intervals with quality checks
if checks fail, then send emails to operators, freeze dashboard, look at DAG logs to figure out what went wrong
Make it available to 100+ people

Redshift with auto-scaling capabilities and good read performance
Cassandra with pre-defined indexes to optimize read queries




