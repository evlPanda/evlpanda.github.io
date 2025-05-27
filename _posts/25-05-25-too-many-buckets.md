---
layout: post
title:  "Insights / Kibana : Too Many Buckets"
date:   2025-05-25 00:00:00 +1000
categories: Kibana, Insights
blurb: How to increase bucket size.

---

So, you've run into a 'too_many_buckets_exception' error in one of your Kibana/Insights visualisations?

You can perhaps reduce the number of buckets you have in your visualisation, but usually they are all required by business.

The alternaitve is to simply increase the max_buckets parameter.

1. Shutdown Elasticsearch
2. Locate your elasticsearch.yml file which normally resides under: [ES_HOME]/config
3. Add/update the parameter: search.max_buckets: [new value]
4. Start Elasticsearch
