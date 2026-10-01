---
layout: page
title: "Case Studies"
permalink: /case-studies/
description: "Sanitized case studies from production distributed-systems engagements covering Kafka, Cassandra, Spark and reporting platform scalability."
---

<p class="section-lede">
  Sanitized accounts of real engagements. Company, customer and internal project details are
  removed. The focus is the diagnostic process and the architectural reasoning, since that is
  the part that transfers.
</p>

<p>
  No performance metrics are quoted where the exact figures are not known. A plausible-sounding
  number is worse than no number when you are trying to learn from it.
</p>

<h2 id="kafka-cassandra-commit-throughput">A commit pipeline that was too slow</h2>

<p><strong>Stack:</strong> Scala, Akka/Pekko, Kafka, Cassandra, Docker</p>

<h3>Problem</h3>
<p>
  Commit rate on a production event-driven platform was too slow to keep up with the workload
  arriving on the topic.
</p>

<h3>Investigation</h3>
<p>
  The investigation did not point at a single component. Four contributing factors stacked up
  on the same path:
</p>

<ul>
  <li>synchronous thread blocking in the application</li>
  <li>inefficient Kafka topic configuration</li>
  <li>a single Kafka partition, which capped parallelism</li>
  <li>a Cassandra cache pattern of quick reads, writes and deletes</li>
</ul>

<pre class="flow-diagram">Application
    &darr;
Synchronous thread blocking
    &darr;
Kafka
    &darr;
Single partition
    &darr;
Cassandra
    &darr;
Cache read/write/delete</pre>

<p>
  The useful conclusion was not any individual fix. It was that the bottleneck crossed several
  system layers, so no single component's metrics would have revealed it. The single partition
  in particular looked like a configuration detail until it was placed next to the blocking
  calls upstream of it.
</p>

<hr>

<h2 id="kafka-lag-cassandra-tombstones">Kafka lag traced back to Cassandra tombstones</h2>

<p><strong>Stack:</strong> Scala, Akka/Pekko, Kafka, Cassandra, Docker</p>

<h3>Problem</h3>
<p>
  Kafka consumer lag kept increasing. The natural assumption was that the consumers were too
  slow, or that the producers were too fast.
</p>

<h3>Investigation</h3>
<p>
  Consumer throughput was not the origin of the problem. Downstream Cassandra read performance
  had degraded because expired rows were being deleted individually, producing a large number
  of tombstones. Slower reads reduced processing throughput, which is what consumer lag was
  actually measuring.
</p>

<pre class="flow-diagram">Kafka lag
    &darr;
Processing throughput
    &darr;
Cassandra read performance
    &darr;
Tombstones
    &darr;
Data lifecycle/deletion strategy
    &darr;
Data model redesign</pre>

<h3>Resolution</h3>
<p>
  The data model was changed to a week-based partitioning and data lifecycle strategy. Instead
  of deleting expired rows one at a time, the previous week's data could be removed with the
  primary-key operation that matched the partitioning.
</p>

<p>
  The general lesson: when a lagging queue is the visible symptom, look at what the consumers
  are actually waiting on before tuning the consumers.
</p>

<hr>

<h2 id="reporting-system-at-scale">A reporting system that failed at scale</h2>

<p><strong>Stack:</strong> Java, MySQL, BIRT, Servlets</p>

<h3>Problem</h3>
<p>
  An MIS and reporting system worked fine with small datasets and became effectively unusable
  in production with large customer datasets.
</p>

<h3>Resolution</h3>
<ul>
  <li>restricted reporting data to a one-month window instead of querying indefinitely</li>
  <li>made report generation asynchronous, so a user could submit a request and return later to collect the generated file</li>
  <li>added MySQL indexing</li>
  <li>divided database and data handling for large customers</li>
</ul>

<h3>Architectural lesson</h3>
<p>
  Scaling this system required changing the data-access model and the user workflow, not merely
  optimising queries. Making report generation asynchronous changed the shape of the problem
  rather than shaving time off it, and the one-month window was a data-model boundary rather
  than a filter.
</p>