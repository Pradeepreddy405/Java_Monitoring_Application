# Java EC2 Monitoring Dashboard

A beginner-friendly Spring Boot project that displays basic Linux/EC2 system metrics in a browser.

## Features

- CPU usage
- Memory usage
- Root filesystem usage
- System uptime
- CPU load average
- JSON health/metrics API
- Browser dashboard that refreshes every 60 seconds

## Requirements

- Java 21
- Maven 3.8+
- Linux EC2 instance (Amazon Linux 2023 recommended)

## Project structure

```text
java-ec2-monitoring/
├── README.md
├── logs/
│   └── .gitkeep
├── pom.xml
└── src/
    └── main/
        ├── java/com/devops/monitoring/
        │   ├── MonitoringApplication.java
        │   ├── controller/MonitoringController.java
        │   ├── model/SystemMetrics.java
        │   └── service/SystemMonitoringService.java
        └── resources/
            ├── application.properties
            ├── static/css/style.css
            ├── static/js/dashboard.js
            └── templates/dashboard.html
```

## 1. Build the application

From the project root:

```bash
mvn clean package
```

The executable JAR will be created at:

```text
target/ec2-monitoring-dashboard-1.0.0.jar
```

## 2. Run it

```bash
java -jar target/ec2-monitoring-dashboard-1.0.0.jar
```

Open:

```text
http://EC2_PUBLIC_IP:8080
```

Make sure the EC2 security group allows inbound TCP port `8080` from your trusted IP address. Avoid exposing the dashboard publicly without authentication and network restrictions.

## 3. API endpoints

| Endpoint | Purpose |
|---|---|
| `/` | Monitoring dashboard |
| `/api/metrics` | Current system metrics as JSON |
| `/api/ping` | Simple application liveness response |

Example:

```bash
curl http://localhost:8080/api/ping
curl http://localhost:8080/api/metrics
```

## 4. Run in the background

```bash
nohup java -jar target/ec2-monitoring-dashboard-1.0.0.jar > logs/application.log 2>&1 &
```

View logs:

```bash
tail -f logs/application.log
```

Find the process:

```bash
ps -ef | grep '[e]c2-monitoring-dashboard'
```

Stop it gracefully using its PID:

```bash
kill <PID>
```

## Notes

- OSHI is used to read operating-system metrics. CPU usage is sampled over a short interval, so the metrics endpoint may take a moment to respond.
- Filesystem usage is reported for the root mount point (`/`).
- This is a learning project, not a full production monitoring platform. For production, add authentication, TLS, access controls, alerting, and a metrics backend such as Prometheus.
