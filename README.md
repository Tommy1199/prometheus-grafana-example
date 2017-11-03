This is a little playground to see how prometheus and grafana can be used to monitor the state of systemd services. This setup is made up of 4 parts

<dl>
  <dt>Dummy Services</dt>
  <dd>10 systemd services which can be used to demonstrate their state's visualization on a Grafana's dashboard. They are called dummy1 to dummy10.</dd>
  <dt>Node Exporter</dt>
  <dd>A metrics collector application for collecting system metrics like CPU, Memory or in this case state of the systemd services. It populates these metrics via a http endpoint.</dd>
  <dt>Prometheus</dt>
  <dd>A monitoring toolkit for scraping data from several metric sources and storing it in a time series database to make queryable.</dd>
  <dt>Grafana</dt>
  <dd>A visualization frontend for monitoring and analysis purposes.</dd>
</dl>

### Prerequistes

1. [Install Virtualbox](https://www.virtualbox.org/)
2. [Install Vagrant](https://www.vagrantup.com/)

### Startup

Clone the repo, go into the project's directory and run

```
vagrant up
```

That's it. During the startup of the VM the necessary applications will be installed via [Ansible](http://docs.ansible.com/ansible/latest/index.html). After the VM is started there are several http endpoints to browse to check that everything is running correctly.

<dl>
  <dt>http://localhost:9100/metrics</dt>
  <dd>This is the http endpoint of the Node Exporter. This shows all metrics provided, also the systemd services' states.</dd>
  <dt>http://localhost:9090/targets</dt>
  <dd>Displays all sources prometheus scrapes metrics from. As prometheus itself is also a source for metrics and prometheus is configured to scrape the data from itself as well, it can be seen here. (see prometheus.yml to see how sources can be configured)</dd>
  <dt>http://localhost:9090/graph</dt>
  <dd>Can be used to try out some queries directly with prometheus. It can display it in a small graph or in a console.</dd>
  <dt>http://localhost:3000/dashboard/db/dummy-services-overview?refresh=5s&orgId=1</dt>
  <dd>Login with admin/admin. It displays the final dashboard displaying the state of the 10 services and a graph to display the state over the last hours.</dd>
</dl>

Now as everything is up and running, you can play a bit with the services to see how the dashboard will change its state. You can login to the machine with

```
vagrant ssh
```

And then you can shutdown some of the dummy services with

```
sudo systemctl stop dummy1 dummy2
```

or kill one of the process by checking the pid with

```
sudo systemctl status dummy6
```

and then

```
sudo kill -9 PID
```

All of these should have an impact on the dashboard. To start up the services again, call

```
sudo systemctl start dummy1
```