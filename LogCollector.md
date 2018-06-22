

  目前业界比较流行的日志采集主要有Fluentd、Logstash、Flume、scribe等，阿里巴巴内部则是LogAgent、阿里云则是LogTail，这些产品中Fluentd占据了绝对的优势并成功入驻CNCF阵营，它提出的统一日志层(Unified Logging Layer)大大的减少了整个日志采集和分析的复杂度。

  Logstash和Fluentd类似是属于ELK技术栈，在业界也被广泛使用，关于两者的对比可以参考这篇文章[Fluentd vs. Logstash: A Comparison of Log Collectors](https://logz.io/blog/fluentd-logstash/)
