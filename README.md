# College-Football-Stat-Leaders
This project automatically ingests weekly college football player statistics from  the CollegeFootballData API, stores them in BigQuery, and displays weekly statistical leaders through a Flask web application hosted on a VM from Compute Engine. 

# Project Overview
This project builds an automated cloud-based data pipeline that collects, updates, and displays weekly college football player statistics. This system ingests data from CollegeFootballData API, loads it into BigQuery, computes weekly leaders using SQL views, and presents the results on a webpage. The goal of this project was to demonstrate how several cloud services can work together in order to create a fully functional application.

# Google Cloud Services Used
- Compute Engine
- Cloud Function
- BigQuery
- Cloud Scheduler

# Compute Engine
- Hosts the Flask web application (cfb_app.py)
- Hosts the HTML page and Python backend
- Pulls the latest data from BigQuery to display on the weekly leaderboards

# Cloud Function
- Function which retrieves data from CollegeFootballData API
- Inserts this data into my BigQuery table
- Triggered automatically by Cloud Scheduler

# BigQuery
- Acts as the centralized database for all player statistics
- Created several SQL views inside BigQuery to organize the data and calculate the latest week of results

# Cloud Scheduler
- Automatically triggers my Cloud Function once a week
- Sends an HTTP request to the Cloud Function on a timed schedule
- Ensures the database updates consistently
- Cron schedule: 0 4 * * 0

# How Do These Services Work Together?
- Cloud Scheduler first triggers my Cloud Function to run
- My Cloud Function then calls the CollegeFootballData API and inserts updated statistics into BigQuery
- BigQuery stores all the data and uses SQL views to compute the weekly leaderboards
- The Compute Engine VM hosts the Flask application that queries these BigQuery views
- The end user then sees the updated, accurate weekly stats on the web page

# Application Setup
1. Create a Flask application to host your web page
- Flask application must include cfb_app.py, requirements.txt, and index.html files
- cfb_app.py - the main Flask server code
- requirements.txt - a list of Python dependencies
- index.html - the main HTML page for displaying weekly leaders

2. Configure the Flask application to connect to BigQuery
- This application uses SQL queries against BigQuery views that compute weekly leaders
- Initialize BigQuery client in cfb_app.py
- Write SQL queries to call BigQuery views
- Convert results into Python dictionaries
- Pass the results into index.html using Flask's render_template() function

3. Create Cloud Scheduler Job
- Set up a Cloud Scheduler job so that your ingestion function (main.py) runs automatically. This keeps the BigQuery data and web page up-to-date without any manual work
- In the Google Cloud Console, navigate to Cloud Scheduler to create a job
- Configure the job to run at the desired day/time. I configured mine to run at 4:00 AM every Sunday (0 4 * * 0)
- Set the Time Zone to your local zone ( for example, America/Detroit)
- Set the URL method to GET
- After the job is created, it will automatically trigger your Cloud Function at the desired date/time which will then update the BigQuery table

4. Prepare the Compute Engine VM
- Install Python, Flask, gunicorn, and Google Cloud libraries
- Upload the project files to the VM
- Install any dependencies
- Ensure that firewall rules allow HTTP traffic (or another chosen port)

5. Launch the Flask application
- Once the application files are ready, start the application on the VM
- Visit the VM's external IP address to view the web page
- If the data appears empty, make sure your Cloud Function ran properly, and that your SQL views are working as expected
