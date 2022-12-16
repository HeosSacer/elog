# WIP

-------------------------------------------------------


# eLog <img src="elog.png" width="60" height="60" alt=":log:" class="emoji" title=":log:"/> [![Build Status](https://github.com/HeosSacer/elog/workflows/CI/badge.svg)](https://github.com/HeosSacer/elog/actions?query=workflow%3ACI) [![Go Reference](https://pkg.go.dev/badge/github.com/sirupsen/logrus.svg)](https://pkg.go.dev/github.com/sirupsen/logrus)

Elog is wrapper around [logrus](https://github.com/sirupsen/logrus) to provide a 
quick way to setup logging to TTY, syslog, and more with some good defaults already included.


```text
time="2015-03-26T01:27:38-04:00" level=debug msg="Started observing beach" animal=walrus number=8
time="2015-03-26T01:27:38-04:00" level=info msg="A group of walrus emerges from the ocean" animal=walrus size=10
time="2015-03-26T01:27:38-04:00" level=warning msg="The group's number increased tremendously!" number=122 omg=true
time="2015-03-26T01:27:38-04:00" level=debug msg="Temperature changes" temperature=-4
time="2015-03-26T01:27:38-04:00" level=panic msg="It's over 9000!" animal=orca size=9009
time="2015-03-26T01:27:38-04:00" level=fatal msg="The ice breaks!" err=&{0x2082280c0 map[animal:orca size:9009] 2015-03-26 01:27:38.441574009 -0400 EDT panic It's over 9000!} number=100 omg=true
```
To ensure this behaviour even if a TTY is attached, set your formatter as follows:

```go
	log.SetFormatter(&log.TextFormatter{
		DisableColors: true,
		FullTimestamp: true,
	})
```

#### Logging Method Name

If you wish to add the calling method as a field, instruct the logger via:
```go
log.SetReportCaller(true)
```
This adds the caller as 'method' like so:

```json
{"animal":"penguin","level":"fatal","method":"github.com/sirupsen/arcticcreatures.migrate","msg":"a penguin swims by",
"time":"2014-03-10 19:57:38.562543129 -0400 EDT"}
```

```text
time="2015-03-26T01:27:38-04:00" level=fatal method=github.com/sirupsen/arcticcreatures.migrate msg="a penguin swims by" animal=penguin
```
Note that this does add measurable overhead - the cost will depend on the version of Go, but is
between 20 and 40% in recent tests with 1.6 and 1.7.  You can validate this in your
environment via benchmarks: 
```
go test -bench=.*CallerTracing
```


