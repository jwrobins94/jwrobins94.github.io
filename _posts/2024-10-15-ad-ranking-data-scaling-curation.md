---
layout: post
title: "Data Curation and Scaling in Ad Ranking"
date: 2024-10-15
categories: blog
---

"Data" has received a lot of attention over the last few years in the language modeling space, and much progress has been made in answering questions such as:
1. How do we curate the right dataset for our use case?
2. How does model quality scale with data volume?
3. How can we take an existing model and improve it using domain-specific data?
4. ...

In ad ranking systems, "data" is a comparatively underexplored topic. A couple publicly available works that touch on this include:
1. [Understanding Scaling Laws for Recommendation Models](https://arxiv.org/abs/2208.08489) from Meta
2. [Practical Lessons from Conversion Ads at Pinterest](https://aiconference.com/wp-content/uploads/2023/10/Aayush-Mudgal-Practical-Lessons-from-Conversion-Modeling-on-Pinterest-Ads_-AI-Conference_Aayush.pptx-1.pdf)

Below are a few questions that I feel are poorly answered in available literature on ad ranking systems:
1. How does model quality improve as we accumulate more data for an advertiser?
2. How do misconfigured data integrations and spam affect model quality for other advertisers?
3. What is the impact of (nearly) duplicate impressions in training data?
4. Are there opportunities to better specialize models for high-value advertisers without introducing excessive overhead?

These are expanded on in the sections below.

> How does model quality improve as we accumulate more data for an advertiser?

A common question that ad ranking teams may face is "how much data do we need from this advertiser for the model to learn?" The answer to this question has important implications on which products we recommend to an advertiser, how much we recommend they spend, and how long they should wait before they should expect to meet their KPIs.

Let's break this down.

"how much data do we need": There are a few dimensions by which we can think about this:
1. Total accumulated data volume
2. Rate of data accumulation (~= fraction of dataset from this advertiser)
3. Signal per unit of data volume (e.g. click-through rate)

It is reasonable to expect that a conversion prediction model performs best for advertisers who receive lots of impressions per day, have a high conversion rate (e.g. lots of clicks), and who have been in such a state for a long time.

"from this advertiser": We are usually not in a "true" cold start scenario for advertisers. When a new advertiser joins an advertising platform, it is likely that there already exists other advertisers like them on the platform (e.g. same industry, advertising similar products). Existing advertisers will also frequently start new ad campaigns, but again, these campaigns aren't 100% new -- the conversion patterns that the model has learned from that advertiser's other campaigns should still be useful here.

"for the model to learn": Metrics like AUC-ROC or normalized cross entropy are typically used as offline metrics to measure the quality of ad ranking models. These metrics tend to correlate _directionally_ with the online metrics that the advertiser cares about, such as cost-per-conversion and total conversions.

> How do misconfigured data integrations and spam affect model quality for other advertisers?

TODO misconfigured pixels, e.g. page view sent as purchase
purchase can mean "free trial" or "buy something expensive"

> What is the impact of (nearly) duplicate impressions in training data?

TODO many negatives from the same user, spread across different batches, repetition caps and penalties