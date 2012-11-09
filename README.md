LogEntries Logs Downloader
====================

A simple Bash script to download your logs for backup purpose, thanks to LogEntries API.

Requirements
---------------------

You need to have **cURL** (http://curl.haxx.se/) installed on your machine.
Moreover, **mktemp** (http://www.mktemp.org/) and **readlink** must be available in your distribution.

Installation
---------------------
To install the script, just clone this repository somewhere:

    git clone https://github.com/simpleit/logentries-logs-downloader.git
    git checkout v0.1

or

    wget https://github.com/simpleit/logentries-logs-downloader/archive/v0.1.tar.gz logentries-logs-downloader-v0.1.tar.gz
    tar zxvf logentries-logs-downloader-v0.1.tar.gz

You can add `logentries-logs-downloader/logentries-downloader` script to your PATH for easier access.

Usage
---------------------

    usage: logentries-downloader options

    OPTIONS:
       -h               Show this message
       -s timestamp     Start date (in seconds since epoch)
       -e timestamp     End date (in seconds since epoch)
       -y               Get yesterday logs (replace -s & -e options)
       -a account_key   [Optional] Logentries account key
       -l log_addr      [Optional] Logentries log address
       -f filter        [Optional] Filter to use
       -c count         [Optional] Maximal number of events downloaded
       -q               [Optional] Quiet mode: Do not display anything 
                        other than errors

The `account_key` and `log_addr` can be optional in this conditions:
*   The relevant variables are set in the configuration file (see below)
*   The relevant environment variables are set (see below)

### Use the configuration file  
A configuration file can be use together with the script file. It's a good way if you allways want to get logs from the same Logentires account and/or the logs sent by the same web app.

The configuration file must be named `config` and be placed in the script installation directory (you can copy the `config.sample` file and fill it with your options).

This two options can be set in the `config` file : `ACCOUNT_KEY` and `LOG_ADDR`, witch respectively sets the LogEntries Account Key and the LogEntries Log Adress (aka the server you want to download logs from. You can get this info in your LogEntries account).

Example:

    ACCOUNT_KEY=49cec5a8-01e1-4461-abbf-c20c7e7faad1
    LOG_ADDR=hosts/Web/e1a5f74f-a087-4aa5-844c-f94d7adf41a5

### Use environment variable

You can also use environment variables to set the LogEntries account and the LogEntries log address.
Sometime, depending of your use case, it can be convenient than configuration file or script arguments.

The available environment variables are :
*   `LOGENTRIES_ACCOUNT_KEY`
*   `LOGENTRIES_LOG_ADDR`

Example:

    export LOGENTRIES_ACCOUNT_KEY=49cec5a8-01e1-4461-abbf-c20c7e7faad1
    export LOG_ADDR=/Web/e1a5f74f-a087-4aa5-844c-f94d7adf41a5
    logentries-downloader -y -f /login -c 10

### Usage Examples

**Get the first 20 log entries witch contains "login", for the given account and log address, between timestamp 1347034650000 and 1347034939000**

    logentries-downloader -s 1347034650000	-e 1347034939000 -a 49cec5a8-01e1-4461-abbf-c20c7e7faad1 -l /Web/e1a5f74f-a087-4aa5-844c-f94d7adf41a5 -f /login -c 20

**Get all log entries between timestamp 1347034650000 and 1347034939000** *(with account and log address sets in config file or environment variables)*

    logentries-downloader -s 1347034650000	-e 1347034939000

**Get all yesterday (12:00:00 AM to 11:59:59 PM) log entries, and just output logs and errors (perfect for CRON job)** *(with account and log address sets in config file or environment variables)*

    logentries-downloader -y -q > yesterday.log 2> error.log
