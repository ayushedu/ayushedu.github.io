---
layout: post
title:  "Stop Designing Databases Around Write Throughput Alone"
date:   2026-10-02 04:04:04
author: Ayush Vatsyayan
categories: system-design
tags:	    systemDesign
---

While designing a simple feature or application, many people jump quickly from functional requirements directly to implemenation. While some may consider the non functional requirements, but very few will consider non-functional requirements but that too only limited. 

I've seen this happen in large organizations: teams are under pressure to deliver a feature quickly, so the design discussion is kept brief—or happens entirely verbally. There is no design document, no detailed capacity analysis, and no systematic discussion of how the system will behave as the customer, data, and retention period grow. And if they do consider non-functional they will limit themselves to write throughput.

Once the feature gets implemented, the tests pass, and everything appears to work.But the problem is that a **system that receives a relatively modest number of writes per second can still become an enormous system if those writes are retained for years.** 

And that's a design constraint we often overlook. When designing a database-backed application, one of the first questions we usually ask is: 

> **“How many writes per second does this system need to handle?”**. 

That's an important question. But it's not the only one.

---
## The problem doesn't appear on day one

Initially, everyone is happy. 
The feature works as expected. The customer creates data, retrieves it, and everything looks fine.

But then time passes, the customer grows and along with grows the amount of data. Eventually, the database starts claiming significantly more disk space than anyone anticipated. 
At some point, the customer may be forced to make an uncomfortable choice: delete valuable historical data or reduce the retention period.

Imagine the customer originally expected to retain a year's worth of data, but the system can realistically handle only a month's worth without running into storage problems.At that point, the question isn't simply:

“Why did the database get so large?”

The deeper question is:

“Did we design the feature correctly in the first place?”

---
## Retention is a functional requirement

This is where functional and non-functional requirements become closely connected.

Consider the requirement:

    “The customer needs access to historical data.”

That sounds functional. But we need to clarify it. How much historical data?

    Seven days?

    One month?

    One year?

    Five years?

    Ten years?

If the customer expects one year of historical data, then one year of retention is part of the functional requirement. Now comes the non-functional question:

    How much storage will we need to support that retention period at the expected scale?

That's a capacity and scalability requirement. The two questions cannot really be separated. 

Consider the below example:

## A URL shortener is a perfect example
Imagine we're designing a URL-shortening service. Our requirements are:
 - 100 million new URLs every day
- URLs are retained for 10 years
- Each URL needs to be retrievable using its short code

 At first glance, the write throughput doesn't look particularly scary.  
 Let's calculate it:

-  100 million URLs per day means:
 **100,000,000 / 86,400 ≈ 1,157 writes/second**
- Even if we design for a 10× traffic spike, we're looking at roughly:
 **11,570 writes/second**

 That's certainly a substantial workload, but it's not a number that immediately tells us:
 > “We absolutely need a distributed database.”

 A properly designed PostgreSQL system can handle significant write throughput. So we might be tempted to say:
 > “Let's use PostgreSQL.”

 And that might be the right answer. But we're missing another number.

## How much data are we keeping?
 The requirement says we retain URLs for 10 years. So:
 **100 million URLs/day × 365 days × 10 years**

 gives us approximately:
 **365 billion URLs.**

 That's a very different problem. Our write rate was only around 1,157 writes per second. But our eventual dataset contains **365 billion records**.
 
 This is the part of the problem that can fundamentally change the architecture.

---

## Throughput and storage are different dimensions

It's useful to separate two questions:

### Question 1: How quickly does data arrive?

That's our **throughput** problem.

```
1,157 writes/sec
```

### Question 2: How much data will we eventually have?

That's our **storage** problem.

```
365 billion records
```

These aren't interchangeable.

A system can have:

```
Low write throughput
+
Long retention
=
Huge dataset
```

And that's exactly what we're seeing here.

---

## Let's estimate the storage

Suppose each URL record requires roughly 150 bytes, most people will consider this as the final data size. But what they are missing is the additional columns required to persist this info in DB along with metadata, row overhead, indexes, and other database overhead. This 150kb could change to 300 or 400kb. For e.g.

For e.g. we will need below minimal schema:

|Name|Type|Size (bytes)|
| :--- | :---: | ---: |
|user_id|UUID|16|
|long_url|String|150|
|short_url|String|8|
|created_at|Date/Time|8|
|expired_at|Date/Time|8|
|row overhead||70-200|
| **Total**|| **260-390** |
{:.table .table-striped}

Do note that this isn't an exact number—it depends heavily on the database and schema—but it's useful for capacity planning.
Then:

```
365 billion × 300 bytes
≈ 109.5 TB
```

That's before considering replication.

With three copies:

```
109.5 TB × 3
≈ 571.5 TB
```

And in a real production system we'd need additional capacity for things such as:

- Compaction
- Free space
- Backups
- Replication
- Indexes
- Logs
- Operational headroom
- Temporary storage during maintenance

Suddenly, our seemingly simple URL shortener is a **hundreds-of-terabytes system**.

That's the important realization.

---

# Why write throughput alone can mislead us

Suppose someone asks during a system-design interview:

> "How many writes per second do we have?"

You calculate:

```
100M/day
≈ 1.16K writes/sec
```

Then you might think:

> "That's manageable with PostgreSQL."

And from a pure throughput perspective, that may be true.

But imagine we stop the conversation there.

We haven't asked:

- How long are records retained?
- How large is each record?
- How large will indexes become?
- How much replication do we need?
- How much storage will we need in five years?
- How will backups work?
- How long will recovery take?
- Can we scale storage independently of compute?
- What happens when the database reaches hundreds of billions of records?

Those questions can completely change the architecture.