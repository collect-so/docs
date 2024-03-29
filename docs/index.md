---
sidebar_position: 1
---
# Introduction

:::info

Please bear in mind that Collect is actively under development. If you encounter any issues while using it, we kindly 
request that you notify us through any available channels or contact us directly at 
[artemiy@collect.so](mailto:artemiy@collect.so).

:::


## TL;DR

The core concept is simple: send <u>any data</u> to Collect, and effortlessly retrieve it as if a dedicated backend team had
spent months building the APIs. Collect automatically normalizes and labels incoming data, mapping it seamlessly to
[Dynamic APIs](/api-reference/dynamic-apis).

Here are the steps to get started:

1. Begin by creating a new project in the [Dashboard](https://app.collect.so).
2. Once your project is set up, obtain an SDK Token from the Project page.
3. Use this token to submit any data to either `https://api.collect.so/api/v1/collect/json` or
   `https://api.collect.so/api/v1/collect/csv`. [Import API Documentation](/api-reference/import)
4. Retrieve your data by making requests to `https://api.collect.so/api/v1/record/search`.
   [Search API Documentation](/core-concepts/search)

## Let's start

Welcome to Collect, a powerful all-in-one data-warehouse and API-first solution that designed to streamline and
improve data management to meet the complex data storage needs of modern applications and data-analysis challenges.

Collect allows to efficiently store and retrieve data without the need for complex processes like normalizations, type
definitions, scheming, or migrations. It ensures high availability, making it suitable even for handling large datasets
and ambitious projects.

Collect ensures that every piece of data is readily accessible through robust,
dynamically generated APIs, instantly available to empower your applications. Additionally, Collect provides suggestions
for data types and effectively maps them onto the graph, enhancing data management and retrieval capabilities.

## Purpose
Collect serves two primary objectives:

1. To significantly reduce costs in rapidly changing API development requirements, especially in startups where time
   matters. It does this by using semi-strong typed graph structures to make data normalization completely automated.
   It also lets you build powerful, fast and reliable APIs by mapping data to instantly assigned endpoints.

2. To consolidate meaningful data into a singular repository, providing a unified platform
   for both API development and granting accessibility to developers, data analysts, sales managers, and decision-makers.
   This platform offers a dashboard where collected data can be comprehensively interpreted to cater to diverse business needs.

Both of these objectives are realized through several essential components crafted within Collect's core. Check out 
the [Core Concepts](/core-concepts/) section.

## How this achieved?
Collect achieves its flexibility through a unique hybrid approach to data storage by harnessing the capabilities of 
Neo4j's graph database, extending beyond its original features. This approach combines the
strengths of a strongly typed graph on one hand while offering the flexibility of saving data within the built-in
mechanisms of dynamically extending graph structures on the other.

Internal algorithms, including customized BFS (Breadth-First Search) and batching techniques, are intentionally crafted
for efficiency and speed. Their primary goal is to provide an outstanding user and developer experience, ensuring smooth
and high-speed operations throughout the platform.

In this documentation, you will find comprehensive information on how to use Collect to its fullest potential,
from getting started to advanced features.