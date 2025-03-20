---
layout: default
title: Search options
nav_order: 5
has_children: true
has_toc: false
redirect_from: /opensearch/ux/
---

# Search options

OpenSearch provides a variety of search options to enhance the search experience and meet diverse user needs. This page outlines the available search features and provides links to detailed documentation for each.

## Overview of search options

| Option | Description |
|--------|-------------|
| [Autocomplete functionality](#autocomplete-functionality) | Suggest phrases as the user types |
| [Did-you-mean functionality](#did-you-mean-functionality) | Check spelling of phrases as the user types |
| [Paginate results](#paginate-results) | Separate search results into pages |
| [Point in Time](#point-in-time) | Run different queries against a dataset fixed in time |
| [Sort results](#sort-results) | Allow sorting of results by different criteria |
| [Filter results](#filter-results) | Filter search results |
| [Collapse results](#collapse-results) | Collapse search results |
| [Highlight query matches](#highlight-query-matches) | Highlight the search term in the results |
| [Retrieve inner hits](#retrieve-inner-hits) | Retrieve underlying hits in nested and parent-join objects |
| [Retrieve specific fields](#retrieve-specific-fields) | Retrieve only specific fields |

## Autocomplete functionality

Autocomplete suggests phrases as the user types, improving the search experience by helping users find relevant content more quickly. This feature can be particularly useful for large datasets or complex search scenarios.

For more information, see [Autocomplete functionality]({{site.url}}{{site.baseurl}}/opensearch/search/autocomplete/).

## Did-you-mean functionality

The did-you-mean feature checks the spelling of phrases as the user types, offering suggestions for potential misspellings. This helps users correct errors and find relevant results even when their initial query contains typos or incorrect spellings.

For more information, see [Did-you-mean functionality]({{site.url}}{{site.baseurl}}/opensearch/search/did-you-mean/).

## Paginate results

Instead of returning a single, long list of results, pagination separates search results into pages. This improves the user experience by making large result sets more manageable and easier to navigate.

For more information, see [Paginate results]({{site.url}}{{site.baseurl}}/opensearch/search/paginate/).

## Point in Time

Point in Time (PIT) allows you to run different queries against a dataset that is fixed in time. This feature is useful for scenarios where you need consistent results across multiple queries or for implementing features like "scroll" pagination.

For more information, see [Point in Time]({{site.url}}{{site.baseurl}}/search-plugins/searching-data/point-in-time/).

## Sort results

Sorting allows users to order search results based on different criteria. This feature enhances the search experience by enabling users to prioritize results according to their specific needs or preferences.

For more information, see [Sort results]({{site.url}}{{site.baseurl}}/opensearch/search/sort/).

## Filter results

Filtering allows users to narrow down search results based on specific criteria. This feature helps users quickly find relevant information within large datasets.

For more information, see [Filter results]({{site.url}}{{site.baseurl}}/search-plugins/filter-search/).

## Collapse results

Result collapsing groups similar results together, reducing redundancy and improving the diversity of search results presented to the user.

For more information, see [Collapse results]({{site.url}}{{site.baseurl}}/search-plugins/collapse-search/).

## Highlight query matches

Highlighting emphasizes the search terms within the results, making it easier for users to identify why a particular result is relevant to their query.

For more information, see [Highlight query matches]({{site.url}}{{site.baseurl}}/opensearch/search/highlight/).

## Retrieve inner hits

The inner hits feature allows retrieval of the underlying hits in nested and parent-join objects. This is particularly useful when working with complex, nested document structures.

For more information, see [Retrieve inner hits]({{site.url}}{{site.baseurl}}/search-plugins/searching-data/inner-hits/).

## Retrieve specific fields

This option allows you to retrieve only specific fields from the search results, optimizing performance and reducing unnecessary data transfer.

For more information, see [Retrieve specific fields]({{site.url}}{{site.baseurl}}/search-plugins/searching-data/retrieve-specific-fields/).
