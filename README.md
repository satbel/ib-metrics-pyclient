# IBM IB metrics exporter

[![Build Status](https://travis-ci.com/AATools/ib-metrics-pyclient.svg?branch=master)](https://travis-ci.com/AATools/ib-metrics-pyclient) [![Coverage Status](https://coveralls.io/repos/github/AATools/ib-metrics-pyclient/badge.svg?branch=master)](https://coveralls.io/github/AATools/ib-metrics-pyclient?branch=master)

This is python client for collecting IBM Integration Bus metrics and exporting to [Prometheus pushgateway](https://github.com/prometheus/pushgateway).
The collected metrics can be explored in Prometheus or Grafana.

The metrics are collected using [mqsilist](https://www.ibm.com/support/knowledgecenter/en/SSMKHH_9.0.0/com.ibm.etools.mft.doc/an07250_.htm) command. So, you need to install `IBM Integration Bus`.

Tested for IBM IB v9 and Python 2.6, 2.7, 3.6 on Linux.

## Collected metrics

By default, metrics are collected every 60 seconds.

The metrics provided by the client:

* `ib_broker_status...` - current status of IB broker;
* `ib_exec_group_status...` - current status of IB execution group;
* `ib_application_status...` - current status of IB application;
* `ib_message_flow_status...` -  current status of IB message flow.


See [detailed description of the metrics](#metrics-detailed-description) for an in-depth understanding.

You can run `IB metrics pyclient` and [MQ metrics pyclient](https://github.com/AATools/mq-metrics-pyclient) together. Metrics from both clients will be sent to the same pushgateway. Conflicts will not arise.

## Getting Started

Download Prometheus Pushgateway from the [release page](https://github.com/prometheus/pushgateway/releases) and unpack the tarball.

### Run Prometheus Pushgateway

```bash
cd pushgateway
nohup ./pushgateway > pushgateway.log &
```

For Pushgateway the default port is used (":9091").

### Run ib-metrics-pyclient

```bash
git clone https://github.com/AATools/ib-metrics-pyclient
cd ib-metrics-pyclient
nohup python3 iib_metrics_client.py &
```

After that, you should set up your Prometheus server to collect metrics from Pushgateway (`http://<hostname>:9091/metrics`).

## Grafana dashboard

The Grafana dashboard visualizes collected metrics. This is an example of a dashboard. You can create your own dashboards to analyze metrics.

## Simple process scheme

![ib_metrics_pyclient_scheme](../images/ib_metrics_pyclient_scheme.jpg?raw=true)

## Metrics detailed description

| Metric | Description |
|:---|:---|
| ib_broker_status | The metric shows current status of IB broker.<br> Metric type: `gauge`.<br> If there are several brokers on host, there will be a own metric for each broker.<br> Possible values:<br> <span style="margin-left:2em">`0` - STOPPED;</span><br> <span style="margin-left:2em">`1` - RUNNING.</span><br> Example display in Pushgateway:<br> `ib_broker_status{brokername="BRK1",instance="",job="BRK1",qmname="QM1"} 1` |
| ib_exec_group_status | The metric shows current status of IB execution group.<br> Metric type: `gauge`.<br> If there are several execution groups on host, there will be a own metric for each execution group.<br> Possible values:<br> <span style="margin-left:2em">`0` - STOPPED;</span><br> <span style="margin-left:2em">`1` - RUNNING.</span><br> Example display in Pushgateway:<br> `ib_exec_group_status{brokername="BRK1",egname="EG1",instance="",job="BRK1"} 1` |
| ib_application_status | The metric shows current status of IB application.<br> Metric type: `gauge`.<br> If there are several applications on host, there will be a own metric for each application.<br> Possible values:<br> <span style="margin-left:2em">`0` - STOPPED;</span><br> <span style="margin-left:2em">`1` - RUNNING.</span><br> Example display in Pushgateway:<br> `ib_application_status{appname="Application1",brokername="BRK1",egname="EG1",instance="",job="BRK1"} 1` |
| ib_message_flow_status | The metric shows current status of IB message flow.<br> Metric type: `gauge`.<br> If there are several message flows on host, there will be a own metric for each message flow.<br> Possible values:<br> <span style="margin-left:2em">`0` - STOPPED;</span><br> <span style="margin-left:2em">`1` - RUNNING.</span><br> Example display in Pushgateway:<br> `ib_message_flow_status{appname="Application1",brokername="BRK1",egname="EG1",instance="",job="BRK1",messageflowname="adapter.reply"} 1` |
