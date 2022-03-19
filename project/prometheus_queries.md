## Availability SLI

### The percentage of successful requests over the last 5m

sum(rate(flask_http_request_total{instance="13.59.232.90:80",code!~"5.."}[2d])) / sum(rate(flask_http_request_total{instance="13.59.232.90:80"}[2d]))

## Latency SLI

### 90% of requests finish in these times

histogram_quantile(0.9, sum(rate(flask_http_request_duration_seconds_bucket{instance="13.59.232.90:80"}[5m])) by (le,verb))

## Throughput

### Successful requests per second

sum(rate(flask_http_request_total{instance="13.59.232.90:80",status=~"2.."}[5m]))

## Error Budget - Remaining Error Budget

### The error budget is 20%

1 - ((1 - (sum(increase(flask_http_request_total{instance="13.59.232.90:80", code="200"}[7d])) by (verb)) / sum(increase(flask_http_request_total{instance="13.59.232.90:80"}[7d])) by (verb)) / (1-0.8))
